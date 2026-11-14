# SOC Incident Investigation Report

**Case ID:** SOC-PHISH-001  
**Incident Type:** Suspected Email Phishing  
**Classification:** Likely Phishing  
**Assessment Confidence:** High  
**Investigation Environment:** Controlled SOC Analysis Lab  
**Status:** Analysis Completed — No Confirmed Compromise

---

## 1. Executive Summary

This report documents an investigation into a suspicious email impersonating Banco Bradesco / Livelo, a Brazilian financial services and rewards brand.

The email claimed that the recipient had 92,990 reward points that were about to expire and encouraged immediate redemption.

The investigation identified several phishing indicators, including trusted-brand impersonation, urgency-based social engineering, and an external hyperlink directing the recipient to a domain unrelated to the claimed institution.

The email's Base64-encoded HTML body was decoded using CyberChef, allowing the embedded URL to be identified and investigated using VirusTotal.

**Final Assessment: Likely Phishing — High Confidence.**

No successful credential theft, malicious code execution, or system compromise was confirmed.

## 2. Investigation Objectives

The objectives were to:

- Examine the suspicious email and its headers.
- Evaluate sender authentication results.
- Identify social engineering indicators.
- Decode Base64-encoded HTML content.
- Extract suspicious URLs without visiting them.
- Investigate domain and historical IP reputation.
- Map supported behavior to MITRE ATT&CK.
- Recommend appropriate defensive actions.

## 3. Tools Used

| Tool | Investigation Purpose |
|---|---|
| Notepad++ | Raw email and header inspection |
| CyberChef | Base64 decoding |
| MXToolbox | Email header analysis |
| VirusTotal | Domain and IP reputation investigation |
| MITRE ATT&CK | Adversary technique mapping |
| GitHub | Evidence preservation and documentation |

## 4. Email Triage and Social Engineering Analysis

The investigated message claimed to originate from Banco Bradesco / Livelo.

**Observed pretext:** Expiring reward points.

**Claimed reward balance:** 92,990 points.

**Requested action:** Redeem the points through a hyperlink.

### Social Engineering Indicators

**Brand impersonation:** The sender presented the message as a notification associated with a trusted financial institution.

**Urgency:** The claim that reward points would expire that day encouraged immediate action.

**Financial incentive:** The reward-points offer was designed to attract the recipient's attention.

**External redirection:** The redemption link pointed to a domain that did not visibly correspond to the impersonated brand.

These indicators are consistent with a phishing attempt.

### Supporting Evidence

[Email Sample Screenshot](../Evidence/02-Malicious%20email%20sample%20for%20analysis.png)

## 5. Email Header Authentication Analysis

The email headers were inspected for authentication results.

| Security Control | Observed Result | Interpretation |
|---|---|---|
| SPF | temperror | Temporary error prevented a conclusive SPF evaluation |
| DKIM | none | No DKIM signature was identified in the reported result |
| DMARC | temperror | Temporary error prevented a conclusive DMARC evaluation |

The authentication results did not independently establish maliciousness.

However, they provided useful context when considered alongside the email's social engineering content and external destination.

### Supporting Evidence

[Email Header Analysis](../Evidence/03-email-header-analysis.png)

[MXToolbox Header Analysis](../Evidence/04-mxtoolbox-header-analysis.png)

## 6. Base64 Decoding and URL Extraction

Inspection of the email source revealed Base64-encoded HTML content.

CyberChef was used to decode the content for further examination.

The decoded HTML contained an external hyperlink associated with the reward-redemption message.

**Extracted URL (defanged):**

`hxxps://blog1seguimentmydomaine2bra[.]me/`

### Analysis

The URL was considered suspicious because:

- It appeared in a message impersonating a financial institution.
- The destination domain did not visibly correspond to Banco Bradesco or Livelo.
- The surrounding email content encouraged urgent interaction.

Base64 encoding alone was not treated as proof of malicious intent because it is also commonly used in legitimate MIME email messages.

The suspicious destination was not visited during the investigation.

### Supporting Evidence

[Base64 Decoding and Extracted URL](../Evidence/03-base64-decoded-phishing-url.png)

## 7. Threat Intelligence Investigation

### 7.1 Domain Reputation

The suspicious domain was investigated using VirusTotal.

**Observed result:** 0/92 security vendors flagged the domain during the investigation.

This result was not interpreted as proof that the destination was safe.

Threat intelligence detections may be incomplete, and phishing infrastructure can remain undetected by reputation-based tools.

### Supporting Evidence

[VirusTotal Domain Analysis](../Evidence/06-virustotal-domain-analysis.png)

### 7.2 Historical IP Reputation

VirusTotal passive DNS records associated the investigated domain with several historical IP addresses.

One historical association was:

`172.67.201.113`

VirusTotal reported 1/92 vendor detections for this IP, with Gridinsoft identifying it as phishing-related.

The address was associated with Cloudflare infrastructure.

Because Cloudflare IP addresses may serve multiple unrelated websites, the IP detection was not treated as definitive proof that the investigated domain was malicious.

The association was historical and did not establish the domain's current hosting address.

### Supporting Evidence

[VirusTotal IP Reputation](../Evidence/06-virustotal-ip-reputation.png)

### 7.3 Passive DNS Analysis

Passive DNS information was reviewed to identify historical domain-to-IP relationships.

These records provided infrastructure context but did not establish shared ownership, attacker control, or malicious coordination between domains using the same IP address.

### Supporting Evidence

[Passive DNS Relationships](../Evidence/07-passive-dns-relations.png)

## 8. MITRE ATT&CK Mapping

**Tactic:** Initial Access (TA0001)

**Technique:** Phishing (T1566)

**Sub-technique:** Spearphishing Link (T1566.002)

### Mapping Justification

The suspicious message used trusted-brand impersonation, an urgent reward-redemption pretext, and an embedded external hyperlink to encourage recipient interaction.

This observed behavior is consistent with MITRE ATT&CK T1566.002.

The technique was selected instead of T1566.001 — Spearphishing Attachment because the investigated delivery mechanism involved a hyperlink rather than an identified malicious attachment.

The available evidence did not establish whether the message was individually targeted or distributed broadly.

No link click, credential submission, malware execution, or successful compromise was confirmed.

### Supporting Evidence

[MITRE ATT&CK T1566.002 Reference](../Evidence/08-MITRE-ATTACK-T1566-002.png)

[Detailed MITRE ATT&CK Mapping](../MITRE-ATTACK-Mapping.md)

## 9. Indicators of Interest

| Indicator Type | Observed Indicator | Assessment |
|---|---|---|
| Claimed brand | Banco Bradesco / Livelo | Impersonation pretext |
| URL | `hxxps://blog1seguimentmydomaine2bra[.]me/` | Suspicious phishing destination |
| Historical IP | `172.67.201.113` | Historical association; shared Cloudflare infrastructure |
| Social engineering | Expiring reward points | Urgency and incentive |
| Email body | Base64-encoded HTML | Decoded for analysis; not independently malicious |

Indicators should be interpreted in context and validated before applying network-wide blocking decisions.

## 10. Final Assessment

**Verdict: Likely Phishing**

**Confidence: High**

The assessment is supported by the combination of:

- Trusted financial-brand impersonation.
- Urgency-based reward-redemption messaging.
- An embedded external destination unrelated to the claimed brand.
- Supporting email and threat intelligence investigation findings.

The available evidence supports classification as a likely phishing attempt.

**No successful compromise was established.**

## 11. Recommended Incident Response Actions

1. Report and quarantine the suspected phishing email.
2. Avoid visiting the suspicious destination.
3. Search the organization's email security logs for related messages.
4. Review relevant DNS and proxy logs for attempted access.
5. Investigate any confirmed recipient interaction.
6. Preserve email headers, extracted indicators, and investigation evidence.
7. Escalate confirmed malicious activity according to organizational procedures.
8. Reinforce user awareness regarding financial-brand impersonation and urgent reward notifications.

## 12. Lessons Learned

This investigation demonstrated the importance of correlating multiple evidence sources rather than relying on a single security vendor's reputation score.

It also reinforced the distinction between observed attacker behavior and unconfirmed outcomes.

The investigation provided hands-on experience in phishing triage, email header analysis, Base64 decoding, threat intelligence, passive DNS interpretation, MITRE ATT&CK mapping, and incident reporting.

## 13. Investigation Conclusion

The suspicious Banco Bradesco / Livelo email was assessed as likely phishing based on the available evidence.

The observed phishing-link behavior was mapped to MITRE ATT&CK T1566.002 under the Initial Access tactic.

No evidence confirmed that the recipient interacted with the suspicious destination or that a successful compromise occurred.

**Case Status:** Analysis completed; no confirmed compromise.

---

## References

MITRE ATT&CK — T1566.002: Spearphishing Link  
https://attack.mitre.org/techniques/T1566/002/

VirusTotal  
https://www.virustotal.com/

CyberChef  
https://gchq.github.io/CyberChef/

**Portfolio Disclaimer:** This report documents an educational investigation of a publicly sourced phishing email sample. Findings are limited to the evidence examined and do not represent a confirmed real-world security breach.
