---
artifact_type: epic
epic_id: DELAYCAM-EPIC-2
title: Buffer & Codec
ticket: DELAYCAM-001
status: Proposed
date: 2026-04-30
related_adrs: [ADR-002]
related_fr: [FR-2, FR-3]
priority: P0
---

# DELAYCAM-EPIC-2 — Buffer & Codec

## 목표
HW 인코더로 비트스트림 링버퍼를 구축하고 디코더를 통해 임의 PTS의 재생 텍스처를 만든다.
연속 지연 재생·리플레이·저장의 공통 데이터 레이어.

## 포함 Story
| Story | 제목 | 추정 |
|---|---|---|
| S004 | HW 인코더 + Surface 입력 (HEVC 기본·H.264 폴백) | 2d |
| S005 | BufferSkill — 비트스트림 링버퍼 + GC + 보호 토큰 | 3d |
| S006 | HW 디코더 + Delayed SurfaceTexture | 2d |

## 완료 조건 (Epic DoD)
- 60초 딜레이 + 1080p@60fps 5분 연속 시 RSS ≤2.5GB (NFR-5)
- 인코드+디코드 RTT ≤33ms (NFR-2)
- HEVC 미지원 기기에서 H.264 자동 폴백
