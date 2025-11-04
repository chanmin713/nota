# Tasks: Nota

> Generated from plan.md/spec.md. Organized by phases and user stories.
> Phase 1/2는 공통 기반 작업으로 의도적으로 스토리 라벨을 사용하지 않습니다.

## Phase 1 — Setup
- [x] T001 Create project structure directories in app/: app/editor/, app/ai/, app/commands/, app/ontology/, app/privacy/, app/search/
- [x] T002 Create feature flags file for MVP toggles in app/config/feature-flags.json
- [x] T003 Add AI call log storage placeholder in app/privacy/logs/README.md
- [x] T004 Create ontology store skeleton in app/ontology/README.md
- [x] T005 Create README for local model detection flow in app/ai/README.md

## Phase 2 — Foundational
- [x] T006 Define keyboard shortcuts map in app/commands/shortcuts.md
- [x] T007 Create data model doc from entities in specs/1-desktop-docs-app/data-model.md
- [x] T008 Create contracts skeleton for actions in specs/1-desktop-docs-app/contracts/README.md
- [x] T009 Add quickstart usage scenarios in specs/1-desktop-docs-app/quickstart.md

## Phase 3 — [US1] Editor Core (WYSIWYG + Markdown)
- [x] T010 [US1] Define editor toolbar/ribbon spec in app/editor/ui/toolbar.md
- [x] T011 [P] [US1] Define Markdown import/export rules in app/editor/io/markdown-rules.md
- [x] T012 [US1] Define Tab-based fill behaviors and acceptance in app/editor/assist/tab-fill.md
 - [ ] T028 [US1] Define Docx export scope and mapping in app/editor/io/docx-export.md  <!-- Gated by feature flag: docxExport (plan.md M2 기본 OFF) -->
 - [x] T038 [US1] Specify hidden block engine model (blocks/inline graph) in app/editor/engine/blocks.md
 - [x] T039 [US1] Define FIM matrix (Word/Sentence/Paragraph × Tab/→/Click/Typing) in app/editor/assist/fim-matrix.md
 - [x] T055 [US1] Specify FIM inputs (prefix/suffix 300–800t) in app/editor/assist/fim-inputs.md
 - [x] T056 [US1] Define stop rules per grain (Word/Sentence/Paragraph) in app/editor/assist/fim-stop-rules.md
 - [x] T057 [US1] Implement candidate generation (2–3) spec in app/editor/assist/fim-candidates.md
 - [x] T058 [US1] Specify lightweight cross-encoder reranking in app/ai/rerank/cross-encoder.md
 - [x] T059 [US1] Define ghost suggestion UX policy in app/editor/ui/ghost-suggestion.md
 - [x] T060 [US1] Add latency measurement plan (Word≤80ms/Sent≤150ms/Para 300–800ms, 95p) in specs/1-desktop-docs-app/metrics.md
 - [x] T046 [US1] Specify markdown auto-recognition rules (#, -, 1.) in app/editor/io/markdown-auto.md
 - [x] T047 [P] [US1] Define paste preservation policy in app/editor/io/paste-policy.md
 - [x] T048 [US1] Define block attributes (ID/IRI/anchor/version/comments/patch) in app/editor/engine/block-attrs.md
 - [x] T049 [US1] Set default FIM grain to Sentence with settings in app/editor/assist/fim-settings.md
 - [x] T050 [US1] Specify patch card(diff) UX and merge rules in app/editor/ui/patch-card.md

## Phase 4 — [US2] AI Sidebar (Local-first Ollama)
- [x] T013 [US2] Specify sidebar Q&A/요약/편집 flows in app/ai/sidebar/flows.md
- [x] T014 [P] [US2] Specify local model detection & fallback consent flow in app/ai/sidebar/local-first.md
- [x] T015 [US2] Define AI call logging schema in app/privacy/logs/schema.md
 - [x] T031 [US2] Design evidence-first panel layout (snippets/refs/scores) in app/ai/sidebar/evidence.md
 - [x] T032 [US2] Define feedback events (accept/reject/edit) schema in app/ai/sidebar/feedback.md
 - [x] T033 [US2] Specify task routing rules (query→model/procedure) in app/ai/routing/rules.md  <!-- MVP: 읽기 전용 경로 중심 -->
 - [x] T040 [US2] Specify RAG output templates (table/bullets) and anchor rendering in app/ai/sidebar/rag-templates.md  <!-- MVP: Hybrid+TopK only -->
 - [x] T041 [US2] Define no-evidence fallback (blur/low-confidence) behavior in app/ai/sidebar/no-evidence.md
 - [x] T061 [US2] Define multi-query expansion strategies (2–4 variants) in app/ai/rag/mqe.md  <!-- Deferred: M2 toggle -->
 - [x] T062 [US2] Specify hybrid retrieval (BM25 ∩ vector ∪ entity) in app/ai/rag/hybrid-retrieval.md
 - [x] T063 [US2] Define top-50 reranking criteria in app/ai/rag/rerank-top50.md
 - [x] T064 [US2] Specify context compression to 2–4k tokens in app/ai/rag/compression.md  <!-- Deferred: M2 toggle -->
 - [x] T065 [US2] Wire pipeline: MQE → Hybrid → Top50 rerank → Compress → Generate in app/ai/rag/pipeline.md  <!-- MVP: Hybrid+TopK only; others M2 -->
 - [x] T066 [US2] Define confidence thresholds T1/T2 and calibration method in app/ai/rag/confidence.md
 - [x] T072 [US2] Configure ko unigram tokenizer + user dictionary in app/search/ko-tokenizer.md
 - [x] T073 [P] [US2] Integrate mecab-ko-lite(wasm) option and fallback in app/search/mecab-lite.md
 - [x] T074 [US2] Define sentence-level reranker with morph-weight features in app/ai/rerank/sentence-morph.md
 - [x] T075 [US2] Add routing templates (WRITE/STRUCTURE/SUMMARIZE/TABLE/TIMELINE/TRANSLATE) in app/ai/routing/templates.md
 - [x] T067 [US2] Specify fallback behaviors (hedged/shortened, extract-only, normal) in app/ai/sidebar/fallback.md
 - [x] T068 [US2] Design "Find more evidence" action (re-search) in app/ai/sidebar/find-more-evidence.md
 - [x] T069 [US2] Specify "insufficient evidence" card and 3 related docs suggestion in app/ai/sidebar/insufficient.md
 - [x] T070 [US2] Add cloud burst retry option toggle and flow in app/ai/sidebar/cloud-burst.md
 - [x] T071 [US2] Define table/numeric query extractor-only pipeline and routing in app/ai/rag/extractor-numeric-table.md  <!-- keep -->

## Phase 5 — [US3] Commands and Mentions (/ · @ · Tab)
- [x] T016 [US3] Specify command palette commands list in app/commands/palette/commands.md
- [x] T017 [P] [US3] Specify mentions resolution rules in app/commands/mentions/rules.md
- [x] T018 [US3] Define execution log format in app/commands/logs/format.md
- [x] T112 [US3] Implement `[[` link search dropdown and new-doc action in app/editor/ui/link-insert.md
 - [x] T113 [US4] Build backlink index update on save (with snippet) in app/ontology/backlinks.md
- [x] T114 [US3] Link preview card spec (hover/panel) in app/editor/ui/link-preview.md
 - [x] T115 [US4] Broken link detection and resolution flows in app/ontology/broken-links.md
 - [x] T116 [US4] Title sync on rename (display text update, override rules) in app/ontology/title-sync.md

## Phase 6 — [US4] Ontology (Tags/Links/Properties) + Search
- [x] T019 [US4] Define tag/link/property schemas in app/ontology/schemas.md
- [x] T020 [P] [US4] Define search/filter query syntax in app/search/query.md
- [x] T021 [US4] Map AI metadata suggestions UX in app/ontology/ai-suggestions.md
 - [x] T034 [US4] Specify internal IRI scheme and mapping policy in app/ontology/iri.md
 - [x] T035 [US4] Define PROV metadata schema and storage plan in app/ontology/prov.md
 - [x] T036 [US4] Draft SHACL-lite validation rules and warnings in app/ontology/shacl-lite.md
 - [x] T043 [US4] Specify CRUD UX for tags/links/properties in app/ontology/ux/crud.md
 - [x] T044 [US4] Define validation/constraints (unique tags, no self-loop, property limits) in app/ontology/validation.md
 - [x] T045 [US4] Specify filter UI and acceptance criteria in app/search/filter-ui.md
 - [x] T076 [US4] Design schema registry (versioned JSON) in app/ontology/schema/registry.md
 - [x] T077 [US4] Specify entity types/fields/constraints extensibility in app/ontology/schema/types.md
 - [x] T078 [US4] Define relation metadata (directionality/multiplicity/inverse) in app/ontology/schema/relations.md
 - [x] T079 [US4] Specify namespace/IRI policy and collision handling in app/ontology/schema/namespaces.md
 - [x] T080 [US4] Draft schema migration flow (preview→apply→rollback) in app/ontology/schema/migration.md
 - [x] T081 [US4] Define validation engine (SHACL‑lite now, upgrade path) in app/ontology/schema/validation-engine.md
 - [x] T082 [US4] Add inference hooks interface (save/edit events) in app/ontology/schema/inference-hooks.md
 - [x] T083 [US4] Specify PROV recording for schema/data changes in app/ontology/schema/prov.md

## Phase 7 — [US5] Privacy & Transparency Surfaces
## Phase 8 — [US7] Security & Encryption
- [x] T089 [US7] Design AES‑256‑GCM encryption schema for `.nota` in app/security/encryption.md  <!-- keep -->
- [x] T090 [US7] Specify key management (keychain/master password) in app/security/key-management.md
- [x] T091 [US7] Define PII detection patterns (email/phone/SSN) in app/security/pii-detection.md
- [x] T092 [US7] Specify PII filtering before AI calls in app/security/pii-filtering.md
- [x] T093 [US7] Design app lock (password/biometric) and session timeout in app/security/app-lock.md
- [x] T094 [US7] Define log sanitization (masking/hashing) rules in app/security/log-sanitization.md
- [x] T095 [US7] Specify audit log schema (auth failures/encryption failures) in app/security/audit-log.md
- [x] T096 [US7] Add XSS/injection prevention in editor input in app/security/input-sanitization.md
- [x] T097 [US7] Define file upload validation and size limits in app/security/file-upload.md
- [x] T098 [US7] Specify memory cleanup (key zeroization) and swap prevention in app/security/memory-protection.md

## Phase 8 — [US8] Plugin System
- [x] T099 [US8] Define manifest.json schema in app/plugins/manifest-schema.md  <!-- keep; local install only -->
- [x] T100 [US8] Specify permission model and prompts in app/plugins/permissions.md
- [x] T101 [US8] Design sandbox/IPC execution model in app/plugins/sandbox-ipc.md
- [x] T102 [US8] Public APIs spec (Editor/Ontology/AI/Command/Sidebar/Storage) in app/plugins/public-apis.md
- [x] T103 [US8] Define hooks lifecycle (draft/fim/rag/evidence/save/validate) in app/plugins/hooks.md
- [x] T104 [US8] Bundle/signature/hash verification spec in app/plugins/distribution.md
- [x] T105 [US8] MVP loader/validator/permission prompts flow in app/plugins/loader.md
- [x] T106 [US8] Define MCP provider registry schema in app/mcp/registry.md  <!-- MVP: single provider RO -->
- [x] T107 [US8] Specify MCP client lifecycle (connect/health/retry) in app/mcp/client.md
- [x] T108 [US8] Map permissions for MCP tools (network/file/model) in app/mcp/permissions.md
- [x] T109 [US8] Implement proxy/logging/rate-limit design in app/mcp/proxy.md
- [x] T110 [US8] Integrate '/' command search with MCP tools in app/mcp/command-integration.md
- [x] T111 [US8] Sidebar result panel with evidence anchors in app/mcp/sidebar.md

## Phase 9 — [US6] Draft Mode (New Doc Wizard)
- [x] T051 [US6] Specify 3-question wizard (purpose/tone/audience) UI in app/editor/ui/draft-wizard.md
- [x] T052 [P] [US6] Define prompt templates for outline+first-paragraph in app/ai/prompts/draft.md
- [x] T053 [US6] Specify outline generation rules (depth, number of items) in app/editor/assist/draft-outline.md
- [x] T054 [US6] Define insertion behavior (preview→apply, placement rules) in app/editor/assist/draft-insert.md
 - [x] T022 [US5] Design data path badges UI in app/privacy/ui/data-path-badges.md
 - [x] T023 [P] [US5] Design consent dialogs content in app/privacy/ui/consent-dialogs.md
 - [x] T024 [US5] Design AI call log viewer spec in app/privacy/ui/log-viewer.md
 - [x] T037 [US5] Specify Sync/Share switch behavior and states in app/privacy/ui/sync-switch.md
 - [x] T042 [US5] Specify document version history/diff/rollback UX in app/privacy/ui/versioning.md

## Phase 10 — Testing & QA
- [x] T148 Define acceptance test scenarios per user story in qa/acceptance/scenarios.md
- [x] T149 Build conversion golden set and validators in qa/conversion/goldenset.md
- [x] T150 Setup performance benchmarks and 95p reporting in qa/perf/benchmarks.md
- [x] T151 Security/penetration checklist and test matrix in qa/security/pentest.md
- [x] T152 A11y tests (screen reader/focus/contrast/keyboard) in qa/a11y/tests.md
- [x] T153 Sync conflict cases (block 3-way) and patch-card flows in qa/sync/conflicts.md
- [x] T154 RAG evaluation harness and anchor coverage in qa/rag/eval.md
- [x] T155 FIM latency/accuracy harness in qa/fim/tests.md
- [x] T156 Plugin/MCP permission/denial/rate-limit tests in qa/integrations/plugins-mcp.md
- [x] T157 CI pipeline design and reporting in qa/ci/pipeline.md

## Final Phase — Polish & Cross-Cutting
- [x] T025 Define MVP toggle defaults in app/config/feature-flags.json
- [x] T026 Add performance checklist for large docs in app/editor/perf/checklist.md
- [x] T027 Add UX guardrails for ontology complexity in app/ontology/ux/guardrails.md
 - [x] T029 Add success-criteria measurement plan in specs/1-desktop-docs-app/metrics.md
 - [x] T030 Script offline AI summary timing (≤10s) scenario in specs/1-desktop-docs-app/quickstart.md
 - [ ] T084 Define `.nota` container spec and JSON schemas in app/format/spec.md
 - [ ] T085 Implement export (writer) spec: content/ontology/prov/meta in app/format/writer.md
 - [ ] T086 Implement import (reader) spec with validation in app/format/reader.md
 - [ ] T087 Specify schemaVersion strategy and migration notes in app/format/versioning.md
 - [ ] T088 Define share-safe export (strip embeddings/rag caches) in app/format/share-export.md
- [ ] T117 [US1] Define lazy-loading plan (modules/routes) in app/perf/lazy-loading.md
- [ ] T118 [US1] Feature toggle matrix and defaults in app/config/feature-toggles.md
- [ ] T119 [US1] Performance telemetry (local-only) and weekly 95p report in app/perf/telemetry.md
- [ ] T120 [US1] Startup time/memory budgets test plan in app/perf/startup-budgets.md
- [ ] T121 [US4] Draft SHACL rules catalog and messages in app/ontology/shacl/catalog.md
- [ ] T122 [US4] Define JSON Schemas for meta/document/ontology/prov in app/format/schemas.md
- [ ] T123 [US4] Implement SHACL-lite validation flow spec in app/ontology/shacl/engine.md
- [ ] T124 [US4] Indexer design (incremental/global) and query keys in app/search/indexer.md
- [ ] T125 [US4] Link graph indexing and IRI alias search in app/search/link-graph.md
- [ ] T126 [P] [US4] File conversion pipeline (PDF/PPTX/DOCX→.nota) in app/import/pipeline.md
- [ ] T127 [US4] Evidence anchors from sources (page/slide/range) in app/import/evidence.md
- [ ] T128 [US1] Sidebar resize interaction spec (drag handle/snap/dblclick) in app/ui/sidebar/resize.md
- [ ] T129 [US1] Implement constraints (min/max) and keyboard resize bindings in app/ui/sidebar/constraints.md
- [ ] T130 [US1] Persist/restore sidebar widths (global default + session) in app/ui/sidebar/state.md
- [ ] T131 [US1] A11y checklist (WCAG AA) and screen reader labels in app/a11y/checklist.md
- [ ] T132 [US1] Focus order and keyboard navigation spec in app/a11y/keyboard.md
- [ ] T133 [US4] Threat model and security review checklist in app/security/threat-model.md
- [ ] T134 [US4] Golden set & metrics for converters in app/import/goldenset.md
- [ ] T135 [US4] Block-level 3-way merge policy & patch card UX in app/sync/merge-policy.md
- [ ] T136 [US1] Define theme JSON schema and token map in app/theme/schema.md
- [ ] T137 [US1] Theme switcher UX and persistence in app/theme/switcher.md
- [ ] T138 [US1] Advanced table behaviors (merge/sort/header) in app/editor/table/advanced.md
- [ ] T139 [US1] Inline/block LaTeX spec in app/editor/latex/spec.md
- [ ] T140 [US1] Mermaid diagrams (toggle) in app/editor/diagram/mermaid.md  <!-- Deferred: M2 -->
- [ ] T141 [US1] Media embed (image/audio/video) a11y/caption in app/editor/media/embed.md
- [ ] T142 [US1] Citation/bibliography model in app/editor/citation/model.md  <!-- Deferred: M2 -->
- [ ] T143 [US4] HTML/Markdown import mapping rules in app/import/html-md.md
- [ ] T144 [US4] Reverse export `.nota`→Markdown/Docx rules in app/export/reverse.md
- [ ] T145 [US4] Query language grammar/operators in app/search/query-language.md
- [ ] T146 [US4] Similarity A/B experiment design in app/search/ab-testing.md
- [ ] T147 [US4] Index rebuild strategy and progress UI in app/search/rebuild.md

## Phase 11 — Collaboration & Sync
- [ ] T158 [US1] CRDT layer design (block granularity) in app/collab/crdt.md  <!-- Deferred: M2 -->
- [ ] T159 [US1] Suggestion mode and review flow in app/collab/suggest.md
- [ ] T160 [US1] Comments/mentions model and notifications in app/collab/comments.md
- [ ] T161 [US4] Sync protocol (gRPC/HTTPS) and RBAC in app/sync/protocol.md
- [ ] T162 [US4] Incremental patch sync and audit logging in app/sync/incremental.md

## Phase 12 — Distribution & OS Integration
- [ ] T163 [US1] Installer packaging and code signing in app/distribution/installer.md  <!-- keep -->
- [ ] T164 [US1] Differential updates and rollback in app/distribution/updates.md  <!-- Deferred: M2 -->
- [ ] T165 [US1] `.nota` MIME/icon/association and context menu in app/os/integration.md

## Phase 13 — Ops & Policies
- [ ] T166 [US1] Third‑party licenses and alternatives table in legal/licenses.md
- [ ] T167 [US1] i18n/l10n strategy and locale rules in app/i18n/strategy.md
- [ ] T168 [US1] Logging levels/retention and crash reporting in app/ops/logging.md
- [ ] T169 [US1] Backup/recovery(journaling/autosave) policies in app/ops/backup.md
- [ ] T170 [US1] Undo/redo transaction boundaries and merge rules in app/editor/undo.md

## Phase 14 — Migration
- [ ] T171 Notion export mapping (HTML/MD/CSV→.nota) in app/migration/notion.md
- [ ] T172 Obsidian vault mapping (MD/assets→.nota) in app/migration/obsidian.md
- [ ] T173 Google Docs Docx mapping (Docx→.nota) in app/migration/gdocs.md
- [ ] T174 Review panel UX and report format in app/migration/review.md
- [ ] T175 Lossy cases and fallback snapshot rules in app/migration/fallbacks.md

## Dependencies (Story Order)
1) US1 Editor Core → 2) US2 AI Sidebar → 3) US3 Commands/Mentions → 4) US4 Ontology/Search → 5) US5 Privacy/Transparency

## Parallel Opportunities
- [P] 표시된 태스크는 병렬 수행 가능(서로 다른 파일/디렉터리, 독립적 산출물)

## Implementation Strategy
- MVP: US1 + US2 기본 플로우, /·@·Tab 최소 기능, 약한 온톨로지 CRUD, 프라이버시 표면 최소 셋

- [ ] T176 [US1] Skeleton pattern catalog (editor/sidebar/list) in app/ui/skeleton/patterns.md
- [ ] T177 [US1] Implement aria-busy and announce loading in app/ui/skeleton/a11y.md
- [ ] T178 [US1] Measure skeleton time-to-first-paint (≤150ms) in app/perf/skeleton-metrics.md

