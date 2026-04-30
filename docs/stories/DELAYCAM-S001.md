---
artifact_type: story
story_id: DELAYCAM-S001
epic: DELAYCAM-EPIC-1
title: 프로젝트 부트스트랩
ticket: DELAYCAM-001
priority: P0
estimation: 1d
status: Ready
date: 2026-04-30
related_fr: [FR-12, FR-13]
related_nfr: [NFR-13, NFR-16]
related_adrs: []
constraints:
  - INTERNET permission 미선언
  - minSdk 26
explicit_ambiguities: []
---

# DELAYCAM-S001 — 프로젝트 부트스트랩

## User Story
**As a** 개발자, **I want** Gradle·Compose·Manifest·권한 흐름이 셋업된 빈 앱을 가진다, **so that** 이후 카메라·버퍼·렌더 작업에 바로 착수할 수 있다.

## Acceptance Criteria
- [ ] **AC-1** Given Android Studio 환경, When 앱을 빌드한다, Then 빈 Compose 화면이 S26 Ultra에 설치·실행된다.
- [ ] **AC-2** Given AndroidManifest.xml, When 검사한다, Then `<uses-permission INTERNET>` 가 **미선언**이고, `CAMERA` + `READ_MEDIA_VIDEO`(API33+) 만 선언되어 있다.
- [ ] **AC-3** Given 콜드 스타트, When 앱을 실행한다, Then 첫 화면 표시 ≤1.5s (NFR-9의 여유 한도, 이후 S008에서 ≤1.0s 강화).
- [ ] **AC-4** Given Gradle 설정, When `./gradlew test` 실행한다, Then `0 tests, 0 failures`로 종료되어 테스트 골격이 통과한다.
- [ ] **AC-5** Given build.gradle.kts, When 검사한다, Then `minSdk = 26`, `targetSdk = 34+`, `compileSdk` 일치.

## Implementation Steps
1. [ ] Android Studio 프로젝트 생성 (Empty Compose Activity)
2. [ ] Kotlin 2.1+, Compose BOM, CameraX, lifecycle 의존성 설정
3. [ ] AndroidManifest.xml에 CAMERA / READ_MEDIA_VIDEO 추가, INTERNET 미선언 명시
4. [ ] 권한 요청 흐름 (Compose `rememberLauncherForActivityResult`) 골격
5. [ ] 빈 Compose 화면 (`Hello DelayCam`)
6. [ ] `./gradlew test` 통과 확인

## Test Cases
- Unit: Manifest 분석 (`PackageManager` 조회로 INTERNET 권한 부재 검증)
- Instrumentation: 권한 거부 시 안내 다이얼로그 표시

## References
- PRD: docs/prd/DELAYCAM-001.md §3 FR-12·13, §4 NFR-13·16
- CLAUDE.md Stack
