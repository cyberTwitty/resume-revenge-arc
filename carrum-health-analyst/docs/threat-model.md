# Threat Model: Patient File Upload Workflow for Carrum Health

## Focus Area

> This model focuses on the patient document upload feature within Carrum's care coordination platform. Patients are required to create an account to access certain features of the site, which may include uploading medical documents for pre-approval, provider matching, and post-surgery support.¹

## Actors

- Patient: External user uploading files via authenticated portal
- Care Coordinator: Internal staff viewing or downloading documents uploaded by patients
- Attacker: External actor without valid credentials, passively or actively targeting the upload flow

## Assets

- Sensitive Content:
  - Medical documents (contains PHI/PII)
  - File metadata (names, timestamps, user IDs)
- Infrastructure:
  - Authentication tokens/session cookies
  - File storage system (currently assumed to be cloud-based)

## Entry Points

- File upload form inside the patient portal
- API endpoint related to document handling
- File preview or download feature
- Notification systems (e.g. emails, webhooks) triggered by uploads

## Potential Threats

> The following risks may arise if controls are insufficient or improperly configured

| Threat | Description | Likelihood | Impact | Risk Level |
|--------|-------------|------------|--------|------------|
| XSS via file metadata | Malicious content in file names or previews could trigger JavaScript execution in browser | Medium | High | High |
| IDOR | Users may access others’ documents by manipulating file IDs in API requests | High | High | Critical |
| File type spoofing / polyglots | Dangerous file types disguised as safe formats may bypass validation | Medium | Medium | Medium |
| Cloud storage misconfiguration | Improper bucket permissions could expose sensitive files | Low | High | Medium |
| No antivirus/malware scanning | Malicious files may be stored and later executed internally | Medium | Medium | Medium |
| Session hijacking | Stolen tokens via referrer headers or weak session handling | Low | High | Medium |

> Risk ratings are based on assumed infrastructure and observed patterns from similar healthcare platforms. Actual values may vary based on Carrum Health's internal access controls and audit configurations.

## Business Impact

- HIPAA violations (data exposure, unauthorized access)
- Patient trust erosion
- Regulatory fines/breach notification requirements
- Brand damage in healthcare space

## Inferred/Recommended Mitigations

- Use signed URLs or strict RBAC (role-based access control)
- Apply least privilege policies to API tokens and access roles
- Sanitize all metadata and filenames before rendering in UI
- Enforce MIME-type, extension validation, magic byte validation
- Implement strict access controls for cloud storage
- Integrate antivirus scanning before storing uploads
- Set short TTLs on session cookies with secure flags

## References

### Primary Source

| ¹ [Carrum Health Terms of Service](https://carrumhealth.com/terms-of-use/) |

### Supporting Methodologies & Frameworks

| Reference | Link |
|----------|------|
| SecureFlag – Threat Modeling Process | [Link](https://blog.secureflag.com/2025/04/03/threat-modeling-process/?utm_source=chatgpt.com) |
| Praetorian – Threat Modeling Approach | [Link](https://www.praetorian.com/blog/always-be-modeling-how-to-threat-model-effectively/?utm_source=chatgpt.com) |
| OWASP Threat Modeling Cheat Sheet | [Link](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html?utm_source=chatgpt.com) |
| Microsoft Threat Modeling Tool Docs | [Link](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-getting-started?utm_source=chatgpt.com) |
| STRIDE Model (Wikipedia) | [Link](https://en.wikipedia.org/wiki/STRIDE_model?utm_source=chatgpt.com) |
| Red Team Report Template (redteam.guide) | [Link](https://redteam.guide/docs/Templates/report_template/?utm_source=chatgpt.com) |
| Red Team Scenarios (Prism Infosec) | [Link](https://prisminfosec.com/red-team-scenarios-modelling-the-threats/?utm_source=chatgpt.com) |

## Notes

- All information used is derived from public sources.

- No active scanning, exploitation, or unauthorized access was performed.

- This simulation is for educational and portfolio purposes only.

- For scope and methodology details, see the disclaimer in the project README.
