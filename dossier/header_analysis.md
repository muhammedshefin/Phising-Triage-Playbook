# Dataset Email Forensic Analysis

> **Evidence source:** Kaggle, [Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset), file `CEAS_08.csv`, dataset preview record shown below. This is a historical, dataset-labeled spam record, not a raw `.eml` message. The dataset preview supplies normalized sender, recipient, date, subject, body, label, and URL fields only. It does not include full transport headers or an attachment.

## Executive assessment

The selected record is labeled `1` in the CEAS dataset and contains one URL in its message body. Its subject is `Your order` and the body advertises celebrity content. This supports describing it as a spam-labeled unsolicited message with an embedded URL; it does **not** by itself establish that the URL is currently malicious, that credentials were harvested, or that a user was compromised.

The supplied dataset record is insufficient for a real transport-header or payload forensic determination. SPF, DKIM, DMARC, message hops, sending IP addresses, attachments, and an attachment hash are not present. Their statuses are **unknown / not provided**, not pass or fail.

## Exact record evidence

| Dataset field | Value |
|---|---|
| Dataset file | `CEAS_08.csv` |
| Sender | `Linwood Sloan <Linwood@goline.ca>` |
| Recipient | `user7-ext4@gvc.ceas-challenge.cc` |
| Date field | `Tue, 05 Aug 2008 20:27:34 -0300` |
| Subject | `Your order` |
| Body excerpt | `Britney spears p0rn video, Jennifer Lopez pictures ... http://somateco.com.br/folderz/ready.php` |
| Dataset `label` | `1` (spam-labeled under the dataset's spam/legitimate classification) |
| Dataset `urls` | `1` |

The preview does not provide a stable row identifier or the complete body in a machine-readable export. Do not treat the excerpt as the complete original message.

## Header and hop analysis

The CSV `sender` and `receiver` columns are dataset fields, not proof of the original RFC 5322 `From`, `To`, or envelope identities. The record does not show `Return-Path`, `Reply-To`, `Message-ID`, `Received`, `Authentication-Results`, or any other raw header lines.

| Check | Finding | Reason |
|---|---|---|
| SPF | Not provided / unknown | No trusted receiver `Authentication-Results` or envelope sender details |
| DKIM | Not provided / unknown | No `DKIM-Signature` or verifier result |
| DMARC | Not provided / unknown | No receiver result or policy/alignment data |
| Received hops | Not available | No raw `Received` header chain |
| Sending / relay IP | Not available | No IP is present in the selected record |

No sender IP, relay IP, or hop order can be responsibly inferred from the sender email domain or from the URL.

## URL and domain extraction

The body excerpt contains the literal URL `http://somateco.com.br/folderz/ready.php`. For safe display, it is defanged as `hxxp://somateco[.]com[.]br/folderz/ready[.]php`; the host is `somateco.com.br`.

The dataset's `urls` value of `1` is consistent with the body excerpt. The dataset preview does not establish a DNS resolution, URL reputation, historical ownership, redirect chain, server IP, or present-day malicious status. Do not visit the URL from a production environment.

`goline.ca` is the domain in the sender field. This single record does not establish that the sender domain is malicious, so it is recorded as sender context rather than a malicious IOC.

## Payload and hash

No attachment name, attachment bytes, or payload field is present in the selected CEAS record. A SHA-256 digest **cannot be extracted or computed** without the actual file. No payload hash is reported; any hash not computed from supplied bytes would be fabricated.

## Disposition and limitations

**Evidence-based disposition:** spam-labeled dataset message with one embedded URL; URL and host extracted from the displayed body excerpt. Maliciousness of the URL is not independently verified. No evidence in this record establishes phishing credential capture, attachment execution, authentication outcome, message delivery path, or account compromise.

For stronger attribution, obtain the original `.eml` with trusted gateway headers and, if present, the actual attachment. Preserve provenance and compute hashes from the collected bytes. Use passive, approved URL enrichment before any controlled detonation.

## Dataset attribution

Source: Naser Abdullah Alam, [Phishing Email Dataset on Kaggle](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset), `CEAS_08.csv`, licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). This project reproduces a short record excerpt and adapts it for forensic analysis; the source's limits and this adaptation are stated above.
