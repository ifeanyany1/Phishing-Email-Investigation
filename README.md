# SOC Phishing Email Investigation

## Project Overview

This project documents a hands-on Security Operations Center (SOC) investigation of a suspicious email impersonating Banco Bradesco / Livelo, a Brazilian financial services and rewards brand.

The objective was to analyze email headers, identify social engineering techniques, decode encoded email content, investigate suspicious URLs, evaluate threat intelligence, and map the observed activity to the MITRE ATT&CK framework.

**Incident Classification:** Likely Phishing  
**Assessment Confidence:** High  
**Investigation Type:** Email Security / Threat Analysis

## Investigation Objectives

- Examine email headers and sender authentication results.
- Identify potential phishing and social engineering indicators.
- Decode Base64-encoded HTML content using CyberChef.
- Extract and safely analyze suspicious URLs.
- Investigate domain and historical IP reputation using VirusTotal.
- Map supported attacker behavior to MITRE ATT&CK.
- Document findings and recommend appropriate incident response actions.

## Tools Used

| Tool | Purpose |
|---|---|
| Notepad++ | Inspect the raw email and headers |
| CyberChef | Decode Base64-encoded HTML |
| VirusTotal | Investigate domain and IP reputation |
| MITRE ATT&CK | Identify and document adversary techniques |

## Investigation Summary

The suspicious email impersonated Banco Bradesco / Livelo and claimed that 92,990 reward points would expire that day.

The message used urgency-based social engineering to encourage the recipient to redeem the points.

Analysis of the encoded HTML revealed an external hyperlink directing the recipient to a domain unrelated to the claimed financial institution.

### Suspicious Indicator

**Defanged URL:** `hxxps://blog1seguimentmydomaine2bra[.]me/`

The URL was not opened during the investigation.

## Email Authentication Findings

The examined email headers contained the following results:

| Authentication Check | Observed Result |
|---|---|
| SPF | temperror |
| DKIM | none |
| DMARC | temperror |

These findings were considered alongside the email content and other indicators. Authentication results alone do not establish whether an email is malicious.

## Threat Intelligence Findings

VirusTotal reported 0/92 security vendor detections for the suspicious domain during the investigation.

Historical passive DNS information identified several IP associations. One historical Cloudflare IP received a phishing detection from one vendor; however, shared infrastructure limits the conclusions that can be drawn from that finding.

A zero-detection reputation score does not guarantee that a domain is safe.

## MITRE ATT&CK Mapping

**Tactic:** Initial Access (TA0001)  
**Technique:** Phishing (T1566)  
**Sub-technique:** Spearphishing Link (T1566.002)

The email contained an external link accompanied by trusted-brand impersonation and urgency-based social engineering.

The mapping describes the observed phishing-link behavior. The investigation did not establish that the message was individually targeted or that the recipient clicked the link.

Reference: https://attack.mitre.org/techniques/T1566/002/

## Final Assessment

**Verdict: Likely Phishing — High Confidence**

The combination of brand impersonation, urgency, and an unrelated external hyperlink strongly suggests an attempted phishing attack.

No successful credential theft, malicious code execution, or system compromise was confirmed.

## Recommended Response

1. Report and quarantine the suspicious email.
2. Avoid interacting with the embedded URL.
3. Search email security logs for related messages.
4. Investigate relevant DNS and proxy logs for attempted access.
5. Preserve evidence and document the investigation.
6. Escalate confirmed malicious activity according to organizational procedures.

## Evidence and Documentation

Supporting investigation screenshots, the detailed incident report, and the MITRE ATT&CK mapping will be organized in this repository.

## Skills Demonstrated

Email Security Analysis | Phishing Triage | Threat Intelligence | Base64 Decoding | Indicator Analysis | MITRE ATT&CK Mapping | Incident Documentation

---

**Disclaimer:** This project is an educational SOC investigation using a publicly sourced phishing sample. No live phishing link interaction or real-world compromise was performed or established.
