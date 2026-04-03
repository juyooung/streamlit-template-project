---
description: [02_design_architect] Architect Design Workflow
---

# 🚀 2단계: 시스템 설계 및 아키텍처 (Architecture & Design)

## 👤 담당 에이전트: Architect

## 📌 역할 및 목표
PM이 정의한 데이터 유형을 바탕으로 **`ai-operational-standard.md`**의 리스크 프로토콜 및 비즈니스 언어 규칙을 준수하며, 아래 Streamlit 기술 표준을 적용하여 **최종 위젯 매핑**과 **세션 상태** 구조를 확정합니다.

## 📝 표준 산출물
* **`docs/Architecture.md`**: 위젯-데이터 1:1 매핑 표, 전역 상태(Type Hints), 모듈 시그니처 명세
* **`docs/Project_Blueprint.md` (업데이트)**: 확정된 파일 구조 및 기술 스택 반영
* **표준 폴더 구조 준수**:
    - **`src/`**: Pure Python 로직 (UI 의존성 없음)
    - **`pages/`**: 페이지 단위 UI 조립
    - **`src/components/`**: 재사용 위젯 함수 전용 (필요 시)

## ⚙️ 워크플로우 실행 지침

1. **위젯 매핑 및 UI 표준화 (Critical)**
    * PM이 정의한 데이터 성격에 따라 최적의 Streamlit 위젯을 확정합니다. 
    * **[위젯 매핑 표준 (Component Mapping - Strict)]**:
        | 데이터 성격 | 추천 컴포넌트 | **구현 가이드 및 제약 (Strict)** |
        | :--- | :--- | :--- |
        | **활성/비활성 (On/Off)** | **`st.toggle`** | **`st.checkbox` 사용 엄격 금지** |
        | **단일 선택 (항목 3개 이하)** | `st.segmented_control` | 프리미엄 세그먼트 사용 |
        | **단일 선택 (항목 4개 이상)** | `st.selectbox` | 가독성 확보, 필요 시 검색 활성화 |
        | **다중 선택 (필터/태그)** | `st.multiselect` | 칩(Chip) UI 리스트 관리 |
        | **데이터 편집 (YAML/Table)** | **`st.data_editor`** | 구조 보전 편집 필수 |
        | **프로세스 진행/상태** | `st.status`, `st.toast` | 실무적 공정 상태 표현 |
        | **수치/문자열 입력** | `st.number_input`, `st.text_input` | 타입 일치 및 실시간 Validation |
    * **[공식 API 최우선]**: 사용자 요청 없이 외부 커스텀 컴포넌트를 사용하지 않으며, 공식 Streamlit API를 최우선으로 사용하여 설계한다.
    * **[위젯 키 컨벤션 (Mandatory)]**: `DuplicateWidgetID` 에러 방지를 위해 아래 네이밍 문법을 아키텍처에 명시한다.
        * **멀티 페이지**: `[Page]__[Section]__[Variable]` (예: `SIR__Settings__Enable`)
        * **단일 페이지**: `[Section]__[Variable]` (예: `Validation__Uploader`)
    * **[위젯 매핑 (Widget Mapping from Spec)]**: PM이 정의한 `Spec.md`의 데이터 필드와 비즈니스 의도를 바탕으로 최적의 위젯을 매핑합니다. 아키텍트는 소스 파일을 다시 분석하는 대신, 기획서에 명시된 모든 입력/출력 항목에 대해 기술적 구현 방식(위젯 결정, ID 부여)을 확정하는 데 집중합니다.
    * **[구조적 소형화 (Structural Compression)]**: 매핑 항목이 방대할 경우, 중복 패턴은 추상적으로 정의하고 상세 데이터는 표(Table)로 나열하여 린 문서 표준을 유지합니다. 
    * **[매핑 우선순위 (Mapping Priority)]**: 린 문서 표준(Lean)을 준수하되, 아키텍처 설계 단계에서의 **'위젯-데이터 1:1 매핑 표'는 생략 불가능한 필수 섹션**으로 간주한다.
    * **[압축 표현 규격]**: 설계서 작성 시 장황한 서술을 지양하고 아래 규격을 준수한다.
        * **세션 상태**: Python `TypedDict` 및 `Literal`을 사용하여 스키마를 엄격히 정의한다.
        * **모듈 인터페이스**: 로직 본문 없이 단일 행 시그니처(e.g., `def func(path: Path) -> None: ...`)로 정의한다.

2. **상태 관리 및 세션 설계 (State Strategy)**
    * 위젯의 상태를 관리할 `st.session_state` 스키마와 초기화 로직을 설계합니다.
    * 페이지 이동 시의 상태 보존 전략과 `@st.fragment`를 사용한 리런 최적화 지점을 확정합니다.

3. **입출력 데이터 파이프라인 설계**
    * 입출력 구조 설계 시 **구현 워크플로우(03_implementation_dev.md)에 정의된 무손실 IO 및 경로 처리 가이드**를 기반으로 데이터 흐름을 설계합니다.
    * 비즈니스 로직(`src/`)과 UI(`pages/`) 사이의 인터페이스(Type Hinting 포함)를 정의합니다.

4. **성능 및 복잡도 관리**
    * 3초 이상 소요될 로직에 대한 피드백(spinner/status) 적용 계획을 설계서에 포함합니다.
    * Mermaid 다이어그램 작성 시 노드 수를 15개 이내로 제한하여 가독성을 유지합니다.

### 🏁 Validation Gate

Phase 2 완료 조건 확인:
- **[언어 준수]**: 모든 설계 문서(Architecture.md, Project_Blueprint.md)가 **영어(English)**로 작성되었는가?
- Architecture.md에 전역 상태(Session State), 모듈 구조, UI 레이아웃 패턴 정의 완료
- Spec.md의 각 데이터 요소가 기술 표준의 위젯 매핑 규칙을 준수하는지 확인
- Project_Blueprint.md에 설계 결정 사항 반영 및 마일스톤 업데이트
- AI 운영 표준(agent-core-rules.md) 준수 여부 (리스크 프로토콜, 비즈니스 언어)
- 워크플로우별 공통 기술 수칙 및 위젯 매핑 표준 준수

→ 모든 항목 충족 시 사용자 승인 요청