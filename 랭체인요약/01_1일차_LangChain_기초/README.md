# 1일차 - LangChain 기초 요약

원본 노트북: `1일차.ipynb`

## 한 줄 요약

LangChain은 LLM, 프롬프트, 출력 정리, 메모리를 순서대로 연결해서 챗봇을 쉽게 만드는 도구입니다.

## 핵심 개념

- LLM: 질문을 받고 답변을 만드는 AI 모델입니다.
- Prompt: AI에게 역할과 답변 형식을 알려주는 지시문입니다.
- LCEL: `prompt | model | parser`처럼 단계를 파이프처럼 연결하는 방식입니다.
- OutputParser: AI 응답에서 필요한 텍스트만 깔끔하게 꺼냅니다.
- Memory: 이전 대화를 저장해서 챗봇이 맥락을 기억하게 합니다.

## 단계별 정리

### 1단계. 환경 준비

```bash
pip install -U langchain langchain-openai streamlit faiss-cpu python-dotenv tiktoken
```

API 키는 코드에 직접 쓰지 말고 `.env` 파일에 저장합니다.

```bash
OPENAI_API_KEY=sk-...
```

### 2단계. 가장 기본 체인 만들기

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from dotenv import load_dotenv

load_dotenv()

llm = ChatOpenAI(model="gpt-5-nano")
prompt = PromptTemplate.from_template("'{topic}' 주제를 한 문장으로 설명해줘.")
parser = StrOutputParser()

chain = prompt | llm | parser
result = chain.invoke({"topic": "LangChain"})
print(result)
```

### 3단계. 역할이 있는 챗봇 만들기

`system` 메시지로 AI의 역할을 정하면 답변 스타일이 달라집니다.

```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "너는 친절한 여행 전문가야."),
    ("user", "부산 2박 3일 여행 일정을 만들어줘.")
])
```

### 4단계. Few-shot 예시 넣기

AI에게 예시 답변을 몇 개 보여주면 비슷한 말투와 형식으로 답합니다.

예시 흐름:

1. 사용자가 라면 비법을 질문합니다.
2. AI가 특정 말투로 답한 예시를 보여줍니다.
3. 새 질문을 넣으면 AI가 같은 스타일로 답합니다.

### 5단계. 대화 기억 붙이기

- Buffer Memory: 모든 대화를 저장합니다.
- Window Memory: 최근 몇 개 대화만 저장합니다.
- Summary Memory: 이전 대화를 요약해서 저장합니다.

## 쉬운 예시

```text
사용자 질문 -> 프롬프트 -> LLM -> 출력 정리 -> 답변
```

사람에게 “너는 여행 가이드야. 이전 대화를 기억하고 추천해줘.”라고 말하는 것을 코드로 만든다고 생각하면 됩니다.

## 체크포인트

- API 키는 반드시 `.env`에 둡니다.
- LCEL은 `|` 기호로 단계를 연결합니다.
- 프롬프트를 잘 쓰면 답변 품질이 크게 좋아집니다.
- 메모리는 챗봇이 대화 맥락을 이어가게 합니다.
