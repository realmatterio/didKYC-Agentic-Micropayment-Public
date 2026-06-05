# did_identity-address-credential_vc-verification Method
## 3-tier Certificate Authority

## Purpose
This document defines a 3-tier Certificate Authority (CA) implementation pattern for DID identity, wallet address binding, VC issuance, and verification in didKYC agentic micropayment.

The method follows a PKI-style trust hierarchy:
- Tier 1: Payment Gateway as Issuer and All-Verifier with Root CA
- Tier 2: Hybrid Wallets as End-Entity-Verifier with Immediate CAs
- Tier 3: Agents and Browsers as Subjects with End-Entity CAs

```text
Trust Hierarchy (3 Tiers)

Payment Gateway (Tier 1)
   Root CA (per human user)
           |
           | signs
           v
Hybrid Wallet (Tier 2)
   Immediate CA (per wallet)
           |
           | signs
           v
Subjects (Tier 3)
   +-- Agent End-Entity Cert + Agent DID
   +-- Browser End-Entity Cert (TLS/SSL)
```

## 1. Trust Hierarchy and Roles

### Tier 1: Payment Gateway (Root CA Layer)
- Operates the Root CA service.
- Issues one user-scoped Root CA certificate chain per human user after eKYC.
- Issues User DID and User VC (identity credential) bound to the user Root CA.
- Verifies all downstream certificate chains and VCs during payment authorization and settlement proof.

### Tier 2: Hybrid Wallet (Immediate CA Layer)
- Each wallet instance is provisioned under an Immediate CA derived from the user Root CA.
- Wallet blockchain address and Wallet DID are derived and cryptographically bound to the Immediate CA key material.
- Wallet VC is derived from User VC and includes wallet-specific claims.
- Acts as end-entity verifier for Agent DID authorization and Browser trust context used in payment execution.

### Tier 3: Agent and Browser (End-Entity CA Layer)
- Agent and Browser are subjects receiving end-entity certificates from wallet-side CA hierarchy.
- Agent DID is used by wallet to verify agent identity and authorize delegated payment actions.
- Browser certificate is used for TLS/SSL setup when connecting to Payment Gateway services.
- Subject proofs are presented to wallet and payment gateway in transaction flow.

## 2. Data Objects

### Certificate Objects
- Root CA Certificate (per human user): issued by payment gateway trust anchor.
- Immediate CA Certificate (per wallet): signed by user Root CA.
- End-Entity Certificate (per agent/browser instance): signed by wallet Immediate CA.

### DID Objects
- User DID: derived from Root CA public key fingerprint.
- Wallet DID: derived from Immediate CA public key fingerprint.
- Agent DID (subject DID): derived from end-entity agent key and bound to agent certificate.

### VC Objects
- User VC: issued after eKYC, subject is User DID.
- Wallet VC: derived credential from User VC, subject is Wallet DID, includes wallet address binding.
- Agent Authorization VC (optional but recommended): links Agent DID to wallet policy scope.

## 3. Derivation Rules

### 3.1 User Root CA and User DID
1. Human user passes eKYC at payment gateway.
2. Payment gateway generates user Root CA keypair in HSM-backed environment.
3. User DID is generated from Root CA public key fingerprint.
4. Payment gateway issues User VC with claims:
   - identity assurance level
   - KYC completion timestamp
   - jurisdiction and policy profile
   - root certificate thumbprint reference

### 3.2 Immediate CA, Wallet Address, and Wallet DID
1. User creates a hybrid wallet instance.
2. Wallet Immediate CA keypair is generated in secure wallet module.
3. Immediate CA certificate is signed by user Root CA.
4. Wallet DID is derived from Immediate CA public key fingerprint.
5. Wallet blockchain address is derived from wallet signing key.
6. Wallet VC is issued as a derived credential from User VC with claims:
   - parent User DID
   - Wallet DID
   - wallet blockchain address
   - custody model (hybrid)
   - spending policy constraints

### 3.3 End-Entity CA for Agent and Browser
1. Wallet or gateway policy engine requests end-entity issuance.
2. Agent instance keypair and browser TLS keypair are generated.
3. End-entity certificates are signed by wallet Immediate CA.
4. Agent DID is bound to agent end-entity certificate.
5. Browser certificate is bound to gateway domain policy for TLS mutual trust.

```text
Issuance and Derivation Map

[eKYC Passed]
   |
   v
[Gateway issues User Root CA] ---> [User DID]
   |
   +--------------------------> [User VC]
   |
   v
[Wallet Immediate CA issued]
   |
   +--> [Wallet DID]
   +--> [Wallet Address]
   +--> [Wallet VC derived from User VC]
   |
   v
[Agent/Browser End-Entity Certs]
   |
   +--> [Agent DID bound to Agent Cert]
   +--> [Browser TLS Cert bound to Gateway policy]
```

## 4. Verification Method

### 4.1 Gateway Verification (All-Verifier)
For every payment initiation, payment gateway verifies:
1. Certificate chain validity:
   - End-Entity Cert -> Immediate CA -> User Root CA
2. Revocation and status:
   - CRL/OCSP or equivalent status service for each tier
3. DID integrity:
   - DID document key material matches certificate public keys
4. VC integrity:
   - issuer signature valid
   - credential status active
   - expiration and nonce checks
5. Binding consistency:
   - User DID in User VC links to Root CA thumbprint
   - Wallet DID in Wallet VC links to Immediate CA
   - wallet address in Wallet VC matches transaction sender
6. Policy checks:
   - amount limits
   - merchant whitelist
   - delegated authority scope and time window

### 4.2 Wallet Verification (End-Entity-Verifier)
Before signing payment transaction, wallet verifies:
1. Agent end-entity certificate chain to Immediate CA.
2. Agent DID proof-of-possession and challenge response.
3. Agent authorization claims against wallet policy.
4. Browser trust status when payment is browser-mediated.

### 4.3 Subject-Side Proof (Agent and Browser)
Agent must present:
- Agent certificate chain
- Agent DID proof
- Optional Agent Authorization VC

Browser must present:
- Browser certificate for TLS/SSL context
- session integrity evidence required by gateway

## 5. End-to-End Flow

1. eKYC onboarding:
   - Gateway issues user Root CA and User VC.
2. Wallet provisioning:
   - Wallet Immediate CA issued from user Root CA.
   - Wallet DID and wallet address created.
   - Wallet VC derived from User VC.
3. Agent/browser enrollment:
   - End-entity certs issued from Immediate CA.
   - Agent DID bound to agent cert.
4. Payment initiation:
   - Agent requests payment via wallet.
5. Local wallet authorization:
   - Wallet verifies agent identity and policy scope.
6. Gateway verification:
   - Gateway verifies full chain, DID/VC bindings, and policy.
7. Settlement and proof:
   - Wallet submits signed transaction.
   - Gateway records payment proof linked to Wallet DID and Wallet VC.

```text
Runtime Verification Flow

Agent ---- proof ----> Wallet ---- signed tx + proofs ----> Gateway
   |                      |                                   |
   |                      | verifies:                         | verifies:
   |                      | - Agent Cert chain               | - Full cert chain
   |                      | - Agent DID PoP                  | - DID/VC integrity
   |                      | - Policy scope                   | - Revocation/status
   |                      |                                   | - Policy + binding
   |<----- allow/deny ----|<------------ allow/deny ---------|
```

## 6. Required Security Controls
- HSM or equivalent protection for gateway Root CA private keys.
- Secure enclave or hardware-backed storage for wallet Immediate CA keys where possible.
- Short-lived end-entity certificates for agents and browsers.
- Mandatory revocation endpoints and cache freshness controls.
- Anti-replay (nonce, timestamp, bounded validity window).
- Key rotation policy at all tiers with backward-compatible trust update process.

## 7. Minimum Claim Set (Recommendation)

### User VC Claims
- sub: User DID
- kyc_level
- jurisdiction
- issued_at
- expires_at
- root_ca_thumbprint

### Wallet VC Claims
- sub: Wallet DID
- parent_sub: User DID
- wallet_address
- custody_type: hybrid
- daily_limit
- allowed_use: agentic_micropayment
- immediate_ca_thumbprint

### Agent Authorization Claims
- sub: Agent DID
- wallet_sub: Wallet DID
- permitted_actions
- max_amount
- validity_window
- end_entity_cert_thumbprint

## 8. Implementation Notes
- Use deterministic DID derivation from certificate public key fingerprints to keep certificate and DID layers cryptographically aligned.
- Keep personally identifiable information out of on-chain payloads; anchor only hashes, proofs, and references.
- Treat wallet VC as the principal payment proof object that the gateway verifies and stores with transaction evidence.

## 9. Compliance and Audit Trace
For each payment, persist an audit package containing:
- certificate chain fingerprints (all tiers)
- DID resolution result snapshots
- VC verification result and status proof
- policy decision log (allow/deny with reasons)
- transaction hash and settlement timestamp

This method provides a CA-compatible and DID/VC-native trust model for agentic micropayment while preserving verifiability, policy control, and auditable compliance.
