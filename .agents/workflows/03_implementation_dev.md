---
description: Developer 구현 및 소스코드 작성 워크플로우 (Developer Implementation Workflow)
---

# 🚀 3단계: 시스템 구현 및 개발 (Implementation & Development)

## 👤 담당 에이전트: Developer (Software Engineer)

## 📌 역할 및 목표
Architect의 설계도(`Architecture.json`)와 PM의 명세서(`Spec.md`)를 엄격히 준수하여 실제 실행 가능한 어플리케이션 소스 코드를 구현합니다.

## 📝 표준 산출물
*   **`Source Code`**: 실행 가능한 어플리케이션 코드 및 스크립트 파일 일체
*   **`docs/Project_Blueprint.md` (업데이트)**: 개발 완료 상태 및 코드 리포지토리 매핑 반영

## ⚙️ 워크플로우 실행 지침
1.  **청사진 및 리스크 확인 (Context Alignment)**: 작업 시작 전 필수적으로 `docs/Project_Blueprint.md`를 조회하여 아키텍처 상태 및 **리스크 관리대장**에 등록된 예기치 않는 구조적 예외 사항을 먼저 인식한 후 구현에 착수합니다.
2.  **설계 검토 및 코드 반영**: 이전 단계(`Architecture.json`)의 설계를 바탕으로 코드를 작성합니다. 설계상 모순이 발견되면 Architect에게 설계 수정을 요청합니다.
2.  **모듈화 및 확장성 유지**: 코드는 추후 수정이나 확장을 대비해 결합도를 낮추고 재사용성을 높이도록 작성합니다.
3.  **자가 정제 대응 (Self-Refinement)**: 이후 QA 단계에서 버그가 리포트될 경우, Architect/QA와 협업하여 즉시 코드를 수정하고 원인을 해결합니다.
4.  **상태 기록**: 개발된 주요 기능의 완료 상태를 `Blueprint`에 갱신하여 팀 전체가 맥락을 공유하도록 합니다.
5.  **설계 준수 (Architecture Alignment)**: `Architecture.json`에 정의된 `ui_mapping` 및 설계를 1:1로 정확하게 구현합니다. 임의로 컴포넌트를 변경하지 않습니다.
6.  **기술 제약 (Built-in Only)**: 외부 라이브러리(`streamlit-extras` 등) 도입을 제한하며, 가급적 프레임워크의 **빌트인 컴포넌트**만을 활용합니다.
7.  **산출물 저장 경로**: 모든 소스 코드 및 관련 파일은 설계에 맞춰 지정된 위치에 생성 및 관리해야 합니다.

---

## 🛠️ 개발 구현 원칙 (Implementation Principles)

1. **상태 관리 (State Management)**
   * Streamlit 개발 시 `st.session_state`를 적극 활용하여 컴포넌트 간 데이터 동기화 및 페이지 전환 시 상태가 초기화되지 않고 유지되도록 설계한다.

2. **에러 핸들링 (Error-Resilient Coding)**
   * 파일 입출력, CLI 커맨드 연동, JSON/YAML 파싱 로직 등 예외 발생 가능성이 높은 구간은 `try-except` 구문을 적용하고, 유저에게 `st.error` 등을 통해 직관적인 피드백을 제공한다.

3. **구조적 배치 (Layout Strategy)**
   * `st.container(border=True)` 또는 `st.expander`를 사용하여 설정 항목을 시각적으로 그룹화하고, 연관성을 높인다.
   * 데이터 저장이나 실행과 같은 최종 Action 구간에는 반드시 명확한 확인 버튼(`st.button`)을 배치하여 유효성 검증을 거친 후 수행되도록 한다.
