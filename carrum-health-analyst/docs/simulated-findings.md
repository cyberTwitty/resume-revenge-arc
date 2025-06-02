# Simulated Findings - Carrum Health  
Operation: Second Opinion  
Author: Tracie T.  
Date: May 22, 2025  

---

## 🧠 Executive Summary

As part of a simulated onboarding exercise, I conducted a non-invasive analysis of Carrum Health’s patient document upload workflow. The focus was to explore the external attack surface and assess risk exposure related to file handling, authentication, and storage mechanisms. Based on public information, system behavior, and reasonable inferences, several potential risks were identified that may warrant review by the internal security team.

---

## 🔬 Methodology

This simulation was performed using only publicly available data, behavioral observation of the frontend application, and threat modeling techniques (including STRIDE). No scanning, exploitation, or unauthorized access was performed. All findings are speculative and based on educated assumptions aligned with common cloud-based healthcare architectures.

For system assumptions and diagrammatic flow, see `threat-model.md`.

---

## 🚩 Simulated Findings

### 1. IDOR Risk in File Access APIs  
**Description:**  
Insecure Direct Object Reference (IDOR) vulnerabilities may exist if file access is controlled by predictable or user-submitted identifiers without enforced ownership checks.

**Impact:**  
Unauthorized access to patient documents would constitute a serious HIPAA violation and potential data breach.

**Evidence/Inferences:**  
Standard RESTful patterns and lack of user-specific scoping in observed URL structures suggest potential exposure.

**Mitigation:**  
Enforce object ownership validation on every access request; use UUIDs or signed URLs to obscure direct object references.

---

### 2. Malicious File Metadata Injection  
**Description:**  
File names or embedded metadata could contain JavaScript or HTML code that, if rendered in a staff dashboard, could trigger reflected or stored XSS.

**Impact:**  
Session hijacking or credential theft from internal staff portals.

**Evidence/Inferences:**  
No visible filename sanitization observed; preview behavior implies rendering without encoding.

**Mitigation:**  
Sanitize all metadata on upload; escape/encode content before displaying in UI.

---

### 3. Cloud Storage Misconfiguration  
**Description:**  
If Carrum’s backend uses AWS S3 or a similar service without strict bucket policies or IAM controls, uploaded documents may be accessible outside intended scopes.

**Impact:**  
Mass data exposure with regulatory consequences.

**Evidence/Inferences:**  
Cloud-native stack inference from job listings; ACLs not confirmed in documentation.

**Mitigation:**  
Use private buckets only; enforce resource-based policies; audit object access logs regularly.

---

### 4. Lack of Antivirus or Content Scanning  
**Description:**  
Uploads may be accepted and stored without virus scanning or file integrity checks, introducing the risk of malicious payload delivery or internal compromise.

**Impact:**  
Risk of malware distribution or compromise of staff systems.

**Evidence/Inferences:**  
No mention of scanning in public-facing docs; common oversight in early-stage healthtech.

**Mitigation:**  
Integrate antivirus scanning and hash validation into the upload pipeline; quarantine suspicious files automatically.

---

## 📌 Recommendations

Focus should be placed on:
- Strengthening access controls for user-submitted content
- Auditing file upload and retrieval endpoints
- Implementing layered defenses for metadata and storage access
- Validating the security posture of cloud object stores

This model is speculative, but reflects real-world risks observed across similar healthcare SaaS architectures. Early mitigation would reduce the likelihood of impactful incidents.

---

## 📎 Notes

This document is part of a simulated red team exercise and should be read as hypothetical.  
All findings are based on open-source observation and modeling best practices.
