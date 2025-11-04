# Metrics & Measurement (초안)

## FIM 지연(95p)
- Word ≤ 80ms, Sentence ≤ 150ms, Paragraph 300–800ms
- 로깅 필드: grain, latencyMs, contextSize, accepted(boolean), timestamp

## 사이드바 요약(오프라인)
- 완료 ≤ 10s, 타임아웃/재시도/취소율 기록

## 시작/메모리/전환
- 시작(콜드/웜), 메모리 아이들/대형 문서, 패널 전환 시간

## 성공 기준 측정 매핑
- 오프라인 요약 10s: 사이드바 타이밍 로깅, 타임아웃/재시도율
- 키보드 중심 입력: 단축키 사용률/성공률 이벤트
- 태그/속성 검색 만족도: 필터 응답시간/정확도 설문 연계

