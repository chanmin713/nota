# Nota 개발 스택(제안)

- 앱 셸
  - Tauri (Rust + WebView2) — 작은 메모리/번들, 권한 화이트리스트, 보안 우수

- 프런트엔드
  - React + TypeScript + Vite
  - 상태: Zustand (가벼운 전역 상태) + TanStack Query(데이터/파일 I/O 캐싱)
  - 스타일: Tailwind CSS(+ Radix UI/Headless UI), 아이콘: Lucide

- 에디터
  - ProseMirror 기반 TipTap (WYSIWYG + Markdown 호환성 용이)
  - Markdown: remark/rehype 파이프라인, gfm 옵션, 커스텀 노드 매핑

- IPC/권한
  - Tauri Commands(화이트리스트) + 이벤트, 파일/네트워크/클립보드 등 권한 최소화

- 파일 포맷(.nota)
  - 컨테이너: zip(tauri-plugin-zip 또는 Rust `zip` 크레이트)
  - 스키마 검증: `jsonschema`(Rust) 또는 JS `ajv`(런타임 검증)

- 보안/암호화
  - AES‑256‑GCM: Rust `aes-gcm`
  - KDF: `argon2`(Argon2id), 난수: `rand`
  - 키 저장: Windows DPAPI/Keychain(tauri-plugin-store + OS 보안 API 래퍼)
  - 로그 마스킹/PII 필터: Rust/JS 공용 유틸

- 로컬 AI/RAG
  - LLM: Ollama HTTP 클라이언트(로컬 우선)
  - 임베딩/검색: BM25(tantivy) + 벡터(옵션: qdrant/llamaindex-lite)
  - 한국어 토크나이저: mecab-ko-lite(wasm, 토글), 사용자 사전

- 검색/인덱싱
  - 텍스트: Tantivy(로컬) 또는 elasticlunr(경량)
  - 링크/온톨로지 인덱스: Rust 측 증분 업데이트 + JS 조회 브릿지

- 플러그인/MCP
  - 권한 스키마(JSON) + 샌드박스 실행(별도 프로세스/권한 제한 IPC)

- 테스트/QA
  - 단위: Vitest + React Testing Library
  - E2E: Playwright(데스크톱 WebView 시나리오)
  - 성능: Web Vitals-lite + 커스텀 TTFP/입력지연 로거(로컬 저장)

- 빌드/배포
  - Tauri Bundler(코드 서명), 차등 업데이트(M2)
  - CI: GitHub Actions(빌드/서명/릴리스 노트)

- i18n/a11y
  - i18next + ICU 메시지, ARIA 패턴(Radix/Headless)

- 규칙/품질
  - ESLint/Prettier(일관 포맷), Commit: Conventional Commits, 릴리스: Changesets(옵션)

- 피처 토글
  - `app/config/feature-flags.json` + 런타임 로더(초기화 시 캐싱)
