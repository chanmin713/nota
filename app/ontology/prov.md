# PROV 메타데이터 스키마(초안)

## fields
- id, actor, action(create|update|link|tag|property), timestamp
- reason(optional), references(IRI[])

## 저장
- 문서별 `prov/history.json`, 전역 인덱스 옵션
