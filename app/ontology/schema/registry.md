# 스키마 레지스트리(초안)

## 포맷
- registry.json: { version, types[], relations[], constraints[] }

## 버전
- schemaVersion 증가 시 마이그레이션 단계 정의

## 저장소
- 초기 로컬 JSON 파일, 향후 마이그레이션 가능
