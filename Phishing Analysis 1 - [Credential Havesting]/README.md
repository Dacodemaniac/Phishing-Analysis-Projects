# 📧 Project Title – [Short Description]

## 📌 Overview
This email was obtained from the Phishing_Pot Repository on Github (sample 1005).

The attacker who impersonated Binance, used phishing as the attack vector to attempt credential harvesting from different users, claiming their PII has been leaked and needed to be updated, after which they get compensated with bitcoin from the crypto company.

This phishing campaign began Sun, 23 Jul 2023 07:04:11 +0300.



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
- **Step 1:** I opened the email with outlook as the MUA and checked the body of the email. I did this to get the context of the whole attack and also know how to begin my investigation. It was obvious at first glance that the email is indeed a malicious one since the attacker who masqueraded as Mary (an employee at att.net)  was the sender on behalf of Binance. This seemed suspicious.
  <p align="center">
  <img src="./Images/Phishing sample 1.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  
    There was also a sense of urgency to get the users to Update their leaked PII and get compensated with bitcoin when done. Urgency is one of the social engineering techniques used by threat actors.
  
<p align="center">
  <img src="./Images/Phishing sample 2.jpg" alt="Email Header Screenshot" width="80%">
</p>

  I also checked for attachments but found  none, only to notice the entire image was embedded as the link to the malicious     domain when hovered over. This also seemed like a red flag as unsuspecting users could easily fall for this trick thinking it  was a normal image.

  
- **Step 2:** I then proceeded to obtain the raw email and trace the origin of the email and the routes taken before it got to the recepient.
  
   <p align="center">
  <img src="./Images/Phishing sample 3.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  The email shown to have originated from a city in Russia while the server involved in this phsihing campaign was from USA.
    <p align="center">
  <img src="./Images/Phishing sample 18.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  <p align="center">
  <img src="./Images/Phishing sample 6.jpg" alt="Email Header Screenshot" width="80%">
  </p>
   <p align="center">
  <img src="./Images/Phishing sample 20.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  I decided to check both IPs on VirusTotal and shockingly both came out as clean. 
   </p>
   <p align="center">
  <img src="./Images/Phishing sample 21.jpg" alt="Email Header Screenshot" width="80%">
  </p>
   </p>
   <p align="center">
  <img src="./Images/Phishing sample 22.jpg" alt="Email Header Screenshot" width="80%">
  </p>
- **Step 3:** Even if the IPs and the domain appeared clean, it still doesn't make the email a legit one. The next thing i did was compare the Reply-To, From, and Return-Path headers to see if they match, and surprisingly they did match.
   </p>
   <p align="center">
  <img src="./Images/Phishing sample 23.jpg" alt="Email Header Screenshot" width="80%">
  </p>
   </p>
   <p align="center">
  <img src="./Images/Phishing sample 24.jpg" alt="Email Header Screenshot" width="80%">
  </p>
 
   This still doesn't prove the email is a legit one as previous signs have indicated the email address was indeed spoofed, but to confirm this I had to look at the Authenticated-Results header for email authentication protocols like SPF, DKIM, and DMARC.
  <p align="center">
  <img src="./Images/Phishing sample 17.jpg" alt="Email Header Screenshot" width="80%">
  </p>
- **Step 4:** Opened URL in a safe sandbox and captured network traffic for IOC extraction.
- **Step 5:** Documented findings and compared with known phishing techniques.

You can also embed small screenshots inline with explanations.

---

## 🔎 Analysis Process (Technical Findings)
Present your actual findings here:
- Show screenshots of headers, decoded URLs, phishing landing page.
- Explain what you observed in each step.

<p align="center">
  <img src="./Images/Phishing sample 1.jpg" alt="Email Header Screenshot" width="80%">
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
