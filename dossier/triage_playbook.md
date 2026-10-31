# Phishing Alert Triage & Response Playbook

> **Defensive triage guidance for dataset record KAG-CEAS08-001.** The selected Kaggle `CEAS_08.csv` row is labeled spam and contains a URL, but does not include raw headers, live reputation results, attachments, or incident telemetry. The response sequence below is guidance, not evidence that any containment action occurred. Validate any indicator independently before production use.

## 1. Purpose and operating principles

Provide a repeatable method to prioritize suspicious email, scope a campaign, contain confirmed messages, protect affected identities, and document recovery. Correlate signals rather than treating one failed authentication check as proof. Preserve evidence, use least-privilege access, and require human approval for disruptive identity or endpoint actions.

## 2. Alert intake and triage

### Minimum alert fields

Capture alert ID, detection time and timezone, original message ID, trusted gateway authentication results, sender and envelope fields, recipient list, subject, URLs (defanged), attachment metadata and hash (only if actually computed), mail disposition, user report, and linked identity/endpoint alerts.

### Triage sequence

1. **Validate provenance.** Obtain the original message from the mail system and identify which `Received` and `Authentication-Results` fields were added by trusted infrastructure.
2. **Classify the message.** Examine sender alignment, reply-to mismatch, language and requested action, URL and attachment metadata, and known-good business context. Do not open a suspicious file on a production endpoint.
3. **Scope.** Search mail telemetry by message ID first, then normalized sender, URL, attachment name/hash, and subject. Record delivered, quarantined, clicked, and reported counts separately.
4. **Correlate.** Review secure web gateway, DNS, identity provider, endpoint detection, and mailbox audit records. Establish whether a user clicked, submitted credentials, approved an MFA prompt, or executed a file.
5. **Choose severity and owner.** Escalate suspected credential disclosure, privileged accounts, broad distribution, confirmed execution, or suspicious post-click authentication.
6. **Contain and document.** Apply only validated actions, capture approval and execution results, and retain an auditable event trail.

## 3. Prioritization model

Use the score to support analyst judgment; it does not replace incident policy. Score the incident from **0–100**, cap at 100, and record the reason for every factor.

| Signal | Points |
|---|---:|
| Trusted gateway reports DMARC fail or aligned SPF/DKIM failure plus other suspicious evidence | +15 |
| Credential or payment lure, impersonation, or unusual urgency | +15 |
| Suspicious URL, redirect chain, or attachment confirmed by analysis | +20 |
| Multiple recipients or campaign recurrence | +10 |
| Confirmed click or attachment open | +15 |
| Credential submission, MFA approval, or suspicious identity event | +25 |
| Confirmed endpoint execution or persistence | +30 |
| Verified known-good sender, expected content, and business context | −20 |

**Suggested bands:** 0–24 Low (queue and enrich); 25–49 Medium (analyst investigation); 50–74 High (prompt containment and incident owner); 75–100 Critical (immediate incident command and coordinated response). A confirmed compromise, privileged identity, or material business impact can require escalation regardless of score.

For the demo case, the phishing lure, authentication anomalies, campaign scope, and reported credential submission justify **High / P2** pending identity review. Do not infer endpoint compromise from email evidence alone.

## 4. SIEM detection and correlation guidance

Build detections around normalized, auditable signals:

- Correlate trusted mail-gateway SPF/DKIM/DMARC outcomes with sender display-name mismatch, external reply-to, newly observed sender, suspicious URL category, or attachment risk.
- Group similar messages by normalized URL, attachment digest (only when genuinely computed), sender infrastructure, and time window; deduplicate alert floods into a campaign case.
- Link mail delivery and URL click telemetry to identity sign-in and endpoint events for the same recipient and time window.
- Exclude trusted internal relays and known forwarding behavior only through maintained, reviewed allowlists. Avoid suppressing alerts solely because SPF passes or a sender is familiar.
- Keep original event identifiers, source, UTC timestamp, rule version, and enrichment provenance for auditability.

## 5. SOAR workflow and approval gates

| Stage | Automated preparation | Analyst / approval gate |
|---|---|---|
| Enrich | Retrieve message metadata, resolve recipient scope from mail telemetry, normalize indicators, and attach reputation results. | Confirm evidence sources are trusted; treat third-party reputation as supporting evidence only. |
| Campaign scope | Search for matching messages and clicks; group related alerts and prepare recipient list. | Review query scope and false-positive risk before bulk action. |
| Mail containment | Prepare a quarantine or purge action for exact, validated message identifiers. | Approve broad purge; preserve a controlled evidence copy and record affected users. |
| Identity protection | Flag potentially exposed users and gather sign-in/session context. | For credible exposure, approve credential reset and session/token revocation; follow privileged-account policy. |
| Network controls | Draft scoped URL/domain block candidates and expiry metadata. | Validate ownership, business impact, scope, and indicator provenance before enforcement. Do not block documentation examples. |
| Endpoint response | Collect EDR status and prepare an isolation recommendation when evidence supports compromise. | Require incident-owner approval for isolation unless pre-approved emergency policy applies. Preserve collection and recovery path. |
| Recovery & closure | Assemble checklist, notification draft, evidence links, and metrics. | Confirm remediation, user communication, monitoring period, and closure criteria. |

SOAR must use least-privilege service identities, approval checks, idempotent actions, rate limits, explicit error reporting, audit logs, and tested rollback procedures. An unavailable enrichment source must be recorded as unavailable, not treated as a clean result.

## 6. Containment and remediation checklist

### Email

- Preserve the original message, trusted headers, gateway verdict, message IDs, and collection metadata.
- Search the tenant for the exact campaign; quarantine confirmed messages and verify the resulting counts.
- Defang and scope URLs for investigation. Apply temporary mail/web controls only to validated live indicators, with an owner and review/expiry date.

### Identity

- Ask whether the recipient clicked, submitted credentials, approved an MFA request, or reused the password elsewhere.
- Review sign-ins, MFA changes, session creation, device registrations, OAuth/app grants, and mailbox rules around the interaction.
- For credible credential exposure, reset credentials, revoke sessions and refresh tokens through approved procedures, require MFA re-verification, and monitor for re-entry. Escalate privileged identities.
- Remove unauthorized mailbox forwarding or rules only after preserving their details and confirming ownership.

### Endpoint

- Correlate click/open time with EDR, browser, DNS, and process telemetry. Email alone is not proof of execution.
- Isolate an endpoint only when evidence and policy justify it; ensure the action is approved and recovery is available.
- Collect artifacts using approved forensic procedures; eradicate confirmed payloads or persistence, patch the exploited control, and validate recovery before reconnecting.

### Recovery and communication

- Notify affected users with clear instructions and a safe reporting route; avoid exposing other recipients.
- Monitor for repeat delivery, suspicious authentication, mailbox-rule changes, and related endpoint indicators.
- Record scope, decisions, approvals, action outcomes, false positives, and residual risk. Close only when the closure criteria in the incident timeline are met.

## 7. Metrics and review

Track time to acknowledge, time to scope, time to quarantine, number of delivered/removed messages, confirmed clicks and credential submissions, affected accounts, false-positive rate, and approval/action failures. Review detection quality and playbook outcomes after exercises or incidents; update rules and documentation through change control.

## 8. Safety and data-quality notes

The companion project files are static mock data. The example hash is synthetic, the payload is not included, reserved domains are non-real, and documentation IPs are not malicious infrastructure. Do not add these demo values to a production blocklist or present them as verified threat intelligence.
