---
artifact_type: story
story_id: DELAYCAM-S008
epic: DELAYCAM-EPIC-3
title: 라이브 모드 + 60fps 출력 검증
ticket: DELAYCAM-001
priority: P0
estimation: 1d
status: Ready
date: 2026-04-30
related_fr: [FR-1, FR-4]
related_nfr: [NFR-1, NFR-3, NFR-9]
related_adrs: [ADR-001, ADR-003]
constraints: []
explicit_ambiguities: []
---

# DELAYCAM-S008 — 라이브 모드 + 60fps 출력 검증

## User Story
**As a** 사용자, **I want** 앱을 켜자마자 끊김 없이 라이브 화면을 본다, **so that** 자세 점검 흐름이 즉시 시작된다.

## Acceptance Criteria
- [ ] **AC-1** Given 콜드 스타트, When 권한 허용 상태, Then 첫 프리뷰 표시 ≤1.0s (NFR-9).
- [ ] **AC-2** Given LIVE 모드 5분 연속, When fps 측정, Then 평균 ≥58fps, dropped frame ≤1% (NFR-3).
- [ ] **AC-3** Given M2P 측정 instrumentation, When LIVE 모드, Then ≤60ms (NFR-1).
- [ ] **AC-4** Given 모드 토글 버튼, When LIVE↔DELAYED 50회 반복, Then 매 전환 ≤16ms, fps 유지.
- [ ] **AC-5** Given 화면 회전, When 발생, Then 검은 화면 ≤200ms.

## Implementation Steps
1. [ ] CaptureSkill SurfaceTexture → RenderSkill LiveTexture 연결
2. [ ] 모드 토글 UI 버튼 (Compose `Switch` 또는 `IconToggleButton`)
3. [ ] M2P 측정용 timestamp marker (capture timestamp ↔ Choreographer frame time)
4. [ ] fps 측정 ConsoleLog + Logcat용 메트릭
5. [ ] 5분 연속 시나리오 instrumentation

## References
- PRD: §3 FR-1·4, §4 NFR-1·3·9
- ADR-001·003
