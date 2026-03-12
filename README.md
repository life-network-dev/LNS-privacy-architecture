# LNS My Health Wallet Privacy Engine (TEE-based)

## 1. 개요 (Overview)
LNS My Health Wallet의 핵심 데이터 프라이버시 엔진입니다. 본 프로젝트는 사용자의 민감한 건강 데이터(Step count, Heart rate 등)를 수집하고 분석하여 토큰 보상을 지급하는 과정에서, 개인정보 노출을 원천 차단하기 위한 **Confidential Computing(기밀 컴퓨팅)** 아키텍처를 구현합니다.

## 2. 보안 컨셉 (Security Concept)
저양한 프라이버시 보호 기술 중, 본 프로젝트는 실시간 대용량 데이터 처리 효율성과 하드웨어 수준의 보안성을 모두 충족하는 **TEE(Trusted Execution Environment)** 방식을 채택하였습니다.

* **Isolation (격리):** AWS Nitro Enclaves를 사용하여 메인 서버 OS 및 관리자로부터 물리적으로 격리된 실행 환경을 구축합니다.
* **Zero-Trust:** 데이터는 전송 중(In-transit), 보관 중(At-rest) 뿐만 아니라 처리 중(In-use)에도 암호화된 상태를 유지합니다.
* **Integrity (무결성):** 하드웨어 수준의 증명(Attestation)을 통해 승인되지 않은 코드의 실행이나 데이터 변조를 방지합니다.

## 3. 핵심 기술 (Tech Stack)
* **Infrastructure:** AWS EC2, AWS Nitro Enclaves
* **Security:** AES-256 (Encryption), SHA-256 (Hashing), KMS (Key Management)
* **Development:** Python (Enclave Logic), FastAPI (Bridge API)
