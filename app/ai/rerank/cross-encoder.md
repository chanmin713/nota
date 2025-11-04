# 경량 Cross‑Encoder 재랭킹(초안)

## 입력
- (query, candidateSentence)

## 출력
- score ∈ [0,1]

## 정책
- 속도 우선, 소형 모델/온디바이스 실행 가정
- 문장 길이 클립, 한국어 토크나이즈 결과 반영(가중치)
