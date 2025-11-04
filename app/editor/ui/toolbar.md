# 에디터 툴바/리본 스펙(초안)

## 목적
- 위지윅 리치텍스트 기본 UX 제공 + Markdown 호환 유지
- 마우스 없이도 단축키로 동일 기능 접근 가능(키맵과 일관)

## 기본 그룹
- 구조: Paragraph, Heading(H1–H3), List(불릿/번호), Table, Code
- 인라인: Bold, Italic, Code, Link, Highlight
- 삽입: Table(기본 3×3), Divider, Image, Quote
- 편집: Undo/Redo, Find, Replace, Clear Formatting

## 동작 규칙
- Markdown 자동 인식과 충돌 금지(서식 적용 시 자동 인식 우선 순위 재조정)
- 붙여넣기 보존 정책 준수(스타일/구조 우선순위는 paste-policy 참조)
- 블록 엔진 위에서 동작(블록/인라인 노드에 반영)

## 상태/피드백
- 현재 선택 범위 기반 활성/비활성 토글
- 툴팁에 단축키 표시(Ctrl+Alt+1 등)
- 접근성 레이블(스크린리더) 제공

## 최소 항목(US1 범위)
- Heading(H1/H2/H3), List(불릿/번호), Table Insert, Code Block
- Bold/Italic/Inline Code, Link

## 향후(토글)
- 고급 표(병합/분할/정렬/머리글 고정)
- 수식/다이어그램/미디어/인용
