---
schema: didkyc.risk_assessment.v1
doc_type: risk_register_summary
project_name: Agentic Micropayment Wallet and Checkout
version: "1.1"
date: "2026-06"
machine_readable: true
target_agents:
   - openclaw
   - generic-llm-agent
overall_risk_level: medium_low
---

# Risk Assessment Summary

## Context
```yaml
scope:
	architecture:
		- hosted_custody_on_besu
		- didkyc_self_custody_cold_wallet
		- four_tier_ca_pki_model
		- ap2_mcp_policy_enforced_checkout_control_plane
	key_controls:
		- hybrid_design
		- did_vc_identity_model
		- ap2_style_signed_mandate_model
		- ca_bound_trust_agent_model
		- dual_verification_pki_chain_and_did_vc
		- fail_closed_verify_and_pay_pipeline
		- wallet_hsm_icchsmatter_server_side_signing
		- relay_only_broadcast_enforcement
		- daily_limit_controls
		- permissioned_blockchain_controls
		- short_lived_tier4_session_certs
		- on_chain_revocation_registry
```

## Risk Register
```yaml
risk_register:
	- id: R1
		category: regulatory_and_compliance
		description: hkma_svf_licensing_interpretation_aml_cft_and_did_vc_legal_status
		likelihood: medium
		impact: high
		mitigation:
			- early_hkma_sandbox_or_regulator_engagement
			- fintech_legal_counsel
			- risk_based_approach
			- clear_hosted_vs_self_custody_responsibility_model

	- id: R2
		category: aml_cft
		description: ai_agent_misuse_for_money_laundering_structuring_or_high_frequency_anomalous_transactions_in_agentic_checkout_flows
		likelihood: medium
		impact: high
		mitigation:
			- mandatory_didkyc_and_vc_verification
			- mandatory_ap2_compatible_signed_mandate_verification
			- mandatory_ap2_verify_before_ap2_pay
			- daily_limit_hkd_100
			- ai_transaction_monitoring
			- immutable_evidence_trail_architecture
			- governed_str_workflow

	- id: R3
		category: technical_and_security
		description: deepfake_attacks_did_vc_forgery_private_key_leakage_ca_compromise_control_plane_abuse_and_smart_contract_vulnerabilities
		likelihood: medium
		impact: high
		mitigation:
			- multi_factor_liveness_detection
			- third_party_security_audit
			- mandate_nonce_and_replay_protection
			- mandate_scope_binding_to_wallet_and_agent_did
			- four_tier_ca_hierarchy_with_offline_root_hsm
			- short_lived_tier4_certs_1_to_24h
			- ocsp_crl_revocation_for_tier2_and_tier3
			- on_chain_revocation_registry_besu
			- dual_verification_cert_chain_and_did_vc_binding
			- fail_closed_ap2_mcp_execution_pipeline
			- deny_direct_sign_and_direct_broadcast_paths
			- periodic_key_rotation_all_tiers
			- zkp_enhanced_privacy_controls

	- id: R4
		category: operational
		description: ai_agent_policy_execution_errors_control_plane_outages_wallet_hsm_unavailability_and_relay_failures
		likelihood: low
		impact: medium
		mitigation:
			- programmable_policy_engine_with_human_in_the_loop
			- mandate_revocation_and_supersede_workflow
			- ap2_verify_and_pay_state_machine_controls
			- redundancy_and_failover_design
			- tier4_cert_auto_expiry_limits_blast_radius
			- revocation_automation_on_policy_breach

	- id: R8
		category: pki_and_ca_governance
		description: tier2_issuing_ca_compromise_revocation_propagation_failure_cert_profile_abuse_wallet_hsm_unavailability_or_misconfiguration
		likelihood: low
		impact: high
		mitigation:
			- offline_tier1_root_ca_with_ceremony_controls
			- cloud_hsm_or_tee_for_tier2_signing
			- strict_cert_template_and_policy_oid_enforcement
			- path_length_constraints_on_tier3_wallet_ca
			- ocsp_responder_sla_and_crl_freshness_monitoring
			- on_chain_revocation_registry_for_transparency
			- automated_cert_issuance_rate_abuse_monitoring

	- id: R9
		category: checkout_control_plane
		description: bypass_attempts_of_ap2_mcp_verify_or_ap2_mcp_pay_leading_to_unauthorized_execution
		likelihood: low
		impact: high
		mitigation:
			- mandatory_ap2_verify_mandate_before_pay
			- server_side_enforcement_of_fail_closed_gates
			- deny_and_log_reason_codes_for_all_bypass_attempts
			- immutable_audit_id_linkage_between_verify_and_pay

	- id: R10
		category: relay_and_broadcast
		description: direct_or_unauthorized_broadcast_paths_that_bypass_policy_engine_controls
		likelihood: low
		impact: high
		mitigation:
			- relay_only_broadcast_policy
			- no_direct_eth_sendRawTransaction_from_agent_paths
			- allowlist_approved_relay_endpoints_only
			- relay_health_and_integrity_monitoring

	- id: R5
		category: custody_boundary
		description: blurred_responsibility_between_hosted_and_self_custody_possible_va_custody_interpretation
		likelihood: medium
		impact: high
		mitigation:
			- explicit_legal_documentation
			- genuine_non_custodial_root_key_control_by_user
			- licensed_operator_model_for_gateway_and_hosted_functions

	- id: R6
		category: privacy_and_data_protection
		description: pdpo_non_compliance_or_excessive_data_collection
		likelihood: low
		impact: medium
		mitigation:
			- selective_disclosure_vc
			- data_minimization
			- decentralized_identity_design
			- periodic_privacy_impact_assessment

	- id: R7
		category: market_and_adoption
		description: low_agentic_payment_adoption_or_high_competition
		likelihood: medium
		impact: medium
		mitigation:
			- clear_agentic_button_and_payment_skill_guidance
			- developer_friendly_api
			- go_to_market_partnerships_with_payment_gateways
```

## Assessment Outcome
```yaml
outcome:
	highest_risk_areas:
		- regulatory_compliance
		- aml
		- checkout_control_plane_integrity
	residual_risk: low_to_medium
	monitoring:
		owner: risk_committee
		review_frequency: quarterly
		activities:
			- risk_matrix_review
			- stress_testing
			- verify_pay_path_integrity_testing
```

## Conclusion
```yaml
conclusion:
	statement: overall_risk_within_acceptable_range_given_current_controls
	conditions:
		- continuous_risk_monitoring
		- adaptive_control_updates
		- alignment_with_hong_kong_regulatory_and_security_requirements
		- strict_ap2_mcp_policy_enforced_checkout_operations
```

## Recommended Attachments
```yaml
recommended_attachments:
	- detailed_risk_matrix_excel
	- risk_register
	- third_party_security_audit_report
	- compliance_mapping_table_hkma_amlo
	- verify_pay_control_trace_samples
	- denial_reason_code_catalog
```
