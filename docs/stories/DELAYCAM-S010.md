---
artifact_type: story
story_id: DELAYCAM-S010
epic: DELAYCAM-EPIC-4
title: 리플레이 버튼 + 재생 1회 후 자동 복귀
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-7a]
related_nfr: [NFR-2]
related_adrs: [ADR-002, ADR-003]
constraints:
  - 재생 중 라이브 캡처·버퍼링 끊김 없음
  - 재생 길이 = 현재 슬라이더 N초
  - 재생 1회 후 자동으로 DELAYED 모드 복귀
explicit_ambiguities: []
---

# DELAYCAM-S010 — 리플레이 버튼

## User Story
**As a** 사용자, **I want** 리플레이 버튼을 누르면 직전 N초가 처음부터 1회 재생된다, **so that** 방금 한 동작을 즉시 다시 본다.

## Acceptance Criteria
- [ ] **AC-1** Given 슬라이더 = 5초, When 리플레이 버튼 탭, Then RenderSkill이 `REPLAY` 모드로 진입하고 5초 길이 재생 시작.
- [ ] **AC-2** Given REPLAY 진행 중, When BufferSkill 상태, Then 라이브 캡처와 인코딩이 계속됨 (`append()` 호출이 멈추지 않음).
- [ ] **AC-3** Given REPLAY 진행 중, When 점유 PTS 범위, Then `acquireProtection(now-5s, now)` 보호 토큰이 활성화.
- [ ] **AC-4** Given REPLAY 5초 재생 완료, When 종료, Then 자동으로 `setMode(DELAYED)` 호출 + 보호 토큰 release.
- [ ] **AC-5** Given REPLAY 중 사용자가 리플레이 버튼 재탭, When 발생, Then 무시 (디바운스) — 또는 처음부터 재시작 (디자인 결정 — 기본은 무시).
- [ ] **AC-6** Given REPLAY 중 슬라이더 변경, When 발생, Then REPLAY는 원래 길이로 끝까지 재생 (변경은 복귀 후 적용).
- [ ] **AC-7** Given UI, When 리플레이 버튼, Then 진행 중 상태(progress bar 또는 색상 변화) 표시.

## Implementation Steps
1. [ ] `CameraDelayAgent.startReplay()` 구현 — ReplayHandle 발급
2. [ ] BufferSkill `acquireProtection(now-N, now)`
3. [ ] DecodeSkill `playRange(startPts, durationUs)` 단일 시퀀스 모드
4. [ ] RenderSkill `setMode(REPLAY)` + 종료 콜백으로 DELAYED 복귀
5. [ ] Compose `ReplayButton` + 진행 상태
6. [ ] 디바운스 로직

## References
- PRD: §1.3 리플레이 모드, §3 FR-7a
- ADR-002·003
