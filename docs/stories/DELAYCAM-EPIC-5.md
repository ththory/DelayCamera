---
artifact_type: epic
epic_id: DELAYCAM-EPIC-5
title: Thermal & Privacy
ticket: DELAYCAM-001
status: Proposed
date: 2026-04-30
related_adrs: [ADR-005, ADR-004]
related_fr: [FR-13, FR-14, FR-12]
priority: P0
---

# DELAYCAM-EPIC-5 — Thermal & Privacy

## 목표
4단계 발열 정책을 시스템 레벨로 통합하고, 권한·네트워크 미사용을 검증 가능한 형태로 보장한다.

## 포함 Story
| Story | 제목 | 추정 |
|---|---|---|
| S014 | 발열 모니터링 + 4단계 정책 (FR-14) | 3d |
| S015 | 권한·프라이버시 검증 (FR-12·13, NFR-12·13·17) | 2d |

## 완료 조건 (Epic DoD)
- 25℃ 30분 1080p@60fps 시 SEVERE 미진입
- 표면 온도 ≤45℃ (별도 SOP)
- `INTERNET` permission lint·런타임 모두에서 0건 검증
- 권한 = `CAMERA` + (API33+) `READ_MEDIA_VIDEO` 만
