# 명령 실행 로그 포맷(초안)

## 필드
- id, timestamp, commandId, args(JSON)
- durationMs, result(success|error)
- error(optional): code, message

## 정책
- PII/민감 인자 마스킹
- 로컬 보존, 레벨별 샘플링(성능)
