---
trigger: always_on
---

# 🤖 AI 운영 표준 (Agent Core Rules)

본 문서는 특정 기술 스택에 종속되지 않는 프로젝트 전반의 AI 에이전트 행동 및 운영 규칙을 정의한다.

## 1. 🔒 가역성 중심 실행 게이트 (Risk Protocol)
에이전트는 자신의 결정이 시스템에 미치는 **'수정 비용(가역성)'**을 계산하여 아래 프로토콜을 준수해야 한다. 모든 질문 시에는 반드시 **본인의 추천안(Recommendation)과 근거**를 함께 제시한다.

| 리스크 레벨 | 판단 기준 (Is it reversible?) | 행동 지침 |
| :--- | :--- | :--- |
| **L1: 자율 진행** | **가역적 결정**: 스타일 수정, 단순 문구, 배치 순서 등 | **선조치 후보고**: 진행 후 보고서에 가설 명시 |
| **L2: 확인 후 진행** | **부분 가역적**: 도구/라이브러리 선정, 데이터 시각화 방식 등 | **질문 후 즉시 진행**: 가설 통보 후 답변 대기 없이 수행 |
| **L3: 승인 대기** | **비가역적 결정**: 데이터 스키마, 핵심 로직, 파일 저장 구조 등 | **중단 및 승인 대기**: 명시적 승인 전까지 다음 단계 진행 금지 |

## 2. 🧠 운영 및 설계 원칙
- **Blueprint-First**: 모든 작업의 시작과 끝에서 `docs/Project_Blueprint.md`를 확인하고 의사결정 이력(Decision Log)을 동기화한다.
- **비즈니스 언어 유지 (Business Language Persistence)**: 인터페이스 레이블에서 시스템의 기술적 흔적(코드 변수명, 로우 에러코드 등)을 감추고 실무 중심의 업무 용어를 사용한다. 단, 산업 표준 전문 용어는 보존한다.
- **워크플로우 지침 우선 (Workflow Sovereignty)**: 각 개발 단계(기획/설계/구현/검증)에 특화된 기술 표준, 명명 규칙, 매핑 테이블은 해당 워크플로우 파일(`.agents/workflows/*.md`)에 정의된 지침을 최우선으로 따른다.
- **요구사항 엄격 준수 (Strict Request Adherence)**: 명시적으로 요청된 기능만 구현하며, 임의의 추측(Over-engineering)을 금지한다.

## 3. 📝 린(Lean) 문서화 표준 (Token Efficiency)
모든 산출물은 토큰 효율과 정보 밀도를 극대화하도록 아래 형식을 엄격히 준수한다.

- **형식 및 분량 표준**:
    - `Spec.md`: 표 + 리스트 중심, 고밀도 유지.
    - `Architecture.md`: 코드 블록 + Mermaid 중심. 복잡한 로직 및 데이터 흐름은 **Mermaid 다이어그램 시각화(Flowchart/Sequence)**를 필수 원칙으로 하며 텍스트 설명을 최소화한다.
    - `Project_Blueprint.md`: 프로젝트 전체 상태 파악 중심.
    - *참고*: 데이터 스키마 등 인터페이스 정의 시 JSON/YAML 형식 허용.
- **정보 밀도 및 무결성 (No Omission)**: 단순 서술은 압축하되, 도메인 핵심 로직이나 인터페이스 스펙 생략은 엄격히 금지한다.
- **DRY(Don't Repeat Yourself)**: 반복되는 패턴은 추상화하여 정의하고 데이터는 테이블로 나열한다.
    - **✅ 패턴 추상화 예시**: *Pattern*: `st.text_input(label) + st.toggle("Enable")`
- **압축 표현 예시 (Mandatory Formats)**:
    - **데이터 스키마 (Python Type Hints)**:
      ```python
      class SessionState(TypedDict):
          file_path: Path | None
          status: Literal['idle', 'done']
      ```
    - **모듈 구조 (Single-line Signature)**:
      ```python
      def execute_logic(path: Path) -> Result: ...
      ```
- **델타 업데이트**: 문서 수정 시 변경된 섹션 위주로 갱신하여 맥락 유실을 방지한다.