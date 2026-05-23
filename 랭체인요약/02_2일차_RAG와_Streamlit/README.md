# 2일차 - RAG와 웹 UI 요약

원본 노트북: `2일차.ipynb`

## 한 줄 요약

RAG는 AI가 모르는 내용을 문서에서 먼저 찾고, 그 근거를 바탕으로 답하게 만드는 방식입니다.

## 핵심 개념

- RAG: 검색해서 답변하는 AI 구조입니다.
- 문서 로딩: PDF 같은 파일을 읽어 텍스트로 바꿉니다.
- 청크 분할: 긴 문서를 작은 조각으로 나눕니다.
- 임베딩: 문장을 숫자 벡터로 바꿔 검색 가능하게 만듭니다.
- FAISS: 비슷한 문서 조각을 빠르게 찾는 벡터DB입니다.
- Retriever: 질문과 가까운 문서 조각을 찾아오는 검색기입니다.
- Streamlit: Python 코드로 웹 화면을 쉽게 만드는 도구입니다.

## 단계별 정리

### 1단계. PDF 읽기

```python
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader("data/Samsung_Card_Manual_Korean_1.3.pdf")
pages = loader.load()
```

### 2단계. 문서를 작게 나누기

긴 문서를 한 번에 넣으면 검색 품질이 떨어집니다. 그래서 작은 조각으로 나눕니다.

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=100)
docs = splitter.split_documents(pages)
```

### 3단계. 벡터DB 만들기

```python
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import FAISS

embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vectordb = FAISS.from_documents(docs, embeddings)
retriever = vectordb.as_retriever(search_kwargs={"k": 3})
```

### 4단계. 질문과 문서를 함께 LLM에 전달하기

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_template("""
다음 참고 문서를 바탕으로 질문에 답하세요.

[참고문서]
{context}

[질문]
{question}
""")

rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | ChatOpenAI(model="gpt-5-nano", temperature=0)
)

answer = rag_chain.invoke("동시에 몇 개의 카드를 인식할 수 있나요?")
print(answer.content)
```

### 5단계. Streamlit으로 화면 만들기

```python
import streamlit as st

st.title("문서 기반 챗봇")
question = st.text_input("질문을 입력하세요")

if question:
    answer = rag_chain.invoke(question)
    st.write(answer.content)
```

## 쉬운 예시

일반 LLM은 “아마도 이런 기능일 것입니다.”처럼 추측할 수 있습니다.

RAG 챗봇은 “매뉴얼의 해당 문서에 따르면 ...”처럼 근거를 보고 답하는 것이 목표입니다.

## 체크포인트

- 문서는 먼저 작게 나눠야 검색이 잘 됩니다.
- 질문도 임베딩되어 문서 조각과 비교됩니다.
- 검색된 문서를 프롬프트에 넣어야 근거 기반 답변이 됩니다.
- Streamlit을 붙이면 노트북 코드가 웹 챗봇처럼 보입니다.
