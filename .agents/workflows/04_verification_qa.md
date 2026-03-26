---
description: QA 검증 및 테스트 워크플로우 (QA Verification Workflow)
---

# 🚀 4단계: 검증 및 품질 보증 (Verification & QA)

## 👤 담당 에이전트: QA (Quality Assurance)

## 📌 역할 및 목표
Developer가 작성한 코드를 실행하고, PM이 최초 작성한 명세(`Spec.md`)와 철저히 대조하여 기능의 정상 작동 및 결함 여부를 검증합니다.

## 📝 표준 산출물
*   **`docs/Test_Report.md`**: 테스트 시나리오, 검증 결과(Pass/Fail) 및 결함 리포트
*   **`docs/Project_Blueprint.md` (최종 업데이트)**: 품질 검증 통과 여부 및 프로젝트 완료 상태 반영

## ⚙️ 워크플로우 실행 지침
1.  **청사진 및 아키텍처 조회 (Context Alignment)**: 검증 전 `docs/Project_Blueprint.md`를 조회하여 디렉토리 구조 및 리스크 항목을 교차 검사하고, 테스트 스코프에 누락이 발생하지 않도록 조율합니다.
2.  **명세 기반 테스트**: `Spec.md` 기반으로 테스트 시나리오를 구성하고 코드 동작을 교차 검증합니다.
2.  **자가 정제 및 개선 루프 (Self-Refinement) 가동**: 테스트 실패(Fail) 발생 시, 로그와 오류 맥락을 분석하여 Developer와 Architect에게 구체적인 실패 원인을 제공하고 수정 피드백 루프를 시작합니다.
3.  **품질 리포트 발행**: 상세한 검증 내역을 `Test_Report.md`로 정리합니다.
4.  **최종 완료 선언**: 모든 검증이 통과될 경우 `Blueprint`에 최종 완료를 마킹하여 단계 종료를 알립니다.
5.  **산출물 저장 경로**: 모든 문서형 산출물은 프로젝트 루트의 `docs/` 폴더 내부에 생성 및 관리해야 합니다.
