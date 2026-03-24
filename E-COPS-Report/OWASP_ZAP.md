# A Guide to OWASP ZAP Web Scanner

## Overview

What is ZAP? Zed Attack Proxy (ZAP), formerly known as OWASP ZAP, is an open-source penetration testing tool. It is a versatile, multi-functional platform 
widely used by penetration testers, bug bounty hunters, and developers to analyze and scan web applications for security vulnerabilities. ZAP creates a proxy server and 
makes the website traffic pass through the server. By acting "manipulator-in-the-middle," we are able to inspect all requests user make to a web application and 
the responses given. The use of auto scanners in ZAP helps to intercept the vulnerabilities on the website.<br/><br/>

<img width="379" height="89" alt="image" src="https://github.com/user-attachments/assets/8e994317-1aca-4d35-b84c-53101ff55f9f" /> <br/>

**ZAP vs Burp Suite**<br/>
While both are widely used as tools in web application security, online discussions and users' feedback highlight few key differences.<br/>
1. User interface
- ZAP is known for its simple and user-friendly interface, providing an easy navigation experience for users.
- Burp Suite is often described as more on the "steep-learning site," but can be considered a good trade off in some ways. For example, it has a comparer tab,
which allows easier change detection like detecting differences in size from time change or tokens and content.
2. Functionality
- Both tools share some a lot of similar capabilities with one might be better than the other in some aspects.
- Burp Suite has an intruder tool, although it is limited to single-core operation. ZAP has an equivalent tool called “Fuzzer.”
- Burp has the ability to detect token entropy and randomness for cryptography analysis, something that ZAP is lacking.
- ZAP is more preferable when it comes to API integration compared to Burp. You can easily access the API from the browser or other user agents like curl or SDKs/libraries.
3. Commercial vs Open Source
- The major key difference can also be summarized based on the type of platfrom they both are.
- While Burp Suite Pro is undeniably helpful especially for web penetration testing, it costed some amount of money. ZAP on the other hand provides each functionality
with no charge to public users.<br/>

To conclude, each user's experience might differ from one another. However, both tools are still relevant in the field and switching from one to another might be good 
depending on the purpose of use.<br/>

## ZAP Setup in Kali Linux

It can be done in two simple steps: updating the environment and installing ZAP using Linux command:
```
sudo apt update -y
sudo apt install zaproxy
```
<br/>
Once installation is finished, we can find ZAP in the menu tab by clicking on "Web Application Analysis" and there it is!<br/>
<img width="595" height="715" alt="image" src="https://github.com/user-attachments/assets/7bbfd978-1d0b-4255-a6cc-d0991289e028"/> <br/>

## Exploring basic functionalities


