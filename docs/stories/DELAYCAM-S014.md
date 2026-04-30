---
artifact_type: story
story_id: DELAYCAM-S014
epic: DELAYCAM-EPIC-5
title: 발열 모니터링 + 4단계 정책
ticket: DELAYCAM-001
priority: P0
estimation: 3d
status: Ready
date: 2026-04-30
related_fr: [FR-14]
related_nfr: [NFR-6, NFR-7]
related_adrs: [ADR-005]
constraints:
  - PowerManager.OnThermalStatusChangedListener (API 29+)
  - API 26~28은 폴백 (CPU temp 추정 timer)
  - 진행 중 저장은 SEVERE에서도 완료 보장
explicit_ambiguities: []
---

# DELAYCAM-S014 — 발열 모니터링 + 4단계 정책

## User Story
**As a** 사용자, **I want** 기기가 뜨거워지면 안전하게 자동/반자동으로 화질이 조정된다, **so that** 기기 손상이나 갑작스런 종료 없이 운동을 계속한다.

## Acceptance Criteria
- [ ] **AC-1** Given 앱 시작 (API 29+), When `PowerManager.addThermalStatusListener` 등록, Then 시스템 thermal 변경이 리스너로 통지됨.
- [ ] **AC-2** Given thermal=MODERATE, When 진입, Then 상단에 노란 배너 표시 + "예/무시" 액션 노출 (자동 변경 금지).
- [ ] **AC-3** Given 사용자가 "예", When 동의, Then 1080p@30fps + 비트레이트 4Mbps로 reconfigure.
- [ ] **AC-4** Given thermal=SEVERE, When 진입, Then 5초 내 자동으로 720p@30fps + 2Mbps + 딜레이 cap 30초로 다운스케일 + 빨간 배너.
- [ ] **AC-5** Given thermal=CRITICAL, When 진입, Then 캡처·인코더·디코더 즉시 정지 + 모달 안내. 진행 중 저장은 완료까지 보장 (≤5초 grace).
- [ ] **AC-6** Given thermal=NONE/LIGHT 복귀, When cool down, Then 자동 복원 (사용자 옵션 — 기본은 사용자 명시 액션 후 복원).
- [ ] **AC-7** Given API 26~28, When thermal API 미사용, Then `BatteryManager` + 5분 타이머 폴백으로 다운스케일 제안.
- [ ] **AC-8** Given 25℃ 환경 30분 1080p@60fps, When 측정, Then SEVERE 미진입 (NFR-6), 표면 ≤45℃ (NFR-7, 별도 SOP).

## Implementation Steps
1. [ ] `ThermalSkill` 인터페이스 + 상태머신 구현
2. [ ] `PowerManager.addThermalStatusListener` 등록
3. [ ] CameraDelayAgent ↔ ThermalSkill 연결 (`applyThermalPolicy`)
4. [ ] CaptureSkill `reconfigure(resolution, fps)` 동적 변경 지원
5. [ ] EncodeSkill 비트레이트 동적 변경 (`KEY_BITRATE_MODE` runtime change)
6. [ ] BufferSkill `setMaxDelay(seconds)` 동적 cap
7. [ ] Compose `ThermalBanner` (MODERATE 노랑 / SEVERE 빨강 / CRITICAL 모달)
8. [ ] Instrumentation: thermal status 시뮬레이션 (`adb shell cmd thermalservice override-status`)
9. [ ] 30분 환경 시험 시나리오

## References
- PRD: §3 FR-14, §4 NFR-6·7
- ADR-005
