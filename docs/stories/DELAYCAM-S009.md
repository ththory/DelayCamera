---
artifact_type: story
story_id: DELAYCAM-S009
epic: DELAYCAM-EPIC-3
title: 연속 지연 재생 + 슬라이더 UI
ticket: DELAYCAM-001
priority: P0
estimation: 3d
status: Ready
date: 2026-04-30
related_fr: [FR-2, FR-3, FR-4]
related_nfr: [NFR-2]
related_adrs: [ADR-002, ADR-003]
constraints:
  - 슬라이더 0~60초 (A-1)
  - 재생 시맨틱: 캡처 t → (t + delay)초 시점에 재생, 라이브와 동일 fps
  - 워밍업 단계는 라이브 fade로 표시
explicit_ambiguities: []
---

# DELAYCAM-S009 — 연속 지연 재생 + 슬라이더 UI

## User Story
**As a** 사용자, **I want** 슬라이더로 N초를 설정하고, 워밍업 후 정확히 N초 지연된 라이브 영상을 보며 운동을 점검한다, **so that** 흐름을 끊지 않고 자세를 확인한다.

## Acceptance Criteria
- [ ] **AC-1** Given 슬라이더 = 15초, When DELAYED 모드 진입, Then 0~15초는 라이브 fade 오버레이로 워밍업 표시.
- [ ] **AC-2** Given 워밍업 종료 후, When 16초 시점, Then 캡처 1초 시점의 frame이 재생됨 (PRD §1.3 시맨틱).
- [ ] **AC-3** Given 정상 재생 중, When 임의 5초 구간 측정, Then 표시 frame의 PTS = (now − 15s) ±33ms (NFR-2).
- [ ] **AC-4** Given 슬라이더 15→5초 변경, When 발생, Then 버퍼 만료 한도 축소 + 디코더 reseek + ≤500ms 내 5초 딜레이로 매끄럽게 전환.
- [ ] **AC-5** Given 슬라이더 0→0.0초, When 즉시, Then DELAYED ≈ LIVE 동작 (디코더 우회 권장).
- [ ] **AC-6** Given UI, When 슬라이더, Then `Slider` Compose 컴포넌트 + 현재 값(예: "15.0s") 표시 + TalkBack 호환.
- [ ] **AC-7** Given 슬라이더 60초 설정, When 60초 + 워밍업 진행, Then 정상 재생 + RSS ≤2.5GB (NFR-5와 통합 검증).

## Implementation Steps
1. [ ] Compose `DelaySliderScreen` — Slider(0f..60f, step=0.5f)
2. [ ] `CameraDelayAgent.setDelaySeconds(Float)` 라우팅
3. [ ] 워밍업 표시: `RenderSkill.setMode(DELAYED)` + overlay alpha animation
4. [ ] BufferSkill 만료 한도 (`maxDelaySetting`) 동기화
5. [ ] DecodeSkill `reseek(newDelay)` 호출
6. [ ] 정확도 측정 instrumentation (PTS 비교)

## References
- PRD: §1.3, §3 FR-2·3·4, §4 NFR-2
- ADR-002·003
