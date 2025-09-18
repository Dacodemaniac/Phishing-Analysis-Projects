# Phishing Analysis 1 – [Credential Harvesting]

## 📌 Overview
This email was obtained from the Phishing_Pot Repository on Github (sample 1005).

The attacker who impersonated Binance, used phishing as the attack vector to attempt credential harvesting from different users, claiming their PII has been leaked and needed to be updated, after which they get compensated with bitcoin from the crypto company.

This phishing campaign began Sun, 23 Jul 2023 07:04:11 +0300.



---

##  Email Details
| Field           | Value |
|----------------|-------|
| **From**       | "Mary" <noreply@att[.]net> |
| **Message-ID** | BCAAD0A3[.]7732363@att[.]net |
| **Return-Path**| noreply@att[.]net |
| **Recepient**  | ruben@ruben[.]org, phishing@pot, collin@colony[.]io,chris.pierce360@gmail[.]com, |
| **Reply-To**   | noreply@att[.]net |
| **Subject**    | #CRYPTO# |
| **Attachment** | none |
| **URL**        | Redacted |
| **Date**      | Sun, 23 Jul 2023 07:04:11 +0300 |


---

## MY Thought Process  

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

The Authentication-Results showed SPF was absent in the DNS record of att[.]net, which meant we wouldn't be able to confirm if the server that sent the mail was an authorized sending server or not.

DkIM showed Ignore which meant the receiving server didn't trust the Public key used by the domain ( heritagejewelryandloan[.]com ) for encryption, so it ignored the DKIM check altogether. Finally, DMARC also showed none which meant the domain (att[.]net ) didn't have a DMARC policy set up in their DNS record. 
 <p align="center">
  <img src="./Images/Phishing sample 5.jpg" alt="Email Header Screenshot" width="80%">
  </p>

  The next step was to check the MX record of att[.]net for servers authorised to receive mails on behalf of the domain. I did the mx lookup and it confirmed the server that sent the mail isn't authorized to receive or send mails on behlaf of the att.net domain. 
  <p align="center">
  <img src="./Images/Phishing sample 25.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  
Normally, This email ought to go straight into the recepient's inbox due to absence of DMARC policy but the email security solution does its own DMARC alignment check and flagged the email as suspicious.
  <p align="center">
  <img src="./Images/Phishing sample 17.jpg" alt="Email Header Screenshot" width="80%">
  </p>
- **Step 4:** Finally since there are no attachments, I got the encoded body data of the email and decoded it using Cyberchef. It happened to be a PNG imaged encoded in base 64 and embedded as the link. This one really took me a while to figure out and my best guess is the attacker did this to bypass filters.

   <p align="center">
  <img src="./Images/Phishing sample 9.jpg" alt="Email Header Screenshot" width="80%">
  </p>

- **Step 5:** To be sure the file was truly a harmless PNG file, I performed static analysis on the file and also checked if the file had an alternate data stream the attacker could have hidden malicious scripts in.
  <p align="center">
  <img src="./Images/Phishing sample 10.jpg" alt="Email Header Screenshot" width="80%">
  </p>
 <p align="center">
  <img src="./Images/Phishing sample 11.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  <p align="center">
  <img src="./Images/Phishing sample 12.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  <p align="center">
  <img src="./Images/Phishing sample 13.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  <p align="center">
  <img src="./Images/Phishing sample 14.jpg" alt="Email Header Screenshot" width="80%">
  </p>
  <p align="center">
  <img src="./Images/Phishing sample 15.jpg" alt="Email Header Screenshot" width="80%">
  </p>
   <p align="center">
  <img src="./Images/Phishing sample 16.jpg" alt="Email Header Screenshot" width="80%">
  </p>
 Although, a vendor flagged the file as malicious in VirusTotal, this because it is associated with the phishing campaign. My static analysis showed this file is truly a PNG file without any malicious capability or alternate data stream.

---

## Indicators of Compromise (IOCs)
| Type     | IOC |
|---------|-----|
| URL     | Redacted |
| IP      |  192[.]232[.]233[.]127, 92[.]38[.]131[.]25 |
| Domain  |  heritagejewelryandloan[.]com |
| MD5 Hash| 8D934BBF6A45A3631AE7CB9829DB4EF7 |

---

##  Mitigation & Recommendations
- Ensure authentication protocols like SPF, DKIM, and DMARC are properly setup for the domain.
- Block the sender domain and IP at the email gateway.
- Add the malicious URL to the web proxy blocklist.
- Educate or train users about  phishing campaigns.

---

##  Lessons Learned
- What worked well?
- What challenges you faced?
- What skills you improved (header analysis, URL decoding, IOC reporting)?
- This analysis improved my skills as regarding email forensics and it also made me understand phishing more better. Until now, I haven't come across an embedded IMG file encoded in base 64 as the main link for a URL. 
