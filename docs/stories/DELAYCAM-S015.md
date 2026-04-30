---
artifact_type: story
story_id: DELAYCAM-S015
epic: DELAYCAM-EPIC-5
title: 권한·프라이버시 검증
ticket: DELAYCAM-001
priority: P0
estimation: 2d
status: Ready
date: 2026-04-30
related_fr: [FR-12, FR-13]
related_nfr: [NFR-12, NFR-13, NFR-17]
related_adrs: [ADR-004]
constraints:
  - INTERNET permission 미선언 — 검증 가능
  - 휘발성 메모리 외 영상 저장 금지 (FR-7 명시 액션 제외)
explicit_ambiguities: []
---

# DELAYCAM-S015 — 권한·프라이버시 검증

## User Story
**As a** 사용자, **I want** 앱이 네트워크에 접근하지 않고 권한도 최소만 요구한다는 사실을 검증할 수 있다, **so that** 영상이 외부로 새지 않음을 신뢰할 수 있다.

## Acceptance Criteria
- [ ] **AC-1** Given AndroidManifest.xml, When 정적 검사, Then `<uses-permission android:name="android.permission.INTERNET">` 미선언.
- [ ] **AC-2** Given Lint 규칙, When `./gradlew lint`, Then INTERNET permission 누설을 탐지하는 커스텀 lint check 통과.
- [ ] **AC-3** Given 런타임, When `getApplicationContext().checkPermission(android.Manifest.permission.INTERNET, ...)`, Then `PERMISSION_DENIED` 반환.
- [ ] **AC-4** Given instrumentation test, When `Socket("8.8.8.8", 53)` 시도, Then `SecurityException` 또는 즉시 실패.
- [ ] **AC-5** Given 앱 종료, When BufferSkill 상태, Then 모든 NAL 데이터가 폐기 (메모리 dump 검증).
- [ ] **AC-6** Given 백그라운드 진입 (수동 녹화 미진행), When 발생, Then 캡처 정지 + 버퍼 폐기.
- [ ] **AC-7** Given 저장 파일, When `ExifInterface` / `MediaMetadataRetriever`로 검사, Then 위치(GPS) 메타데이터 0건.
- [ ] **AC-8** Given 의존성, When `./gradlew :app:dependencies`, Then 네트워크 호출 라이브러리 (Retrofit, OkHttp 등) 미포함.

## Implementation Steps
1. [ ] AndroidManifest 검증 단위 테스트
2. [ ] Custom Lint Rule (`InternetPermissionDeclared` detector)
3. [ ] Network call instrumentation (`Socket` 시도 → SecurityException 기대)
4. [ ] 백그라운드 lifecycle 핸들러 (수동 녹화 중이 아니면 캡처 정지)
5. [ ] 저장 파일 메타데이터 위치 정보 0 보장 (`MediaMetadataRetriever` 단위 테스트)
6. [ ] 의존성 audit 스크립트

## References
- PRD: §3 FR-12·13, §4 NFR-12·13·17
- ADR-004
