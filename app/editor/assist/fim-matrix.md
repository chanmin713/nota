# FIM 동작 행렬(초안)

| Grain \ Gesture | Tab | → | Click | Typing |
|---|---|---|---|---|
| Word | 수락 | 수락 | 수락 | 취소 |
| Sentence | 수락 | 수락 | 수락 | 취소 |
| Paragraph | 수락 | 수락 | 수락 | 취소 |

- 충돌 시 우선순위: 사용자의 입력 > 고스트 제안
- Undo 경계: 수락 시점에 트랜잭션 커밋
