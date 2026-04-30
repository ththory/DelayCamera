---
artifact_type: story
story_id: DELAYCAM-S002
epic: DELAYCAM-EPIC-1
title: 1080p@60fps 카메라 캡처
ticket: DELAYCAM-001
priority: P0
estimation: 3d
status: Ready
date: 2026-04-30
related_fr: [FR-1, FR-6]
related_nfr: [NFR-1, NFR-3, NFR-9]
related_adrs: [ADR-001]
constraints:
  - SurfaceTexture 1개로 출력 통합 (ImageAnalysis 미사용)
  - Camera2 Interop으로 60fps 강제
explicit_ambiguities: []
---

# DELAYCAM-S002 — 1080p@60fps 카메라 캡처

## User Story
**As a** 사용자, **I want** 앱을 열자마자 1080p@60fps 라이브 화면을 본다, **so that** 동작을 부드럽고 또렷이 점검할 수 있다.

## Acceptance Criteria
- [ ] **AC-1** Given S26 Ultra 후면 카메라, When 앱 실행, Then SurfaceView에 1080×1920 (또는 1920×1080) 라이브 프리뷰가 표시된다.
- [ ] **AC-2** Given 1080p 모드, When `Choreographer.frameCallbacks` 카운트, Then 5분 동안 평균 ≥58fps, 최저 ≥55fps.
- [ ] **AC-3** Given Camera2 Interop, When AE 설정 검사, Then `CONTROL_AE_TARGET_FPS_RANGE = (60,60)`, `EDGE_MODE = OFF`, `NOISE_REDUCTION_MODE = FAST`.
- [ ] **AC-4** Given 60fps 미지원 폴백 기기, When 시작, Then 자동으로 30fps로 다운그레이드되며 토스트 알림 표시.
- [ ] **AC-5** Given 콜드 스타트, When 권한 이미 허용 상태, Then 앱 실행→첫 프리뷰 표시 ≤1.0s (NFR-9).
- [ ] **AC-6** Given M2P 측정 instrumentation, When 측정, Then ≤60ms (NFR-1).

## Implementation Steps
1. [ ] `CaptureSkill` 인터페이스 정의 (start/stop/switchFacing/getPreviewSurface)
2. [ ] CameraX `Preview` UseCase + `Camera2Interop.Extender` 적용
3. [ ] FPS 강제 파라미터 설정 (`CONTROL_AE_TARGET_FPS_RANGE`)
4. [ ] 60fps 미지원 시 30fps 폴백 로직
5. [ ] SurfaceTexture 출력 → 임시 SurfaceView 표시 (S007에서 GL로 대체 예정)
6. [ ] `Choreographer.FrameCallback`으로 fps 측정 코드
7. [ ] M2P 측정용 timestamp 계측 (capture timestamp ↔ display timestamp)

## Test Cases
- Instrumentation (S26 Ultra 실기기): 5분 fps 평균·최저 측정
- Instrumentation: 카메라 메타데이터 검증 (Camera2 Interop 적용 확인)
- Instrumentation: M2P latency 측정

## References
- PRD: §3 FR-1·6, §4 NFR-1·3·9
- ADR-001 Camera Capture Pipeline
