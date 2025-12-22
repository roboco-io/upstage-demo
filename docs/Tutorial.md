# 에이전트 기반 프로젝트 부트스트랩 튜토리얼 (Agent-Based Project Bootstrap Tutorial)

이 문서는 아이디어 단계에서부터 실행 가능한 프로젝트 계획, 그리고 GitHub 협업 환경 구축까지 인공지능 에이전트와 함께 수행한 과정을 기록한 튜토리얼입니다. **프롬프트 중심(Prompt-Centric)**으로 어떻게 작업을 지시하고 결과를 얻었는지 설명합니다.

---

## 1. 아이디어 구체화 및 PRD 작성 (Ideation to PRD)

가장 먼저 막연한 아이디어 메모(`Ideation.md`)를 바탕으로 상세한 기능 명세서인 **제품 요구사항 정의서(PRD)**를 작성했습니다.

### 🎯 Prompt
> "@[docs/Ideation.md] 의 내용으로 프로젝트 PRD를 작성해줘. @[docs/PRD.md]"

### ✅ Result
에이전트는 `Ideation.md`의 내용을 분석하여 다음과 같은 섹션을 포함하는 구조적인 `PRD.md`를 생성했습니다.
- **프로젝트 개요 및 핵심 가치** (Data Privacy, Cost Efficiency 등)
- **기술 스택** (Streamlit, LangChain, ChromaDB 등)
- **기능 요구사항** (데이터 파싱, 검색, 답변 생성, 평가)
- **구현 로드맵** (Phase 1 ~ Phase 4)

---

## 2. 실행 계획 수립 (PRD to Tasks)

작성된 PRD를 실제 개발 작업 단위(Task)로 변환하는 과정을 거쳤습니다.

### 🎯 Prompt
> "PRD의 작업내용을 테스크 문서로 작성해줘."

### ✅ Result
PRD의 '구현 로드맵'을 기반으로 체크리스트 형태의 `TASKS.md` 파일이 생성되었습니다. 각 Phase 별로 구체적인 할 일(To-do)이 정리되었습니다.
*(이후 사용자의 요청으로 문서를 한국어로 번역/업데이트하였습니다.)*

---

## 3. 프로젝트 저장소 생성 및 호스팅 (Hosting)

로컬 프로젝트를 초기화하고 GitHub 원격 저장소에 업로드하는 과정을 에이전트에게 위임했습니다.

### 🎯 Prompt
> "이 프로젝트를 github에 호스팅해줘. org 는 roboco-io 로 하고, 퍼블릭 리포지토리로 만들어줘."

### ✅ Result
에이전트는 다음 작업들을 순차적으로 수행했습니다.
1. `.gitignore` 파일 생성 (Python 프로젝트용).
2. `git init`, `add`, `commit` 수행.
3. `gh repo create` 명령어를 사용하여 GitHub `roboco-io/upstage-demo` 리포지토리 생성 및 푸시.

---

## 4. 이슈 트래킹 연동 (Tasks to GitHub Issues)

`TASKS.md`에 정의된 작업 목록을 실제 프로젝트 관리 도구인 GitHub Issues로 등록하여 관리 가능하게 만들었습니다.

### 🎯 Prompt
> "태스크의 항목들을 깃헙 이슈로 등록해줘. 이슈에는 작업 배경, 작업 내용, 인수 조건이 포함 되어야 해."

### ✅ Result
단순히 태스크 제목만 올리는 것이 아니라, 에이전트가 문맥을 이해하여 **작업 배경(Background), 상세 작업 내용(Tasks), 인수 조건(Acceptance Criteria)**까지 포함된 고품질의 이슈 5개를 생성했습니다.

- Phase 0: 프로젝트 초기화
- Phase 1: 데이터 파이프라인 구축
- Phase 2: 검색 시스템 개발
- Phase 3: 답변 생성 및 UI 구현
- Phase 4: 성능 평가 및 최적화

---

## 5. 결론 및 인사이트

이 튜토리얼은 단 몇 번의 자연어 명령(Prompt)만으로 **"아이디어 -> 기획(PRD) -> 계획(Tasks) -> 인프라(GitHub) -> 이슈 관리(Issues)"**로 이어지는 전체 프로젝트 초기화 과정을 완료했음을 보여줍니다.

에이전트를 활용함으로써 개발자는 초기 설정과 문서화에 드는 시간을 획기적으로 단축하고, 곧바로 핵심 로직 구현 단계로 진입할 수 있습니다.
