# AES‑256‑GCM 암호화 스키마(초안)

## 키/IV
- 키: 256-bit, OS 키체인에서 로드
- IV/nonce: 96-bit 랜덤

## 포맷
- header: { schemaVersion, alg: AES-256-GCM }
- body: ciphertext, authTag

## 파일 단위
- `.nota` 컨테이너 전체 또는 주요 JSON 별도 암호화(옵션)
