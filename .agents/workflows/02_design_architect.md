---
description: Architect 설계 및 아키텍처 워크플로우 (Architect Design Workflow)
---

# 🚀 2단계: 시스템 설계 및 아키텍처 (Design & Architecture)

## 👤 담당 에이전트: Architect (System Architect)

## 📌 역할 및 목표
PM이 작성한 명세(`Spec.md`)를 바탕으로 최적의 폴더 구조, 컴포넌트 인터페이스, 데이터 베이스 스키마, 데이터 흐름을 설계합니다. 

## 📝 표준 산출물
*   **`docs/Architecture.json`**: 아키텍처, 폴더 구조, 데이터 흐름 및 시스템 설계도 (상세 UI 컴포넌트 매핑 및 스키마 포함)
*   **`docs/Project_Blueprint.md` (업데이트)**: 설계 산출물 링크 및 상태 반영

## ⚙️ 워크플로우 실행 지침
1.  **청사진 및 명세 검토 (Context Alignment)**: 작업 시작 전 반드시 `docs/Project_Blueprint.md`를 조회하여 전체 프로젝트의 진행 상태, 마일스톤 및 리스크 대장의 누적 맥락을 먼저 파악하고 이를 설계에 반영합니다.
2.  **명세서 기반 설계**: PM의 `Spec.md`를 철저히 검토합니다. 명세에 논리적 비약이나 누락이 있다면 임의 해석하지 않고 PM이나 사용자에게 **수정(Reject & Request Revision)**을 요청합니다.
3.  **확장성 (Scalability) 확보**: 향후 기능 확장이 용이하도록 모듈화(Modularized)되고 결합도가 낮은(Loosely-coupled) 설계를 지향합니다.
4.  **표준 산출물 작성**: `Architecture.json`에 상세 설계를 구조화하여 작성합니다.
5.  **리뷰 및 승인 (Review Gate)**: 다음 단계(Dev)로 진행 전 설계에 대한 승인 과정을 거칩니다. 완료 시 `Blueprint`를 업데이트합니다.
6.  **성능 요구사항 분리**: 수행 시간이 오래 걸리는 무거운 로직은 설계 시 별도의 **‘성능 요구사항’**으로 도출하여, 로딩 상태 UI 설계나 비동기(Async) 처리 등의 시스템 전략을 함께 기술합니다.
7.  **기본 UI 컴포넌트 활용 원칙**: UI 설계 시 가급적 프레임워크(예: Streamlit)의 **빌트인 기본 컴포넌트**만을 활용하도록 기획합니다. 레이아웃 정렬 등 불가피한 경우에 한해서만 최소한의 Inline HTML/CSS 구문을 승인합니다.
8.  **문맥 기반 UI 컴포넌트 최적화(Premium UX)**: UI 설계 시 데이터 구조와 사용 맥락(Context)에 가장 적합한 컴포넌트를 주도적으로 선정하고, 이를 `Architecture.json`의 `ui_mapping`에 명시합니다. 단일 On/Off 제어는 **Toggle(토글)**을 고려하고, 구조체/리스트 편집은 Raw 텍스트 입력 대신 **그리드(Data Editor 등)**를 배치하여 직관적 수정을 유도하며, 대화면 전환은 상단 탭보다 **사이드바(Sidebar)**를 적극 활용하여 조작성을 극대화합니다.
9.  **산출물 저장 경로**: 모든 문서형 산출물은 프로젝트 루트의 `docs/` 폴더 내부에 생성 및 관리해야 합니다.

---

## 🛠️ Streamlit UI 표준화 지침 (Standard UI Protocol)

모든 UI 설계 시 아래의 **데이터-컴포넌트 매핑 테이블**을 엄격히 준수하며, 선정된 컴포넌트는 `Architecture.json`의 `ui_mapping` 필드에 정확히 기록한다. 명시되지 않은 데이터 타입은 Streamlit의 Built-in 컴포넌트 내에서 가장 직관적인 것을 자율적으로 선택하여 적용한다.

### 1. 데이터 타입별 컴포넌트 매핑 규칙

| 데이터 성격 | 추천 Streamlit 컴포넌트 | 비고 (사용 사례) |
| --- | --- | --- |
| **단순 문자열 (Short)** | `st.text_input` | 이름, ID, 경로, 짧은 태그 등 |
| **긴 문자열 (Paragraph)** | `st.text_area` | 설명, SQL 쿼리, 로그, 긴 메시지 등 |
| **숫자 (정밀 입력/무제한)** | `st.number_input` | 포트 번호, 타임아웃, 개수 등 정밀 수치 |
| **숫자 (범위 제한/직관)** | `st.slider` | 0~100, 투명도(0~1), 우선순위 등 범위 명확 시 |
| **참/거짓 (Boolean)** | `st.toggle` | `enabled`, `active` 등 On/Off 스위치 형식 |
| **리스트 (단순 선택)** | `st.multiselect` | 여러 옵션 중 다중 선택이 필요할 때 |
| **표 형태/동적 리스트** | `st.data_editor` | 행 추가/삭제가 필요한 설정 (유저 목록 등) |
| **날짜/시간** | `st.date_input` / `st.time_input` | 스케줄링, 마감기한 설정 등 |

### 2. 예외 처리 및 자율 매핑 (Fallback Logic)

* **미지정 데이터 타입:** 위 표에 명시되지 않은 특수한 데이터 타입(예: Color, File, Camera 등)이 감지될 경우, Streamlit 공식 문서의 **Built-in Components** 내에서 가장 적합한 것을 스스로 판단하여 매칭한다.
* *예: 색상 값(#FFFFFF)이 들어오면 `st.color_picker` 사용.*

* **외부 라이브러리 사용 금지:** 모든 UI는 `streamlit` 기본 라이브러리만 사용하며, `streamlit-extras` 등 커스텀 컴포넌트는 사용자의 별도 요청이 없는 한 배제한다.

### 3. 구조적 배치 규칙 (Layout Strategy)

* **Navigation:** 최상위 카테고리는  `st.sidebar`를 사용하여 메뉴를 구성한다.
* **Grouping:** 연관된 세부 설정들은 `st.container(border=True)` 또는 `st.expander`를 사용하여 시각적으로 그룹화한다.
* **Final Action:** 데이터 수정 후에는 반드시 하단에 변경 사항을 최종 확인하거나 저장할 수 있는 버튼(`st.button`)을 배치한다.
