---
artifact_type: story
story_id: DELAYCAM-S004
epic: DELAYCAM-EPIC-2
title: HW 인코더 + Surface 입력
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-2]
related_nfr: [NFR-5, NFR-7]
related_adrs: [ADR-002]
constraints:
  - HEVC 기본·H.264 폴백
  - GOP 0.5초 (30 frames @ 60fps)
  - 비트레이트 5 Mbps (HEVC) / 8 Mbps (H.264)
explicit_ambiguities: []
---

# DELAYCAM-S004 — HW 인코더 + Surface 입력

## User Story
**As a** 시스템, **I want** SurfaceTexture 프레임을 즉시 HW 인코딩하여 비트스트림으로 변환한다, **so that** 메모리 효율적인 링버퍼를 구성할 수 있다.

## Acceptance Criteria
- [ ] **AC-1** Given S26 Ultra, When 인코더 초기화, Then `MediaCodecInfo`로 `video/hevc` HW 인코더 검출 및 1080p@60fps 지원 확인.
- [ ] **AC-2** Given HEVC 미지원 기기, When 초기화, Then `video/avc` (H.264 Baseline) 으로 자동 폴백.
- [ ] **AC-3** Given 인코더 실행, When 60fps 입력 5분 지속, Then PTS 단조증가, GOP 간격 = 30 ±2 frames.
- [ ] **AC-4** Given 인코더 출력 콜백, When `MediaCodec.Callback.onOutputBufferAvailable`, Then 매 frame `EncodedSample(ptsUs, ByteBuffer 복사본, isKeyFrame, flags)` 발행.
- [ ] **AC-5** Given 5분 인코딩, When CPU sample 측정, Then 메인 스레드 CPU < 5% (HW 가속 확인).
- [ ] **AC-6** Given SPS/PPS NAL, When 첫 키프레임 도달, Then SPS/PPS·VPS가 별도로 캐시되어 저장 시점에 prepend 가능.

## Implementation Steps
1. [ ] `EncodeSkill` 인터페이스 정의
2. [ ] `MediaCodec.createEncoderByType("video/hevc")` + `createInputSurface()` 사용
3. [ ] `MediaFormat`: 1920×1080, 60fps, GOP 0.5s, 5Mbps VBR
4. [ ] `MediaCodecInfo` capability 검사 + H.264 폴백 분기
5. [ ] 출력 콜백에서 NAL+PTS 추출 → `Channel<EncodedSample>` 발행
6. [ ] SPS/PPS/VPS 캐시 (`csd-0`, `csd-1`)
7. [ ] 단위 테스트: codec capability 매트릭스 시뮬레이션

## References
- PRD: §3 FR-2, §4 NFR-5·7
- ADR-002
