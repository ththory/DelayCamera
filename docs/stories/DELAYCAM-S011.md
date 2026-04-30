---
artifact_type: story
story_id: DELAYCAM-S011
epic: DELAYCAM-EPIC-4
title: 저장 — LAST_N (직전 N초)
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-7]
related_nfr: [NFR-11, NFR-12, NFR-13]
related_adrs: [ADR-004]
constraints:
  - 재인코딩 없음 (링버퍼 NAL → MediaMuxer remux)
  - MediaStore 작성 (Movies/DelayCam/)
  - 1080p HEVC 60초 클립 ≤2초 저장
explicit_ambiguities: []
---

# DELAYCAM-S011 — 저장 LAST_N

## User Story
**As a** 사용자, **I want** "저장" 버튼으로 직전 N초 영상을 갤러리에 저장한다, **so that** 좋은 동작을 보존하고 나중에 비교한다.

## Acceptance Criteria
- [ ] **AC-1** Given 슬라이더 = 30초, When "저장" 버튼 탭, Then 직전 30초 영상이 `Movies/DelayCam/delaycam_<timestamp>.mp4` 로 저장됨.
- [ ] **AC-2** Given 저장 작업, When 시작, Then BufferSkill `acquireProtection(now-30, now)` 즉시 호출되어 GC가 해당 범위를 보호.
- [ ] **AC-3** Given 저장 NAL 시퀀스, When MediaMuxer 작성, Then 첫 NAL은 SPS/PPS(/VPS) → IDR → P-frame 순서, 컨테이너 MP4.
- [ ] **AC-4** Given 1080p HEVC 60초, When 저장, Then 저장 시간 ≤2초 (S26 Ultra UFS 4.0).
- [ ] **AC-5** Given 저장 완료, When MediaStore.Uri 반환, Then 갤러리 앱에서 해당 영상이 즉시 보이고 정상 재생.
- [ ] **AC-6** Given 저장 중 라이브 fps, When 측정, Then drop ≤5%.
- [ ] **AC-7** Given 저장 100회 반복, When 측정, Then 실패율 ≤0.1% (NFR-11).
- [ ] **AC-8** Given 저장 파일, When 메타데이터 검사, Then EXIF/위치 정보 0건 (NFR-17 프라이버시).

## Implementation Steps
1. [ ] `StorageSkill.saveLastN(durationSec): Result<Uri>` 구현
2. [ ] `MediaMuxer(MUXER_OUTPUT_MPEG_4)` + `addTrack` (csd-0/csd-1 적용)
3. [ ] `MediaStore.Video.Media` 인서트 + `RELATIVE_PATH = "Movies/DelayCam"`
4. [ ] 보호 토큰 acquire/release lifecycle
5. [ ] 진행 중 UI 인디케이터 (저장중 / 완료 / 실패)
6. [ ] 단위 테스트: 키프레임 미정렬 시 폴백 (다음 키프레임으로 스냅)
7. [ ] 100회 반복 instrumentation

## References
- PRD: §3 FR-7, §4 NFR-11·12·13·17
- ADR-004
