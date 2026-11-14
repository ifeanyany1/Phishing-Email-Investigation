# MITRE ATT&CK Mapping — Banco Bradesco Phishing Investigation

## 1. Technique Identification

**Framework:** MITRE ATT&CK Enterprise

**Tactic:** Initial Access (TA0001)

**Technique:** Phishing (T1566)

**Sub-technique:** Spearphishing Link (T1566.002)

**Mapping Confidence:** High for observed phishing-link behavior

## 2. Observed Suspicious Activity

During the investigation, a suspicious email impersonating Banco Bradesco / Livelo was analyzed.

The message claimed that the recipient had 92,990 reward points expiring that day.

The email used urgency-based social engineering to encourage the recipient to redeem the points immediately.

Analysis of the Base64-encoded HTML body using CyberChef revealed an external URL:

`hxxps://blog1seguimentmydomaine2bra[.]me/`

The destination domain did not visibly correspond to the impersonated financial institution.

## 3. Mapping Justification

The observed activity is consistent with MITRE ATT&CK T1566.002 — Spearphishing Link.

The mapping is supported by:

- Impersonation of a trusted financial institution.
- Urgency-based social engineering.
- An embedded external hyperlink.
- A reward-redemption call to action encouraging recipient interaction.

The link-based delivery distinguishes this behavior from T1566.001 — Spearphishing Attachment.

## 4. Investigation Limitations

The investigation did not establish whether the email was individually targeted or distributed broadly.

There was no evidence confirming that the recipient clicked the link, submitted credentials, downloaded malware, or experienced a successful compromise.

Therefore, subsequent attacker behaviors, including T1204.001 — User Execution: Malicious Link, were not established.

## 5. Recommended Defensive Actions

- Report and quarantine the suspected phishing email.
- Investigate related messages in email security logs.
- Monitor relevant DNS and web proxy activity for the suspicious destination.
- Preserve email headers and extracted indicators.
- Escalate confirmed malicious activity according to organizational incident response procedures.

## 6. Supporting Evidence

- Original phishing email and header analysis.
- CyberChef Base64 decoding results.
- Extracted suspicious URL.
- VirusTotal domain reputation findings.
- MITRE ATT&CK technique reference screenshot.

## 7. MITRE ATT&CK Reference

https://attack.mitre.org/techniques/T1566/002/

**Assessment:** Likely phishing based on observed email content and social engineering indicators. No successful compromise was confirmed.
