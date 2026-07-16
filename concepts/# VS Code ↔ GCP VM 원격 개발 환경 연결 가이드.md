ㅇ
####
배경

- 주피터랩 웹 UI 대신 VS Code에서 직접 VM에 접속해 작업하기 위한 설정
- VM에는 계정이 두 개 있음: `spai1122`(gcloud 계정), `jaewan3182`(JupyterHub 계정) — **실제 프로젝트 파일과 venv는 각자 배정받은 개인 계정(예: jaewan3182) 아래에 있음**

#### 1. 사전 준비

- VS Code 확장 설치: **Remote - SSH** (Microsoft)
- **Google Cloud CLI(gcloud)** 로컬 설치: [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)

#### 2. gcloud 로그인 및 프로젝트 설정

powershell

```powershell
gcloud init
```

- Google 계정 로그인 (GCP 프로젝트 접근 권한 있는 계정)
- 프로젝트 선택: `sprint-ai-chunk4-01`

#### 3. SSH 접속 설정 자동 생성

powershell

```powershell
gcloud compute config-ssh
```

- 이 명령어가 `~/.ssh/config`에 VM 접속 정보를 자동으로 추가하고 SSH 키를 생성함
- ⚠️ 이때 등록되는 SSH 계정은 **현재 gcloud 로그인 계정 기준**이라, 본인이 실제 작업하던 개인 계정(예: `jaewan3182`)과 다를 수 있음

#### 4. 개인 작업 계정으로 SSH 접속 권한 추가 (계정이 다를 경우)

gcloud 관리자 계정(sudo 권한 있음)으로 VM에 먼저 접속한 뒤, 본인의 SSH 공개키를 개인 작업 계정에도 등록:

bash

```bash
sudobash -c'cat /home/[관리자계정]/.ssh/authorized_keys >> /home/[본인계정]/.ssh/authorized_keys'sudochown[본인계정]:[본인계정] /home/[본인계정]/.ssh/authorized_keyssudochmod600 /home/[본인계정]/.ssh/authorized_keyssudochmod700 /home/[본인계정]/.ssh
```

→ 완료 후 `ssh [본인계정]@[VM_IP]`로 비밀번호 없이 접속되면 성공

#### 5. `~/.ssh/config`에 개인 계정용 Host 추가

```
Host [원하는별칭]    HostName [VM_IP]    User [본인계정]
```

#### 6. VS Code에서 원격 연결

- `Ctrl+Shift+P` → `Remote-SSH: Connect to Host` → 위에서 설정한 Host 선택
- 플랫폼 물어보면 **Linux** 선택
- 연결되면 `Ctrl+K Ctrl+O`로 프로젝트 폴더 열기 (예: `/home/[본인계정]/sprint_11_2team_RAG_Project`)

#### 7. Python/Jupyter 확장 설치 (원격)

- 원격 창에서 **Python**, **Jupyter** 확장 설치 (로컬에 있어도 원격엔 별도 설치 필요)
- 기존 venv(`myenv` 등)가 있으면 `.ipynb` 파일 열고 우측 상단 커널 선택에서 해당 venv 선택

---

#### 핵심 포인트 (팀원 공유용)

> VM 안에 리눅스 계정이 개인별로 나뉘어 있다면, **자신에게 배정된 개인 계정 이름**을 정확히 알고 SSH config의 `User` 값에 지정해야 합니다. gcloud 로그인 계정과 실제 작업 계정이 다를 수 있으니 헷갈리지 않도록 주의하세요.