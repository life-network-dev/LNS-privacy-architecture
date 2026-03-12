# System Architecture & Anonymization Design

## 1. 데이터 흐름도 (Data Lifecycle)

1.  **User App:** 건강 데이터를 생성하고, Enclave 전용 공개키로 데이터를 암호화합니다.
2.  **App Server (Bridge):** 암호화된 데이터를 수신하여 Enclave 내부로 전달합니다. (서버 관리자는 복호화 불가)
3.  **Nitro Enclave (TEE):**
    * 데이터 복호화 (격리된 메모리 내에서 수행)
    * **익명화 알고리즘 적용** (개인 식별 정보 제거)
    * 보상 지급 조건(Abuse 필터링) 검증
4.  **Output:** 익명화된 통계 데이터 및 보상 지급 확정 신호(Signal)만 외부로 반환합니다.

## 2. 익명화 로직 (Anonymization Algorithm)

본 엔진은 아래의 단계적 익명화 기법을 사용하여 데이터 재식별 위험을 방지합니다.

### A. 식별자 비식별화 (De-identification)
* User ID, Device ID 등 직접 식별자는 **Salted-Hash** 처리 후 고유 토큰으로 대체됩니다.
* Salt 값은 Enclave 외부로 유출되지 않도록 엄격히 관리됩니다.

### B. 데이터 버킷팅 (Data Binning/Bucketing)
* 정밀한 수치 데이터(예: 103)를 범위형 데이터(예: 100~105)로 변환하여 $k$-anonymity를 확보합니다.
* 정밀 위치 정보(GPS)는 특정 구역 단위(Grid)로 마스킹 처리합니다.

### C. 데이터 파기 정책
* 보상 계산에 사용된 원본 로우 데이터(Raw Data)는 연산 직후 Enclave 메모리에서 즉시 파기되며, 영구 저장소(DB)에는 오직 익명화된 결과값만 기록됩니다.

## 3. 위변조 및 어뷰징 방지 (Anti-Abuse)
* **HMAC Signature:** 모든 데이터 요청에 대해 앱-서버 간 무결성 검증을 수행합니다.
* **Logic-based Filtering:** 비정상적 속도 변화나 중복 기기 신호를 Enclave 내부 로직으로 필터링하여 보상 어뷰징을 차단합니다.
