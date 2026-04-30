---
artifact_type: story
story_id: DELAYCAM-S012
epic: DELAYCAM-EPIC-4
title: 저장 — 리플레이 클립
ticket: DELAYCAM-001
priority: P0
estimation: 1d
status: Ready
date: 2026-04-30
related_fr: [FR-7b]
related_nfr: [NFR-11]
related_adrs: [ADR-004]
constraints:
  - 리플레이 재생 중 "저장" 버튼 → 재생 중인 N초 클립 저장
  - 저장은 비트스트림 remux (재인코딩 0)
explicit_ambiguities: []
---

# DELAYCAM-S012 — 저장 리플레이 클립

## User Story
**As a** 사용자, **I want** 리플레이 재생 중 "저장" 버튼으로 그 클립을 갤러리에 저장한다, **so that** 보고 싶은 장면을 즉시 보존한다.

## Acceptance Criteria
- [ ] **AC-1** Given 리플레이 재생 중 (REPLAY 모드), When "저장" 버튼 탭, Then 현재 ReplayHandle의 PTS 범위가 그대로 MP4로 저장됨.
- [ ] **AC-2** Given 저장 진행, When 리플레이 동시 진행, Then 두 작업 모두 완료될 때까지 BufferSkill 보호 토큰 유지 (이중 보호 카운트).
- [ ] **AC-3** Given 리플레이 종료가 저장보다 먼저 발생, When 발생, Then 보호 토큰이 저장 완료까지 유지된 후 release.
- [ ] **AC-4** Given 저장 후, When MediaStore Uri, Then 갤러리에서 정상 재생 + 길이 = N초 (오차 ±33ms).
- [ ] **AC-5** Given UI, When 저장 진행, Then 토스트 "저장 중…" → "저장 완료" 또는 "저장 실패".

## Implementation Steps
1. [ ] `CameraDelayAgent.saveReplayClip(ReplayHandle)` 라우팅
2. [ ] `StorageSkill.saveRange(handle.startPts, handle.startPts + handle.durationUs)` 호출
3. [ ] 보호 토큰 이중 카운트 처리 (replay 1 + storage 1)
4. [ ] UI: REPLAY 모드일 때 "저장" 버튼이 리플레이 클립 모드로 동작 (라벨 동적 변경 권장)

## References
- PRD: §3 FR-7b, §1.3 리플레이 모드
- ADR-004
