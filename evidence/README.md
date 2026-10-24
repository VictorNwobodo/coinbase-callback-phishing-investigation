# Evidence Handling Notes

Case: **CASE-2026-1020-001**  
Analysis date: **24 October 2026**

## Evidence reviewed

- Original email in .eml format, retained privately.
- PDF attachment: GZXK-Y7PVE0S_20261020.pdf, 300,668 bytes.
- Extracted full-page JPEG images from the two-page PDF.
- Email headers and authentication results.
- Static PDF metadata, structure, rendered pages, OCR output, and hash records.

## Integrity hashes

**PDF**
- SHA-256: 2c2e8f751aea3de37ff2fca6a519cd4ea260daa83187e770e01336053bb54af7
- SHA-1: 4bda07f0067ddaa0f887c8ec696e3cbf5334579a
- MD5: 56bdbec9e332a1c739007108ffa463a5

**Extracted page images**
- Page 1 SHA-256: 9a7f12f0a4b09aa45227ad8c0a5423497e9059fe3c9ab9051b929c2517340e46
- Page 2 SHA-256: 3fa7ab16585073c18eddff4c0f31f205c5ef4cda6503175902484f9b1adad3c3

## Tools and method

The analysis used pdfinfo, pdffonts, pdfimages, qpdf --check, pdftoppm, Tesseract OCR, OpenCV QR-code checking, and hashing utilities. The work was static inspection; the analyst did not call the displayed phone number or interact with the sender.

## Evidence protection

The original .eml, PDF, and any screenshots or extracted artifacts are intentionally **not published here**. Keep originals in access-controlled storage and avoid uploading unredacted evidence to public repositories or public scanning services. Direct contact details are omitted from this public report.

This file documents the evidence reviewed and the method at a high level. It does not claim that the public repository contains the original evidence or a fully reproducible forensic image.
