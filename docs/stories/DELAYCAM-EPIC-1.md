---
artifact_type: epic
epic_id: DELAYCAM-EPIC-1
title: Foundation & Capture
ticket: DELAYCAM-001
status: Proposed
date: 2026-04-30
related_adrs: [ADR-001]
related_fr: [FR-1, FR-5, FR-6, FR-12, FR-13]
priority: P0
---

# DELAYCAM-EPIC-1 — Foundation & Capture

## 목표
프로젝트 골격을 세우고 1080p@60fps 카메라 캡처를 안정화한다. 이후 모든 Epic의 입력 소스가 된다.

## 포함 Story
| Story | 제목 | 추정 |
|---|---|---|
| S001 | 프로젝트 부트스트랩 (Gradle·Compose·Manifest·권한 흐름) | 1d |
| S002 | 1080p@60fps 카메라 캡처 (CameraX + Camera2 Interop) | 3d |
| S003 | 전·후면 카메라 전환 (≤500ms) | 1d |

## 완료 조건 (Epic DoD)
- 앱이 부팅 후 ≤1.0s 내 라이브 프리뷰 표시 (NFR-9)
- 1080p@60fps 5분 연속 fps 변동 ≤5%
- 카메라 전환 ≤500ms (NFR-10)
- `INTERNET` permission 미선언 (검증 가능)
