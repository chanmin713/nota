# Research: Nota

## Decisions

### 저장소 방식
- Decision: SQLite 단일 파일
- Rationale: 백업/이동 용이, 일관된 잠금/트랜잭션, 단일 파일 배포 용이
- Alternatives: 파일 시스템(문서 단위 폴더)

### 포맷 우선순위
- Decision: Markdown + Docx(내보내기)
- Rationale: 상호운용성 강화(외부 공유/제출), 구현 난이도는 내보내기 한정으로 관리
- Alternatives: Markdown만

### OS 1차 범위
- Decision: Windows만
- Rationale: 초기 타깃 비중, 배포/코드사이닝/QA 범위 최소화
- Alternatives: Windows+macOS(후속 검토)

## Notes
- 각 결정은 스펙의 [NEEDS CLARIFICATION]를 해소해야 함
- 결정 후 ./spec.md 및 ./plan.md의 관련 섹션 업데이트


