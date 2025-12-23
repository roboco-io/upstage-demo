---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

# Local-First Enterprise Doc-Informer

### 로컬 보안과 Solar API의 만남

---

## 📋 프로젝트 개요

**Local-First Enterprise Doc-Informer**

*   **목적**: 로컬 리소스(CPU/RAM) 효율적 활용 + Upstage Solar API 고성능 추론 결합
*   **핵심 가치**
    *   🔒 **Data Privacy**: 민감 문서 로컬 처리
    *   💸 **Cost Efficiency**: 고비용 추론만 API 위임
    *   📊 **Quality Assurance**: 정량적 성능 평가(Eval) 내재화

---

## 🎯 주요 목표 (Goals)

1.  **구조적 데이터 파싱**
    *   문서의 계층 구조(Heading, Body) 유지
2.  **로컬 벡터 검색 최적화**
    *   ChromaDB + MMR(Max Marginal Relevance) 활용
3.  **고성능 답변 생성**
    *   Upstage Solar API 활용, 할루시네이션 최소화
4.  **정량적 성능 평가**
    *   Ragas 프레임워크 기반 Faithfulness/Relevance 측정

---

## 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 | 비고 |
| :--- | :--- | :--- |
| **Frontend** | Streamlit | 빠른 프로토타이핑 |
| **Engine** | LangChain / LlamaIndex | Orchestration |
| **LLM** | **Upstage Solar API** | Main Generation |
| **Embedding** | HuggingFace BGE | Local CPU Optimized |
| **Vector DB** | ChromaDB | Local Persistence |
| **Eval** | Ragas | QA Assessment |

---

## ⚙️ 핵심 기능 (1/2)

### 📥 데이터 파싱 (Ingestion)
*   PDF 업로드 및 구조적 파싱 (PyMuPDF/Unstructured)
*   Chunking 및 로컬 ChromaDB 적재

### 🔍 검색 (Retrieval)
*   벡터 유사도 검색 (Similarity Search)
*   **MMR 기법** 적용: 답변 다양성 확보

---

## 🧩 용어 설명: 청킹 (Chunking)

> **"거대한 문서를 한입 크기로 쪼개는 기술"**

*   **정의**: 긴 텍스트를 LLM이 이해하고 처리하기 쉬운 작은 단위(Chunk)로 분할하는 과정
*   **왜 필요한가요?**
    *   **토큰 제한 극복**: LLM이 한 번에 읽을 수 있는 양에는 한계가 있음
    *   **검색 정확도 향상**: 전체 문서 대신 질문과 **가장 관련성 높은 부분**만 쏙 뽑아내기 위함
*   **전략**: 단순 글자 수 자르기, 문단 단위 자르기, 의미(Semantic) 단위 자르기 등

---

## 🗄️ 용어 설명: ChromaDB

> **"AI를 위한 기억 저장소 (Vector Database)"**

*   **정의**: 텍스트의 **의미(Vector)**를 숫자 형태로 저장하고 검색하는 데이터베이스
*   **왜 ChromaDB인가요?**
    *   **Local-First**: 복잡한 서버 구축 없이 **로컬 파일시스템에 저장** 가능
    *   **Open Source**: 무료이며 Python 생태계와 완벽하게 통합
*   **작동 원리**: 질문이 들어오면 저장된 데이터 중 **의미적으로 가장 가까운** 내용을 고속으로 찾아냄 (유사도 검색)

---

## ⚙️ 핵심 기능 (2/2)

### 💬 답변 생성 (Generation)
*   검색 문맥 기반 Solar API 질의
*   시스템 프롬프트: "문서 기반 답변", "출처 명시"

### 📈 성능 평가 (Evaluation)
*   자동화된 평가 프로세스 (Golden Dataset)
*   **Faithfulness** & **Answer Relevance** 지표 산출

---

## 🗺️ 구현 로드맵

1.  **Phase 1: Data Pipeline** 
    *   PDF 구조 분석, 로컬 DB 적재
2.  **Phase 2: Retrieval System**
    *   Retriever 구성, 검색 최적화 (MMR)
3.  **Phase 3: Generation & UI**
    *   Solar API 연동, Streamlit 채팅 UI
4.  **Phase 4: Evaluation**
    *   Ragas 평가, 전후 성능 비교

---

## 📂 프로젝트 구조

```text
local-doc-rag/
├── data/       # 테스트 데이터
├── db/         # ChromaDB 저장소
├── src/
│   ├── ingest.py  # 파싱/적재
│   ├── chain.py   # RAG 로직
│   └── eval.py    # 평가
├── app.py      # Streamlit UI
└── .env        # 환경 변수
```

---

## 🏆 성공 지표 (Metrics)

*   **Ragas Score**: 충실도/관련성 평균 **0.8 이상**
*   **Performance**: 로컬 환경에서도 **수 초 이내** 응답
*   **Accuracy**: 문서의 표/헤더 정보를 **정확히 참조**

---

# Q & A

### 감사합니다
