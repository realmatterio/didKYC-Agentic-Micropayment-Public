---
schema: didkyc.payment_skill.v1
doc_type: agent_payment_protocol
title: Agentic Payment Instructions (didKYC Compatible)
version: "1.0"
last_updated: "2026-05-01"
machine_readable: true
target_agents:
   - openclaw
   - generic-llm-agent
protocol:
   identity: didkyc
   ledger: hyperledger-besu-permissioned
currency_default: HKD
policy_defaults:
   daily_limit_hkd: 100
   requires_did_vc_verification: true
---

# Agentic Payment Skill

## Intent
Machine-readable instructions for AI Agents to execute didKYC micropayments safely and in policy order.

## Required Inputs
```yaml
required_inputs:
   did_document: string
   vc_jwt: string
   amount_hkd: number
   merchant_address: string
optional_inputs:
   memo: string
```

## Authentication Requirements
```yaml
authentication:
   must_present:
      - did
      - verifiable_credential
   vc_constraints:
      issuer: payment_gateway
      must_include:
         - human_owner_did
         - expiration_date
         - spending_policy
      policy_rules:
         daily_limit_hkd: 100
```

## API Endpoints
```yaml
endpoints:
   verify_did_vc:
      method: POST
      path: /api/didkyc/verify
      request:
         - did_document
         - vc_jwt
      success:
         http_status: 200
         returns:
            - session_token
   submit_besu_transaction:
      method: POST
      path: /api/besu/submit
      success:
         settlement: on_chain_finality
         chain: hyperledger-besu-permissioned
```

## Deterministic Payment Flow
```yaml
payment_flow:
   - step: 1
      name: did_vc_verification
      action: call_verify_did_vc_endpoint
      halt_on_failure: true
   - step: 2
      name: policy_check
      checks:
         - daily_limit
         - merchant_whitelist
         - transaction_amount
      halt_on_failure: true
   - step: 3
      name: transaction_preparation
      fields:
         amount: amount_hkd
         currency: HKD
         recipient: merchant_address
         memo: optional
   - step: 4
      name: signature
      mode: delegated_policy_constrained_signing
      supports_offline_signing: true
   - step: 5
      name: submit_transaction
      action: call_submit_besu_transaction_endpoint
```

## Error Map
```yaml
errors:
   "401": invalid_did_or_vc
   "402": payment_required_x402_compatible
   "429": daily_limit_exceeded
```

## Agentic Button Example
```yaml
agentic_button:
   data_agentic: true
   data_amount: "45.00"
   data_currency: HKD
   data_merchant: cloud-gpu-provider
   action: initAgenticPayment
```

## Supported Use Cases
```yaml
supported_use_cases:
   - gpu_token_purchase
   - api_pay_per_use
   - content_micropayment
```
