---
description: [03_implementation_dev] Developer Implementation Workflow
---

# 🚀 3단계: 기능 구현 및 개발 (Implementation & Dev)

## 👤 담당 에이전트: Developer

## 📌 역할 및 목표
설계서(`Architecture.md`)를 바탕으로 검증 가능한 견고한 코드를 작성합니다. **비즈니스 로직과 UI 기능의 무결성(Integrity)**을 확보하고, 오버엔지니어링 없이 요구사항을 완벽히 달성하는 것이 핵심입니다.

## 📝 표준 산출물
* **구현 코드 (`src/`, `pages/`, `app.py`)**: 주석 및 독스트링이 포함된 생산 등급 코드
* **`docs/Project_Blueprint.md` (업데이트)**: 구현 완료된 기능 및 추가된 파일 반영

## ⚙️ 워크플로우 실행 지침

1. **비즈니스 로직 및 모듈화 구현 (Core Implementation)**
    * `src/` 폴더 내에 UI 의존성 없는 독립적인 로직 함수를 먼저 완결합니다.
    * 설계된 데이터 타입과 인터페이스를 엄격히 준수하여 모듈 간 결합도를 낮춥니다.

2. **설계 준수 UI 어셈블리 (Adherence to Design)**
    * Architect가 매핑한 위젯 컨벤션을 1:1로 적용하여 UI를 구성합니다.
    * 위젯의 `key` 규칙 및 `on_change` 콜백을 통한 상태 동기화 로직을 정밀하게 구현합니다.

3. **기술 표준 및 안정성 확보 (Standard Compliance)**
    * **[경로 정규화]**: 파일 입출력이 발생하는 함수 입구(Entry point)에서 반드시 `.resolve()`를 사용하여 절대 경로 객체로 변환하여 처리한다.
    * **[무손실 데이터 관리]**: 기존 설정이나 데이터 파일(YAML 등) 수정 시, 원본의 주석 및 구조를 100% 보존할 수 있는 라이브러리(e.g., `ruamel.yaml`)를 우선 사용한다.
    * 비정상적인 사용자 입력이나 환경 문제에 대한 예외 처리를 포함하여 앱의 중단(Crash)을 방지한다.

4. **검증 가용성 확보 (Testability)**
    * 기능 구현 직후, 스스로 핵심 유즈케이스를 실행하여 로직의 정상 작동 여부를 선검증합니다.
    * **[실행 증명 (Execution Evidence)]**: 코드를 수정하거나 배포한 직후에는 반드시 `read_terminal`을 통해 로그를 확인하거나, 브라우저 스크린샷 등을 통해 UI 상의 Runtime Error(Indentation 포함)가 없음을 1회 이상 교차 검증해야 한다.
    * 구현 완료 시 `Project_Blueprint.md`를 최신화하여 QA 단계로 명확히 인계합니다.

### 🏁 Validation Gate

Phase 3 완료 조건 확인:
- 설계(Architecture.md)에 정의된 모듈 구조 및 UI 레이아웃 100% 구현
- 모든 파일 IO 작업 시 무손실 IO 라이브러리 및 절대 경로 처리 준수
- Project_Blueprint.md의 진행률(Progress) 최신화 및 마감 처리
- Streamlit 통합 표준 전체 준수 (Standard Library Only, Fragment 활용 등)

→ 모든 항목 충족 시 사용자 승인 요청