# 블록 엔진 모델(초안)

## 블록/인라인 노드
- Block: Paragraph, Heading, List, Table, CodeBlock
- Inline: TextRun, Emphasis(B/I), InlineCode, Link, Highlight

## 식별/메타
- blockId(IRI), anchors[], version, comments[], patchRefs[]

## 트랜잭션
- 단위: 블록/메타/온톨로지 경계
- 병합 규칙: 연속 편집 합치기, 구조 변경 분리, 제안 모드 예외

## 범위/선택
- 블록 경계 보존, 인라인 오프셋 기반

## 패치 카드 연계
- 구조/순서 변경은 우측 패널 패치 카드로 노출 → 사용자 적용 시 병합
