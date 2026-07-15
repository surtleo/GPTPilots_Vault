작성자 : 전재완
GCP VM(JupyterHub 멀티유저 환경)에서 팀원 각자의 계정으로 uv를 이용해 파이썬 패키지 환경을 세팅하는 가이드입니다.

---

## 배경: 왜 uv를 쓰는가

- VM의 공용 JupyterHub venv(`/opt/jhub-venv`)는 관리자 소유라 일반 계정으로 `pip install` 불가 (Permission denied)
- `-user` 설치도 이 venv에서는 막혀있음 (`Can not perform a '--user' install`)
- → **각자 계정 안에 자기만의 독립된 가상환경을 uv로 만들어서 해결**
- uv는 `requirements.txt`보다 나은 버전 관리를 제공: `pyproject.toml`(내가 쓰는 패키지) + `uv.lock`(하위 의존성까지 전부 고정) 조합으로, 팀원 전체가 **바이트 단위로 동일한 환경**을 재현 가능

---

## 1. uv 설치 (계정별 1회)

관리자 권한 불필요, 내 홈 디렉토리에 설치됨.

bash

```bash
curl -LsSf <https://astral.sh/uv/install.sh|shsource$HOME/.local/bin/env>
```

확인:

bash

```bash
uv --version
```

---

## 2. GitHub 인증 설정 (SSH 키, 계정별 1회)

GitHub는 비밀번호 인증을 지원하지 않으므로 SSH 키 필요.

bash

```bash
ssh-keygen -t ed25519 -C"본인_이메일@example.com"
```

- 파일 경로, 패스프레이즈 질문은 모두 엔터(기본값)로 진행

공개키 확인 및 복사:

bash

```bash
cat ~/.ssh/id_ed25519.pub
```

GitHub 등록: **Settings → SSH and GPG keys → New SSH key** → 복사한 내용 붙여넣기 → Add SSH key

연결 테스트:

bash

```bash
ssh -T git@github.com
```

`Hi <내_GitHub아이디>! You've successfully authenticated...` 메시지가 뜨면 성공.

---

## 3. Repo Clone (SSH 방식)

bash

```bash
git clone git@github.com:surtleo/GPTPilots_Project.gitcd GPTPilots_Project
```

> `https://` 주소로 clone 시도하면 비밀번호를 요구하다 인증 실패함. 반드시 `git@github.com:...` 형식(SSH 주소) 사용.

> ⚠️ **Repo 이름 변경 이력**: 원래 이름은 `sprint_11_2team_RAG_Project`였으나 팀장이 `GPTPilots_Project`로 변경함. 옛 이름으로 clone/push해도 GitHub가 자동 리다이렉트해주지만, 계속 옛 이름을 쓰면 나중에 혼동될 수 있으니 반드시 새 주소(`GPTPilots_Project`)로 clone할 것. 이미 옛 이름으로 clone해둔 경우 아래처럼 원격 주소만 갱신하면 됨:
> 
> bash
> 
> ```bash
> git remote set-url origin git@github.com:surtleo/GPTPilots_Project.gitgit remote -v# 정상 반영됐는지 확인
> ```

---

## 4. uv 프로젝트 초기화 (최초 1회만, repo에 커밋됨)

bash

```bash
uv init --python3.11 --no-readme
```

- `-python 3.11`: 파이썬 버전 고정 (VM 시스템 기본 3.14는 pandas 등 호환성 불안정 가능성 있어 회피)
- `-no-readme`: 기존 [README.md](http://README.md) 유지

생성되는 파일: `pyproject.toml`, `.python-version`, `main.py`

---

## 5. 패키지 추가

bash

```bash
uvadd pandas numpy matplotlib seaborn ipykernel
```

이 명령어가 하는 일:

1. `pyproject.toml`에 패키지 기록
2. `.venv` 폴더에 실제 설치 (최초 실행 시 가상환경 자동 생성)
3. `uv.lock` 생성 — 하위 의존성까지 전부 버전 고정

이후 새 패키지가 필요할 때마다:

bash

```bash
uvadd 패키지명
```

(예: `uv add langchain faiss-cpu`)

---

## 6. Git 최초 설정 & Commit/Push 방법 (계정별 1회 설정 필요)

이 VM 계정에서 Git을 처음 쓴다면, 커밋하기 전에 "너가 누구인지" 등록해야 함. 안 하면 `Author identity unknown` 에러가 남.

**1) 최초 1회, 이름/이메일 등록**

bash

```bash
git config --global user.name"본인_이름_또는_GitHub아이디"git config --global user.email"본인_GitHub_이메일@example.com"
```

**2) 바뀐 파일 확인**

bash

```bash
git status
```

`Untracked files` 또는 `modified` 항목이 뜸. (`.venv/`는 `.gitignore`에 이미 등록돼 있어 여기 안 뜨는 게 정상)

**3) 올릴 파일 선택 (staging)**

bash

```bash
gitadd pyproject.toml uv.lock .python-version main.py
```

> 파일명을 직접 나열하는 대신 `git add .`로 한 번에 올릴 수도 있지만, `.venv`처럼 무거운 폴더가 실수로 포함되지 않았는지 먼저 `git status`로 확인하는 습관을 추천.

**4) 커밋 (로컬에만 기록)**

bash

```bash
git commit -m"uv 환경 세팅: pyproject.toml, uv.lock 추가"
```

**5) Push (GitHub 서버로 전송)**

bash

```bash
git push
```

> `commit`은 로컬 기록, `push`는 그 기록을 GitHub로 실제 전송하는 것 — 둘은 다른 단계이며 `push`까지 해야 팀원들 화면에도 반영됨.

**6) 확인**  
GitHub repo 웹페이지 새로고침해서 `pyproject.toml`, `uv.lock` 등이 올라갔는지 확인.

---

## 7. Jupyter 커널로 등록

노트북에서 이 `.venv` 환경을 커널로 선택할 수 있게 등록:

bash

```bash
uv run python -m ipykernelinstall --user --name bidmate-uv --display-name"Python (BidMate - uv)"
```

이후 JupyterLab 노트북 우측 상단 커널 선택 메뉴에서 **"Python (BidMate - uv)"** 선택.

---

## 8. 실행 방법 두 가지

|방식|명령어|언제 쓰나|
|---|---|---|
|`uv run` 매번 붙이기|`uv run python src/baseline.py`|자동화 스크립트, 공유용 명령어 (환경 활성화 깜빡할 실수 방지)|
|가상환경 활성화|`source .venv/bin/activate` 후 `python src/baseline.py`, 끝나면 `deactivate`|터미널에서 반복적으로 스크립트 실행/테스트할 때 (매번 `uv run` 안 붙여도 됨)|

> 활성화는 터미널 세션이 꺼지면 초기화되므로, 새 터미널 열 때마다 다시 `source .venv/bin/activate` 필요.

---

## 9. 프로젝트 구조 (권장)

```
GPTPilots_Project/
├── .venv/              ← 패키지 창고 (Git에는 커밋 안 됨, .gitignore 처리)
├── pyproject.toml       ← 사용 패키지 목록 (Git 커밋)
├── uv.lock              ← 버전 고정 파일 (Git 커밋, 필수!)
├── notebooks/
│   └── eda.ipynb         ← EDA는 여기서, 커널은 "Python (BidMate - uv)" 선택
└── src/
    └── baseline.py       ← 베이스라인 모델은 `uv run python src/baseline.py`로 실행
```

- **노트북(EDA)과 스크립트(베이스라인) 모두 같은 `.venv` 하나만 공유하면 됨** — 따로 환경을 만들 필요 없음
- 패키지 추가는 `uv add`로 한 번만 하면 노트북/스크립트 양쪽에서 바로 사용 가능

---

## 10. 파이썬 버전 변경 (예: Colab과 버전 맞추기)

Colab 등 다른 실행 환경과 파이썬 버전을 통일하고 싶을 때, uv는 버전 자체를 자동으로 받아와서 손쉽게 바꿀 수 있음 (관리자 권한 불필요, uv 전용 영역에 설치됨).

**1) 원하는 버전 설치**

bash

```bash
uv pythoninstall3.12.13
```

**2) 프로젝트가 이 버전을 쓰도록 고정** (`.python-version` 파일 갱신)

bash

```bash
uv python pin3.12.13
```

**3) 기존 `.venv`(이전 버전 기준) 삭제 후 새 버전으로 재생성**

bash

```bash
rm -rf .venvuvsync
```

`uv.lock`에 기록된 패키지 목록을 새 파이썬 버전 기준으로 그대로 재설치함.

**4) 버전 확인**

bash

```bash
uv run python --version
```

**5) ⚠️ Jupyter 커널 재등록 필수**`.venv`를 삭제하고 새로 만들었기 때문에, 기존에 등록해둔 커널(`bidmate-uv`)이 옛 `.venv`를 가리키고 있어 깨져있음. 반드시 다시 등록:

bash

```bash
uv run python -m ipykernelinstall --user --name bidmate-uv --display-name"Python (BidMate - uv)"
```

(같은 이름으로 실행하면 자동으로 덮어써짐)

**6) 노트북에서 최종 확인**  
노트북 커널을 "Python (BidMate - uv)"로 선택 후:

python

```python
import sys, pandasas pd, matplotlibprint(sys.version)print(pd.__version__)print(matplotlib.__version__)
```

버전이 의도한 대로 출력되면 성공.

> 팀원들도 버전을 맞추려면 각자 계정에서 위 1~6단계를 동일하게 수행하거나, `pyproject.toml`의 `requires-python` 값을 프로젝트 표준으로 맞춰두고 `uv sync` 시 uv가 알아서 해당 버전을 요구하도록 관리하는 방법도 있음.

---

## 11. 팀원들이 각자 계정에서 따라야 할 순서 (요약)

1. 위 1~3단계 (uv 설치, SSH 키 설정, repo clone) — **계정별로 각자 1회**
2. repo 안에서 아래 명령어 **한 줄**이면 나머지는 자동:

bash

```bash
   uvsync
```

→ `pyproject.toml`/`uv.lock`을 읽어서 팀장/재완이 만든 것과 **완전히 동일한 환경**을 자동 구성함 (`uv init`, `uv add` 다시 할 필요 없음)  
3. Jupyter 커널 등록 (7단계) 각자 1회 실행

> 즉, 최초 세팅한 사람만 `uv init` + `uv add`를 하고, 나머지 팀원은 `uv sync` 한 줄로 끝.

---

## 참고: 자주 헷갈리는 개념 정리

- **Contributors 목록**: GitHub 협업자 초대를 수락해도, 실제 커밋을 올리기 전까지는 이 목록에 이름이 안 뜸 (정상 동작)
- **pip vs uv**: pip은 `requirements.txt`로 직접 쓰는 패키지만 기록하지만, uv의 `uv.lock`은 하위 의존성까지 전부 고정해서 재현성이 더 높음
- **`uv run` vs 그냥 `python`**: 가상환경을 활성화(`source .venv/bin/activate`)하지 않은 상태에서는 `python`이 시스템 기본 파이썬을 가리킴. `uv run`을 붙이면 활성화 여부와 상관없이 항상 프로젝트 `.venv`의 파이썬으로 실행됨