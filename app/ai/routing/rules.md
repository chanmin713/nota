# Task Routing 규칙(초안)

## 의도 분류
- WRITE / STRUCTURE / SUMMARIZE / TABLE / TIMELINE / TRANSLATE

## 매핑
- SUMMARIZE → 요약 템플릿 + 하이브리드 검색
- QnA → 검색 → 생성(표/불릿 출력)
- TABLE/NUMERIC → 생성 금지, 추출 전용 파이프라인

## 후처리
- 앵커 렌더, 부족 시 폴백 규칙 적용
