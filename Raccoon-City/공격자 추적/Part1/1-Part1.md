 ### Warming up: 실전 Threat Intelligence 추적 챌린지  

> TI팀에서 근무하는 당신, 사이버 보안에서 TI는 무엇의 약자일까요?
<br/>
The answer to this question is pretty obvious since we've just discussed about it in the introduction lol. But to follow the flow of the game, we can start by viewing
the given ```.eml``` file and analyze the clues from there.<br/>  

![TI.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/TI.png)  

This is the first email file we were presented with. It's an email from the IT Support team to... oh look we have our answer. To the "Threat Intelligence (TI) team!  

**Flag: Threat Intelligence**<br/><br/>

### #1 Suspicious Domain
> 공격자가 피싱에 사용한 피싱 도메인 (Phishing Domain)은 무엇인가요?  
<br/>
![Domain.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Domain.png)<br/>  

This is also a straight-forward question. We can easily find the domain used by the attacker from this part of the email.  
> Tony Raccoon<tony.raccoon@racooncoin.site>

**Flag: racooncoin.site**<br/><br/>  

### #2 What is this Domain?
> 이처럼 사람의 오타 가능성을 노려 유사한 철자(또는 문자)를 등록해 피싱·사기 등에 악용하는 기법을 무엇이라고 부르나요?  
<br/>
There are five options given and we need to select one that fits the situation. Let's look at all of them one by one:  

1. Domain Hijacking: Also known as Domain Theft, a situation where the attacker gain unauthorized access over the domain name/accounts. This is wrong because in this scenario, the attackers did not have a direct control over the actual domain, but rather created a new one close to it.
2. Typosquatting: Typically tricks users into visiting fake websites with mispelled URLs of legitimate webiste. Aha! This sounds so close to the one we're dealing with.
3. Host Spoofing: A webserver can host multiple websites on the same IP address, so when a client makes a request to access a webiste, she needs to specify which webserver we're talking about in the host header. Attackers can manipulate the host header to bypass web filters. This is not the answer for this question because it has nothing to do with spelling errors, but rather about manipulating the vulnurabilities in web filters.
4. Homograph Attack: A homograph attack is a type of cyber deception where attackers exploit visually similar characters from different scripts to create fraudulent domain names that mimic legitimate ones. So for example, an attacker might uses 0 instead of O. Kinda similar to what we're facing but not exactly.
5. Typo Attack: Nope, this is not an official term. Searching this on the internet brings us to "Typosquatting" instead.

**Flag: Typosquatting**<br/><br/>  

### #3 Phishing Link
> 이메일 본문에 포함된 링크를 통해 공격자가 시도한 공격 유형은 무엇일까요?  
<br/>
To answer this, we need to try clicking on the phishing link and see for ourselves.  
![Phishing Link.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Phishing%20Link.png)<br/>  

We're presented with this interface. What we can understand from here, the attacker built this fake website with the intention of stealing confidential credentials from users by expecting them to type in their email and password. Once the log in button is clicked, these data will be obtained by the attacker to be used for malicious reasons.<br/>  

**Flag: Credential Harvesting**<br/><br/>  

### #4 Suspicious IP
> 해당 공격 사이트를 살펴보던 중 의심스러운 IP의 흔적을 발견하였습니다.  
<br/>
To approach this question, we can analyze the backend process underneath the interface layers.  
![IP Address](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/IP%20Address.png)<br/>  

We can see that once users click on submit, the credentials will be sent to an IP address while "연결 중.." is being displayed on the screen. This is a cliche situation with phishing websites built to steal important information.  

**Flag: 140.238.194.224**<br/><br/>  

### #5 What is this IP for?
> 공격자는 다양한 공격 인프라를 구축합니다. 전 문제 (Suspicious IP)에서 찾은 공격자 호스트는 어떤 타입의 인프라일 가능성이 있을까요?  
<br/>
As mentioned before, the main reason behind the IP address is to send the credentials to another location. In another word, this describes the term "redirect." 
Also, from http://140.238.194.224, we know the protocol is HTTP. Thus, the flag is..<br/>  

**Flag: HTTP Redirector**<br/><br/>

### #6 They still make mistakes!
> 공격자의 호스트를 찾아낸 당신. 이제 이 IP에 대해 조사해야 합니다. 앗! 해당 호스트를 조사하던 중 보통 리다이렉터들이 오픈한 포트 외 다른 포트가 열려있습니다! 해당 포트는 무엇일까요?  
<br/>
This is where Linux commands come into action to help us identify the open ports available withon the IP address.  
![Port.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Port.png)<br/>  

Using the command ```nmap -sV [IP address]```, we can identify the services running on the domain. We see there's a service called "SimpleHTTPServer." I think it's fair to say that it's a direct hint to the HTTP redirector case that we have here.<br/>  

**Flag: 8081**<br/><br/>

### #7 What's their attack plan?
> 해당 포트로 조사하던 당신.. 앗! 해당 호스트에는 굉장히 흥미로운 파일들이 있습니다. 해당 파일들을 조사하고 FLAG를 찾아내고 제출하세요!  

After trying to access the following URL: http://140.238.194.224:8081, we were directed to this page:<br/>  
![Attack Plan](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Attack%20Plan.png)<br/>

The zipped file consist of three other files. Among them is what seems to be closer to the challenge in question called ```plan.txt``` so I checked it out first<br/>

![Plan txt](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Plan%20txt.png)<br/><br/>

Woah it seems to be a very detailed description of planned attacks, including those for what I assume to be for the upcoming challenges. However, no flags are in sight, so I proceed to look into the ```db.sql``` file.<br/>
![DB File.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/DB%20File.png)<br/><br/>

There we got it!  
**Flag: FLAG{KOREAN#1COMMUNITYRR}**<br/><br/>

### #8 Sock Puppets
> 라쿤코인 IT팀은 라쿤코인 직원을 사칭해 고객에게 링크드인 DM(피싱 링크 포함)을 발송한 정황을 발견하였습니다. 하지만 라쿤코인은 직원 개인의 공식 LinkedIn 프로필을 운영하지 않는다 합니다! 가짜(사칭) LinkedIn 프로필은 무엇일까요?
<br/>
In the previous ```plan.txt```file, the plan for creating a LinkedIn profile is described by using the name "Soyeong Park."  
![Linkedin-1.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Linkedin-1.png)<br/><br/> 

Just with a couple searches in Linkedin, it doesn't take long until I was able to find the said account.  
![Linkedin-1.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part1/Linkedin-2.png)<br/><br/> 

**Flag: https://www.linkedin.com/in/soyeong-park-5046b7391/**<br/><br/>
