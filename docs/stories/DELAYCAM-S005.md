---
artifact_type: story
story_id: DELAYCAM-S005
epic: DELAYCAM-EPIC-2
title: BufferSkill — 비트스트림 링버퍼 + GC + 보호 토큰
ticket: DELAYCAM-001
priority: P0
estimation: 3d
status: Ready
date: 2026-04-30
related_fr: [FR-2, FR-3, FR-7, FR-7a, FR-7b]
related_nfr: [NFR-5]
related_adrs: [ADR-002]
constraints:
  - 최대 60초 보관 (A-1 해소)
  - 저장 진행 중 PTS 범위 보호 카운트 필요
  - 동시성 안전 (encoder thread · decoder thread · storage thread · UI thread)
explicit_ambiguities: []
---

# DELAYCAM-S005 — BufferSkill (비트스트림 링버퍼)

## User Story
**As a** 시스템, **I want** 인코딩된 NAL을 시간 정렬 큐로 보관하고 만료된 것을 GC한다, **so that** 메모리 한계 내에서 임의 시점 재생·저장이 가능하다.

## Acceptance Criteria
- [ ] **AC-1** Given append() 호출, When 동시 100개 thread, Then 발행 순서대로 PTS 단조증가 보장 (race condition 없음).
- [ ] **AC-2** Given 60초 1080p@60fps 저장 후, When `approxBytesInMemory` 조회, Then ≤50MB (HEVC 5Mbps 기준).
- [ ] **AC-3** Given 만료된 PTS, When GC 코루틴 1회 sweep, Then `now - maxDelaySetting - 0.5s` 이전 NAL 제거됨. 단, 보호 토큰이 점유한 범위는 유지.
- [ ] **AC-4** Given `acquireProtection(from, to)`, When 호출 후 `release()`, Then 그 사이 GC가 해당 범위를 폐기하지 않음.
- [ ] **AC-5** Given `queryAtDelay(15s)`, When 호출, Then `now - 15s` 이전 가장 가까운 키프레임 + 그 이후 NAL 시퀀스가 반환됨.
- [ ] **AC-6** Given `queryRange(from, to)`, When 호출, Then 해당 범위에 포함되는 모든 NAL이 PTS 순으로 반환됨.
- [ ] **AC-7** Given OOM 시뮬레이션 (메모리 압박), When append, Then 보호되지 않은 가장 오래된 NAL부터 즉시 폐기.

## Implementation Steps
1. [ ] `BufferSkill` 인터페이스 정의
2. [ ] `ConcurrentLinkedDeque<EncodedSample>` 기반 구현
3. [ ] PTS 기반 binary search (queryAtDelay, queryRange)
4. [ ] GC 코루틴 (1초 주기 sweep) + `Mutex` 보호
5. [ ] `ProtectionToken` 발급·반환 (`AtomicLong` ID + interval map)
6. [ ] 단위 테스트: 동시성·GC·보호·메모리 한도 시나리오 8건+
7. [ ] 메모리 측정 instrumentation

## Test Cases (필수)
- Unit: 100 thread 동시 append, PTS 단조성
- Unit: GC와 동시 acquireProtection의 race condition
- Unit: queryAtDelay 정확도 (1프레임 단위)
- Unit: 메모리 한도 도달 시 폐기 우선순위

## References
- PRD: §3 FR-2·3·7·7a·7b, §4 NFR-5
- ADR-002
