# System Architecture & Anonymization Design

## 1. Data Lifecycle

1. **User App:** Generates health data and encrypts it using an Enclave-specific public key.
2. **App Server (Bridge):** Receives the encrypted data and passes it to the Enclave. (The server administrator cannot decrypt the data.)
3. **Nitro Enclave (TEE):**
    * **Data Decryption:** Performed within isolated memory.
    * **Anonymization Algorithm:** Application of logic to remove Personally Identifiable Information (PII).
    * **Verification:** Validates reward distribution conditions and performs abuse filtering.
4. **Output:** Returns only the anonymized statistical data and the reward confirmation signal to the external environment.

## 2. Anonymization Logic

The engine employs the following step-by-step anonymization techniques to prevent the risk of data re-identification.

### A. De-identification
* Direct identifiers such as User ID and Device ID are replaced with unique tokens after undergoing **Salted-Hash** processing.
* Salt values are strictly managed to ensure they are never leaked outside the Enclave.

### B. Data Binning/Bucketing
* Precise numerical data (e.g., 103) is converted into range-based data (e.g., 100~105) to ensure **$k$-anonymity**.
* Precise location information (GPS) is masked and processed into specific grid units.

### C. Data Destruction Policy
* Original raw data used for reward calculations is immediately purged from the Enclave memory after computation. Only anonymized results are recorded in the permanent storage (DB).

## 3. Anti-Abuse
* **HMAC Signature:** Performs integrity verification for all data requests between the app and the server.
* **Logic-based Filtering:** Blocks reward abuse by filtering out abnormal speed changes or duplicate device signals using internal Enclave logic.
