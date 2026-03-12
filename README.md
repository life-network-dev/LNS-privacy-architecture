# LNS My Health Wallet Privacy Engine (TEE-based)

## 1. Overview
This is the core data privacy engine for **LNS My Health Wallet**. The project implements a **Confidential Computing** architecture designed to fundamentally prevent the exposure of personal information while collecting and analyzing sensitive health data (e.g., step count, heart rate) for token reward distribution.

## 2. Security Concept
Among various privacy-preserving technologies, this project adopts the **TEE (Trusted Execution Environment)** approach to satisfy both high-efficiency real-time processing of large-scale data and hardware-level security.

* **Isolation:** Utilizes **AWS Nitro Enclaves** to create an execution environment physically isolated from the main server OS and administrators.
* **Zero-Trust:** Data remains encrypted not only while **in-transit** and **at-rest** but also while **in-use** (during processing).
* **Integrity:** Prevents unauthorized code execution or data tampering through hardware-level **Attestation**.

## 3. Tech Stack
* **Infrastructure:** AWS EC2, AWS Nitro Enclaves
* **Security:** AES-256 (Encryption), SHA-256 (Hashing), AWS KMS (Key Management Service)
* **Development:** Python (Enclave Logic), FastAPI (Bridge API)
