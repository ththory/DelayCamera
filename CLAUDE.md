# Project: Delay Camera

## Stack

Language: Kotlin 2.1+ (Main), Python 3.13 (Logic Mockup / Architecture Testing)

UI Framework: Jetpack Compose (Modern Android UI)

Camera API: CameraX (안정성) + Camera2 Interop (정밀 제어)

Graphics: OpenGL ES 3.0 (GPU 기반 프레임 버퍼링 및 렌더링)

Build System:

Android: Gradle (KTS)

Backend/Logic Test: uv (Python 의존성 및 로직 검증용)

## Architecture

- Layer 1 (L1-User Interface): Jetpack Compose 기반의 직관적인 스포츠 분석 UI (슬라이더를 통한 실시간 딜레이 시간 조절).

Layer 2 (L2-Agent/Service): CameraDelayAgent (UI 신호를 받아 버퍼 큐를 제어하고 출력을 관리하는 가상 에이전트 로직).

Layer 3 (L3-Skill/Function):

FrameBufferSkill: 고성능 순환 큐(Circular Queue) 기반 프레임 저장 기술.

RenderSkill: OpenGL ES를 이용한 저지연 프레임 렌더링.

Layer 4 (L4-Data): 카메라 원천 피드 데이터 및 메모리 내 임시 버퍼 스트림.

Layer 5 (L5-Infra): Android Camera2 API / CameraX 하드웨어 가속 레이어.

- 상세: `../Workspace_AX_Enabling_Tech_AIDD/doc/AIDD_2026_05월_아키텍처.md`

## Conventions

- Code style: ruff (Py) / biome (TS)
- Commit: Conventional Commits
- Branch: feature/<ticket>-<slug>

## AI Usage Rules

1. 300 LOC 초과 diff 금지
2. 신규 의존성: 공급망 체크
3. 비밀·PII 입력 금지
4. 블랙리스트 명령은 2인 확인
5. Spec 없는 기능 구현 금지

## HITL

- L1 (자동): 린트·단위 테스트·PR diff 요약
- L2 (인간): Spec·ADR·PR 최종 승인
- L3 (2인+): 프로덕션 배포·파괴적 작업

## Agents (8종, 한국어 역할명)

- `agent-analyst` 🔍 분석가 — 요구 분석
- `agent-pm` 📋 PM — PRD·Story
- `agent-architect` 🏗️ 아키텍트 — ADR·Design
- `agent-dev` 💻 개발자 — 구현
- `agent-qa` 🧪 QA — 테스트·경계면
- `agent-reviewer` 🔎 리뷰어 — PR 리뷰
- `agent-deployer` 🚀 배포 담당 — Release·Canary
- `agent-janitor` 🧹 청소부 — 주간 감사

## 변경 이력

| 날짜       | 변경      | 대상 | 사유                     |
| ---------- | --------- | ---- | ------------------------ |
| 2026-04-20 | 초기 구성 | 전체 | Agent 8종 검증 환경 구축 |
