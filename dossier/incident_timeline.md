# Evidence Timeline

> **Record KAG-CEAS08-001 · source: Kaggle `CEAS_08.csv` preview.** This is an evidence timeline, not a reconstructed campaign timeline. The dataset contains a message date but no gateway, delivery, click, identity, containment, or SOC event logs.

| Time | Event | Evidence | Status |
|---|---|---|---|
| Tue, 05 Aug 2008 20:27:34 -0300 | Date recorded for the email sample. | `CEAS_08.csv` preview: sender `Linwood Sloan <Linwood@goline.ca>`, recipient `user7-ext4@gvc.ceas-challenge.cc`, subject `Your order`. | Dataset field |
| Same record | Message body excerpt contains `http://somateco.com.br/folderz/ready.php`. | Preview indicates `label = 1` and `urls = 1`. | Dataset evidence; URL reputation not independently verified |
| Not supplied | Transport delivery, mail gateway verdict, URL click, user interaction, or SOC detection. | No event or telemetry fields for these activities in the selected record. | Unknown |
| Not supplied | Quarantine, endpoint isolation, credential reset, or other containment. | No response/action records in the dataset sample. | Unknown |

## Scope and limits

The row is historical email evidence, not proof of a real-world phishing campaign or a current live threat. The dataset preview does not provide the raw MIME message, trusted `Received` chain, SPF/DKIM/DMARC results, sending IP, payload, attachment hash, or incident-response telemetry. Do not create additional timestamps or claim containment without separate evidence.

For a real incident timeline, correlate the original message with mail gateway and mailbox audit records, secure web gateway or DNS logs, identity-provider events, endpoint telemetry, and case-management action logs. Record each source's timezone and distinguish observed events from analyst actions.
