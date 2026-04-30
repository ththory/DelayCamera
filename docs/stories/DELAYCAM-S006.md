---
artifact_type: story
story_id: DELAYCAM-S006
epic: DELAYCAM-EPIC-2
title: HW 디코더 + Delayed SurfaceTexture
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-2, FR-3]
related_nfr: [NFR-2]
related_adrs: [ADR-002]
constraints:
  - 디코더 출력 = OpenGL ES SurfaceTexture (zero-copy)
  - 슬라이더 변경 시 재시킹 가능
explicit_ambiguities: []
---

# DELAYCAM-S006 — HW 디코더 + Delayed SurfaceTexture

## User Story
**As a** 시스템, **I want** BufferSkill에서 NAL을 가져와 HW 디코더로 텍스처를 생성한다, **so that** RenderSkill이 지연 영상을 GPU에 표시할 수 있다.

## Acceptance Criteria
- [ ] **AC-1** Given BufferSkill이 NAL 시퀀스를 발행, When 디코더에 입력, Then `Surface` 출력으로 SurfaceTexture가 매 frame 업데이트.
- [ ] **AC-2** Given 인코드+디코드 RTT 측정 instrumentation, When 측정, Then ≤33ms (NFR-2, 1프레임 @ 30fps).
- [ ] **AC-3** Given 슬라이더 5초→3초 변경, When `reseek(3s)`, Then 디코더가 flush + 가장 가까운 키프레임에서 재개되며 ≤500ms 내 새 영상 출력.
- [ ] **AC-4** Given 코덱 매칭, When 인코더가 HEVC면 디코더도 HEVC, H.264면 H.264, Then 코덱 일관성 유지.
- [ ] **AC-5** Given 디코더 에러 (`MediaCodec.CodecException`), When 발생, Then 자동 재시작 + 사용자 토스트 ("일시 중단 — 자동 복구 중").

## Implementation Steps
1. [ ] `DecodeSkill` 인터페이스 정의
2. [ ] `MediaCodec.createDecoderByType("video/hevc")` + 출력 SurfaceTexture
3. [ ] BufferSkill `queryAtDelay()` → 디코더 입력 큐
4. [ ] reseek() — flush + 키프레임 시점부터 재공급
5. [ ] 에러 복구 핸들러
6. [ ] RTT 측정 instrumentation

## References
- PRD: §3 FR-2·3, §4 NFR-2
- ADR-002
