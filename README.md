# 팀 스프린트 옵시디언 볼트 템플릿

팀 단위 스프린트 프로젝트를 위한 Obsidian 볼트 템플릿입니다. 협업일지, 게시판(자유/Q&A), 전체 공지사항, Q&A 칸반 보드, GitHub 이슈 동기화(tasks), Quartz 정적 사이트 배포, Discord 웹훅 알림을 포함합니다.

> 🌐 **배포 사이트 (Quartz)**: https://surtleo.github.io/GPTPilots_Vault/
> main에 푸시하면 `deploy-quartz.yml`이 자동 빌드·배포합니다. (사이트가 안 뜨면 레포 **Settings → Pages → Source: `gh-pages` 브랜치**가 켜져 있는지 확인)

## 포함된 것

- **협업일지/** — 팀원별 일일 협업일지 (Templater 템플릿 + dataview index 자동 목록)
- **게시판/** — 자유게시판·Q&A 게시판 (템플릿으로 작성, Q&A는 Discord 알림 연동)
- **전체 공지사항/** — 공지 템플릿 (대상 체크리스트 포함)
- **Q&A 칸반.md** — Q&A 상태 추적 칸반 보드
- **tasks/** — GitHub 이슈와 동기화되는 태스크 파일 (`issue-{N}.md`, frontmatter 기반 dataview 표)
- **_templates/** — Templater 템플릿 모음 (협업일지, 게시판, 공지, Discord 알림)
- **.github/** — Quartz 배포 워크플로우 + 이슈 동기화 스크립트
- **.githooks/** — 커밋/푸시 시 Discord 알림 훅
- **홈.md / 프로젝트 개요.md** — 볼트 인덱스·프로젝트 개요 스켈레톤

## 시작 방법

1. 이 레포를 클론합니다 (또는 "Use this template"으로 새 레포 생성)
2. Obsidian에서 **"Open folder as vault"**로 클론한 폴더를 엽니다
3. 커뮤니티 플러그인 사용을 활성화하면 `.obsidian/`에 포함된 플러그인이 자동 로드됩니다

## GitHub Actions 시크릿 설정

레포 **Settings → Secrets and variables → Actions**에서 아래 시크릿을 등록해야 관련 기능이 동작합니다. 시크릿은 템플릿으로 새 레포를 만들 때 복사되지 않으므로 반드시 직접 등록해야 합니다.

| 시크릿 | 용도 | 없을 때 동작 |
|---|---|---|
| `GH_PAT` | GitHub Projects → `tasks/` 이슈 상태 동기화 (Personal Access Token, **repo + project 권한** 필요) | `Sync Issue Status` 워크플로우가 조용히 스킵됨 (실패 메일 없음) |
| `DISCORD_WEBHOOK_URL` | Q&A·리마인더 Discord 알림 | 알림 워크플로우 실패 |

> **주의**: PAT는 만료 기한이 있습니다. 만료되면 `Sync Issue Status`가 3시간마다 실패하며 매번 실패 메일이 옵니다. 만료 시 새 토큰으로 시크릿을 갱신하세요.

## 커스터마이즈 체크리스트

- [ ] **팀원 폴더명 변경** — `협업일지/`, `메모/` 아래 `팀원A(PM)` ~ `팀원D(Data)` 폴더를 실제 팀원 이름으로 변경 (각 index.md 안의 폴더명도 함께 수정)
- [ ] **Discord 웹훅 설정** — `.githooks/` 및 `_templates/discord-*-알림.md`의 `YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN`을 실제 웹훅 URL로 치환
- [ ] **GitHub 연동** — `.github/scripts/`의 `YOUR_GITHUB_ID`, `YOUR_CODE_REPO`를 실제 값으로 치환. `_templates/게시판.md`·`공지사항.md`의 `authorMap`에 팀원 GitHub ID 등록
- [ ] **Actions 시크릿 등록** — 위 [GitHub Actions 시크릿 설정](#github-actions-시크릿-설정) 참고 (`GH_PAT`, `DISCORD_WEBHOOK_URL`)
- [ ] **`_config/me.md` 생성** — 파일 내용으로 본인 GitHub ID 한 줄 작성 (템플릿이 글쓴이 식별에 사용, 최초 템플릿 실행 시 자동 생성됨)
- [ ] **Quartz 설정** — `quartz.config.ts`의 `baseUrl`을 배포 주소(`surtleo.github.io/GPTPilots_Vault`)로 수정 + 레포 Settings → Pages에서 `gh-pages` 브랜치 게시 활성화
- [ ] **git hooks 활성화** — 클론 후 한 번 실행:
  ```bash
  git config core.hooksPath .githooks
  ```
  (또는 Obsidian에서 `_templates/_hooks-setup.md` 템플릿 실행)
- [ ] **홈.md / 프로젝트 개요.md** — 프로젝트 이름·미션·목표 등 플레이스홀더 채우기

## 포함된 플러그인

- **dataview** — 협업일지·tasks 목록 자동 생성
- **templater** — 협업일지/게시판/공지 템플릿 실행
- **kanban** — Q&A 칸반 보드
- **obsidian-git** — 볼트 자동 커밋/푸시
- **omnisearch** — 전문 검색
- **metadata-menu** — frontmatter 필드 편집 UI
- **editing-toolbar** — 편집 툴바
- **kanban-status-updater** — 칸반 이동 시 상태 필드 자동 업데이트
- **file-shift** — 파일 이동 유틸리티

## 폴더 구조

```
├── 홈.md                  # 볼트 인덱스
├── 프로젝트 개요.md        # 프로젝트 개요
├── Q&A 칸반.md            # Q&A 상태 보드
├── 협업일지/팀원X(역할)/   # 팀원별 일일 협업일지 (+index.md)
├── 메모/팀원X(역할)/       # 팀원별 자유 메모
├── 게시판/                # 자유·Q&A 게시글
├── 전체 공지사항/          # 공지
├── tasks/                 # GitHub 이슈 동기화 파일
├── architecture/          # 아키텍처 문서
├── concepts/              # 개념 정리
├── workflow/              # 팀 운영·워크플로우 문서
├── experiment/            # 실험 기록
├── discord_bot/           # Discord 봇 (선택)
└── _templates/            # Templater 템플릿
```

## Dataview 쿼리 규칙 (Quartz 배포 시)

CI가 빌드 전에 `dataview` TABLE 쿼리를 정적 마크다운 표로 변환합니다. tasks 데이터를 보여줄 때는 `dataview` TABLE 쿼리를 사용하세요:

```dataview
TABLE issue as "#", title as "작업", st as "상태", priority as "우선순위", target as "마감", milestone as "마일스톤"
FROM "tasks"
WHERE contains(assignee, "YOUR_GITHUB_ID")
SORT priority ASC
```

협업일지 index는 `dataviewjs` + `// @prerender` 주석 패턴을 사용합니다 (포함된 index.md 참고).
