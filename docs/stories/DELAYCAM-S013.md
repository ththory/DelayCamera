---
artifact_type: story
story_id: DELAYCAM-S013
epic: DELAYCAM-EPIC-4
title: 저장 — 수동 녹화 시작·종료
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-7]
related_nfr: [NFR-11]
related_adrs: [ADR-004]
constraints:
  - "녹화 시작" 시점부터 "녹화 종료" 시점까지의 NAL을 보호 + 저장
  - 임의 길이 (최대 디스크 용량 한계 또는 30분 cap)
explicit_ambiguities: []
---

# DELAYCAM-S013 — 저장 수동 녹화 시작·종료

## User Story
**As a** 사용자, **I want** 운동 시작 시 "녹화 시작" → 끝날 때 "녹화 종료" 버튼을 누른다, **so that** 임의 길이의 세션을 저장한다.

## Acceptance Criteria
- [ ] **AC-1** Given "녹화 시작" 탭, When 발생, Then `RecordHandle` 발급 + 그 시점부터의 NAL 보호 카운트 시작 + UI에 빨간 점 표시.
- [ ] **AC-2** Given "녹화 종료" 탭, When 발생, Then 시작~종료 PTS 범위가 MP4로 저장 + Uri 반환.
- [ ] **AC-3** Given 녹화 진행 중 30분 초과, When 발생, Then 자동 분할 또는 경고 다이얼로그 (디자인 결정 — 기본 30분 cap + 경고).
- [ ] **AC-4** Given 녹화 중 BufferSkill 메모리 90% 도달, When 발생, Then 보호된 가장 오래된 부분을 디스크 임시파일로 spool (메모리 폭주 방지).
- [ ] **AC-5** Given 녹화 중 앱 백그라운드 진입, When 발생, Then Foreground Service 알림으로 녹화 지속 표시 + 사용자가 노티 탭으로 복귀.
- [ ] **AC-6** Given 녹화 5분, When 측정, Then 라이브 fps drop ≤5%, 발열 ≤MODERATE.

## Implementation Steps
1. [ ] `StorageSkill.startManualRecording(): RecordHandle` + `stopManualRecording(handle): Result<Uri>`
2. [ ] Foreground Service (`FOREGROUND_SERVICE_TYPE_CAMERA`) 가동
3. [ ] BufferSkill 보호 카운트 + 메모리 90% spool 폴백
4. [ ] UI: 녹화 시작/종료 토글, 경과 시간 표시
5. [ ] 30분 cap 경고

## References
- PRD: §3 FR-7
- ADR-004
