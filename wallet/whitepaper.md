# didKYC Agentic Micropayment Wallet
## Agent-Friendly Wallet Technology: A Bridge Between Human-Controlled Wallets and KYC-Enabled Agentic Micropayments

> **Version:** 1.0  
> **Release Date:** May 2026  
> **Author:** Real Matter

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Introduction and Problem Statement](#1-introduction-and-problem-statement)
3. [Business Case](#2-business-case)
4. [Why This Bridge Model](#3-why-this-bridge-model)
5. [Product Architecture](#4-product-architecture)
6. [didKYC Implementation Method (DID + VC Core)](#5-didkyc-implementation-method-did--vc-core)
7. [Agentic Payment Transaction Flow](#6-agentic-payment-transaction-flow)
8. [FinTech and RegTech Integration](#7-fintech-and-regtech-integration)
9. [Overall Feasibility Assessment](#8-overall-feasibility-assessment)
10. [Appendices](#appendices)

---

![didKYC Wallet Cover](Frontpage%20of%20Real%20Matter%20Wallet%20%28Stablecoin%20with%20Agentic%20Micro-Payment%29%20-%202026APR.png)

## Executive Summary
**didKYC Agentic Micropayment Wallet** is an **agent-friendly wallet bridge** that connects a human-controlled wallet model with KYC-enabled agentic micropayment execution. The design combines **DID (Decentralized Identifier)** and **VC (Verifiable Credential)** technologies so AI agents can act autonomously under explicit human and policy boundaries.

The product adopts a **Hybrid Custody** architecture:
- **Hosted Custody Layer**: Based on a permissioned blockchain (Hyperledger Besu), with a maximum stored value of HK$3,000.
- **didKYC Self-Custody Cold Wallet Layer**: Daily transaction cap of HK$100, focused on DID + VC verification.

Before any Agentic Payment is executed, the AI Agent must first complete **didKYC identity verification** (VC verification) with the Payment Gateway. Only after successful verification can it execute on-chain transactions. This bridge design significantly improves autonomy, security, and compliance, and is suitable for high-frequency AI-agent micropayments.

---

## 1. Introduction and Problem Statement
As the AI Agent economy grows rapidly, standalone wallet models and traditional KYC/payment systems can no longer meet the needs of autonomous agentic payments. The core gap is the lack of an operational bridge between human-controlled funds and machine-executed transactions. Key challenges include:
- Traditional centralized KYC creates high data-exposure risk and does not natively support privacy-preserving, machine-verifiable AI Agent identity (including zero-knowledge-proof identity assertions)
- Lack of programmable and verifiable authorization mechanisms
- Difficulty balancing offline operations, compliance auditability, and autonomy

Using W3C DID and VC standards, together with a permissioned blockchain (Besu), **didKYC Agentic Micropayment Wallet** provides a complete Hong Kong-oriented compliance solution centered on an agent-friendly wallet bridge.

---

## 2. Business Case
**Market Opportunity**:  
Agentic Commerce is becoming a mainstream payment trend in 2026. As a fintech hub, Hong Kong has an SVF framework and a DLT-friendly environment, with strong global connectivity.

**Key Use Cases**:
1. **API and AI Service Pay-per-Use**: AI agents pay per request for LLM inference, data queries, or real-time market information.
2. **Recurring Cloud Supply Chain Auto-Payments**: AI agents automatically process recurring fees to cloud providers, such as webcam cloud analytics and storage.
3. **Agent-to-Agent Collaboration Marketplaces**: Different AI agents complete subtasks and settle compensation automatically.
4. **Autonomous Commercial IoT Agents**: Business planning, price comparison, automatic ordering, and payment.
5. **GenAI Content Micropayments**: Paying for single articles, videos, or premium datasets beyond paywalls.

**Business Value**: Low transaction fees with strong recurring-revenue potential.

---

## 3. Why This Bridge Model
This section explains the design choices behind positioning the product as an agent-friendly wallet bridge between human control and KYC-enabled agentic micropayments.

- **Why Hybrid Custody (Risk Management)**  
Hybrid custody separates higher-value storage from autonomous execution. A hosted layer handles larger balances with stronger institutional controls, while a constrained self-custody layer limits blast radius for agent mistakes or compromise. This creates layered risk controls across value tiers.

- **Why Cold Wallet (Human Control)**  
The cold-wallet model preserves clear human ownership over root keys and final authority. Agents do not hold unrestricted owner keys; they operate through policy-constrained delegated signing rights. This keeps automation accountable to human intent and governance.

- **Why didKYC (VC-Verifiable Identity)**  
didKYC uses DID + VC as a verifiable, reusable identity trust layer. Instead of repeatedly exposing full identity records, the system verifies required attributes cryptographically, enabling privacy-aware compliance checks and machine-verifiable authorization.

- **Why Micropayment (Agent-to-Agent Economy)**  
Agent ecosystems increasingly require frequent, low-value settlement across services, APIs, and autonomous workflows. Micropayments fit agent-to-agent collaboration by reducing transaction friction, enabling granular pay-per-action pricing, and supporting programmable recurring value exchange.

---

## 4. Product Architecture
The didKYC Agentic Micropayment Wallet uses a Hybrid Custody + Agent-Friendly Payment Gateway design, specifically for AI Agentic Payments. The core architecture has three parts:

- **Hosted Custody Layer** (based on Hyperledger Besu)  
A permissioned blockchain operated by the Agent-Friendly Payment Gateway provides hosted custody services. Users (Human Owners) can top up via traditional payment methods (such as bank transfer and credit card), up to HK$3,000. Based on preset policies, the system transfers a limited amount daily to the didKYC Self-Custody Cold Wallet.

- **didKYC Self-Custody Cold Wallet Layer**  
A truly non-custodial cold wallet controlled by the user. Daily transaction cap: HK$100. Core capabilities include transaction signing and DID + VC verification to ensure every agent payment is identity-verified before execution.  
For security and custody-boundary clarity, the owner's root private key remains under user control, while the AI Agent uses policy-constrained delegated signing authority (for example, short-lived delegated keys/session keys), not unrestricted custody over the owner's root key.

- **AI Agentic Payment via an Agent-Friendly Payment Gateway**  
The Payment Gateway provides an AI-agent-friendly interface, including:
   - Agentic Payment Button: a WebMCP HTML button with embedded structured and machine-readable attributes.
   - [payment_skill.md](payment_skill.md) instruction document: machine-readable guidance for AI Agents to understand payment flow, API endpoints, and requirements.
   - DID + VC verification channel: the Agent must complete didKYC verification before payment is triggered.
   - Policy Engine: real-time checks for daily limit, merchant whitelist, and other compliance rules.

### Governance Guideline Integration
- **Client-Side Agent Machine Governance**: The agent runtime must load and apply [risk_assessment.md](risk_assessment.md) as a policy baseline before initiating payment actions. At minimum, it must enforce risk controls related to daily spending caps, merchant allow-list behavior, delegated key usage constraints, and escalation paths for suspicious patterns.
- **Gateway-Side Governance**: The Payment Gateway control plane and policy engine must use [risk_assessment.md](risk_assessment.md) as the canonical risk-control reference for verification, monitoring, STR workflow routing, and periodic control reviews. Gateway decisions should be traceable to the relevant risk category and mitigation controls in the document.

This Agent-Friendly Payment Gateway enables AI Agents to "browse" and complete payments like humans, while maintaining full auditability and compliance.  
The two custody layers are connected seamlessly through a unified DID mechanism and Agent-Friendly Gateway, enabling layered risk management (higher-value hosted custody + low-value autonomous agent payments).

---

## 5. didKYC Implementation Method (DID + VC Core)
**didKYC** is the product's innovative identity system:
- After the user (Human Owner) completes initial eKYC, the system issues **Verifiable Credentials (VCs)** bound to the user's **Decentralized Identifier (DID)**.
- The didKYC Cold Wallet stores DID Document references and VC materials required for verification.
- The AI Agent holds the corresponding DID-based delegated authorization for policy-bounded payment execution.

**Advantages**:
- Decentralized and privacy-preserving (Selective Disclosure)
- Cross-platform verifiability without repeatedly submitting identity data
- Aligned with HKMA's direction on digital identity and DLT development

---

## 6. Agentic Payment Transaction Flow
1. **User Top-Up**: Funds are topped up through traditional payment methods into Hosted Custody (Besu).
2. **Daily Authorized Transfer**: According to policy, the system transfers a limited amount daily (<= HK$100) into the didKYC Self-Custody Cold Wallet.
3. **AI Agent Initiates Payment**:
   - 3.1 The Agent connects to the Payment Gateway's **Agentic Payment Button** (special HTML code).
   - 3.2 The Agent automatically reads the [payment_skill.md](payment_skill.md) guide to understand payment protocol requirements.
   - 3.3 The Agent first submits **DID + VC verification** (didKYC verification) to the Payment Gateway.
   - 3.4 The Payment Gateway validates VC effectiveness and policy conformance (daily limit, merchant whitelist, etc.).
   - 3.5 Once verification passes, the Agent signs the transaction using policy-constrained delegated signing authority under the Cold Wallet control model, then submits to Besu.
4. **Transaction Completion**: Settlement finality is achieved on-chain, and the system generates a complete audit trail.

This flow ensures: "Agent verifies identity first, then executes transaction," enabling truly controlled agentic micropayments.

---

## 7. FinTech and RegTech Integration
**FinTech Side**:
- **Permissioned Blockchain**: Hyperledger Besu is used as the core ledger, providing high performance, low fee overhead, and enterprise-grade governance.
- **Programmable Policy**: Smart contracts and policy services enforce spending rules.
- **Offline + Online Hybrid**: The Cold Wallet supports offline signing, followed by broadcast.
- **Agentic Interface**: Traditional payment button + dedicated Agentic Payment Button + [payment_skill.md](payment_skill.md) guidance for automated AI-agent execution.

**RegTech Side**:
- **Immutable Audit Trail**: To balance compliance and privacy, only necessary hashes/proofs/references are committed on-chain, while sensitive identity evidence is protected in controlled off-chain stores with strict access control.
- **Transaction Monitoring**: Real-time anomaly monitoring based on on-chain and policy telemetry.
- **STR Workflow**: Suspicious activities are automatically flagged and routed into a governed review process before filing/reporting to JFIU as required.
- **Compliance Benefit**: DID + VC provide verifiable identity attributes, reducing repeated KYC costs while supporting AMLO- and PDPO-aligned controls.

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
- Technical architecture diagram (SVG): [technical_architecture_diagram.svg](technical_architecture_diagram.svg) (Mermaid): [technical_architecture_diagram.mmd](technical_architecture_diagram.mmd)
- DID/VC flow diagram (SVG): [did_vc_flow_diagram.svg](did_vc_flow_diagram.svg) (Mermaid): [did_vc_flow_diagram.mmd](did_vc_flow_diagram.mmd)
- Payment skill specification: [payment_skill.md](payment_skill.md)
- Risk assessment summary: [risk_assessment.md](risk_assessment.md)
