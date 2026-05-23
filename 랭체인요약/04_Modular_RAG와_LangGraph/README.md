# Modular RAG와 LangGraph 요약

원본 노트북: `Modular_RAG와Langraph.ipynb`

## 한 줄 요약

Modular RAG는 RAG 단계를 고정된 한 줄 흐름이 아니라, 필요하면 분기하고 되돌아갈 수 있는 모듈형 흐름으로 만드는 방식입니다.

## 핵심 개념

- 기존 RAG: `Query -> Retrieve -> Generate`처럼 한 방향으로 흐릅니다.
- Modular RAG: Rewrite, Retrieve, Rerank, Compress, Generate 같은 단계를 모듈로 나눕니다.
- LangGraph: 이런 흐름을 그래프 구조로 설계하는 도구입니다.
- Node: 실제 작업을 하는 함수입니다.
- Edge: 다음에 어떤 Node로 갈지 정하는 연결선입니다.
- State: 전체 흐름에서 공유되는 데이터입니다.
- Routing: 조건에 따라 다음 단계를 다르게 선택하는 방식입니다.

## 단계별 정리

### 1단계. 기존 RAG의 한계 이해하기

```text
Query -> Retrieve -> Generate
```

기본 RAG는 단순하지만, 검색 결과가 이상하거나 문서가 너무 길면 다시 검색하거나 압축하는 흐름이 필요합니다.

### 2단계. Modular RAG 흐름으로 바꾸기

```text
Query
 -> Rewrite
 -> Retrieve
 -> 조건 판단
      -> Rerank
      -> Compress
      -> Generate
      -> Rewrite로 재시도
```

### 3단계. State 정의하기

State는 그래프 전체가 공유하는 가방 같은 역할입니다.

```python
from typing import TypedDict, List

class RAGState(TypedDict):
    query: str
    rewritten_query: str
    docs: List[str]
    answer: str
```

### 4단계. Node 만들기

각 Node는 입력 State를 보고 필요한 값을 추가하거나 바꿉니다.

```python
def rewrite_node(state: RAGState):
    rewritten = f"{state['query']} (정책 보고서 기준)"
    return {"rewritten_query": rewritten}


def retrieve_node(state: RAGState):
    docs = [
        "2025년 농업기술 정책 보고서 요약본",
        "2024년 농업 R&D 투자 방향"
    ]
    return {"docs": docs}


def generate_node(state: RAGState):
    return {"answer": f"총 {len(state['docs'])}개 문서를 기반으로 답변했습니다."}
```

### 5단계. Graph로 연결하기

```python
from langgraph.graph import StateGraph

graph = StateGraph(RAGState)
graph.add_node("rewrite", rewrite_node)
graph.add_node("retrieve", retrieve_node)
graph.add_node("generate", generate_node)

graph.add_edge("rewrite", "retrieve")
graph.add_edge("retrieve", "generate")
```

### 6단계. Routing 추가하기

```text
Retrieve 결과 확인
 -> 문서가 많음: Compress로 이동
 -> 문서가 적당함: Generate로 이동
```

## 쉬운 예시

질문이 “2025년 농업기술 정책 보고서를 요약해줘.”라고 들어왔다고 가정합니다. 검색 결과가 오래된 문서 위주라면 다시 검색하고, 문서가 너무 많다면 먼저 압축해야 합니다. Modular RAG는 이런 판단을 흐름 안에 넣을 수 있습니다.

## 체크포인트

- Node는 하나의 작업 단위입니다.
- State는 Node 사이를 이동하는 데이터입니다.
- Edge는 작업 순서를 정합니다.
- Conditional Edge를 쓰면 조건별 분기가 가능합니다.
- LangGraph는 복잡한 LLM 작업 흐름을 눈에 보이게 설계할 때 유용합니다.
