# 피드백 이벤트 스키마(초안)

## events
- accept { itemId, context, timestamp }
- reject { itemId, reason(optional), timestamp }
- edit { itemId, before, after, timestamp }

## 신호 적용
- 최근 수락/거부 가중치로 재랭킹 조정
- 사용자별 로컬 학습(요약)
