# MCP 기초 요약

원본 노트북: `MCP기초.ipynb`

## 한 줄 요약

MCP는 LLM이 파일 읽기, 외부 API 호출, Notion 저장 같은 실제 행동을 표준 방식으로 실행하게 해주는 연결 규칙입니다.

## 핵심 개념

- MCP Host: 사용자와 대화하고 전체 흐름을 지휘하는 앱입니다.
- MCP Client: Host와 Server 사이에서 요청과 응답을 전달합니다.
- MCP Server: 실제 작업을 수행하는 도구 모음입니다.
- Tool: 서버가 제공하는 하나의 기능입니다. 예: JSON 읽기, Notion 업로드.
- fastmcp: Python 함수로 MCP Server를 쉽게 만드는 라이브러리입니다.

## 단계별 정리

### 1단계. 구조 이해하기

```text
사용자 요청 -> Host 판단 -> Client 전달 -> Server 실행 -> 결과 반환
```

비유하면 다음과 같습니다.

- Host: 주방장, 무엇을 만들지 결정합니다.
- Client: 웨이터, 지시를 전달합니다.
- Server: 조리 도구, 실제 작업을 합니다.

### 2단계. 실습 목표 정하기

노트북의 기본 실습은 이 문장을 자동 처리하는 것입니다.

> train_result.json 파일을 요약해서 Notion에 기록해줘.

실제로는 아래 일이 순서대로 일어납니다.

1. JSON 파일을 읽습니다.
2. LLM이 내용을 요약합니다.
3. Notion 페이지에 요약을 저장합니다.

### 3단계. 더미 JSON 준비하기

```json
{
  "epoch": 0.12,
  "global_step": 500,
  "loss": 0.3358,
  "learning_rate": 0.0000188
}
```

### 4단계. MCP Server 만들기

Python 함수 하나를 MCP Tool 하나로 등록합니다.

```python
from fastmcp import FastMCP
import json

mcp = FastMCP("ExperimentResultServer")

@mcp.tool()
def read_experiment_result(file_path: str) -> dict:
    with open(file_path, "r", encoding="utf-8") as f:
        return json.load(f)

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

### 5단계. Notion 같은 외부 서비스와 연결하기

Notion에 저장하려면 보통 아래 정보가 필요합니다.

- `NOTION_TOKEN`
- `NOTION_PAGE_ID`

이 값도 API 키처럼 `.env` 파일에 넣는 것이 안전합니다.

### 6단계. PDF 요약으로 확장하기

```text
PDF 읽기 -> 텍스트 추출 -> LLM 요약 -> Notion 저장
```

## 쉬운 예시

MCP가 없으면 사용자가 파일 열기, 내용 복사, AI에게 붙여넣기, 요약 복사, Notion 붙여넣기를 직접 해야 합니다.

MCP를 쓰면 자연어 한 문장으로 이 과정을 자동화할 수 있습니다.

## 체크포인트

- LLM이 직접 파일이나 Notion을 만지는 것이 아닙니다.
- 실제 작업은 MCP Server의 Tool이 합니다.
- Host, Client, Server 역할을 구분하면 구조가 쉬워집니다.
- 토큰과 페이지 ID는 `.env`에 보관합니다.

## 추가 실습 파일

- `python_기초_실습.ipynb`: MCP 실습 전에 바로 실행해 볼 수 있는 Python 기초 문법 노트북입니다. 출력, 변수, 리스트, 딕셔너리, 조건문, 반복문, 함수, JSON 파일 읽기/쓰기를 순서대로 연습합니다.
