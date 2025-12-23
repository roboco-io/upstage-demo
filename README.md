# Local-First Enterprise Doc-Informer

**Local-First Enterprise Doc-Informer**는 로컬 환경의 보안성과 속도를 유지하면서, **Upstage Solar API**의 강력한 추론 능력을 결합한 하이브리드 RAG(Retrieval-Augmented Generation) 시스템입니다.

기업의 민감한 문서를 로컬 서버(CPU/RAM)에서 안전하게 처리(Embedding & Storage)하고, 핵심적인 질의응답(Reasoning) 단계에서만 고성능 LLM API를 활용하여 효율성을 극대화합니다.

## 🌟 주요 특징 (Key Features)

*   **🔒 Local-First Data Pipeline**: 모든 문서는 로컬에서 파싱되고 벡터화되어 저장됩니다. 데이터가 외부로 유출될 걱정이 없습니다.
*   **🏎️ Cost-Effective Inference**: 무거운 임베딩 모델 대신 로컬 경량 모델을 사용하고, 꼭 필요한 추론에만 API를 사용하여 비용을 절감합니다.
*   **🧠 Intelligent RAG**: 단순 텍스트 매칭이 아닌, 문서 구조를 이해하는 파싱과 MMR(Max Marginal Relevance) 검색을 통해 정확성을 높입니다.
*   **📊 Quantitative Evaluation**: `ragas`를 활용한 정량적 성능 평가 프로세스가 내장되어 있어, 시스템의 신뢰도를 수치로 증명합니다.

## 🛠️ 기술 스택 (Tech Stack)

*   **Frontend**: Streamlit
*   **Orchestration**: LangChain
*   **LLM**: Upstage Solar API
*   **Embedding**: HuggingFace BGE (Local)
*   **Vector DB**: ChromaDB (Local Persistence)
*   **PDF Processing**: PyMuPDF, Unstructured
*   **Evaluation**: Ragas

## 🚀 시작하기 (Getting Started)

### 사전 요구사항
*   Python 3.10+
*   Upstage Solar API Key

### 설치 및 실행

```bash
# 1. 저장소 클론
git clone https://github.com/roboco-io/upstage-demo.git
cd upstage-demo

# 2. 가상환경 생성 및 활성화
python -m venv venv
source venv/bin/activate  # Mac/Linux
# venv\Scripts\activate  # Windows

# 3. 의존성 설치
pip install -r requirements.txt

# 4. 환경 변수 설정 (.env 파일 생성)
echo "UPSTAGE_API_KEY=your_api_key_here" > .env

# 5. 애플리케이션 실행
streamlit run app.py
```

## 📅 프로젝트 로드맵 (Roadmap)

- [ ] **Phase 1**: 데이터 파이프라인 (PDF Ingest -> Vector DB)
- [ ] **Phase 2**: 검색 시스템 고도화 (MMR Retrieval)
- [ ] **Phase 3**: RAG 체인 및 Chat UI 구현
- [ ] **Phase 4**: 성능 평가 시스템 (Eval) 구축

## 📚 문서 (Documentation)

*   [초기 아이디어 (Ideation)](docs/Ideation.md)
*   [제품 요구사항 정의서 (PRD)](docs/PRD.md)
*   [개발 태스크 (TASKS)](docs/TASKS.md)
*   [튜토리얼 (Tutorial)](docs/Tutorial.md)
*   [GitHub Actions 완벽 가이드](docs/github-actions-guide.md) 📘 *대학생을 위한 자동 배포 설명서*

## 🎤 프레젠테이션 (Presentation)

프로젝트 소개 프레젠테이션을 GitHub Pages에서 확인할 수 있습니다:

**🔗 [프레젠테이션 보기](https://roboco-io.github.io/upstage-demo/)**

### 로컬에서 프레젠테이션 보기

```bash
# Marp CLI 설치 (한 번만 실행)
npm install -g @marp-team/marp-cli

# HTML로 변환
marp --no-stdin --html docs/presentation.md -o docs/dist/index.html

# 브라우저로 열기
open docs/dist/index.html  # Mac
# start docs/dist/index.html  # Windows
```

---
Developed by **Roboco IO**
