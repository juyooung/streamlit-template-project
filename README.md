# 🤖 Role-Based Multi-Agent Workflows for Streamlit

이 저장소는 AI 에이전트(PM, Architect, Dev, QA)가 프로젝트 기획부터 검증까지 순차적이고 체계적인 소프트웨어 개발 라이프사이클을 수행하도록 유도하는 **에이전트 워크플로우 지침** 모음입니다.

특히 **Streamlit 프레임워크** 기반 애플리케이션 개발에 최적화된 설계 및 구현 가이드라인을 내장하고 있으며, **모델이 최대한 일관된 UI를 도출해낼 수 있도록 설계**되었습니다.

---

## 📂 저장소 구조 (File Structure)

```text
.agents/
├── rules/
│   └── agent-core-rules.md       # AI 운영 표준 (리스크 프로토콜 & 린 문서화)
└── workflows/
    ├── 01_planning_pm.md         # 1단계: 기획 및 요구사항 정의 (PM)
    ├── 02_design_architect.md    # 2단계: 시스템 설계 및 아키텍처 (Architect)
    ├── 03_implementation_dev.md  # 3단계: 시스템 구현 및 개발 (Developer)
    └── 04_verification_qa.md     # 4단계: 검증 및 품질 보증 (QA)
```

모든 문서형 산출물(Spec, Architecture, Report 등)은 프로젝트 루트의 `docs/` 폴더 내부 및 `docs/Project_Blueprint.md` 허브를 통해 문서의 결합성을 유지하도록 설계되어 있습니다.

---

## 👥 단계별 워크플로우 구성 (Workflow Stages)

### 🚀 1단계: 기획 및 요구사항 정의 (`01_planning_pm.md`)

- **담당:** PM (Product Manager)
- **역할:** 포괄적인 아이디어를 개발 가능한 구체적인 기능 명세로 기획 및 전환합니다.
- **주요 산출물:** `docs/Spec.md` (명세서), `docs/Project_Blueprint.md` (초안)

### 🚀 2단계: 시스템 설계 및 아키텍처 (`02_design_architect.md`)

- **담당:** Architect (System Architect)
- **역할:** 최적의 폴더 구조, 컴포넌트 인터페이스, 데이터 베이스 스키마, 데이터 흐름 설계
- **특징 (Streamlit 표준화):**
  - 빌트인 컴포넌트 우선 원칙 및 데이터 타입별 자동 매핑 가이드라인 제공
  - `st.sidebar` 및 그리드 형태 데이터 편집기 등 프리미엄 UX 구조를 강제화하여 일관성 유지
- **주요 산출물:** `docs/Architecture.md` (아키텍처 명세)

### 🚀 3단계: 시스템 구현 및 개발 (`03_implementation_dev.md`)

- **담당:** Developer (Software Engineer)
- **역할:** 설계도(`Architecture.md`)를 엄격히 준수하여 실행 가능한 응용 코딩 및 모듈 결합도 관리
- **특징:** `st.session_state`를 통한 상태 관리 및 에러 핸들링 원칙을 적용하고 임의의 외부 UI 라이브러리 도입을 규제하여 성능 고립성 확보
- **주요 산출물:** `Source Code` (실행 가능한 앱 스크립트)

### 🚀 4단계: 검증 및 품질 보증 (`04_verification_qa.md`)

- **담당:** QA (Quality Assurance)
- **역할:** 개발된 코드 구동검증, 최초 명세 대비 기능 동작 비교 및 오류 자가개선 루프 가동
- **특징:** 메인 엔트리포인트를 통한 **통합 E2E 검증** 강제화로 위젯 누락 및 임포트 오류 사전 차단
- **주요 산출물:** `docs/QA_Report.md` (보고서 및 피드백 전파)

---

## 🚀 에이전트 구동 지침 (Usage)

AI 에이전트에게 작업을 지시할 때, 프로젝트의 세부 요구사항을 정리한 프롬프트와 함께 첫 단계 지침을 포함하여 호출합니다.

1. **프로젝트 시작:** 초기 요구사항을 상세히 적은 프롬프트와 함께 `01_planning_pm.md` 워크플로우를 호출합니다.

   ```
   /01_planning_pm 다음 요구사항으로 프로젝트를 기획해줘: [요구사항 상세]
   ```

2. **산출물 검토:** 에이전트가 요구사항을 분석하여 `docs/Spec.md` 및 `Blueprint` 산출물을 생성하면, 내용을 검토하고 확정합니다.
3. **순차적 단계 진행:** 기획 단계 산출물이 확정되면 다음 단계인 `02_design_architect.md`를 호출하여 아키텍처 설계를 진행합니다. 동일한 방식으로 산출물 확인 후 `03_implementation_dev.md` (구현), `04_verification_qa.md` (QA 및 검증) 순으로 단계별 턴을 분리하여 순차적으로 진행합니다.
