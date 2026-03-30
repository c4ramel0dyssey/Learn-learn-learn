# A Guide to OWASP ZAP Web Scanner

## Overview

What is ZAP? Zed Attack Proxy (ZAP), formerly known as OWASP ZAP, is an open-source penetration testing tool. It is a versatile, multi-functional platform 
widely used by penetration testers, bug bounty hunters, and developers to analyze and scan web applications for security vulnerabilities. ZAP creates a proxy server and 
makes the website traffic pass through the server. By acting "manipulator-in-the-middle," we are able to inspect all requests user make to a web application and 
the responses given. The use of auto scanners in ZAP helps to intercept the vulnerabilities on the website.<br/><br/>

<img width="379" height="89" alt="image" src="https://github.com/user-attachments/assets/8e994317-1aca-4d35-b84c-53101ff55f9f" /> <br/>

**ZAP vs Burp Suite**<br/>
While both are widely used as tools in web application security, online discussions and users' feedback highlight few key differences.<br/>
**1. User interface**
- ZAP is known for its simple and user-friendly interface, providing an easy navigation experience for users.
- Burp Suite is often described as more on the "steep-learning site," but can be considered a good trade off in some ways. For example, it has a comparer tab,
which allows easier change detection like detecting differences in size from time change or tokens and content.<br/>
**2. Functionality**
- Both tools share some a lot of similar capabilities with one might be better than the other in some aspects.
- Burp Suite has an intruder tool, although it is limited to single-core operation. ZAP has an equivalent tool called “Fuzzer.”
- Burp has the ability to detect token entropy and randomness for cryptography analysis, something that ZAP is lacking.
- ZAP is more preferable when it comes to API integration compared to Burp. You can easily access the API from the browser or other user agents like curl or SDKs/libraries.<br/>
**3. Commercial vs Open Source**
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

After a whole damn journey of setting up ZAP in Kali Linux, installing and uninstalling Apache2, finally I managed to get it running!<br/>

<img width="1881" height="693" alt="Screenshot 2026-03-24 202812" src="https://github.com/user-attachments/assets/d244a02d-8731-4dd5-97d9-4354497ebb12"/><br/>
ZAP consist of various helpful functionalities. Some of the familiar elements can be summarized as follows:<br/>
**1. Automated Scan**<br/>
- Scans the target website to detect common security vulnerabilities without requiring manual input.
- Combines crawling and attack techniques to quickly identify issues.<br/>
**2. Manual Exploration**<br/>
- Manually browses the website while OWASP ZAP records all requests and responses. This helps analyze how the application behaves and ensures hidden pages can also be tested.<br/>
**3. Active Scan**<br/>
- Actively sends malicious or unexpected inputs to the web application to identify vulnerabilities such as SQL injection or XSS to stimulate real attack behavior.<br/>
**4. Passive Scan**<br/>
- Similar to the above, but it is rather not intrusive and inspects traffic without doing any alteration and only raises informational or low impact alerts.<br/>
**5. AJAX Spider**<br/>
- Used to crawl modern web applications that rely on JavaScript (AJAX) to load content dynamically.
- Unlike regular spider that only follows static links, it stimulates a real browser, executes Javascript, and interacts with dynamic elements that can only be reached after client side rendering (buttons, menus, forms).<br/>

More functionalities can be discovered in the ```Tools``` bar at the left top.<br/>

## Analyzing and understanding vulnurabilities

To start with the scanning tool, I launched OWASP ZAP and selected the ```Automated Scan``` mode. Next, I configured the target URL to http://127.0.0.1/dvwa/ and initiated the attack by clicking the attack button.<br/>
<img width="1066" height="397" alt="image" src="https://github.com/user-attachments/assets/5c1cde10-da1a-4708-8503-9bc35bab2528" /><br/>

**1. User Agent Fuzzing<br/>**
   <img width="1883" height="373" alt="image" src="https://github.com/user-attachments/assets/4635e183-5067-405b-aecb-789b97cbb749" /><br/>
   - Alert type: Informational
   - Description: ZAP's fuzzer tested various User-Agent strings by sending many unusual or malicious inputs to see how the system reacts.
   - Reason for alert: The server responds differently to fuzzed User-Agent values or ZAP detects potential unsafe handling. However, this can be considered low-risk and informative rather than an actual threat.

**2. X-Content-Type-Option header missing<br/>**
   <img width="1883" height="349" alt="image" src="https://github.com/user-attachments/assets/1df15bbd-6fa2-4274-9488-7e3a180691eb" /><br/>
   - Alert type: Low risk
   - Description: This detects whether the web includes ```X-Content-Type-Options: nosniff``` security header, which allows browsers to perform MIME type sniffing. This may cause the browser (especially older ones) to interpret response content as a different type than intended, potentially leading to security issues such as Cross-Site Scripting (XSS). Adding this header ensures that browsers strictly follow the declared content type.
   - Reason for alert: DVWA does not contain the security header, leaving room for malicious codes to be executed.
  
**3. Missing Anti-clickjacking Header<br/>**
<img width="1675" height="370" alt="image" src="https://github.com/user-attachments/assets/07fdfece-ed1e-4c05-bb0d-844d45dd3f36" /><br/>
- Alert Type: Medium risk
- Description: Clickjacking is an attack where a malicious site tricks users into clicking something that lies underneath a fake element like buttons on a webite. This is done by embedding the web into a frame.
- Reason for alert: The site does not have protection against being embedded in a frame such as ```Content-Security-Policy: frame-ancestors 'none';``` header.

## Remediation Suggestions

**1. User-Agent Fuzz**
   - Ensure that all User-Agent header values are properly validated and sanitized before being processed or stored. Since HTTP headers can be manipulated by attackers, they should be treated as untrusted input.
   - Avoid directly logging or rendering User-Agent values without encoding, and use secure logging mechanisms to prevent injection attacks such as Cross-Site Scripting (XSS) or log injection.

**2. X-Content-Type-Options Header Missing**
   - Configure the server to include the X-Content-Type-Options: nosniff header in all HTTP responses. This prevents browsers from performing MIME type sniffing and ensures that content is interpreted strictly according to the declared Content-Type.
   - Verify that all responses use correct and consistent Content-Type headers, especially when serving user-generated content.

**3. Anti-Clickjacking Header Missing**
   - Protect the application from clickjacking by implementing anti-framing headers such as X-Frame-Options with values like DENY or SAMEORIGIN. For more robust and modern protection, use the Content-Security-Policy header with the frame-ancestors directive to control which domains are allowed to embed the application.
   - This is especially important for sensitive pages such as login or transaction interfaces.

## Conclusion

Using OWASP ZAP to perform the scan provided practical insight into how automated tools can identify common web application vulnerabilities. Through this process, it became clear that many security issues, such as missing headers and improper input handling, may not be immediately visible during development but can still pose potential risks. The scan also highlighted the importance of interpreting results carefully, as not all alerts indicate critical vulnerabilities. Overall, this exercise helped me gain knowledge about web vulnerabilties in a practical manner, as well as introducing me to intentionally-made vulnerable website made for cyber security practice and awareness.  
I believe that there are so much more functions in ZAP, and also vulnurabilities in DVWA to be discovered. Will spend more time in exploring them in the future!  
<br/>
Thank you!

