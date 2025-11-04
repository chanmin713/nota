# AI 호출 로그 스키마(초안)

## record
- id, timestamp, userAction(summarize|qna|edit)
- model(local|remote, name, version)
- tokens(prompt, completion)
- durationMs
- context(meta: docId/title/anchors)
- consent(required:boolean, granted:boolean)
- piiMasked:boolean

## 보존/보안
- 로컬 전용 저장, 보존 기간/로테이션
- 민감 필드 마스킹, 식별자 해시 옵션
