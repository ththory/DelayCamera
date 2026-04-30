---
artifact_type: story
story_id: DELAYCAM-S003
epic: DELAYCAM-EPIC-1
title: 전·후면 카메라 전환
ticket: DELAYCAM-001
priority: P0
estimation: 1d
status: Ready
date: 2026-04-30
related_fr: [FR-5]
related_nfr: [NFR-10]
related_adrs: [ADR-001]
constraints:
  - 전환 시 버퍼 리셋 (이전 영상 폐기)
explicit_ambiguities: []
---

# DELAYCAM-S003 — 전·후면 카메라 전환

## User Story
**As a** 사용자, **I want** 토글 버튼으로 전·후면 카메라를 즉시 전환한다, **so that** 거치 위치에 맞게 사용한다.

## Acceptance Criteria
- [ ] **AC-1** Given 후면 카메라 활성, When 전환 버튼 탭, Then ≤500ms 내 전면 카메라 라이브가 표시된다.
- [ ] **AC-2** Given 전환 발생, When 직후 BufferSkill 상태 조회, Then 이전 NAL 시퀀스가 모두 폐기되고 새로운 SPS/PPS NAL 발행 대기 상태이다.
- [ ] **AC-3** Given 전면→후면 전환 5회 연속, When 측정, Then 각 ≤500ms, 평균 ≤350ms.
- [ ] **AC-4** Given 전환 직후, When 화면 표시, Then 검은 화면 ≤200ms (overlay fade로 시각적 끊김 최소화).
- [ ] **AC-5** Given 전환 중 사용자가 추가 탭, When 처리, Then 진행 중인 전환이 끝날 때까지 입력 무시 (idempotent).

## Implementation Steps
1. [ ] `CaptureSkill.switchFacing()` 구현
2. [ ] CameraX `bindToLifecycle` 재바인딩
3. [ ] `BufferSkill.clear()` + 새 코덱 instance reset
4. [ ] UI 토글 버튼 + 진행 중 disabled 상태
5. [ ] 전환 시간 측정 instrumentation

## References
- PRD: §3 FR-5, §4 NFR-10
- ADR-001
