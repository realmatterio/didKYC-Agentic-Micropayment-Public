# Agentic Micropayment Wallet and Checkout
### A CA-Bound Identity, DID-KYC-Verified Bridge Between Human-Controlled Wallets and Policy-Enforced Agentic Checkout Micropayments
#### Trust Agent Wallet and AP2-MCP Checkout Technology

> **Version:** 1.0  
> **Release Date:** May 2026  
> **Author:** Real Matter

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Introduction and Problem Statement](#1-introduction-and-problem-statement)
3. [Business Case](#2-business-case)
4. [Why This Bridge Model](#3-why-this-bridge-model)
5. [Agent-Wallet-Custodian-Gateway Architecture](#4-agent-wallet-custodian-gateway-architecture)
6. [didKYC Trust Model: CA-Bound Identity, DID, and VC Verification](#5-didkyc-trust-model-ca-bound-identity-did-and-vc-verification)
7. [Agentic Payment Transaction Flow](#6-agentic-payment-transaction-flow)
8. [FinTech and RegTech Integration](#7-fintech-and-regtech-integration)
9. [Overall Feasibility Assessment](#8-overall-feasibility-assessment)
10. [Appendices](#appendices)

---

![didKYC Wallet Cover](Frontpage%20of%20Real%20Matter%20Wallet%20(Stablecoin%20with%20Agentic%20Micro-Payment)%20-%202026APR.png)

## Executive Summary
**Agentic Micropayment Wallet and Checkout** is a **trust-agent wallet and checkout bridge** that connects a human-controlled wallet model with KYC-enabled agentic micropayment execution. The design combines **DID (Decentralized Identifier)** and **eKYC-VC (Verifiable Credential)** technologies so AI agents can act autonomously under explicit human and policy boundaries.

The product adopts a **Hybrid Custody** architecture:
- **Hosted Custody Layer**: Based on a permissioned blockchain (Besu) hosted by a payment gateway, with a maximum stored value of HK$3,000.
- **didKYC Self-Custody Cold Wallet Layer**: Daily transaction cap of HK$100 held by a human, focused on DID + VC verification.

Before any Agentic Payment is executed, the AI Agent must first complete **didKYC identity verification** with the Payment Gateway. In this model, checkout execution is **Policy-Enforced Agentic Checkout via AP2-MCP**, where verification and execution are fail-closed and fully auditable. This bridge design significantly improves autonomy, security, and compliance, and is suitable for high-frequency AI-agent micropayments.

---

## 1. Introduction and Problem Statement
As the AI Agent economy grows rapidly, standalone wallet models and traditional KYC/payment systems can no longer meet the needs of autonomous agentic payments. The core gap is the lack of an operational bridge between human-controlled funds and machine-executed transactions. Key challenges include:
- Traditional centralized KYC creates high data-exposure risk and does not natively support privacy-preserving, machine-verifiable AI Agent identity (including zero-knowledge-proof identity assertions)
- Lack of programmable and verifiable authorization mechanisms
- Difficulty balancing offline operations, compliance auditability, and autonomy

Using W3C DID and VC standards, together with zero-knowledge proofs, **Agentic Micropayment Wallet and Checkout** provides a complete Hong Kong-oriented compliance solution centered on a trust-agent wallet bridge and AP2-MCP checkout control plane.

---

## 2. Business Case
**Market Opportunity**:  
Agentic Commerce is becoming a mainstream payment trend in 2026. As a fintech hub, Hong Kong has an SVF framework and a DLT-friendly environment, with strong global connectivity.

**Key Use Cases**:
1. **API and AI Service Pay-per-Use**: AI agents pay per request for LLM inference, data queries, or real-time market information.
2. **Recurring Cloud Supply Chain Auto-Payments**: AI agents automatically process recurring fees to cloud providers, such as webcam cloud analytics and storage.
3. **Agent-to-Agent Collaboration Marketplaces**: Different AI agents complete subtasks and settle compensation automatically.
4. **AI-IoT Agents**: Autonomous commercial business planning, price comparison, automatic ordering, and payment.
5. **GenAI Content Micropayments**: Paying for single articles, videos, or premium datasets beyond paywalls.

**Business Value**: Low transaction fees with strong recurring-revenue potential.

---

## 3. Why This Bridge Model
This section explains the design choices behind positioning the product as a trust agent wallet bridge between human control and KYC-enabled agentic micropayments.

- **Why Hybrid Custody (Risk Management)**  
Hybrid custody separates higher-value storage from autonomous execution. A hosted layer handles larger balances with stronger institutional controls, while a constrained self-custody layer limits blast radius for agent mistakes or compromise. This creates layered risk controls across value tiers.

- **Why Cold Wallet (Human Control)**  
The cold-wallet model preserves clear human ownership over root keys and final authority. Agents do not hold unrestricted owner keys; they operate through policy-constrained delegated signing rights. This keeps automation accountable to human intent and governance.

- **Why didKYC (VC-Verifiable Identity)**  
didKYC uses DID + VC as a verifiable, zero-knowledge-proof identity trust layer. Instead of repeatedly exposing full identity records, the system verifies required attributes cryptographically, enabling privacy-aware compliance checks and machine-verifiable authorization.

- **Why Micropayment (Agent-to-Agent Economy)**  
Agent ecosystems increasingly require frequent, low-value settlement across services, APIs, and autonomous workflows. Micropayments fit agent-to-agent collaboration by reducing transaction friction, enabling granular pay-per-action pricing, and supporting programmable recurring value exchange.

---

## 4. Agent-Wallet-Custodian-Gateway Architecture
The didKYC Agentic Micropayment Wallet uses a Hybrid Custody + Trust Agent Payment Gateway design, specifically for AI Agentic Payments. The core architecture has three parts:

- **Hosted Custody Layer** (based on Hyperledger Besu)  
A permissioned blockchain operated by the Trust Agent Payment Gateway provides hosted custody services. Users (Human Owners) can top up via traditional payment methods (such as bank transfer and credit card), up to HK$3,000. Based on preset policies, the system transfers a limited amount daily to the didKYC Self-Custody Cold Wallet.

- **didKYC Self-Custody Cold Wallet Layer**  
A truly non-custodial cold wallet controlled by the user. Daily transaction cap: HK$100. Core capabilities include transaction signing and DID + VC verification to ensure every agent payment is identity-verified before execution.  
For security and custody-boundary clarity, the owner's root private key remains under user control, while the AI Agent uses policy-constrained delegated signing authority (for example, short-lived delegated keys/session keys), not unrestricted custody over the owner's root key.

- **AI Agentic Payment via a Trust Agent Payment Gateway**  
The Payment Gateway provides a trust-agent-compatible interface, including:
   - Agentic Payment Button: a WebMCP HTML button with embedded structured and machine-readable attributes.
   - [payment_skill.md](payment_skill.md) instruction document: machine-readable guidance for AI Agents to understand payment flow, API endpoints, and requirements.
   - AP2-MCP Checkout Control Plane: two mandatory operations, preflight verification (`ap2.verify_mandate`) and policy-enforced execution (`ap2.pay`).
   - DID + VC verification channel: the Agent must complete didKYC verification before payment is triggered.
   - AP2-style signed mandate channel: each payment request must map to a valid cryptographically signed user mandate.
   - Policy Engine: real-time checks for daily limit, merchant whitelist, and other compliance rules.

**Checkout Trust Boundary**: The Agent cannot directly call signing or broadcast endpoints. Wallet HSM (ICCHSMatter) signing and Besu broadcast are executed server-side through AP2-MCP after policy checks pass.

**PKI Trust Hierarchy**: The gateway operates a 4-tier CA model that underpins all identity, wallet, and agent trust. Tier 1 Root CA (offline HSM) → Tier 2 Shared Issuing CA (per region or risk class) → Tier 3 Wallet CA (per wallet instance) → Tier 4 short-lived session certs (1–24h per agent session). See [didKYC_ca_bound_identity_address_verification_architecture.md](didKYC_ca_bound_identity_address_verification_architecture.md) for the full specification.

### Governance Guideline Integration
- **Client-Side Agent Machine Governance**: The agent runtime must load and apply [risk_assessment.md](risk_assessment.md) as a policy baseline before initiating payment actions. At minimum, it must enforce risk controls related to daily spending caps, merchant allow-list behavior, delegated key usage constraints, and escalation paths for suspicious patterns.
- **Gateway-Side Governance**: The Payment Gateway control plane and policy engine must use [risk_assessment.md](risk_assessment.md) as the canonical risk-control reference for verification, monitoring, STR workflow routing, and periodic control reviews. Gateway decisions should be traceable to the relevant risk category and mitigation controls in the document.

This Trust Agent Payment Gateway enables verified AI Agents to execute payments within CA-bound, policy-enforced boundaries, while maintaining full auditability and compliance.  
The two custody layers are connected seamlessly through a unified DID mechanism and Trust Agent Gateway, enabling layered risk management (higher-value hosted custody + low-value autonomous agent payments).

---

## 5. didKYC Trust Model: CA-Bound Identity, DID, and VC Verification
**didKYC** is the product's innovative identity system:
- After the user (Human Owner) completes initial eKYC, a CA-trusted Tier 2 Issuing CA attests the KYC result and issues **Verifiable Credentials (VCs)** bound to the user's **Decentralized Identifier (DID)**.
- The didKYC Cold Wallet is provisioned with a Tier 3 Wallet CA certificate, cryptographically binding the Wallet DID, wallet blockchain address, and custody policy.
- The AI Agent operates using a short-lived Tier 4 session certificate (1–24h) and an Agent Authorization VC scoped to permitted actions, spending cap, and validity window.
- The wallet issues AP2-style signed mandates that capture verifiable user intent (merchant, amount, allowed actions, validity window, nonce).
- Payment authorization requires **dual verification**: the gateway checks both the full PKI certificate chain (Tier 4 → Tier 3 → Tier 2 → Tier 1) and the DID/VC binding proof simultaneously.

In production profile, authorization is effectively **triple-bound**:
1. PKI chain and session cert validity,
2. DID/VC identity and delegation consistency,
3. AP2-style mandate signature and scope validity.

**Key design principle**: KYC claims are attested by a CA-trusted issuer and expressed as VC claims bound to DID-controlled keys. Decentralized identity ownership and centralized trust governance are complementary, not conflicting.

**Advantages**:
- Decentralized, privacy-preserving, and enabled by zero-knowledge proofs (selective disclosure).
- Cross-platform verifiability without repeatedly submitting identity data.
- CA trust provides enterprise-grade revocation, audit, and key lifecycle governance.
- Aligned with HKMA's direction on digital identity and DLT development.

For the full technical specification of the CA hierarchy, DID/VC binding rules, eKYC issuance controls, revocation strategy, and audit requirements, see [didKYC_ca_bound_identity_address_verification_architecture.md](didKYC_ca_bound_identity_address_verification_architecture.md).

---

## 6. Agentic Payment Transaction Flow
1. **User Top-Up**: Funds are topped up through traditional payment methods into Hosted Custody (Besu).
2. **Daily Authorized Transfer**: According to policy, the system transfers a limited amount daily (<= HK$100) into the didKYC Self-Custody Cold Wallet.
3. **AI Agent Initiates Payment**:
   - 3.1 The Agent connects to the Payment Gateway's **Agentic Payment Button** (special HTML code).
   - 3.2 The Agent automatically reads the [payment_skill.md](payment_skill.md) guide to understand payment protocol requirements.
   - 3.3 The Agent submits its Tier 4 session certificate chain plus DID proof, VC presentation, and AP2-style signed mandate to the Payment Gateway using `ap2.verify_mandate`.
   - 3.4 The Payment Gateway performs **binding verification**: (a) validates the full PKI cert chain from Tier 4 to Tier 1 and checks revocation status, (b) verifies DID integrity, VC signature, credential status, and identity bindings, and (c) verifies mandate signature, nonce, validity window, and scope constraints.
   - 3.5 The Policy Engine evaluates daily limit, merchant whitelist, delegated scope, mandate constraints, replay resistance, and anomaly signals.
   - 3.6 After verification allow, the Agent requests execution through `ap2.pay`; AP2-MCP re-validates fail-closed checks before signing.
   - 3.7 AP2-MCP invokes Wallet HSM (ICCHSMatter) server-side signing and broadcasts through the approved Besu relay.
   - 3.8 AP2-MCP returns tx hash, policy decision metadata, and audit reference to the Agent.
4. **Transaction Completion**: Settlement finality is achieved on-chain, and the system generates a complete audit trail.

This flow ensures: "Agent proves identity and user intent first, then executes transaction," enabling truly controlled agentic micropayments.

### Policy Enforcement Rules (Fail-Closed)
The checkout execution model is Policy-Enforced Agentic Checkout via AP2-MCP with mandatory fail-closed controls:
1. Valid mandate scope and signature are required.
2. DID/VC and PKI chain binding checks must pass.
3. Amount caps and merchant allow-list rules must pass.
4. Nonce and intent replay checks must pass.
5. Validity window and policy hash consistency must pass.
6. Any failed check denies execution and emits auditable denial reasons.

---

## 7. FinTech and RegTech Integration
**FinTech Side**:
- **Permissioned Blockchain**: Hyperledger Besu is used as the core ledger, providing high performance, low fee overhead, and enterprise-grade governance.
- **Programmable Policy**: Smart contracts and policy services enforce spending rules.
- **Execution Mediation**: Agent checkout execution is mediated by AP2-MCP; signing is performed by Wallet HSM (ICCHSMatter) and broadcast is performed via approved relay routes.
- **Agentic Interface**: Traditional payment button + dedicated Agentic Payment Button + [payment_skill.md](payment_skill.md) guidance for automated AI-agent execution.
- **Intent Contract Layer**: AP2-style signed mandates provide verifiable user intent, bounded authority, and replay-resistant payment authorization.

**RegTech Side**:
- **Immutable Audit Trail**: To balance compliance and privacy, only necessary hashes/proofs/references are committed on-chain, while sensitive identity evidence is protected in controlled off-chain stores with strict access control.
- **Transaction Monitoring**: Real-time anomaly monitoring based on on-chain and policy telemetry.
- **Policy Decision Evidence**: Every verify/execute call records allow-or-deny decisions, denial reason codes, mandate references, and replay-check outcomes.
- **STR Workflow**: Suspicious activities are automatically flagged and routed into a governed review process before filing/reporting to JFIU as required.
- **Compliance Benefit**: DID + VC provide verifiable identity attributes, reducing repeated KYC costs while supporting AMLO- and PDPO-aligned controls.
- **Audit Benefit**: Mandate hash, signer DID, nonce, and policy hash can be recorded as immutable evidence for dispute handling and forensic reconstruction.

For the **Hosted Layer**, licensing and exemption eligibility (including any low-value treatment) should be confirmed through formal legal analysis and early HKMA engagement. The **didKYC Self-Custody Layer** maintains low-risk positioning through strong DID binding and daily limits.

---

## 8. Overall Feasibility Assessment
- **Technical Feasibility**: High (Besu and DID/VC standards are mature).
- **Regulatory Feasibility**: Medium-high, subject to early regulator engagement and licensing-interpretation confirmation.
- **Commercial Feasibility**: High (innovative didKYC and Agentic Button provide clear differentiation).

**Key Risks and Mitigations**:
- Regulatory interpretation risk -> Engage legal counsel + HKMA sandbox/early consultation.
- Privacy and verification risk -> Use ZKP-enhanced selective disclosure where appropriate.
- Technical security risk -> Third-party security audits + periodic VC/key governance updates.

---

## Appendices
- didKYC CA-Bound Certificate Identity and Policy-Enforced Checkout Architecture:[didKYC_ca_bound_identity_address_verification_architecture.md](didKYC_ca_bound_identity_address_verification_architecture.md)
- Technical architecture diagram: 
[technical_architecture_diagram.png](technical_architecture_diagram.png) (mermaid)
- didKYC agentic payment flow: 
[didkyc_agentic_payment.png](didkyc_agentic_payment.png) (mermaid)
- Payment skill specification: [payment_skill.md](payment_skill.md)
- Risk assessment summary: [risk_assessment.md](risk_assessment.md)
