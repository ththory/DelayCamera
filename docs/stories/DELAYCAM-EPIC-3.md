---
artifact_type: epic
epic_id: DELAYCAM-EPIC-3
title: Render & Playback Modes
ticket: DELAYCAM-001
status: Proposed
date: 2026-04-30
related_adrs: [ADR-003]
related_fr: [FR-2, FR-3, FR-4]
priority: P0
---

# DELAYCAM-EPIC-3 — Render & Playback Modes

## 목표
OpenGL ES 3.0 렌더러로 라이브·딜레이 듀얼 텍스처를 출력하고 슬라이더로 딜레이 시간을 즉시 조절한다.

## 포함 Story
| Story | 제목 | 추정 |
|---|---|---|
| S007 | OpenGL ES 3.0 렌더러 (SurfaceView, 듀얼 텍스처) | 3d |
| S008 | 라이브 모드 + 60fps 출력 검증 | 1d |
| S009 | 연속 지연 재생 + 슬라이더 UI (FR-2·3·4) | 3d |

## 완료 조건 (Epic DoD)
- 60fps dropped frame ≤1% (NFR-3)
- M2P latency ≤60ms (NFR-1)
- 슬라이더 변경 후 ≤500ms 내 새 딜레이 반영
- LIVE↔DELAYED 모드 전환 ≤16ms
