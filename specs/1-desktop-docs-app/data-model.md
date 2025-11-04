# 데이터 모델(초안)

## 엔티티

### Document
- id (string, IRI)
- title (string)
- content (block graph, 별도 파일 참조)
- createdAt (datetime)
- updatedAt (datetime)

### Tag
- id (string)
- name (string, workspace‑unique, case‑insensitive)

### Link
- id (string)
- fromDocumentId (IRI)
- toDocumentId (IRI)
- relation (string, optional)

### Property
- id (string)
- documentId (IRI)
- key (string, non‑empty)
- type (enum: text|number|date)
- value (typed)

## 제약(Shallow SHACL‑lite)
- unique‑tag(workspace, ci)
- no‑self‑link(document→document)
- property‑key‑not‑empty
- property‑type‑valid(text|number|date)
- per‑document properties ≤ 20 (기본값)

## 인덱싱 키
- 텍스트: 제목/헤더/블록 텍스트 토큰
- 온톨로지: 태그/속성(key/value), 링크 그래프, IRI 별칭

## PROV(요약)
- revisions[], author, timestamp, reason
- 생성/수정/출처 링크 기록
