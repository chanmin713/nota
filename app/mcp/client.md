# MCP 클라이언트 라이프사이클(초안)

- connect → health → retry/backoff → disconnect
- 오류 시 기본 거부, 호출 로깅
