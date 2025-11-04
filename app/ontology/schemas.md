# 태그/링크/속성 스키마(초안)

## Tag
- id: string
- name: string (workspace-unique, ci)

## Link
- id: string
- fromDocumentId: IRI
- toDocumentId: IRI
- relation?: string

## Property
- id: string
- documentId: IRI
- key: string (non-empty)
- type: text|number|date
- value: any (type-safe)
