# Phishing Investigation Report

**Case reference:** CASE-2026-1020-001  
**Prepared by:** Nwobodo Chukwuemeka Victor  
**Sample received:** 20 October 2026, 16:43:06 UTC  
**Analysis performed:** 24 October 2026  
**Case type:** Suspected callback phishing (telephone-oriented attack delivery, or TOAD)  
**Assessment:** Malicious / high confidence

> **Privacy note:** This public report redacts direct contact details. The original email and attachment are not published in this repository.

---

## 1. Executive summary

**Verdict: MALICIOUS. High-confidence suspected callback phishing impersonating Coinbase.**

I investigated an email received in my own Gmail inbox on 20 October 2026. The email contained a PDF presenting an alleged cryptocurrency transaction and urging the reader to use one of two call-to-action buttons. The document displayed a telephone number for a supposed Coinbase support interaction.

The assessment is based on the combined context and construction of the message: a cryptocurrency transaction the recipient may not recognize, a callback-oriented PDF, mismatched email subject and attachment content, and inconsistencies within the document. The sender authenticated through Gmail infrastructure, but those authentication results do not establish that the sender represents Coinbase.

No call was made to the displayed number, no links or buttons were used, and no account access was attempted. I did not establish who controlled the phone number or whether any other recipient was affected. This is a sample-based assessment, not attribution to a known threat actor.

## 2. Email and attachment overview

| Field | Observation |
|---|---|
| Case ID | CASE-2026-1020-001 |
| Email received | 20 October 2026, 16:43:06 UTC |
| Subject | Thank You for Your Order I30M7ZAZ151DWNLID09H52WBCF |
| Display name | Amy Sanchez |
| Sender address | Redacted in this public report |
| Attachment | GZXK-Y7PVE0S_20261020.pdf |
| Attachment size | 300,668 bytes |
| PDF version | 1.7 |
| PDF pages | 2 |
| SHA-256 | 2c2e8f751aea3de37ff2fca6a519cd4ea260daa83187e770e01336053bb54af7 |

The email body contained an order identifier in plain-text and HTML versions. The attachment instead presented a cryptocurrency transaction notification styled as a Coinbase alert.

## 3. Investigation process

The investigation focused on preserving the sample, reviewing message headers, inspecting the PDF structure, extracting and examining its page images, and recording file hashes. Analysis was performed without interacting with the callback number or the PDF's call-to-action elements.

Tools used included:

- pdfinfo to inspect PDF metadata and page count.
- pdffonts to inspect embedded fonts.
- pdfimages -list and pdfimages -j to inspect and extract embedded images.
- qpdf --check to check PDF structural integrity.
- pdftoppm to render pages for visual inspection.
- Tesseract OCR to extract text from image-based pages.
- OpenCV-based QR-code checking.
- sha256sum and related hashing utilities to record file hashes.

These tools were used for static examination. No claim is made that a full sandbox detonation or endpoint investigation was performed.

## 4. Findings

### 4.1 Email content and social engineering

The subject references an order, while the attachment describes a cryptocurrency transaction. That inconsistency can make the recipient open the attachment to understand what happened.

The PDF claims that a transaction involving **12.75 ETH (approximately USD 30,240)** was submitted on **14 October 2026 at 09:17 AM** and was awaiting confirmation. It displays a partially truncated wallet address and provides two prominent actions, labelled “Approve this send” and “Cancel and report.” The actions were nonfunctional PDF buttons/links in the sample examined.

The document also displays a callback telephone number, redacted here. Its use alongside a high-value, potentially unauthorized transaction is consistent with a callback-phishing approach: the recipient may call the displayed number to dispute the supposed transaction and be directed into a fraudulent support interaction.

Additional quality and consistency issues included a generic Coinbase-user greeting, a placeholder email address in the footer, a missing-glyph box, repeated telephone information, and generic anti-scam wording. These are supporting observations, not individually conclusive proof.

The transaction date stated in the PDF is about six days before the email arrived. This delay, together with the order-related subject and crypto-alert attachment, adds to the message's inconsistencies.

### 4.2 PDF structure

The attachment is image-based: each page contains a full-page JPEG image at 1488 × 2105 pixels, and the PDF does not contain a conventional text layer or embedded fonts for the displayed page content.

Static structural inspection did not identify PDF JavaScript, OpenAction, Launch, additional actions (AA), embedded files, AcroForms, or a Names structure. No Link or URI annotations were identified. QR-code checking did not identify a QR code, and visual review did not find hidden low-contrast text.

These findings reduce evidence for a conventional clickable or script-driven PDF exploit in this sample, but they do **not** make the file safe. An image-only PDF can still deliver persuasive social-engineering content and a telephone number.

### 4.3 Message headers and email authentication

The observed header chain indicates submission through gmailapi.google.com via HTTPREST at approximately 16:43:03 UTC. Google MX infrastructure accepted the message from mail-sor-f41.google.com (209.85.220.41) at 16:43:06 UTC.

The observed authentication results were:

| Check | Result | Interpretation |
|---|---|---|
| SPF | Pass | The sending infrastructure was authorized under the evaluated sender policy. |
| DKIM | Pass, d=gmail.com | The message passed a signature check associated with Gmail. |
| DMARC | Pass, policy p=NONE | The message aligned with the evaluated gmail.com From domain under the reported policy. |

These results do not authenticate the message as an official Coinbase communication. A person can send a deceptive message from a Gmail account that successfully passes Gmail-related authentication. The originating client IP was not exposed in the observed header chain. The IP 209.85.220.41 is observed Google mail infrastructure and should not be treated as an attacker IP based on this evidence.

A numeric value observed in the headers may be an application or cloud-project identifier, but its meaning was not confirmed and it is not treated as a malicious indicator.

### 4.4 Reputation and external validation

Public reputation checks did not establish that the redacted sender or callback number was known malicious or legitimate. The phone number's ownership was not independently verified. No phone call was made, and no live interaction with the sender occurred.

The conclusion therefore rests on the message's content and context, rather than a confirmed reputation hit or verified phone-number attribution.

## 5. Assessment

The evidence supports classifying this sample as **high-confidence suspected callback phishing impersonating Coinbase**.

Key supporting indicators:

1. The attachment presents a high-value cryptocurrency transaction that could alarm a recipient.
2. It directs attention to a callback number and uses transaction-dispute language.
3. The email subject and attachment content do not match.
4. The document contains generic placeholders and visual/content inconsistencies.
5. Email authentication validates the evaluated Gmail sending domain and infrastructure, not Coinbase brand authorization.

**Confidence applies to the phishing assessment, not to actor attribution.** The investigation did not establish who sent the email, who controls the phone number, whether the displayed wallet belongs to an attacker, or whether anyone suffered loss.

## 6. Indicators of compromise and observables

| Observable | Value | Handling |
|---|---|---|
| Email subject | Thank You for Your Order I30M7ZAZ151DWNLID09H52WBCF | Useful for searching related mail; subject alone is not unique proof. |
| Attachment name | GZXK-Y7PVE0S_20261020.pdf | Sample-specific filename; may be changed by another campaign. |
| Attachment SHA-256 | 2c2e8f751aea3de37ff2fca6a519cd4ea260daa83187e770e01336053bb54af7 | Strong sample-level identifier. |
| Attachment SHA-1 | 4bda07f0067ddaa0f887c8ec696e3cbf5334579a | Legacy hash for sample correlation. |
| Attachment MD5 | 56bdbec9e332a1c739007108ffa463a5 | Legacy hash for sample correlation. |
| Page image 1 SHA-256 | 9a7f12f0a4b09aa45227ad8c0a5423497e9059fe3c9ab9051b929c2517340e46 | Extracted image hash. |
| Page image 2 SHA-256 | 3fa7ab16585073c18eddff4c0f31f205c5ef4cda6503175902484f9b1adad3c3 | Extracted image hash. |
| Observed mail server IP | 209.85.220.41 | Google mail infrastructure in the observed hop; **not identified as attacker infrastructure**. |
| Sender address / callback number | Redacted | Preserved in private evidence; not published because ownership and maliciousness were not independently verified. |
| Truncated wallet address | Partially shown in the attachment | Insufficient for reliable attribution or on-chain analysis. |

Avoid blocking solely on a display name, subject, or the Google mail-server IP. Validate any proposed block indicator against additional samples and local telemetry.

## 7. Recommended response

### For the recipient

- Use Gmail's phishing-reporting option and preserve the original message and attachment for investigation.
- Do not call the number printed in the PDF, reply to the sender, or use the PDF's action buttons.
- If concerned about Coinbase account activity, open the official Coinbase app or manually enter the official website address rather than using contact details in the email.
- Review account activity and security settings. If credentials, verification codes, or other sensitive information were disclosed, secure the account from a trusted device and contact official support through a verified channel.

### For security teams

- Search mail telemetry for the subject, attachment hash, filename, sender patterns, and similar transaction-themed messages.
- Analyze image-only PDFs with OCR and image inspection, not just text extraction.
- Correlate brand impersonation, urgent financial claims, mismatched sender domains, and callback instructions.
- Use layered detection and analyst review. Do not build a broad blocking rule around the observed Google mail-server IP.
- Preserve the original .eml, attachment, timestamps, hashes, and relevant headers in restricted evidence storage.
- If other copies are found, compare hashes, phone numbers, sender infrastructure, and document layout before determining whether they belong to the same campaign.

## 8. Limitations

- This report covers one email and its PDF attachment.
- The original email, attachment, sender address, and callback number are not included in the public repository.
- The phone number's ownership and reputation were not conclusively established.
- No call was made and no interaction with the alleged support contact occurred.
- No recipient impact or financial loss was verified.
- The truncated wallet address was not sufficient for meaningful on-chain analysis.
- The observed headers did not reveal the original client IP.
- Static PDF inspection does not establish the absence of every possible exploit or malicious behavior.
- No threat actor or campaign attribution is claimed.

## 9. References

- Cisco Talos, *PDFs: Portable Documents or Perfect Deliveries for Phish?*  
  https://blog.talosintelligence.com/pdfs-portable-documents-or-perfect-deliveries-for-phish/
- Coinbase, *Avoiding Phone Scams* (reference for general consumer-safety context; not used to verify ownership of the number in this sample).  
  https://www.coinbase.com/fr/blog/avoiding-phone-scams
- HuntAEGIS, reference article consulted during investigation.  
  https://huntaegis.com/article/80ecd45dd4412758159b92f676f4046d
- MITRE ATT&CK knowledge base.  
  https://attack.mitre.org/

## 10. Disclaimer

This is an educational, sample-based investigation prepared for a cybersecurity portfolio. Findings reflect the evidence available at the time of analysis and may change if additional evidence becomes available. The report is not an official statement by Coinbase, Google, or any other organization. Brand names are used only to describe the impersonation observed in the sample.
