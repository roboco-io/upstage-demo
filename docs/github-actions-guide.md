# GitHub Actions 워크플로우 완벽 가이드 🚀

> **대상 독자**: GitHub Actions를 처음 접하는 대학생 및 초보 개발자  
> **목표**: `.github/workflows/deploy-presentation.yml` 파일의 모든 부분을 이해하기

---

## 📚 목차

1. [GitHub Actions란?](#github-actions란)
2. [워크플로우 파일 구조](#워크플로우-파일-구조)
3. [코드 한 줄씩 이해하기](#코드-한-줄씩-이해하기)
4. [실행 흐름 다이어그램](#실행-흐름-다이어그램)
5. [자주 묻는 질문 (FAQ)](#자주-묻는-질문-faq)

---

## GitHub Actions란?

**GitHub Actions**는 GitHub에서 제공하는 **자동화 도구**입니다. 

### 🤔 왜 필요한가요?

개발자가 매번 수동으로 해야 하는 반복적인 작업들을 자동화할 수 있습니다:

- ✅ 코드를 푸시할 때마다 자동으로 테스트 실행
- ✅ 코드를 병합할 때마다 자동으로 배포
- ✅ 문서를 수정할 때마다 자동으로 웹사이트 업데이트

### 💡 우리 프로젝트에서는?

`docs/presentation.md` 파일을 수정하고 GitHub에 푸시하면:
1. 자동으로 Marp가 Markdown을 HTML로 변환
2. 자동으로 GitHub Pages에 배포
3. 웹사이트에서 바로 확인 가능!

**수동 작업 없이 모든 것이 자동으로 진행됩니다!** 🎉

---

## 워크플로우 파일 구조

GitHub Actions는 `.github/workflows/` 디렉토리 안의 YAML 파일로 정의됩니다.

```
프로젝트 루트/
├── .github/
│   └── workflows/
│       └── deploy-presentation.yml  ← 이 파일!
├── docs/
│   └── presentation.md
└── ...
```

### YAML이란?

**YAML** (YAML Ain't Markup Language)은 사람이 읽기 쉬운 데이터 직렬화 형식입니다.

**특징:**
- 들여쓰기로 계층 구조를 표현 (탭 대신 **스페이스 2칸** 사용!)
- `key: value` 형식으로 데이터 저장
- 리스트는 `-`로 표현

**예시:**
```yaml
name: 내 이름
age: 20
hobbies:
  - 코딩
  - 독서
  - 운동
```

---

## 코드 한 줄씩 이해하기

이제 실제 워크플로우 파일을 한 줄씩 분석해봅시다!

### 1️⃣ 워크플로우 이름 정의

```yaml
name: Deploy Presentation to GitHub Pages
```

**설명:**
- 이 워크플로우의 이름을 정의합니다
- GitHub Actions 탭에서 이 이름으로 표시됩니다
- 알아보기 쉬운 이름을 사용하는 것이 좋습니다

---

### 2️⃣ 트리거 조건 (언제 실행할까?)

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'docs/presentation.md'
      - '.github/workflows/deploy-presentation.yml'
  workflow_dispatch:
```

**설명:**

#### `on:` - 워크플로우 실행 조건
이 워크플로우가 **언제** 실행될지 정의합니다.

#### `push:` - Git Push 이벤트
코드를 GitHub에 푸시할 때 실행됩니다.

#### `branches: - main`
- `main` 브랜치에 푸시할 때만 실행
- 다른 브랜치(예: `develop`, `feature/xxx`)에는 반응하지 않음

#### `paths:` - 특정 파일 변경 감지
다음 파일들이 변경되었을 때만 실행:
- `docs/presentation.md` - 프레젠테이션 파일
- `.github/workflows/deploy-presentation.yml` - 워크플로우 파일 자체

**왜 이렇게 하나요?**
- 불필요한 빌드를 방지하여 리소스 절약
- 예: `README.md`를 수정했을 때는 프레젠테이션을 다시 빌드할 필요가 없음

#### `workflow_dispatch:`
- GitHub 웹사이트에서 **수동으로** 워크플로우를 실행할 수 있게 함
- Actions 탭에서 "Run workflow" 버튼이 생김

**실제 사용 예:**
```
상황: 프레젠테이션 파일은 안 바뀌었지만 재배포하고 싶을 때
해결: GitHub Actions 탭 → "Run workflow" 버튼 클릭!
```

---

### 3️⃣ 권한 설정

```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

**설명:**

이 워크플로우가 GitHub에서 **무엇을 할 수 있는지** 권한을 정의합니다.

#### `contents: read`
- 저장소의 코드를 **읽을** 수 있는 권한
- 코드를 체크아웃(다운로드)하려면 필요

#### `pages: write`
- GitHub Pages에 **쓸** 수 있는 권한
- 빌드된 파일을 GitHub Pages에 배포하려면 필요

#### `id-token: write`
- OIDC (OpenID Connect) 토큰을 **생성**할 수 있는 권한
- GitHub Pages 배포 시 인증에 사용

**보안 원칙: 최소 권한의 원칙**
- 필요한 권한만 부여하여 보안 위험 최소화
- 예: 삭제 권한은 필요 없으므로 부여하지 않음

---

### 4️⃣ 동시성 제어

```yaml
concurrency:
  group: "pages"
  cancel-in-progress: false
```

**설명:**

#### `concurrency:` - 동시 실행 제어
여러 워크플로우가 동시에 실행되는 것을 제어합니다.

#### `group: "pages"`
- 같은 그룹에 속한 워크플로우는 동시에 실행되지 않음
- "pages"라는 그룹으로 묶음

#### `cancel-in-progress: false`
- 이미 실행 중인 워크플로우를 취소하지 않음
- `true`로 설정하면 새 워크플로우가 시작될 때 기존 실행을 취소

**왜 필요한가요?**

```
시나리오:
1. 프레젠테이션 수정 후 푸시 (워크플로우 A 시작)
2. 오타 발견! 바로 다시 수정 후 푸시 (워크플로우 B 시작)

cancel-in-progress: false
→ A와 B 모두 순차적으로 실행 (안전하지만 느림)

cancel-in-progress: true
→ A를 취소하고 B만 실행 (빠르지만 A의 작업은 버려짐)
```

GitHub Pages 배포는 안전성이 중요하므로 `false`로 설정했습니다.

---

### 5️⃣ 작업(Job) 정의 - Build

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # ... (단계들)
```

**설명:**

#### `jobs:` - 작업 목록
워크플로우는 하나 이상의 **Job(작업)**으로 구성됩니다.

#### `build:` - 첫 번째 작업 이름
- 프레젠테이션을 빌드하는 작업
- 이름은 자유롭게 지을 수 있음 (예: `build-presentation`, `compile` 등)

#### `runs-on: ubuntu-latest`
- 이 작업을 **어떤 운영체제**에서 실행할지 지정
- `ubuntu-latest`: 최신 Ubuntu Linux 환경
- 다른 옵션: `windows-latest`, `macos-latest`

**왜 Ubuntu인가요?**
- 무료 (GitHub Actions에서 Linux가 가장 저렴)
- 빠름 (부팅 시간이 짧음)
- 대부분의 개발 도구가 Linux를 기본 지원

#### `steps:` - 단계 목록
작업은 여러 **Step(단계)**로 구성됩니다. 순서대로 실행됩니다.

---

### 6️⃣ Step 1: 코드 체크아웃

```yaml
- name: Checkout
  uses: actions/checkout@v4
```

**설명:**

#### `name: Checkout`
- 이 단계의 이름 (로그에서 이 이름으로 표시됨)

#### `uses: actions/checkout@v4`
- **미리 만들어진 액션**을 사용
- `actions/checkout`: GitHub에서 공식 제공하는 액션
- `@v4`: 버전 4를 사용

**이 액션이 하는 일:**
1. GitHub 저장소의 코드를 워크플로우 실행 환경으로 다운로드
2. 마치 `git clone`을 한 것과 같은 효과

**왜 필요한가요?**
- 워크플로우는 빈 환경에서 시작됩니다
- 우리 코드(`presentation.md`)를 빌드하려면 먼저 다운로드해야 함

---

### 7️⃣ Step 2: Node.js 설치

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
```

**설명:**

#### `uses: actions/setup-node@v4`
- Node.js를 설치하는 공식 액션

#### `with:` - 액션에 전달할 파라미터
- 액션의 동작을 커스터마이징

#### `node-version: '20'`
- Node.js 버전 20을 설치

**왜 Node.js가 필요한가요?**
- Marp CLI는 Node.js 기반 도구
- npm (Node Package Manager)을 사용하여 설치

---

### 8️⃣ Step 3: Marp CLI 설치

```yaml
- name: Install Marp CLI
  run: npm install -g @marp-team/marp-cli
```

**설명:**

#### `run:` - 쉘 명령어 실행
- `uses`와 달리 직접 명령어를 실행
- Linux 터미널 명령어를 그대로 사용

#### `npm install -g @marp-team/marp-cli`
- `npm install`: npm을 사용하여 패키지 설치
- `-g`: 전역(global) 설치 (어디서든 사용 가능)
- `@marp-team/marp-cli`: Marp CLI 패키지 이름

**Marp CLI란?**
- Markdown을 프레젠테이션 HTML로 변환하는 도구
- 명령줄(CLI)에서 사용 가능

---

### 9️⃣ Step 4: 프레젠테이션 빌드

```yaml
- name: Build presentation
  run: |
    mkdir -p docs/dist
    marp --no-stdin --html docs/presentation.md -o docs/dist/index.html
```

**설명:**

#### `run: |` - 여러 줄 명령어
- `|` 기호: 여러 줄의 명령어를 순서대로 실행

#### `mkdir -p docs/dist`
- `mkdir`: 디렉토리 생성 (make directory)
- `-p`: 부모 디렉토리도 함께 생성 (없으면 만들기)
- `docs/dist`: 빌드 결과물을 저장할 폴더

#### `marp --no-stdin --html docs/presentation.md -o docs/dist/index.html`
- `marp`: Marp CLI 실행
- `--no-stdin`: 표준 입력을 사용하지 않음 (파이프 입력 비활성화)
- `--html`: HTML 출력 활성화 (JavaScript 등 포함)
- `docs/presentation.md`: 입력 파일 (Markdown 프레젠테이션)
- `-o docs/dist/index.html`: 출력 파일 경로

**결과:**
```
docs/presentation.md (Markdown)
        ↓ (Marp 변환)
docs/dist/index.html (HTML 프레젠테이션)
```

---

### 🔟 Step 5: GitHub Pages 설정

```yaml
- name: Setup Pages
  uses: actions/configure-pages@v4
```

**설명:**

#### `actions/configure-pages@v4`
- GitHub Pages 배포를 위한 환경 설정
- 내부적으로 Pages API와 통신하여 설정 확인

**이 액션이 하는 일:**
1. GitHub Pages가 활성화되어 있는지 확인
2. 배포에 필요한 메타데이터 생성
3. 다음 단계에서 사용할 설정 준비

---

### 1️⃣1️⃣ Step 6: 아티팩트 업로드

```yaml
- name: Upload artifact
  uses: actions/upload-pages-artifact@v3
  with:
    path: 'docs/dist'
```

**설명:**

#### `actions/upload-pages-artifact@v3`
- GitHub Pages 배포용 아티팩트를 업로드하는 액션

#### `with: path: 'docs/dist'`
- `docs/dist` 폴더의 모든 파일을 아티팩트로 업로드

**아티팩트(Artifact)란?**
- 워크플로우에서 생성된 파일들
- Job 간에 파일을 전달하거나 나중에 다운로드할 수 있음

**왜 업로드하나요?**
- `build` Job에서 생성한 HTML 파일을
- `deploy` Job에서 사용하기 위해

```
build Job                    deploy Job
   ↓                            ↑
HTML 생성 → 아티팩트 업로드 → 아티팩트 다운로드 → 배포
```

---

### 1️⃣2️⃣ 작업(Job) 정의 - Deploy

```yaml
deploy:
  environment:
    name: github-pages
    url: ${{ steps.deployment.outputs.page_url }}
  runs-on: ubuntu-latest
  needs: build
  steps:
    # ... (단계들)
```

**설명:**

#### `deploy:` - 두 번째 작업
- 빌드된 파일을 GitHub Pages에 배포

#### `environment:`
- 배포 환경 정의

#### `name: github-pages`
- 환경 이름 (GitHub에서 이 이름으로 표시)

#### `url: ${{ steps.deployment.outputs.page_url }}`
- 배포 후 접속할 URL
- `${{ }}`: GitHub Actions 표현식 (변수/함수 사용)
- `steps.deployment.outputs.page_url`: `deployment` 단계에서 출력된 URL

#### `needs: build`
- **중요!** 이 작업은 `build` 작업이 **성공적으로 완료된 후**에만 실행
- 의존성 정의

**실행 순서:**
```
build Job 시작
  ↓
build Job 완료 (성공)
  ↓
deploy Job 시작
  ↓
deploy Job 완료
```

만약 `build`가 실패하면 `deploy`는 실행되지 않습니다!

---

### 1️⃣3️⃣ Step 7: GitHub Pages에 배포

```yaml
- name: Deploy to GitHub Pages
  id: deployment
  uses: actions/deploy-pages@v4
```

**설명:**

#### `id: deployment`
- 이 단계에 `deployment`라는 ID 부여
- 다른 곳에서 이 단계의 출력을 참조할 수 있음
- 예: `${{ steps.deployment.outputs.page_url }}`

#### `actions/deploy-pages@v4`
- GitHub Pages에 실제로 배포하는 액션

**이 액션이 하는 일:**
1. 이전에 업로드한 아티팩트 다운로드
2. GitHub Pages 서버에 파일 업로드
3. 배포 완료 후 URL 반환

**최종 결과:**
- `https://roboco.io/upstage-demo/`에서 프레젠테이션 확인 가능! 🎉

---

## 실행 흐름 다이어그램

### 전체 워크플로우 흐름

```
┌─────────────────────────────────────────┐
│  트리거: presentation.md 파일 푸시      │
│  또는 수동 실행 (workflow_dispatch)     │
└────────────────┬────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────┐
│         🔨 Build Job (ubuntu-latest)    │
├─────────────────────────────────────────┤
│  1. Checkout 코드                       │
│     └─ Git 저장소에서 코드 다운로드     │
│                                         │
│  2. Setup Node.js                       │
│     └─ Node.js v20 설치                 │
│                                         │
│  3. Install Marp CLI                    │
│     └─ npm으로 Marp CLI 설치            │
│                                         │
│  4. Build presentation                  │
│     └─ presentation.md → index.html     │
│                                         │
│  5. Setup Pages                         │
│     └─ GitHub Pages 설정 확인           │
│                                         │
│  6. Upload artifact                     │
│     └─ docs/dist 폴더 업로드            │
└────────────────┬────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────┐
│       🚀 Deploy Job (ubuntu-latest)     │
├─────────────────────────────────────────┤
│  needs: build (build 성공 후 실행)      │
│                                         │
│  1. Deploy to GitHub Pages              │
│     └─ 아티팩트를 Pages에 배포          │
│     └─ URL 반환                         │
└────────────────┬────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────┐
│   ✅ 배포 완료!                         │
│   🌐 https://roboco.io/upstage-demo/    │
└─────────────────────────────────────────┘
```

### 타임라인 예시

```
00:00 - 트리거: main 브랜치에 푸시
00:01 - Build Job 시작
00:02 - Checkout 완료
00:03 - Node.js 설치 완료
00:05 - Marp CLI 설치 완료
00:06 - 프레젠테이션 빌드 완료
00:07 - Pages 설정 완료
00:08 - 아티팩트 업로드 완료
00:09 - Build Job 완료 ✅
00:10 - Deploy Job 시작
00:11 - GitHub Pages 배포 시작
00:15 - 배포 완료 ✅
00:16 - 웹사이트 접속 가능! 🎉
```

---

## 자주 묻는 질문 (FAQ)

### Q1: 워크플로우가 실행되지 않아요!

**확인 사항:**
1. ✅ `main` 브랜치에 푸시했나요?
2. ✅ `docs/presentation.md` 또는 워크플로우 파일을 수정했나요?
3. ✅ `.github/workflows/` 경로가 정확한가요?
4. ✅ YAML 문법 오류는 없나요? (들여쓰기 확인!)

**해결 방법:**
```bash
# GitHub Actions 탭에서 수동 실행
gh workflow run "Deploy Presentation to GitHub Pages"

# 또는 GitHub 웹사이트에서
# Actions → Deploy Presentation to GitHub Pages → Run workflow
```

---

### Q2: 빌드는 성공했는데 배포가 실패해요!

**원인:**
- GitHub Pages 설정이 활성화되지 않았을 수 있습니다

**해결 방법:**
1. GitHub 저장소 → Settings → Pages
2. Source를 **GitHub Actions**로 설정
3. 워크플로우 다시 실행

---

### Q3: 다른 파일도 자동 배포하고 싶어요!

**방법:**
`paths:` 섹션에 파일 추가

```yaml
on:
  push:
    paths:
      - 'docs/presentation.md'
      - 'docs/another-file.md'  # 추가!
      - '.github/workflows/deploy-presentation.yml'
```

---

### Q4: 배포 속도를 높이고 싶어요!

**최적화 방법:**

1. **캐싱 사용** (Node.js 모듈 캐시)
```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'  # 추가!
```

2. **동시 실행 허용**
```yaml
concurrency:
  group: "pages"
  cancel-in-progress: true  # false → true
```

---

### Q5: 로컬에서 테스트하고 싶어요!

**방법:**

```bash
# 1. Node.js 설치 확인
node --version

# 2. Marp CLI 설치
npm install -g @marp-team/marp-cli

# 3. 빌드 테스트
mkdir -p docs/dist
marp --no-stdin --html docs/presentation.md -o docs/dist/index.html

# 4. 결과 확인
open docs/dist/index.html  # Mac
# start docs/dist/index.html  # Windows
```

---

### Q6: 워크플로우 실행 기록은 어디서 보나요?

**확인 방법:**

1. **GitHub 웹사이트**
   - 저장소 → Actions 탭
   - 모든 실행 기록과 로그 확인 가능

2. **GitHub CLI**
```bash
# 최근 실행 목록
gh run list

# 특정 실행 상세 보기
gh run view <run-id>

# 실패한 로그만 보기
gh run view <run-id> --log-failed

# 실시간 모니터링
gh run watch <run-id>
```

---

### Q7: 비용이 발생하나요?

**답변:**

- ✅ **Public 저장소**: 완전 무료!
- ⚠️ **Private 저장소**: 월 2,000분 무료, 이후 분당 과금

**우리 프로젝트:**
- Public 저장소 → 무료
- 1회 실행 시간: 약 25초
- 걱정 없이 사용 가능! 💰

---

### Q8: 보안은 괜찮나요?

**안전한 이유:**

1. ✅ **최소 권한 원칙**
   - 필요한 권한만 부여 (`permissions:`)

2. ✅ **공식 액션 사용**
   - `actions/checkout@v4` 등은 GitHub 공식 제공

3. ✅ **버전 고정**
   - `@v4`로 버전 명시 (예상치 못한 변경 방지)

4. ✅ **환경 분리**
   - 매번 새로운 환경에서 실행 (격리됨)

---

### Q9: 다른 브랜치에서도 배포하고 싶어요!

**방법:**

```yaml
on:
  push:
    branches:
      - main
      - develop  # 추가!
      - staging  # 추가!
```

**주의:**
- 여러 브랜치에서 같은 GitHub Pages에 배포하면 덮어쓰기됨
- 브랜치별로 다른 환경에 배포하려면 워크플로우를 분리해야 함

---

### Q10: 에러가 발생하면 알림을 받고 싶어요!

**방법:**

1. **GitHub 알림 설정**
   - GitHub Settings → Notifications
   - "Actions" 섹션에서 알림 활성화

2. **Slack/Discord 연동**
```yaml
- name: Notify on failure
  if: failure()
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

---

## 🎓 학습 자료

### 추가로 공부하면 좋은 것들

1. **GitHub Actions 공식 문서**
   - https://docs.github.com/en/actions

2. **YAML 문법**
   - https://yaml.org/

3. **Marp 문서**
   - https://marp.app/

4. **GitHub CLI**
   - https://cli.github.com/

### 실습 과제

1. ✏️ 프레젠테이션 내용 수정 후 자동 배포 확인
2. ✏️ 워크플로우 파일에 주석 추가해보기
3. ✏️ 다른 브랜치에서 푸시해보고 실행 여부 확인
4. ✏️ GitHub Actions 탭에서 로그 분석해보기

---

## 📝 요약

### 핵심 개념

| 개념 | 설명 |
|------|------|
| **Workflow** | 자동화된 프로세스 전체 |
| **Job** | 워크플로우 내의 작업 단위 |
| **Step** | Job 내의 개별 단계 |
| **Action** | 재사용 가능한 작업 단위 |
| **Trigger** | 워크플로우 실행 조건 |
| **Artifact** | 워크플로우에서 생성된 파일 |

### 우리 워크플로우 요약

```
트리거: presentation.md 푸시 또는 수동 실행
   ↓
Build Job: Markdown → HTML 변환
   ↓
Deploy Job: GitHub Pages에 배포
   ↓
결과: 웹사이트에서 프레젠테이션 확인!
```

---

## 🎉 마치며

이제 GitHub Actions 워크플로우를 완벽하게 이해하셨나요?

**다음 단계:**
1. 직접 프레젠테이션을 수정해보세요
2. 푸시 후 Actions 탭에서 실행 과정을 지켜보세요
3. 배포된 웹사이트를 확인하세요!

**질문이 있다면:**
- GitHub Discussions에 질문 남기기
- Issue 생성하기
- 팀원에게 물어보기

**Happy Coding! 🚀**

---

*작성일: 2025-12-23*  
*버전: 1.0*  
*대상: GitHub Actions 초보자*
