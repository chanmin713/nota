# Specification Quality Checklist: Nota

**Purpose**: Validate specification completeness and quality before proceeding to planning
**Created**: 2025-11-04
**Feature**: ../spec.md

## Content Quality

- [ ] No implementation details (languages, frameworks, APIs)
- [ ] Focused on user value and business needs
- [ ] Written for non-technical stakeholders
- [ ] All mandatory sections completed

## Requirement Completeness

- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable and unambiguous
- [ ] Success criteria are measurable
- [ ] Success criteria are technology-agnostic (no implementation details)
- [ ] All acceptance scenarios are defined
- [ ] Edge cases are identified
- [ ] Scope is clearly bounded
- [ ] Dependencies and assumptions identified

## Feature Readiness

- [ ] All functional requirements have clear acceptance criteria
- [ ] User scenarios cover primary flows
- [ ] Feature meets measurable outcomes defined in Success Criteria
- [ ] No implementation details leak into specification

## Notes

- Items marked incomplete require spec updates before `/speckit.clarify` or `/speckit.plan`



## Project-Specific Checklist (Nota)

- [ ] Evidence thresholds: define T1/T2 initial values and calibration method (spec RAG, plan M2)
- [ ] RAG toggles matrix: default/enablement for MQE, Top50 rerank, compression
- [ ] FIM logging schema: latency/accuracy fields, sampling cadence, retention policy (metrics, T155/T170)
- [ ] Ontology limits policy: default constraints for tags/links/properties and user override rules
- [ ] Key management UX: lost-key recovery/rotation flow, warnings and backup guidance
- [ ] Plugin permission prompts: per-permission copy and default deny/allow behavior with examples
- [ ] Slash command performance logging: start/end timestamps and log points (T018/T150)
- [ ] Offline summary failure handling: retry/timeout/cancel policy
- [ ] Table/numeric queries: enforce extract-only routing rule in templates (T071)
- [ ] Draft wizard prompts: 3 exemplar sets for purpose/tone/audience (T052)
- [ ] Evidence panel: copy for missing-anchor display (blur/shortened) and rules
- [ ] Performance budget → measurement map: log points per budget item (T150)
- [ ] IRI/namespaces: collision avoidance, aliasing, rename tracking rules (T034/T079)
- [ ] SHACL‑lite: minimum violation codes/messages catalog (T036/T121)
- [ ] MVP toggles defaults: docxExport, cloudBurst, mecabLite with rationale (T002/T025/T118)
- [ ] Privacy/PII: pattern coverage and false-positive handling criteria (T091/T092)
- [ ] Audit log: events, fields, retention draft (T095)
- [ ] Backup/restore: journaling interval and snapshot retention table (T169)
- [ ] Large document UX: indexing/virtualization thresholds and user warning copy
