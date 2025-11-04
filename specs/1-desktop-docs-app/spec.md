# Feature Spec: Nota

## Short Name
- nota

## Summary
- 문서 중심 데스크톱 앱. 오른쪽 사이드바 AI 에이전트로 문서 Q&A/요약/편집, @ 언급으로 문서/개체 태깅, / 명령으로 AI 질의/액션을 제공. 위지윅 리치텍스트 경험을 기본으로 하며 Markdown 호환을 유지하고, 구글 독스처럼 헤더/리본 UI로 마크다운을 몰라도 구조화 입력 가능. 에디터는 탭(Tab) 기반 fill(자동완성/확장)을 제공하며, AI는 Ollama 로컬 모델 우선 접근. 온톨로지 네이티브: 초기 약한 온톨로지(태그/링크/속성)에서 시작해 점진 강화. 목표는 개인/소규모 팀이 노션/옵시디언/구글독스를 대체하는 것.

## Goals
- 사용자가 문서 작성/편집을 중단 없이 빠르게 수행
- 로컬‑퍼스트 AI: 인터넷 없이도 Q&A/요약/편집 보조 수행 가능
- 온톨로지‑네이티브: 태그/링크/속성 기반 약한 온톨로지로 시작, 점진 강화 전제
- 리치텍스트 기본 + Markdown 호환 보장, 헤더/리본 UI 제공
- / 명령(액션/질의), @ 언급(문서/개체 참조)으로 키보드 중심 워크플로우
- Tab 기반 fill(자동완성/확장)으로 작성 생산성 향상

## Non-Goals
- 초기 릴리스에서의 고급 협업 실시간 동시편집
- 복잡한 서버 동기화/권한 모델(초기엔 로컬 저장소 중심)
- 무제한 플러그인 마켓/확장 생태계

## Target Users
- 개인 사용자, 소규모 스타트업 팀(온보딩 마찰 최소화)

## User Scenarios & Testing
1) 작성/편집
   - 사용자는 새 문서를 생성하고 헤더/리본 UI로 제목/헤더/리스트/테이블을 즉시 입력한다.
   - Tab 키로 제안된 자동완성(예: 리스트 확장, 서식 패턴)을 수락한다.
   - 수락 기준: 마우스 없이 H1, 리스트, 테이블을 삽입하고 서식을 변경할 수 있다.

2) AI 보조(사이드바)
   - 사용자는 오른쪽 사이드바에서 현재 문서에 대해 Q&A/요약/문체 편집을 요청한다.
   - 네트워크가 없어도 Ollama 로컬 모델로 응답을 받는다.
   - 수락 기준: 네트워크 차단 상태에서 요약이 완료된다(타임아웃 내).

3) / 명령 & @ 언급
   - 사용자가 `/요약`, `/아웃라인` 등 명령으로 액션을 실행하고, `@문서명`으로 다른 문서/개체를 참조한다.
   - 수락 기준: 키보드만으로 명령 팔레트 호출, 최근 항목 우선 제안, 실행 로그 기록.

4) 온톨로지(약한)
   - 사용자가 문서에 태그/링크/속성을 부여하면, 검색/추천에서 활용된다.
   - 수락 기준: 태그/링크/속성으로 필터/검색이 동작하고, AI가 메타데이터를 인식해 답변 품질이 개선된다.

## Functional Requirements
- 편집기
  - 위지윅 리치텍스트 기본, Markdown 내보내기 및 붙여넣기 호환
  - Docx 내보내기 지원(가져오기는 후속 범위)
  - 헤더/리스트/테이블/코드블록/인라인 서식의 구조화 입력 UI(리본/툴바)
  - Tab 기반 fill(자동완성/확장) 활성화 토글 및 수락/거부 제스처
  - Docs‑feel 편집 경험을 제공하되, 내부적으로는 숨은 블록 엔진(블록/인라인 노드 그래프)으로 동작
  - 탭 인필(FIM) 단위 선택: Word / Sentence / Paragraph, 수락 제스처: Tab / → / 클릭 / 타이핑
  - 자유 타이핑/붙여넣기 보존, 마크다운 자동 인식(#, -, 1.)으로 즉시 구조화
  - 블록 엔진 기본 단위: Paragraph=Block, 블록은 ID/IRI/앵커/버전/댓글/패치 단위를 가진다
  - FIM 기본 길이 그레인: Sentence (사용자 설정으로 변경 가능)
  - 패치 카드(diff): 구조·순서 변경은 우측 카드에 나타나며, 사용자가 적용 시 병합된다
  - FIM 파이프라인: 입력은 prefix/suffix 300–800 tokens, 길이 그레인(Word/Sentence/Paragraph)별 stop 규칙 적용, 후보 2–3개 생성 → 경량 cross‑encoder 재랭킹 → 고스트 제안 1개 노출

- 명령/언급
  - `/` 명령 팔레트: 검색, 요약, 아웃라인, 리라이팅, 포맷 적용 등 기본 액션 제공
  - `@` 언급: 문서/개체 자동완성, 선택 시 링크/태그/속성 연결

- AI 사이드바
  - 문맥 인식 Q&A/요약/편집, 결과 적용 전 미리보기/다시쓰기
  - 로컬‑퍼스트: Ollama 모델 우선, 원격 호출 시 명시적 동의 + 호출 로그 기록
  - Model‑in‑the‑loop 기본: 작성/편집/검색에 상시 개입
  - Evidence‑first UX: 인용/스니펫/메타데이터 근거 우선 표시
  - RAG 파이프라인: 멀티쿼리 확장 → 하이브리드 검색(BM25 ∩ 벡터 ∪ 엔터티) → 상위 50 재랭킹 → 컨텍스트 압축(2–4k 토큰) → 생성
  - RAG Q&A 출력 포맷: 표/불릿 중심 정리 + 출처 앵커 제공(각 항목/문장에 앵커가 없으면 흐리게 또는 축약 표시)
  - Evidence 폴백 규칙:
    - Anchor Confidence < T1: 답변을 축약/완곡 표현으로 전환 + "근거 더 찾기" 버튼 노출(재검색)
    - T1 ≤ Confidence < T2: 추출형(원문 문장 인용)으로 자동 강등, 요약 금지
    - Confidence ≥ T2: 일반 답변, 모든 불릿에 앵커 필수
    - 증거 없음(N/A): "자료 부족" 카드로 대체(관련 문서 3개 추천). 클라우드 고품질 모드가 켜져 있으면 재시도(버스트) 옵션 제공
    - 표/숫자 질의: 생성 경로 금지, 추출 전용 파이프라인 사용
  - 한국어 토크나이저/형태소:
    - 기본: 유니그램 토크나이저(ko) + 사용자 사전(고유명사/프로젝트명)
    - 가능 시: mecab-ko-lite(wasm)로 형태소 보정 → BM25 품질 상승(옵션)
    - 재랭커: 문장 단위 점수, 형태소 정보는 가중치로만 사용(특징 보강)

### 5.4 Task Routing 템플릿
- WRITE / STRUCTURE / SUMMARIZE / TABLE / TIMELINE / TRANSLATE 전용 프롬프트·템플릿 제공
- 라우팅 규칙: 질의 의도 분류 → 전용 템플릿/후처리 경로 매핑
  - Feedback→재랭킹 루프: 사용자의 수락/거부/수정 신호를 반영해 제안 품질 향상
  - Task Routing: 질의 유형에 따라 적합한 모델/프로시저 라우팅
  - Draft 모드: 새 문서 생성 시 3질문(목적/톤/독자) 위저드 → 아웃라인+첫 단락 자동 생성(미리보기 후 적용)

- 온톨로지(약한)
  - 태그/링크/속성 스키마(유연 속성 타입: 텍스트/숫자/날짜)
  - AI가 문서 저장 시 메타데이터 제안(비파괴적, 사용자 승인 필요)
  - 내부 IRI로 엔터티/관계 안정 식별, 사용자 UI에는 비노출
  - 변경/출처 이력의 PROV 메타데이터 기본 저장
  - SHACL‑lite 수준의 스키마 검증 경고 제공

- 프라이버시/투명성
  - 데이터 경로 배지(로컬/원격), AI 호출 로그, 권한 요청 다이얼로그
  - PII 기본 로컬 격리, 원격 동기화는 옵트인
  - Local‑first + 선택적 동기화: 개인은 100% 로컬, 공유 필요 시에만 Sync/Share 스위치 활성화
  - 문서 버전 관리: 깃 유사 버전 이력/차이 보기/쉬운 롤백 제공(로컬 우선 저장)

## Security Requirements
- 저장 암호화: `.nota` 파일은 AES‑256‑GCM 암호화(기본 ON, 키체인/마스터 비밀번호로 키 관리)
- 전송 암호화: 원격 동기화 시 TLS 1.3 필수, E2E 암호화 옵션(공유 문서)
- 인증/인가: 로컬 앱 잠금(비밀번호/생체인증/키체인), 세션 타임아웃, 자동 잠금
- 민감 정보 처리: PII 자동 감지(이메일/전화/주민번호), 격리/마스킹 옵션, AI 호출 전 자동 필터링
- 로컬 데이터 보호: 파일 시스템 권한 최소화, 메모리 정리(암호화 키 제로화), 스왑 파일 방지(옵션)
- 원격 동기화 보안: 토큰 기반 인증, 리프레시 토큰, 세션 만료, IP 화이트리스트(옵션)
- AI 호출 보안: PII/민감 키워드 자동 필터링, 클라우드 호출 시 사용자 확인(명시적 동의), 로그에서 민감 정보 제거
- 로그/캐시 보안: 민감 정보 자동 제거(마스킹/해시화), 로그 로테이션/만료, 캐시 암호화(옵션)
- 악성 입력 방어: XSS/인젝션 방지(이스케이프), 파일 업로드 검증, 크기 제한
- 감사 로그: 보안 이벤트(인증 실패/암호화 실패/권한 변경) 기록, 로컬 전용 저장
- 키 관리: 키체인/OS 보안 저장소 사용, 키 순환 정책, 복구 키(비상용)

## Security Success Criteria
- 저장 암호화: 모든 `.nota` 파일이 기본적으로 암호화되어 저장
- PII 감지: 이메일/전화 자동 감지율 95% 이상, 필터링 적용률 100%
- 인증 실패: 3회 실패 시 일시 잠금, 5회 시 영구 잠금(복구 키 필요)
- AI 호출 보안: PII 포함 문서는 클라우드 호출 전 명시적 경고/차단 옵션
- 로그 보안: 민감 정보가 로그에 기록되지 않음을 검증(자동 마스킹)

## Entities & Ontology (Initial)
- Document: id, title, content, createdAt, updatedAt
- Tag: id, name
- Link: id, fromDocumentId, toDocumentId, relation(optional)
- Property: id, documentId, key, type(text|number|date), value

### Minimum Ontology Baseline (MVP)
- 지원 항목
  - Tag: 생성/변경/삭제, 워크스페이스 내 중복 방지(대소문자 무시 비교)
  - Link: 문서→문서 링크(옵션 relation), 자기 자신 링크 금지, 방향성 유지
  - Property: key(문자열) + type(text|number|date), 문서당 기본 20개 제한, 값 기본 검증
  - IRI: 내부 안정 식별자(사용자 UI 비노출)
  - Search/Filter: Tag, Property(key/value 범위) 기준 필터
  - SHACL‑lite: 빈 key, 잘못된 type, self‑loop 링크, 중복 tag 경고

- 수용 기준
  - 태그를 추가/변경/삭제하고, 동일 이름 중복 시 경고가 표시된다.
  - 링크 생성 시 자기 자신 연결은 차단되고, 방향성이 기록된다.
  - 속성에 type 불일치 값 입력 시 즉시 경고가 표시된다.
  - 태그/속성 기준 필터가 적용되어 결과가 즉시 반영된다.

### Ontology Base Layer (for Full Ontology)
- 스키마 레지스트리: 엔터티 타입/관계/속성 정의를 버전으로 관리(초기 로컬 JSON, 이후 마이그레이션 가능)
- 엔터티 타입: `Document`, `Tag`, `Property`, `Link`의 타입 시스템 확장 가능성을 전제(서브타입, 필드 제약)
- 관계 모델: 방향성/다중성/역관계 메타데이터, 향후 규칙·추론에 사용
- 네임스페이스/IRI: 워크스페이스별 네임스페이스 + 안정 IRI 스킴(충돌 방지 정책 포함)
- 버전/마이그레이션: 스키마 변경 시 마이그레이션 단계 정의(미리보기→적용), 롤백 경로 명시
- 검증: SHACL‑lite 규칙을 기본으로 하고, 향후 강한 SHACL로 업그레이드 경로 제공
- 추론 훅: 저장/편집 시 온톨로지 규칙/추천/자동 분류를 삽입할 수 있는 훅 인터페이스(비차단형)
- 프로비넌스: 스키마/데이터 변경의 PROV 기록(누가/언제/무엇을/왜)

## SHACL Rules Catalog (Lite → Strong)
- Lite(기본):
  - property-key-not-empty, property-type-valid(text|number|date)
  - no-self-link(document→document), unique-tag(workspace, case-insensitive)
- Extended(옵션):
  - required-properties(per type), link-multiplicity bounds, inverse-consistency
- Violation UX: 비파괴 경고 배지 + 상세 패널, 자동 수정 제안(가능 시)

## .nota JSON Schemas
- meta.json: { title, createdAt, updatedAt, version, schemaVersion }
- content/document.json: block graph(Paragraph/Heading/List/Table/Code/Inline runs)
- ontology/entities.json: { tags[], links[], properties[] , iri, namespaces }
- prov/history.json: { revisions[], author, reason }
- embeddings/* (optional), rag/cache.json(optional)
- 스키마는 `$schema`로 버전 명시, 역/전방 호환 가이드 포함

## Indexing (Cursor-like)
- 색인 단위: 문서별(제목/헤더/블록/속성/태그/링크), 워크스페이스 전역(엔터티/IRI)
- 색인 시점: 저장 시 증분 업데이트 + 백그라운드 초기 빌드
- 검색 키: 텍스트 토큰, 태그/속성 키·값, 링크 그래프, IRI 별칭
- 임베딩: 로컬 옵션(오프라인), 공유/내보내기 시 제거 가능

## Auto Import & Conversion
- 지원 포맷: PDF, PPT(X), DOCX
- 파이프라인: 업로드 → 추출(텍스트/구조) → 구조화(Heading/List/Table) → 단락/블록 맵핑 → 메타/태그 제안 → `.nota`로 저장
- Evidence: 원본 페이지/슬라이드/위치 앵커 보존(미리보기에서 열람)
- 실패 처리: 불가 자산은 첨부로 보존 + 추출 로그 제공

## Assumptions
- 저장소는 SQLite 단일 파일 사용, 동기화는 후속 범위
- 지원 OS는 1차로 Windows만(추후 macOS/Linux 검토)
- 모델 설치(Ollama)는 사용자가 사전 설치 또는 앱 내 가이드로 해결

## Decisions
1. 지원 OS 1차 범위: Windows만
2. 포맷 우선순위: Markdown + Docx(내보내기)
3. 초기 데이터 저장소: SQLite 단일 파일

## Success Criteria
- 90% 사용자가 마우스 없이 기본 서식 입력 가능(설문/행태 데이터)
- 오프라인 상태에서도 AI 요약 요청의 95%가 10초 내 완료
- 문서 검색/필터에서 태그/속성 기준 결과 만족도 4/5 이상
- / 명령 실행의 평균 시간 1초 이내, 최근 명령 재실행 가능
 - Draft 모드: 3질문 완료 후 5초 내 아웃라인과 첫 단락 미리보기 제공, 사용자의 적용/수정 수락률 70% 이상
 - FIM 지연 목표: Word ≤80ms, Sentence ≤150ms, Paragraph 300–800ms(95th percentile 기준)

## Risks
- 로컬 모델 설치 난이도 → 설치 가이드/자동 감지로 완화
- 온톨로지 복잡도 확장 시 UX 과부하 → 점진 활성화/가드레일

## Dependencies
- Ollama(로컬 모델), 로컬 저장소(SQLite 또는 파일 시스템)

## Document Format
- 확장자: `.nota`
- 컨테이너: zip‑based 컨테이너(폴더 구조) 또는 단일 JSON 패키지(초기 선택은 zip‑based)
- 포함 요소(최소):
  - content/ document.json (리치텍스트/블록 그래프)
  - ontology/ entities.json (Tag/Link/Property, IRI, 네임스페이스)
  - prov/ history.json (버전/변경/작성자/타임스탬프)
  - media/ ... (첨부 자원)
  - embeddings/ (옵션, 로컬 전용 캐시)
  - rag/ cache.json (옵션, 근거 스니펫 캐시)
  - meta.json (문서 메타: title, createdAt, version, schemaVersion)
- 요구 사항:
  - 역호환/전방호환을 위한 schemaVersion 필수
  - 파일 단위 암호화는 후속(키체인/옵션)
  - 외부 공유 시 민감 캐시(embeddings/rag)는 제거 옵션 제공

## Collaboration
- 실시간 동시편집: 문단/블록 단위 CRDT, 커서/선택 표시, 충돌 시 패치 카드
- 제안 모드: 변경 제안/수락/거부, 히스토리 기록
- 댓글/멘션: 인라인/블록 범위, 구독/알림(로컬 우선), 권한 기반 가시성

## Sync Server (Spec Outline)
- 프로토콜: gRPC/HTTPS, 증분 동기화(블록 패치), 압축/서명
- 권한(RBAC): 워크스페이스/폴더/문서/댓글, 역할(Owner/Editor/Viewer)
- 저장소: 버전 스토어 + 메타DB, 감사 로그 보존 정책
- 호스팅: 자가 호스팅/클라우드 선택, 키/비밀 관리 분리

## Deployment & Updates
- 설치: 코드 서명된 설치 패키지(Win 우선)
- 자동 업데이트: 차등(배포 채널 Stable/Insider), 실패 시 롤백
- 무결성: 해시/서명 검증, 교차 검증 로그

## OS Integration
- `.nota` MIME: `application/x-nota+zip` 등록, 아이콘/썸네일
- 파일 연계: 더블클릭 열기, 드래그&드롭 임포트, 컨텍스트 메뉴(변환/요약)

## Licensing & 3rd‑party
- 변환 도구/PDF/PPTX/DOCX, mecab‑ko‑lite(wasm), Mermaid 등 라이선스 표와 대체안
- 서드파티 업데이트/보안 공지 추적 정책

## Internationalization (i18n/l10n)
- 문자열 리소스 분리, 날짜/숫자/세기 표기 로케일 규칙, 한국어 분절과 공존
- 문서 언어 자동 감지(옵션) 및 맞춤법/스타일 가이드 훅

## Logging & Observability
- 로컬 진단 로그 레벨(OFF/ERROR/INFO/DEBUG), PII 마스킹, 보존 기간
- 크래시 덤프/이슈 리포터(옵트인), 성능 텔레메트리와 연계

## Backup & Recovery
- 자동저장 주기, 저널링(트랜잭션 로그), 크래시 복구 흐름
- 백업/복원(스냅샷)과 보존 정책

## Undo/Redo Model
- 트랜잭션 경계: 블록/메타/온톨로지 작업 단위
- 병합 규칙: 연속 편집 합치기, 구조 변경 분리, 제안 모드 예외

## Extension Architecture
- Manifest: `manifest.json` (name, version, permissions, entrypoints, commands, sidebar panels, settings schema)
- Permissions: granular(clipboard, file, network, ai-call, ontology, editor, sidebar); runtime 동적 요청 + 사용자 승인 필요
- Sandbox: 플러그인은 독립 프로세스/워커(IPC)로 실행, 리소스/시간 제한, 파일 접근 가드
- Public APIs:
  - Editor API: 블록/선택/삽입/서식, 고스트 제안, 패치 카드 등록
  - Ontology API: Tag/Link/Property CRUD, 질의/필터, IRI 발급(제한적)
  - AI API: 로컬 모델 호출(로깅/동의 정책 준수), RAG 파이프라인 훅
  - Command API: `/` 명령 등록, 키바인딩, 팔레트 노출
  - Sidebar API: 패널 등록, 상태 저장
  - Storage API: 플러그인 스코프 스토리지(암호화/네임스페이스)
- Hooks:
  - Draft, FIM, RAG, Evidence 폴백, 저장 전/후, 온톨로지 검증 후
- Security:
  - 권한 최소화 원칙, 민감 호출 전 경고, 로그 마스킹, 네트워크 기본 차단(옵트인)
- Distribution:
  - 플러그인 번들(zip) + 서명/해시 검증, 로컬 설치 우선 (마켓 없음)

### MCP Integration
- MCP Client: 표준 프로토콜 준수, 다중 서버 등록/해제, 상태 모니터링
- Provider Registry: 서버 엔드포인트/스키마/권한(스코프) 메타를 로컬에 등록
- Permissions: 각 MCP 호출은 네트워크/파일/모델 권한과 매핑, 런타임 승인 필요
- Sandbox Bridge: 플러그인/에이전트에서 MCP 호출은 브릿지를 통해 프록시(로그/레이트리밋/마스킹)
- Security: PII 필터 선행, 클라우드 호출 시 명시적 동의, 감사 로그 기록
- UX 연계: `/` 명령으로 MCP 도구 검색/실행, 사이드바 패널 결과 표시, Evidence 앵커 포함

## Governance Compliance
- 로컬‑퍼스트 AI(기본값), 클라우드 호출 시 동의/로그
- 온톨로지‑네이티브(태그/링크/속성), 점진 강화
- 리치텍스트×Markdown 양면성, 사이드바 AI, Tab fill
 - AI‑네이티브: Model‑in‑the‑loop, Evidence‑first, Feedback 재랭킹, Task Routing 준수
 - 온톨로지 품질: 내부 IRI, PROV 저장, SHACL‑lite 경고
 - Local‑first + 선택적 동기화 스위치

### 문서 간 링크(Inter-Doc Linking)
- 생성: `[[` 또는 `@`로 문서 검색→선택 시 링크 삽입(제목/별칭 표시), 드롭다운에서 새 문서 만들기 지원
- 안정성: 링크 타깃은 내부 IRI 기준으로 연결(리네임/이동에도 끊기지 않음)
- 백링크: 대상 문서의 "참조" 패널에 자동 집계(문맥 스니펫 포함), 정렬/필터 제공
- 미리보기: 링크 위에 호버 시 카드(제목/요약/최근 수정/태그) 노출, 클릭 시 패널 열기
- 깨진 링크: 삭제/권한 변경으로 접근 불가 시 경고 표시 및 해결 액션(복구/대체/제거)
- 이동/리네임: 표시 텍스트는 최신 제목으로 자동 동기화(수동 오버라이드 가능)

## Doc IDE Principles (Lightweight)
- 목적: 코드를 위한 IDE가 아니라 문서를 위한 IDE 감각 제공(패널/탭/팔레트/상태바), 과도한 무게감 없이 즉응성 강조
- 핵심 UX: 사이드바 패널(Outline/AI/Backlinks), 명령 팔레트(`/`), 단축키, 탭/최근 문서 전환, 패치 카드 패널
- 경량성: 모듈 지연 로딩, 기능 토글로 비핵심 꺼짐, 오프라인 우선·캐시 절제(필요 시에만 생성)

## Performance Budgets
- 시작 시간: 콜드 스타트 ≤ 1.5s(SSD 기준), 워밍 스타트 ≤ 500ms
- 메모리: 아이들 상태 ≤ 250MB, 대형 문서(100k 토큰) 편집 ≤ 600MB
- 응답성: 키 입력→렌더 ≤ 16ms(평균), 패널 전환 ≤ 150ms, 검색 ≤ 300ms
- 번들 크기: 기본 앱 ≤ 25MB, 선택 기능은 지연 로딩
- 측정: metrics.md에 주간 리포트, 95p 기준 준수

### 사이드바 리사이즈
- 좌/우 사이드바는 경계를 드래그하여 크기 조절 가능
- 제약: 최소 240px, 최대 편집 영역 폭의 40%까지
- 스냅: 280/320/360px 스냅 포인트, 더블클릭 시 최소화/복원
- 상태: 문서별이 아닌 앱 전역 기본값 + 세션 지속(재시작 시 마지막 값 복원)

## Accessibility (a11y)
- 스크린리더: 모든 패널/버튼/커맨드 라벨/상태를 ARIA로 노출
- 키보드 내비게이션: 탭 순서 정의, 사이드바 리사이즈 키보드 조작(Alt+[ / Alt+])
- 명도 대비: WCAG AA 이상, 하이라이트/고스트 제안 대비 기준 준수
- 에러/경고: 스크린리더 알림(aria-live)

## Sync Conflict Resolution
- 충돌 단위: 블록(Paragraph/Heading/List item)
- 알고리즘: 블록 단위 3‑way merge + 패치 카드로 충돌 미리보기/선택 병합
- 링크/IRI: 타깃 IRI 우선, 표시 텍스트는 최신 제목으로 동기화
- 보안/권한: 접근 불가 블록은 비공개 플레이스홀더로 마스킹

## Threat Model (요약)
- 자산: 문서(.nota), 온톨로지 메타데이터, 키/토큰, 로그
- 공격 경로: 플러그인 권한 오남용, 링크 통한 권한 상승, 원격 동기화 탈취, 임베딩/캐시 유출
- 완화: 최소 권한, 서명/해시 검증, PII 필터, E2E 옵션, 감사 로그, 네트워크 옵트인

## Acceptance Criteria (정량)
- 플러그인: 무권한 호출 차단율 100%, 서명 검증 실패 차단율 100%
- MCP: 권한 프롬프트 응답 없을 때 기본 거부, 호출 로깅 100%
- 변환: PDF/PPTX/DOCX→.nota 텍스트 추출 정확도 ≥ 95% (골든셋), 문서 50p 기준 변환 시간 ≤ 20s(95p)
- 보안: 로그 민감정보 마스킹 적중률 100%, 암호화 실패 시 쓰기 차단 100%

## Theme System (VS Code‑like)
- 테마 포맷: JSON(이름, 팔레트, 토큰 매핑: heading/list/table/code/inline/ghost/patch)
- 적용: 전역/문서별(옵션), 라이트/다크, 고대비 테마 지원
- 스위처: 명령 팔레트/설정 패널에서 전환, 플러그인 테마 등록 허용(권한 제한)

## Editor Extensions
- 표 고급: 병합/분할/정렬/머리글 고정, CSV 붙여넣기 매핑
- LaTeX 수식: 인라인/블록 렌더링, 수식 번호/참조
- 다이어그램: Mermaid 지원(옵션), 이미지로 내보내기
- 미디어 임베드: 이미지/오디오/비디오 기본 컨트롤, 캡션/대체텍스트(a11y)
- 인용/참고문헌: 인용 키/서지 항목, 서지 형식(APA/MLA 등) 선택

## Conversion Quality & Range
- 추가 가져오기: HTML/Markdown 고정밀 파서(헤더/리스트/테이블/링크/각주)
- 역변환: `.nota` → Markdown/Docx 품질 기준 정의(헤더/목차/표/링크 보존)
- 품질 지표: 구조 보존율 ≥ 98%, 링크/태그 매핑 정확도 ≥ 97% (골든셋)

## Advanced Search & Indexing
- 쿼리 언어: (tag:Design AND prop:priority>=2) NOT link:archived
- 유사도 튜닝: BM25 파라미터/BERT 임베딩 가중 A/B 실험 슬롯
- 리빌드 전략: 대규모 워크스페이스 백그라운드 세그먼트 리빌드, 스로틀링/진행 UI

## Testing Strategy
- 수용 테스트: 사용자 스토리별 시나리오/성공 기준 자동화(명령 팔레트/에디터/사이드바/링크)
- 변환 골든셋: PDF/PPTX/DOCX/HTML/Markdown → .nota 구조/링크/태그 보존율 측정(≥ 목표)
- 성능 회귀: 시작/메모리/입력→렌더/검색/RAG/FIM 지연(95p) 주간 측정
- 보안/침투: 플러그인 샌드박스, 키/로그/PII, 네트워크 옵트인 시나리오 테스트
- a11y: 스크린리더/포커스/명도/키보드 네비 자동/수동 점검
- 동기화 충돌: 블록 3‑way merge 케이스(동시 편집/삭제/이동) 패치 카드 병합
- RAG 평가: MQE/하이브리드/Top50/압축 품질 샘플셋, 앵커 누락 폴백 검증
- FIM: 단위별(stop 규칙) 정확/지연 측정, 고스트 제안 수락률
- 플러그인/MCP: 권한 프롬프트/로그/레이트리밋·거부 동작 100% 검증
- CI: 로컬‑퍼스트 테스트 러너, 스냅샷/골든 파일 관리

## Migration (Notion/Obsidian/Google Docs → Nota)
- 입력 소스
  - Notion: Export(HTML/Markdown+media/CSV) 패키지
  - Obsidian: Vault(폴더/Markdown/자산)
  - Google Docs: Docx/PDF/HTML 내보내기
- 매핑 원칙
  - 제목 → meta.title, 헤더/본문 → content.blocks, 태그/프로퍼티 → ontology.tags/properties
  - 링크/백링크 → ontology.links(IRI 매핑), 첨부 → media/
  - 주석/코멘트 → prov/history 또는 comments(협업 도입 시)
- 단계
  1) 가져오기(파일/폴더) → 2) 파서/정규화 → 3) 구조 매핑 → 4) 태그/속성 추출 → 5) 검토 패널 → 6) `.nota` 저장
- 성공 기준
  - 구조 보존율 ≥ 98%, 링크/태그 매핑 정확도 ≥ 97%, 실패 항목은 보고서로 안내
- 제한/예외
  - 고급 위젯/임베드(데이터베이스/스프레드시트)는 스냅샷 또는 첨부로 보존

## MVP Scope & Feature Toggles
- 협업 실시간 CRDT: M2 (MVP는 제안 모드/댓글만)
- MCP 통합: MVP는 단일 프로바이더 + 읽기 전용 도구만(쓰기/변경 도구는 토글)
- RAG: MVP=하이브리드+TopK(≤20), MQE/압축/Top50 재랭킹은 토글(M2)
- Evidence 폴백: MVP=T2 단일 규칙(앵커 없으면 축약 표시), 버스트/카드 옵션은 M2
- 온톨로지 베이스: MVP=IRI+SHACL‑lite, 레지스트리/마이그레이션/추론 훅은 M2
- 보안: MVP=저장 암호화+키체인+로그 마스킹, 침투 테스트는 M2
- 변환/마이그레이션: MVP=Markdown/Docx+Obsidian, Notion은 M2
- 편집기 확장: MVP=고급 표+인라인 LaTeX, Mermaid/인용은 M2
- 테마: MVP=라이트/다크 + 핵심 토큰(heading/code), 고대비는 M2
- OS 통합: MVP=아이콘+더블클릭, 차등 업데이트/컨텍스트 메뉴는 M2


