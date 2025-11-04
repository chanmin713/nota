# Implementation Plan: Nota

## Context
- 입력 요약: 데스크톱 앱, 로컬‑퍼스트(Ollama), 온톨로지(문서/블록/개념/관계, 초기 약함), 리치텍스트+Markdown, 사이드바 AI, /·@·Tab 생산성, 개인/소규모 팀 대상.
- 레퍼런스 스펙: ./spec.md
- 헌법: ../../.specify/memory/constitution.md

## Technical Context
- 앱 형태: 데스크톱(1차 Windows만)
- 저장소: SQLite 단일 파일(백업/이동 용이), 동기화는 후속
- AI 추론: Ollama 로컬 모델 우선. 네트워크 호출 시 동의/로그 요구
- 온톨로지: 약한 온톨로지(태그/링크/속성) → 점진 강화(스키마/규칙/추론)
- 편집기: 위지윅 기본, Markdown 호환 + Docx 내보내기(가져오기는 후속)
- 생산성: `/` 명령 팔레트, `@` 언급, Tab 기반 fill(자동완성/확장)
- 대상: 개인/소규모 팀, 온보딩 마찰 최소화
 - 에디터 엔진: Docs‑feel UI + 숨은 블록 엔진(블록/인라인 노드 그래프)
 - 탭 인필(FIM): Word/Sentence/Paragraph 단위, 수락 제스처(Tab/→/클릭/타이핑)
 - RAG Q&A: 표/불릿 출력, 출처 앵커, 무근거 시 흐림/낮은 확신도 표시
 - 버전관리: 깃 유사 버전/차이/롤백(로컬 저장 우선)
  - 커스텀 문서 포맷: `.nota` zip‑based 컨테이너(콘텐츠/온톨로지/PROV/메타/옵션 캐시 포함), schemaVersion로 호환성 관리
 - Docs‑feel: 자유 타이핑/붙여넣기 보존, 마크다운 자동 인식(#, -, 1.)
  - 한국어 토크나이징: 유니그램+사용자 사전(기본), mecab-ko-lite(wasm) 보정(옵션, 플래그)
  - 재랭커: 문장 단위 스코어, 형태소 특징은 가중치로만 반영
  - 라우팅 템플릿: WRITE/STRUCTURE/SUMMARIZE/TABLE/TIMELINE/TRANSLATE
 - 블록 기본: Paragraph=Block, 블록 속성(ID/IRI/앵커/버전/댓글/패치)
 - FIM 기본 그레인: Sentence(설정으로 변경 가능)
 - 패치 카드(diff): 구조/순서 변경을 우측 카드로 미리보기 후 병합

## Constitution Check (Gates)
- 로컬‑퍼스트 AI: 기본 로컬 모델, 원격 호출 동의/로그 표시 → PASS(설계 반영)
- 온톨로지‑네이티브: 문서/블록/개념/관계를 일급화(초기 약함) → PASS
- 리치텍스트×Markdown: 위지윅 기본, Markdown 호환 → PASS
- 에디터 생산성: Tab fill, /·@ 워크플로우, 사이드바 AI → PASS
- 프라이버시/투명성: 데이터 경로/호출 로그/권한 다이얼로그 → PASS(구현 체크 필요)
- 증분적 전달: MVP → 확장, 기능 토글 → PASS(플래그 설계 포함)
 - AI‑네이티브: Model‑in‑the‑loop, Evidence‑first, Feedback 재랭킹, Task Routing → ADD
 - 온톨로지 품질: 내부 IRI, PROV 저장, SHACL‑lite 경고 → ADD
 - Local‑first + 선택적 동기화 스위치 → ADD

## Milestones & Phases

### Phase 0: Outline & Research (M0)
- 산출물: ./research.md
- 작업
  - 결정 사항 문서화(저장소=SQLite, 포맷=Markdown+Docx 내보내기, OS=Windows만)
  - 대안 및 근거 기록
- 완료 기준
  - 연구 문서에 의사결정/근거/대안 기록
  - 스펙/계획 반영 동기화 완료

### Phase 1: Design & Contracts (M1)
- SHACL 규칙 카탈로그(라이트/확장) 정의, 위반 메시지/자동수정 정책
- `.nota` 스키마( meta/document/ontology/prov ) JSON Schema 초안
- 색인 설계: 증분 인덱서, 워크스페이스 전역 인덱스, 질의 키 전략
- 파일 변환 설계: PDF/PPTX/DOCX 추출 백엔드(오프라인 우선), 구조화 맵핑 규칙, Evidence 앵커
- 산출물: ./data-model.md, ./contracts/, ./quickstart.md
- 작업
  - 엔티티 정제: Document/Tag/Link/Property 필드/관계/검증 규칙
  - 명령/언급/사이드바 상호작용 흐름 → 계약 정의(REST/IPC 선택 TBD)
  - Evidence‑first 결과 구조 설계(스니펫/출처/스코어)
  - Feedback 이벤트 스키마(수락/거부/수정)와 재랭킹 신호 정의
  - Task Routing 규칙(질의 유형→모델/프로시저) 명세
  - PROV 메타데이터 스키마와 기록 전략
  - SHACL‑lite 규칙/경고 카탈로그 설계
  - 숨은 블록 엔진 구조(블록/인라인 노드, 식별/범위/트랜잭션) 설계
  - FIM 동작 행렬(단위×제스처×충돌 처리) 명세
  - FIM 파이프라인 설계: prefix/suffix 300–800 tokens, grain별 stop 규칙, 후보 2–3개 생성, 경량 cross‑encoder 재랭킹, 고스트 노출 정책
  - FIM 지연 예산/측정 계획: Word ≤80ms, Sentence ≤150ms, Paragraph 300–800ms(95p)
  - RAG 출력 포맷 템플릿(표/불릿)과 무근거시 처리 정책
  - RAG 파이프라인 설계: 멀티쿼리 확장 → 하이브리드 검색(BM25 ∩ 벡터 ∪ 엔터티) → 상위 50 재랭킹 → 컨텍스트 압축(2–4k) → 생성
  - 재랭킹 기준: 쿼리-문서/쿼리-스니펫 의미 유사도 + 엔터티 정합 점수
  - 압축 정책: 증거 보존 우선, 중복 제거, 엔터티 스팬 유지
  - Evidence 폴백 설계: 임계값 T1/T2 정의, 축약/완곡 전환, 추출형 강등, 일반 모드, 자료 부족 카드/추천, 클라우드 버스트 재시도 조건
  - 표/숫자 질의 전용 추출 파이프라인(생성 금지) 설계 및 라우팅 규칙
  - 한국어 토크나이저 설계: 유니그램+사용자 사전 구조, 사전 업데이트 워크플로우
  - mecab-ko-lite(wasm) 통합 옵션 설계(BM25 토큰 보정 경로, 플래그/폴백)
  - 문장 단위 재랭커와 형태소 가중치 스키마 정의
  - 라우팅 의도 분류 규칙과 템플릿/후처리 스펙(WRITE/…/TRANSLATE)
  - `.nota` 포맷 스펙: 디렉터리 구조, JSON 스키마, schemaVersion 전략, 캐시/민감 데이터 분리 원칙
  - 문서 입출력 계약: 저장/불러오기/무결성(해시) 메타 정의
  - 외부 공유 모드: 캐시 제거/축약 저장 정책
  - 버전 스토어/스냅샷/차이/롤백 흐름 정의(UX 포함)
  - Minimum Ontology Baseline 정의(태그/링크/속성 CRUD, IRI, 필터, SHACL‑lite 경고)
  - Ontology Base Layer 설계: 스키마 레지스트리(버전), 타입/관계 메타, 네임스페이스/IRI 정책, 검증/추론 훅, 마이그레이션 전략, PROV 기록
  - 마크다운 자동 인식 규칙(헤더/리스트/번호 리스트)과 붙여넣기 보존 정책
  - 블록 속성(ID/IRI/앵커/버전/댓글/패치) 스키마
  - 패치 카드(diff) UX 플로우와 병합 규칙
  - Draft 모드 설계: 3질문(목적/톤/독자) 프롬프트, 아웃라인 생성 규칙, 첫 단락 길이/스타일 가이드, 미리보기→적용 흐름
  - 보안 설계: AES‑256‑GCM 암호화 스키마, 키 관리(키체인/마스터 비밀번호), PII 감지/필터링 알고리즘, 인증/인가 플로우, 로그/캐시 보안 정책, 감사 로그 스키마
  - OpenAPI/스키마 초안 작성, 샘플 요청/응답
  - 에이전트 컨텍스트 업데이트(새 기술만 추가)
- 완료 기준
  - 모든 사용자 액션에 대응하는 계약 정의
  - 데이터 모델과 계약 간 일관성 확인

### Phase 1: Design & Contracts (M1)
- MCP 설계: 클라이언트 라이프사이클, 프로바이더 레지스트리 스키마, 권한 매핑(네트워크/파일/모델), 프록시/로그/레이트리밋, PII 마스킹, 실패/재시도 폴백
- 문서 링크 설계: `[[`/`@` 검색 UX, IRI 기반 타깃 연결, 리네임 추적 규칙
- 백링크 인덱스: 저장 시 역참조 업데이트, 스니펫 추출/정렬/필터 스펙
- 링크 미리보기 카드: 제목/요약/최근 수정/태그 템플릿
- 깨진 링크 처리: 삭제/권한 변경 감지, 복구/대체/제거 플로우

### Phase 1: Design & Contracts (M1)
- 협업: CRDT 계층, 제안 모드/댓글·멘션 모델, 알림 정책
- Sync 서버: 프로토콜(gRPC/HTTPS), RBAC/저장소/호스팅, 감사 로그
- 배포/업데이트: 패키징/서명, 차등 업데이트/롤백 전략
- OS 통합: `.nota` MIME/아이콘/연계, 컨텍스트 메뉴
- 라이선스: 서드파티 목록/라이선스 표/대체안, 알림 프로세스
- i18n/l10n: 문자열/로케일 전략, 한국어 분절 공존 규칙
- 로깅/옵저버빌리티: 로그 레벨/보존, 크래시 리포터, 텔레메트리 연계
- 백업/복구: 자동저장/저널링/복구 플로우, 보존 정책
- Undo/Redo: 트랜잭션 경계/병합 규칙

### Phase 1: Design & Contracts (M1)
- 사이드바 리사이즈 설계: 최소/최대 폭, 스냅 포인트(280/320/360), 더블클릭 최소화/복원, 접근성(키보드 리사이즈)
- 상태 저장: 전역 기본값 + 세션 지속(재시작 복원), 단위/반응형 규칙

### Phase 1: Design & Contracts (M1)
- 위협 모델 문서화: 자산/공격경로/완화/잔여위험, 보안 검토 체크리스트
- a11y 설계: WCAG AA 체크리스트, 스크린리더/포커스/명도, 키보드 리사이즈
- 변환 골든셋/지표: PDF/PPTX/DOCX 정확도 지표/샘플셋/측정 방식
- 동기화 충돌: 블록 단위 3‑way merge 정책/패치 카드 UX

### Phase 1: Design & Contracts (M1)
- 테마: JSON 스키마/토큰 매핑 정의, 라이트/다크/고대비 팔레트, 스위처 UX
- 편집기 확장: 표/LaTeX/Mermaid/미디어/인용 모델·저장·내보내기 규칙
- 변환 확장: HTML/Markdown 파서 스펙, 역변환(Markdown/Docx) 매핑 규칙/한계
- 검색 고도화: 쿼리 언어 문법/연산자, 유사도 A/B 설계, 리빌드 전략

### Phase 1: Design & Contracts (M1)
- 테스트 계획: 스토리 수용 테스트 목록/데이터 픽스처, 변환 골든셋 정의, 성능 지표와 임계, 보안/침투 항목, a11y 시나리오, 동기화 충돌 케이스, RAG/FIM 측정법

### Phase 1: Design & Contracts (M1)
- 마이그레이션 설계: Notion/Obsidian/GDocs 입력 형식 분석, 필드 매핑 표, 손실/대체 전략, 검토 패널 UX

### Phase 2: MVP Implementation Plan (M2-ready items 제외)
- 협업: 제안 모드/댓글만(실시간 CRDT=차기)
- MCP: 단일 프로바이더 + 읽기 전용 도구(쓰기/변경=토글)
- RAG: 하이브리드+TopK(≤20)만, MQE/압축/Top50 재랭킹=토글
- Evidence 폴백: T2 단일 규칙만 적용(버스트/카드=차기)
- 온톨로지: IRI + SHACL‑lite(레지스트리/마이그레이션/추론=차기)
- 보안: 저장 암호화+키체인+로그 마스킹(침투 테스트=차기)
- 변환: HTML/Markdown/Docx, Obsidian 우선(Notion=차기)
- 편집기: 고급 표+인라인 LaTeX(Mermaid/인용=차기)
- 테마: 라이트/다크+핵심 토큰(고대비=차기)
- OS: 아이콘+더블클릭(차등 업데이트/컨텍스트 메뉴=차기)

### Phase 2: MVP Implementation Plan (M2)
- SHACL‑lite 실행 + 경고 UX
- `.nota` 스키마 v1 적용(검증 포함)
- 인덱서 MVP: 저장 훅 증분 색인 + 전역 초기 빌드
- 업로드→변환→`.nota` 저장 파이프라인(PDF/PPTX/DOCX), 실패 시 첨부+로그
- 지연 로딩 도입(사이드바/AI/플러그인/Docx/검색 등 비핵심)
- 기능 토글 기본 OFF 설정(원격 호출/Docx/플러그인 일부)
- 성능 테레메트리(로컬 집계)와 주간 리포트(95p) 생성
- 범위: 오프라인 편집 + 사이드바 요약/Q&A(로컬), /·@·Tab 기본, 약한 온톨로지 CRUD
- 기능 토글: 원격 호출(기본 OFF), Docx 내보내기(기본 OFF)
- 작업 묶음
  - Editor Core(WYSIWYG, Markdown 호환 I/O)
  - Command Palette(`/`), Mentions(`@`), Tab Fill
  - Ontology Store(태그/링크/속성), Search/Filter(기본)
  - AI Sidebar(Ollama 연동, 호출 로그/동의, Evidence‑first 패널)
  - Feedback 수집/저장 및 간단 재랭킹 적용(최근 신호 가중치)
  - Task Routing 최소 규칙(요약/질의/편집 3가지)
  - PROV 기록(생성/수정/출처 링크)
  - SHACL‑lite 검증 경고(비파괴적 안내)
  - 블록 엔진 최소 스펙(블록 식별/범위/기본 트랜잭션)
  - FIM 최소 스펙(Word/Sentence/Paragraph 제안과 제스처 수락)
  - FIM 파이프라인 MVP(2–3 후보 생성 → 경량 재랭킹 → 고스트 1개 노출)
  - FIM 지연 모니터링(타임스탬프 로깅, 95p 보고)
  - RAG 표/불릿 템플릿과 출처 앵커 렌더링
  - RAG 파이프라인 MVP: 멀티쿼리(2–4 변형) → 하이브리드 검색 → Top50 재랭킹 → 2–4k 압축 → 생성
  - Evidence-first 렌더링: 문장/항목 단위 앵커 없으면 흐림/축약 규칙 적용
  - Evidence 폴백 MVP: T1/T2 임계 적용, "근거 더 찾기" 재검색 트리거, 자료 부족 카드+추천, 클라우드 버스트 옵션(토글)
  - 표/숫자 질의 라우팅: 추출 전용 파이프라인 실행 및 생성 경로 차단
  - 한국어 토크나이저: 유니그램+사용자 사전 MVP, 사전 편집 UI/파일 기반 관리
  - mecab-ko-lite(wasm) 옵션: 기능 토글로 실험적 보정 경로 제공
  - 재랭커: 문장 단위 스코어 산출 및 형태소 가중치 적용
  - 라우팅 템플릿: WRITE/STRUCTURE/SUMMARIZE/TABLE/TIMELINE/TRANSLATE 기본 세트 적용
  - `.nota` MVP: 저장/불러오기, schemaVersion=1 초안, 캐시 제외 공유 내보내기 옵션
  - Markdown/Docx 내보내기와의 동기화: 제목/헤더/본문/링크/태그 최소 매핑
  - 문서 버전 스냅샷/히스토리/단일 클릭 롤백 MVP
  - Minimum Ontology Baseline 구현(태그/링크/속성 CRUD + 필터 + 경고)
  - Ontology Base Layer MVP: 로컬 JSON 스키마 레지스트리, 네임스페이스/IRI 발급, SHACL‑lite 검증 실행, 간단 추론 훅(메타데이터 추천), 스키마 변경 미리보기→적용(롤백 포함)
  - 보안 MVP: `.nota` AES‑256‑GCM 기본 암호화, 키체인 통합, PII 감지(이메일/전화) 및 AI 호출 전 필터링, 로컬 앱 잠금(비밀번호), 로그 마스킹, 감사 로그(인증 실패/암호화 실패)
  - 마크다운 자동 인식 및 붙여넣기 보존 초기 구현
  - 패치 카드(diff) 우측 패널 미리보기 및 병합 동작 MVP
  - Draft 모드 MVP: 새 문서 생성 시 3질문 위저드 → 5초 내 아웃라인+첫 단락 생성/미리보기/적용
  - Privacy/Transparency UI(경로 배지, 권한 다이얼로그, 로그 뷰)
- 링크 생성 UI(`[[`/`@`) 및 새 문서 만들기 빠른 액션
- 백링크 인덱싱(저장 훅)과 참조 패널
- 링크 미리보기 카드(호버/패널)
- 깨진 링크 감지/표시 및 해결 액션

### Phase 2: MVP Implementation Plan (M2)
- 협업 MVP: 커서/선택 표시 + 블록 CRDT 기초, 제안 모드 최소, 댓글 베타
- Sync MVP: 증분 동기화(블록 패치) + RBAC 기본, 감사 로그 저장
- 배포/업데이트 MVP: 코드 서명 설치/차등 업데이트/자동 롤백
- OS 통합 MVP: `.nota` 연계/아이콘/더블클릭/드래그&드롭
- i18n MVP: 문자열 리소스/한영 전환, 날짜/숫자 로케일
- 로깅/옵저버빌리티 MVP: 로그 레벨/마스킹, 크래시 리포트, 성능 리포트 연계
- 백업/복구 MVP: 자동저장/저널링/크래시 복구
- Undo/Redo MVP: 블록/메타/온톨로지 경계·병합 동작 적용

### Phase 2: MVP Implementation Plan (M2)
- 마이그레이션 MVP: Notion(HTML/MD), Obsidian(MD), Google Docs(Docx) → `.nota` 변환, 결과 리포트/검토 패널

## Risks & Mitigations
- 로컬 모델 설치 난이도 → 설치 가이드/자동 감지/재시도 제공
- 온톨로지 복잡도 → 점진 활성화, 기본 뷰 단순화, 도움말 온디맨드
- 성능(대용량 문서) → 지연 로딩, 가상화, 인덱싱(후속)

## Success Criteria Mapping
- 오프라인 AI 요약 95% 10초 내 → 사이드바 비동기/진행 표시/재시도
- 키보드 중심 입력(헤더/리스트/테이블) → 단축키/리본 포커스 지원
- 태그/속성 기반 검색 만족도 ≥4/5 → 단순 필터 + 미리보기

## Dependencies
- Ollama(로컬), 로컬 저장소(SQLite 또는 파일 시스템)

## Next Commands
 - 결정 사항 반영 완료(저장소/포맷/OS). 다음 단계로 `/speckit.implement` 또는 설계 심화를 원하면 `/speckit.checklist` 실행


