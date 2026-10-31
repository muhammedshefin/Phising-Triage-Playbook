# Phishing Campaign & Automated SOC Alert Triage Playbook

A defensive SOC training project built around a historical email record from the Kaggle **Phishing Email Dataset**. It includes a responsive triage dashboard, a source-grounded forensic write-up, structured IOC and ATT&CK data, an evidence timeline, and a practical response playbook.

> **Important evidence limitation:** The selected `CEAS_08.csv` record is labeled spam and contains one URL. That dataset label does not independently prove the URL is malicious today. The CSV preview does not include raw email headers, SPF/DKIM/DMARC results, message hops, sending IPs, attachments, or incident-response telemetry. Those facts are explicitly marked unavailable rather than inferred.

## Project contents

| Path | Purpose |
|---|---|
| `index.html` | Responsive SOC dashboard with record summary, email-field analysis, IOCs, ATT&CK mapping, response workflow, and evidence viewer. |
| `style.css` | Dark-mode responsive styling for the standalone dashboard. |
| `dossier/header_analysis.md` | Dataset-backed email record analysis, extracted URL, header/payload limitations, and safe handling notes. |
| `dossier/incident_timeline.md` | Evidence timeline containing only what the dataset record supports; absent incident telemetry is called out. |
| `dossier/triage_playbook.md` | Defensive alert-triage, validation, containment, approval, and recovery guidance. |
| `indicators/ioc_matrix.json` | Structured record metadata and extracted URL/domain, plus explicit unavailable fields for IPs and payload hash. |
| `indicators/mitre_attack.json` | ATT&CK mapping status. No techniques are confirmed from this record alone. |

## Dataset record used

- **Dataset:** [Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset)
- **Publisher:** Naser Abdullah Alam
- **File:** `CEAS_08.csv`
- **Selected record:** Linwood Sloan, dated `Tue, 05 Aug 2008 20:27:34 -0300`
- **Subject:** `Your order`
- **Dataset label:** `1` (spam-labeled by the dataset)
- **URL count:** `1`
- **URL in body excerpt:** `http://somateco.com.br/folderz/ready.php`

The URL is defanged in user-facing analysis as `hxxp://somateco[.]com[.]br/folderz/ready[.]php`. It is included as a dataset-observed indicator, **not an independently confirmed live threat**. The sender domain is retained only as context and is not classified as malicious.

The dataset is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Refer to Kaggle and the dataset publisher when reusing the dataset; the project reproduces only a short record excerpt for analysis.

## Forensic scope and cautions

The source record contains sender, recipient, date, subject, body, label, and URL fields. It is not a raw `.eml` message. As a result:

- SPF, DKIM, and DMARC outcomes are **not provided**.
- The `Received` chain and sending/relay IP addresses are **not provided**.
- No attachment bytes or payload SHA-256 can be extracted or computed.
- The dataset’s spam label is not a current URL reputation verdict.
- No click, delivery, compromise, quarantine, or containment event is documented.
- The MITRE ATT&CK file does not claim techniques are confirmed.

Do not visit the URL from a production environment, treat missing fields as clean, or deploy the URL/domain as a production block without separate validation and approval.

## Preview the dashboard

Open `index.html` directly in a browser, or use VS Code Live Server. The page is static and needs no package installation, backend, or build step. Its stylesheet and dossier/JSON links are relative to the project directory.

## Suggested review workflow

1. Start with the dashboard’s dataset evidence and limitations.
2. Read `dossier/header_analysis.md` for the selected record and extraction rationale.
3. Use `indicators/ioc_matrix.json` as structured, provenance-labeled data—not as a verdict or automatic blocklist.
4. Consult `dossier/triage_playbook.md` for validation and conditional response steps.
5. Add ATT&CK techniques only when additional evidence substantiates the behavior.

## Validation

The IOC and ATT&CK files are valid JSON. Dashboard references point to project-local files, and the dashboard’s URL and unavailable-field descriptions match the structured IOC record.
