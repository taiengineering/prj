# CONSUMER_READINESS_AUDIT_v1

> Consumer Readiness Audit — 실사용 가능 상태 전수 검수
> Date: 2026-05-25
> Auditor: Runtime Core Phase v3
> Status: AUDIT COMPLETE — 수정 필요

## 핵심 요약

현재 Runtime Core 및 monitoring-runner는 실제 데이터 기반으로 정상 동작 중이나, ops-app은 아직 MOCK/provisioning 데이터와 REAL runtime 데이터가 혼재된 상태입니다.

- Runtime Core = REAL
- monitoring-runner = REAL
- ops-app = 일부 MOCK/BROKEN

즉, 엔진 문제가 아니라 운영 신뢰 UX 단계의 문제로 판단됩니다.

---

## 주요 Broken 이슈

### B1 — `/admin/services/SRV-001`
- 500 서버 에러
- 서비스 상세 접근 불가
- CRITICAL

### B2 — `/`
- "workspace 로드 중..." 무한 대기
- 첫 화면 unusable
- HIGH

### B3 — `/controls/collectors`
- 무한 로딩
- collector 상태 확인 불가
- HIGH

### B4 — `admin.taieng.co.kr`
- 빈 화면
- CSR 렌더링 또는 auth 문제 추정
- HIGH

---

## Mock-Reality 불일치

### Collector 수
- UI: 4 active
- 실제: HTTP Probe 1종

### Engine 수집 주기
- UI: 5분/10분
- 실제: 30s~120s

### Collector 종류
- UI: Health/Deploy/Auth/Error
- 실제: HTTP Probe만 구현

---

## 핵심 Gap

monitoring-runner가 생산하는 REAL 데이터:
- snapshot
- timeline
- operational status
- governance signal
- operational cases

가 ops-app에 연결되지 않음.

현재 ops-app은 provisioning 기반 MOCK 데이터를 보여주고 있음.

---

## 우선순위

### Priority 1 — Broken 제거
- 서비스 상세 500
- 메인 무한 로딩
- collectors 무한 로딩

### Priority 2 — Mock → Real 연결
ops-app에서 monitoring-runner API 연결:
- /snapshot
- /timeline
- /status

### Priority 3 — 운영자 언어 정리
Internal 용어:
- Primitive
- Governance
- FGW
- SYN

등을 운영자 언어로 변환.

---

## 결론

현재 시스템은 엔진이 없는 상태가 아니라,
REAL runtime은 존재하지만 consumer UX와 운영 신뢰 화면이 부족한 상태입니다.

앞으로의 우선순위:
- Broken 제거
- Mock 제거
- Real 연결
- 운영자 언어
- 소비자 신뢰 UX
