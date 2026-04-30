---
artifact_type: story
story_id: DELAYCAM-S007
epic: DELAYCAM-EPIC-3
title: OpenGL ES 3.0 렌더러 (SurfaceView, 듀얼 텍스처)
ticket: DELAYCAM-001
priority: P0
estimation: 3d
status: Ready
date: 2026-04-30
related_fr: [FR-2, FR-4]
related_nfr: [NFR-1, NFR-3]
related_adrs: [ADR-003]
constraints:
  - 렌더 스레드 1개 + EGLContext 1개
  - GL_OES_EGL_image_external 텍스처
explicit_ambiguities: []
---

# DELAYCAM-S007 — OpenGL ES 3.0 렌더러

## User Story
**As a** 시스템, **I want** 라이브·딜레이 두 SurfaceTexture를 단일 셰이더로 출력한다, **so that** 모드 전환을 1프레임 안에 처리하면서 60fps를 유지한다.

## Acceptance Criteria
- [ ] **AC-1** Given SurfaceView 부착, When `RenderSkill.start()`, Then EGLContext + 렌더 스레드 1개 생성, 셰이더 컴파일 성공.
- [ ] **AC-2** Given LiveTexture·DelayedTexture 등록, When `setMode(LIVE)`, Then 셰이더가 LiveTexture만 샘플링.
- [ ] **AC-3** Given `setMode(DELAYED)`, When 변경, Then 다음 frame부터 DelayedTexture 샘플링 (≤16ms 전환).
- [ ] **AC-4** Given 60fps 출력, When 5분 측정, Then dropped frame ≤1% (NFR-3).
- [ ] **AC-5** Given M2P latency, When LIVE 모드 측정, Then ≤60ms (NFR-1).
- [ ] **AC-6** Given 화면 회전 (portrait↔landscape), When 발생, Then EGLSurface 재생성 후 ≤200ms 내 정상 출력 복귀.

## Implementation Steps
1. [ ] `RenderSkill` 인터페이스 정의
2. [ ] EGL14 초기화 (Context, Surface, Display)
3. [ ] Vertex/Fragment 셰이더 (`samplerExternalOES` × 2, mode uniform)
4. [ ] SurfaceView 콜백 (`SurfaceHolder.Callback`) ↔ 렌더 스레드 통신
5. [ ] `Choreographer.postFrameCallback` 기반 60fps 페이싱
6. [ ] dropped frame 카운터 instrumentation
7. [ ] 화면 회전 시 EGLSurface 재생성

## References
- PRD: §3 FR-2·4, §4 NFR-1·3
- ADR-003
