---
artifact_type: epic
epic_id: DELAYCAM-EPIC-4
title: Replay & Save
ticket: DELAYCAM-001
status: Proposed
date: 2026-04-30
related_adrs: [ADR-004]
related_fr: [FR-7, FR-7a, FR-7b]
priority: P0
---

# DELAYCAM-EPIC-4 — Replay & Save

## 목표
리플레이 버튼으로 직전 N초를 1회 재생하고, 재인코딩 없이 MP4로 저장한다.

## 포함 Story
| Story | 제목 | 추정 |
|---|---|---|
| S010 | 리플레이 버튼 + 재생 1회 후 자동 복귀 (FR-7a) | 2d |
| S011 | 저장 — LAST_N (직전 N초) (FR-7) | 2d |
| S012 | 저장 — 리플레이 클립 (FR-7b) | 1d |
| S013 | 저장 — 수동 녹화 시작·종료 (FR-7) | 2d |

## 완료 조건 (Epic DoD)
- 리플레이 중 라이브 캡처·버퍼링 끊김 없음
- 1080p HEVC 60초 클립 저장 ≤2초
- 저장 실패율 ≤0.1% (NFR-11)
- MediaStore 등록 직후 갤러리 앱에서 즉시 재생
