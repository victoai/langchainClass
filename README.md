 

## 랭체인 2시간으로 맛보기


## **LangChain으로 만드는 웹 기반 챗봇 실습**

 

### 교육 개요

- 교육목표
  - LLM 기반 챗봇의 구조와 동작 원리 이해
  - LCEL 기반 챗봇 파이프라인 설계 및 구현
  - 문서 임베딩 및 벡터DB 활용 검색형 챗봇 제작
  - Streamlit/Gradio로 웹 UI 구현 및 배포
  - 나만의 챗봇 기획·개발·시연

- 교육수준
  - 중급

- 선수지식
  - Python 기본 문법 (함수, 클래스, 모듈 사용법 등)
 

---
 
 

## 교육 내용 요약

| 단원명 | 학습시간 | 학습방법 | 주요 내용 |
|---------|-----------|------------|-------------|
| 과정소개 (목적) | 1 | 이론 | 과정 목표 및 결과물 소개, LLM 기반 챗봇 개요, LangChain 특징 |
| 개발 환경 준비 | 1 | 이론/실습 | Python 환경, OpenAI API 키 세팅, LangChain 설치 및 기본 테스트 |
| LangChain 기본 구조 | 1 | 이론/실습 | LCEL(LangChain Expression Language) 개념, PromptTemplate/Model/OutputParser 실습 |
| LCEL 실습 | 3 | 실습 | 단순 Q&A 챗봇, 다양한 프롬프트 실습, 구조화된 출력(JSON), ConversationMemory 기반 대화형 챗봇 구현 |
| 문서 기반 챗봇 | 2 | 실습 | 문서 로딩·분할, 임베딩 생성, 벡터DB(Chroma/FAISS), RetrievalQA 구성 |
| 웹 UI 연동 및 배포 | 2 | 실습 | Streamlit/Gradio UI 제작, 로컬 테스트, Streamlit Cloud 배포 |
| 최종 프로젝트 | 2 | 실습/평가 | “나만의 챗봇” 설계·구현·시연 (전공 Q&A, 문서 검색 등) |

---

##  세부 시간표 (수업 진행 상황에 따라 바뀔 수 있음)

| 일차 | 주요 단원 | 학습 내용 | 방법 |
|------|------------|------------|------|
| 1일차 | 과정소개 | 과정 목표 및 LLM 개요 | 이론 |
|  | 개발환경 준비 | Python·OpenAI 세팅, LangChain 설치 | 이론/실습 |
|  | LCEL 실습 | 프롬프트 실습, 대화형 챗봇 구현, MCP 기초 | 실습 |
| 2일차 | 문서 기반 챗봇 | 벡터DB 및 RetrievalQA 구현 | 실습 |
|  | 웹 UI 연동·배포 | Streamlit UI 구성 및 배포 | 실습 |
|  | 최종 프로젝트 | 개인 챗봇 제작 및 과제 제출 | 실습/평가 |

## 실습 환경 준비

본 과정의 모든 실습은 **Python 3.10 기반 Conda 가상환경**에서 진행됩니다.  
원활한 실습 진행을 위해 아래 절차를 **사전에 반드시 완료**해 주시기 바랍니다.

### 1. Anaconda 설치

본 실습은 **Conda 기반 Python 가상환경**을 사용합니다.  
아래 공식 페이지에서 운영체제(Windows / macOS / Linux)에 맞는 **Anaconda**를 먼저 설치해 주세요.

Anaconda 공식 다운로드 페이지:  
https://www.anaconda.com/download/success

### 2. Conda 가상환경 생성 (Python 3.10)

```bash
conda create -n py310 python=3.10
```

### 3. 가상환경 활성화

```bash
conda activate py310
```

### 4. 실습에 필요한 패키지 설치

본 저장소 루트 디렉터리에 포함된 `requirements.txt` 파일을 사용하여  
실습에 필요한 모든 라이브러리를 설치합니다.
설치된 경로로 이동한 뒤 설치합니다

```bash
pip install -r requirements.txt
```

### 5. VS Code 설치 (선택)

본 과정의 실습은 Jupyter Notebook 또는 Visual Studio Code(VS Code)를 활용하여 진행됩니다.  
원활한 실습과 코드 관리, 디버깅을 위해 **VS Code 사용을 권장**합니다.

VS Code가 설치되어 있지 않은 경우, 아래 공식 홈페이지를 통해 설치해 주세요.

https://code.visualstudio.com/

 
 
 # langchainClass
