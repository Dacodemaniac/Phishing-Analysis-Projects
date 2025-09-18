# Phishing Analysis 1 – Credential Harvesting

## 📌 Overview
This email was obtained from the **Phishing_Pot** repository on GitHub (sample `1005`).  

The attacker, impersonating **Binance**, used a phishing campaign with the goal of **credential harvesting**.  
Victims were informed that their PII had been leaked and were urged to update their details to receive a Bitcoin “compensation.”  

This campaign began **Sun, 23 Jul 2023 07:04:11 +0300**.

---

## 🧠 Thought Process  

### **Step 1 – Initial Review**
I opened the email with **Outlook** as the MUA and reviewed the body to understand the context.  

At first glance, it was clear this was a malicious email. The attacker masqueraded as *Mary* (an employee at att[.]net) and attempted to create urgency by asking users to update their leaked PII to receive Bitcoin.  

<p align="center">
  <img src="./Images/Phishing sample 1.jpg" alt="Email Body Screenshot" width="80%">
</p>

This use of **urgency** is a classic social engineering tactic.  

<p align="center">
  <img src="./Images/Phishing sample 2.jpg" alt="Social Engineering Example" width="80%">
</p>

There were no attachments, but hovering over the main image revealed it was embedded with a **malicious link**, another red flag.  

---

### **Step 2 – Header & Routing Analysis**
I obtained the **raw email** and analyzed its headers to trace its origin.  

<p align="center">
  <img src="./Images/Phishing sample 3.jpg" alt="Header Trace Screenshot" width="80%">
</p>

- The email originated from a city in **Russia**  
- The SMTP server was located in the **USA**

<p align="center">
  <img src="./Images/Phishing sample 18.jpg" alt="Geolocation Screenshot" width="80%">
</p>

Both IP addresses were scanned on VirusTotal and surprisingly came back as **clean**, meaning no prior malicious reports.  

<p align="center">
  <img src="./Images/Phishing sample 21.jpg" alt="VirusTotal Results" width="80%">
</p>

---

### **Step 3 – Authentication Checks**
Even if the IPs appeared clean, header comparisons were necessary:

- **From**, **Reply-To**, and **Return-Path** all matched  
- However, authentication results showed:
  - **SPF:** Absent from att[.]net DNS  
  - **DKIM:** `ignore` – receiving server did not trust the public key  
  - **DMARC:** None – no policy set  

<p align="center">
  <img src="./Images/Phishing sample 5.jpg" alt="Authentication Results" width="80%">
</p>

Further MX lookups confirmed that the sending server was **not authorized** to send or receive email for att[.]net.  

<p align="center">
  <img src="./Images/Phishing sample 25.jpg" alt="MX Lookup Result" width="80%">
</p>

Despite the missing DMARC policy, the email security solution flagged the message as suspicious.

<p align="center">
  <img src="./Images/Phishing sample 17.jpg" alt="Security Solution Flag" width="80%">
</p>

---

### **Step 4 – Encoded Content Analysis**
Since there were no attachments, I decoded the **email body** using CyberChef and discovered an embedded **Base64-encoded PNG image**.  

<p align="center">
  <img src="./Images/Phishing sample 9.jpg" alt="CyberChef Decode" width="80%">
</p>

This technique likely helped bypass email filters.

---

### **Step 5 – Static File Analysis**
To confirm the file’s integrity, I performed **static analysis** and checked for hidden data streams.  

<p align="center">
  <img src="./Images/Phishing sample 10.jpg" alt="File Header Analysis" width="80%">
</p>

Results showed the file was a legitimate PNG, with no hidden scripts or alternate data streams.  
VirusTotal flagged it as malicious **only because it was tied to this phishing campaign**.

---

## 📧 Email Details

| Field           | Value |
|----------------|-------|
| **From**       | "Mary" <noreply@att[.]net> |
| **Message-ID** | BCAAD0A3[.]7732363@att[.]net |
| **Return-Path**| noreply@att[.]net |
| **Recipient**  | ruben@ruben[.]org, phishing@pot, collin@colony[.]io, chris.pierce360@gmail[.]com |
| **Reply-To**   | noreply@att[.]net |
| **Subject**    | #CRYPTO# |
| **Attachment** | None |
| **URL**        | Redacted |
| **Date**       | Sun, 23 Jul 2023 07:04:11 +0300 |

---

## 🔎 Indicators of Compromise (IOCs)

| Type     | IOC |
|---------|-----|
| URL     | Redacted |
| IP      | 192[.]232[.]233[.]127, 92[.]38[.]131[.]25 |
| Domain  | heritagejewelryandloan[.]com |
| MD5 Hash| 8D934BBF6A45A3631AE7CB9829DB4EF7 |

---

## 🛡️ Mitigation & Recommendations
- Configure and enforce **SPF**, **DKIM**, and **DMARC** on organizational domains  
- Block the **sender domain** and **source IPs** at the email gateway  
- Add malicious URLs to the **web proxy blocklist**  
- Conduct **user awareness training** on phishing techniques  

---

## 📚 Lessons Learned
- **What worked well:** Header analysis, URL tracing, and static file inspection  
- **Challenges faced:** Identifying the embedded Base64 PNG image as the main malicious link  
- **Skills improved:** Email header forensics, IOC extraction, and phishing campaign investigation  

This case strengthened my ability to handle **image-based phishing campaigns** and reinforced the importance of layered email security.

