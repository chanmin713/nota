# 계약 스켈레톤(초안)

## Editor Contracts
- InsertBlock(payload)
- ApplyFormat(range, format)
- AcceptGhostSuggestion(kind: Word|Sentence|Paragraph)
- PastePolicy(preserveRules)

## AI Sidebar Contracts
- Summarize(documentContext)
- QnA(query, context)
- EditStyle(instructions, selection)
- CallLog(record)  // 로컬 저장, PII 마스킹

## Command/Mentions
- CommandPalette(search, recent)
- Execute(commandId, args)
- MentionResolve(query) // 문서/엔터티 자동완성

## Ontology
- Tag/Link/Property CRUD
- Validate(shaclRules)
- IRI Issue/Resolve(alias)

## Privacy & Security
- Consent(prompt, scope)
- DataPathBadge(status)
- Encrypt(file) // AES‑256‑GCM
- KeyManagement(get/set/rotate)

> 상세 스키마/예시는 후속 세부 파일에서 확장됩니다.
