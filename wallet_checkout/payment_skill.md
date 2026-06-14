---
schema: didkyc.payment_skill.v1
doc_type: agent_payment_protocol
title: Agentic Payment Instructions (Policy-Enforced AP2-MCP Checkout)
version: "1.3"
last_updated: "2026-06-14"
machine_readable: true
target_agents:
   - openclaw
   - generic-llm-agent
protocol:
   identity: didkyc
   trust_model: ca_bound_4tier
   ledger: hyperledger-besu-permissioned
currency_default: HKD
policy_defaults:
   daily_limit_hkd: 100
   requires_did_vc_verification: true
   requires_cert_chain_verification: true
   execution_mode: ap2_mcp_fail_closed
---

# Agentic Payment Skill

## Intent
Machine-readable instructions for AI Agents to execute didKYC micropayments safely and in policy order.

## Required Inputs
```yaml
required_inputs:
   wallet_did: string
   agent_did: string
   tier4_cert_chain_pem: string
   did_proof: string
   vc_presentation: string
   mandate_id: string
   mandate_signature: string
   policy_hash: string
   amount_hkd: number
   merchant_id_or_did: string
   nonce: string
optional_inputs:
   memo: string
   intent_id: string
```

## Authentication Requirements
```yaml
authentication:
   must_present:
      - tier4_session_cert_chain
      - did
      - verifiable_credential
      - ap2_compatible_signed_mandate
      - proof_of_possession
   cert_constraints:
      chain_depth: 4
      tier4_max_lifetime_hours: 24
      key_usage: digital_signature
   vc_constraints:
      issuer: payment_gateway_ca_trusted
      must_include:
         - human_owner_did
         - expiration_date
         - spending_policy
         - issuer_ca_chain_hash
      policy_rules:
         daily_limit_hkd: 100
```

## API Endpoints
```yaml
tool_surface:
   verify_mandate:
      tool: ap2.verify_mandate
      purpose: preflight_verification_without_signing_or_broadcast
      request:
         - wallet_did
         - agent_did
         - tier4_cert_chain_pem
         - did_proof
         - vc_presentation
         - mandate_id
         - mandate_signature
         - policy_hash
         - amount_hkd
         - merchant_id_or_did
         - nonce
      success:
         returns:
            - decision_allow_or_deny
            - reason_codes
            - audit_id
            - request_id
   pay:
      tool: ap2.pay
      purpose: verify_sign_and_broadcast_via_ap2_mcp
      request:
         - all_verify_mandate_fields
      success:
         returns:
            - decision_allow_or_deny
            - reason_codes
            - audit_id
            - request_id
            - tx_hash_on_success
```

## Deterministic Payment Flow
```yaml
payment_flow:
   - step: 1
      name: cert_chain_verification
      action: validate_tier4_to_tier1_cert_chain
      checks:
         - chain_path_validity
         - cert_profile_and_policy_oid
         - revocation_status_ocsp_or_crl
         - tier4_lifetime_within_policy
      halt_on_failure: true
   - step: 2
      name: dual_did_vc_verification
      action: execute_ap2_verify_mandate
      checks:
         - did_resolution_and_key_alignment
         - vc_signature_and_issuer_trust
         - vc_credential_status
         - binding_did_key_equals_cert_key
         - binding_wallet_address_equals_tx_sender
      halt_on_failure: true
   - step: 3
      name: ap2_mandate_verification
      action: verify_signed_mandate
      checks:
         - mandate_id_present
         - mandate_signature_valid
         - mandate_wallet_binding
         - mandate_agent_binding
         - mandate_nonce_or_replay_protection
         - mandate_policy_hash_match
      halt_on_failure: true
   - step: 4
      name: policy_check
      checks:
         - daily_limit
         - merchant_whitelist
         - transaction_amount
         - mandate_constraints
         - delegated_scope
      halt_on_failure: true
   - step: 5
      name: transaction_preparation
      fields:
         amount: amount_hkd
         currency: HKD
         recipient: merchant_id_or_did
         memo: optional
   - step: 6
      name: execution_request
      action: execute_ap2_pay
      checks:
         - ap2_revalidation_fail_closed
         - wallet_hsm_icchsmatter_server_side_signing
         - besu_relay_broadcast_only
   - step: 7
      name: receipt_validation
      action: verify_tx_hash_and_audit_reference
```

## Trust Boundary Rules
```yaml
trust_boundary:
   forbidden_direct_calls:
      - wallet_sign_transaction_endpoint
      - besu_eth_sendRawTransaction
   secret_material_never_provided_to_agent:
      - hsm_slot
      - hsm_pin
      - private_key_material
   required_execution_path:
      - ap2.verify_mandate
      - ap2.pay
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

## Mockup Procedure: Agentic Shop and MCP Checkout

This section defines a UI-level mockup procedure for a demo web app named **Agentic Shop and MCP Checkout**.

### Shop Context (Front Page)
```yaml
shop_front_page:
   app_name: Agentic Shop and MCP Checkout
   currency: HKD
   price_constraint: amount_must_be_less_than_hkd_1
   layout:
      upper_half:
         carousel:
            items:
               - llm_token
               - gpu_time
               - stock_tips
               - micro_insurance
            controls:
               - slide_left_arrow
               - slide_right_arrow
      lower_half:
         embedded_webmcp_agent_context:
            default_state: fully_open
            has_collapse_expand_toggle: true
            parser_tree_links: true
            shows:
               - active_item_context
               - checkout_payload_preview
               - payment_skill_summary
```

### Checkout URL Action
```yaml
checkout_action:
   trigger: checkout_button_click
   method: open_checkout_popup_via_checkout_url
   url_pattern: /checkout?item={item_id}&amountCents={amount_cents}
```

### Human-Pay Mockup Flow
```yaml
human_pay_flow:
   fields:
      - amount_display_readonly
      - visa_card_number
      - card_holder_name
      - expiry_date
   excluded_fields:
      - cvv
      - cvc
   on_click_human_pay:
      - show_processing_status
      - emulate_visa_transaction_seconds: 5
      - show_success_green_tick
      - show_payment_details
      - auto_return_to_shop_seconds: 10
   manual_exit:
      - top_right_x_close_button
```

### Agentic-Pay Mockup Flow
```yaml
agentic_pay_flow:
   fields:
      - amount_display_readonly
      - checkout_session_nonce
      - agent_did_number
      - user_wallet_signature_over_nonce
      - ap2_mandate_id
      - policy_hash
      - ap2_mandate_signature
   on_click_agentic_pay:
      - present_shop_transaction_request:
           chain: besu
           chain_id: besu_chain_id
           recipient: merchant_recipient_address
           amount: amount_cents
      - open_review_window_seconds: 15
      - show_button: challenge_settlement_with_countdown
      - show_button: extend_review_window_plus_60s
      - on_extend_reset_countdown_seconds: 60
      - on_countdown_timeout:
           - emulate_settlement_processing_seconds: 10
           - show_success_green_tick
           - show_payment_details
           - auto_return_to_shop_seconds: 10
      - on_challenge_click:
           - pause_settlement
           - show_manual_follow_up_status
   security_notes:
      - execution_via_ap2_mcp_only
      - no_direct_agent_signing_or_broadcast
      - wallet_hsm_icchsmatter_signing_is_server_side_only
   manual_exit:
      - top_right_x_close_button
```

### Agent Guidance Summary
```yaml
agent_guidance:
   objective:
      - buy_item
      - perform_agentic_checkout
      - complete_agentic_pay
   required_sequence:
      - read_active_item_and_amount
      - open_checkout_url
      - read_checkout_nonce
      - collect_did_and_wallet_signature
      - collect_mandate_id_policy_hash_and_signature
      - validate_mandate_fields_present
      - call_ap2_verify_mandate
      - if_allow_call_ap2_pay
      - confirm_shop_transaction_request
      - verify_success_receipt
```
