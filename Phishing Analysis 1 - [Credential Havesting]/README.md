# 📧 Project Title – [Short Description]

## 📌 Overview
Provide a short summary of this phishing campaign.  
Example:
- Date or context of when you discovered it
- Type of attack (e.g., credential harvesting, malware delivery, BEC scam)
- Target audience (if known)

---

## 📨 Email Details
| Field           | Value |
|----------------|-------|
| **From**       | attacker@example.com |
| **Return-Path**| mail@attacker-domain.com |
| **Reply-To**   | attacker-reply@gmail.com |
| **Subject**    | "Invoice Payment Required" |
| **Attachment** | none |
| **Links**      | suspicious-link.com |

*(Redact or anonymize sensitive victim data.)*

---

## 🛠️ Thought Process
Explain **how you approached the analysis** step by step, and why.  
Example:
- **Step 1:** Collected raw email & inspected headers because I wanted to verify the sender IP.
- **Step 2:** Checked SPF/DKIM/DMARC results to see if the email passed authentication.
- **Step 3:** Investigated links and decoded any base64 data to reveal the redirect path.
- **Step 4:** Opened URL in a safe sandbox and captured network traffic for IOC extraction.
- **Step 5:** Documented findings and compared with known phishing techniques.

You can also embed small screenshots inline with explanations.

---

## 🔎 Analysis Process (Technical Findings)
Present your actual findings here:
- Show screenshots of headers, decoded URLs, phishing landing page.
- Explain what you observed in each step.

<p align="center">
  <img src="./images/header-screenshot.png" alt="Email Header Screenshot" width="80%">
</p>

---

## 🧩 Indicators of Compromise (IOCs)
| Type     | IOC |
|---------|-----|
| URL     | http://phishy-link.com/login |
| IP      | 123.45.67.89 |
| Domain  | phishy-link.com |
| Hash    | `e99a18c428cb38d5f260853678922e03` |

---

## 🛡️ Mitigation & Recommendations
- Block the sender domain and IP at the email gateway.
- Add the malicious URL to the web proxy blocklist.
- Educate users about invoice-themed phishing campaigns.

---

## 📚 Lessons Learned
Reflect on what you gained from this analysis:
- What worked well?
- What challenges you faced?
- What skills you improved (header analysis, URL decoding, IOC reporting)?
