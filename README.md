# CIPA14-LAB4
XSS, Session Management &amp; Authentication Resilience Assessment
Course: CIP-A104: Offensive Security — International Cybersecurity and Digital Forensics Academy (ICDFA)
Cohort: Ethical Hacker Internship
Author: Habeebah Adeyinka Olugbogi (c11.ehit2617337)

Overview  
This is the evidence package for Practical Lab 4. The lab covers reflected, stored, and DOM based XSS. It also looks at session handling and how well auth holds up. Two built in training apps were used: DVWA and OWASP Mutillidae II. Both ran in an isolated Kali Linux VM. The apps were hosted on machines that learners owned locally.  

Scope and note  
- This was for allowed class work only.  
- Testing was limited to local DVWA and Mutillidae sites.  
- Only safe, low risk markers were used. Nothing was meant to break anything.  
- We did not touch real user accounts.  
- No third party service was involved.  
- Nothing was exposed to the public internet.  
- This writeup was made for course grading. It is not meant as a how to for misuse.  

Tools used  
- Kali Linux, run as a VM  
- OWASP ZAP, for capture of requests and replies  
- Firefox DevTools, Inspector panel  
- curl


Testing Markers
Harmless, non-executing identifiers used to prove impact without payload risk: `LAB4_XSS_2026`, `LAB4_DOM_2026`, `LAB4-STORED-2026`.

Summary of Findings
This git covers an allowed test of cross site scripting, session handling, and login protection. The work was done in a separate Kali Linux virtual machine. Only local copies of DVWA and OWASP Mutillidae II were used. The XSS checks looked at reflected, DOM based, and stored cases. I tested both apps with the smallest security level they offer. I used a harmless marker such as LAB4_XSS_2026 and small related changes. When the browser ran the marker, it showed alert pop ups. I also saved the request and response logs to show what happened. After that, I raised the security setting for each app. In every case, the visible result went away. This happened due to output encoding and input checks, or a mix of both. Even so, the core design stayed the same. User data was still put into HTML and the page DOM. In one place, that data still reached a SQL statement by concatenation. Next came session tests. Neither app swapped the session id at sign in. At logout, both apps did end the session on the server side, which stopped the old session from working. I then reviewed the cookies. None of the cookies had the Secure flag in either app. I also saw uneven use of HttpOnly and SameSite. Some cookies had those flags and others did not. For login strength, I tried five wrong logins on one test account. There was no lockout and no added waiting time. No CAPTCHA showed up either. Based on this run, there was no real brute force defense at the settings used. 
Overall, when the security level was raised, the symptoms were reduced. The trust boundary issues were still there. The fix plan in the next part lists root cause changes for each problem.

Full methodology, evidence references, and remediation recommendations are in the file section.

Remediation highlights
- Output encoding is done with context for each place data is shown, not only in a special “secure demo” setup.
- The session ID is changed when a user logs in, and the server clears it when the user logs out.
- Every session or auth cookie uses Secure, HttpOnly, and SameSite.
- Authentication routes have rate limits, lockout rules, and MFA.

