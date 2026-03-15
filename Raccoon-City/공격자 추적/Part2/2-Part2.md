### #10 Find their Geo-Location
> 공격자의 Github 리포를 조사해봅시다.. 재밌는걸 찾으셨나요? 혹시 ... 공격자의 위치를 특정할 무언가를 찾을 수 있나요? 공격자의 머물렀던 지역을 특정하려 합니다. 특히 한국시간 KST 2025년 11월 13일 오전 11시 45분 25초에 공격자가 상주했을 "가능성"이 있는 나라는 어디일까요?  
<br/>
I first started off by looking into the contributions made on the said date.  
![Geolocation-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Geolocation-1.png)<br/><br/>

There seems to be 8 commits made in total. I reviewed the commits done to get a little insights into past actions, as well as the date for each one. By clicking on one commit and adding ```.patch``` at the end of its URL, the raw data of the commit will be displayed. This includes specific date and time of the action being done in the past. One of the commits bring us to this information:  
![Geolocation-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Geolocation-2.png)<br/><br/>

Hmm the time is similar, and we have ```-0500``` at the end. This indicates the timezone when the action was committed. I asked gemini for the UTC time system being used for each of the answer options to know which one matches what we have here.  
1. Columbia: UTC -0500
2. United States West Coast: UTC -0800
3. South Korea: UTC +0900
4. Portugal: UTC +0000Armenia: UTC +0400<br/><br/>

Boom! Looks like we have our answer!  
**Flag: Columbia**<br/><br/>

### #11 KEY TO "HACK-BACK"
> 공격자의 Github 리포를 조사해봅시다.. 혹시 재밌는걸 찾으셨나요? 이런! 공격자는 아주 "치명적인" OPSEC 실수를 저질렀습니다! 해당 정보를 사용하여 공격자의 리다이렉터 호스트로 접근 (Initial Access)하세요! 찾아낸 KEY를 통해 호스트 접근한 후 에 "숨겨진" FLAG를 읽어내세요. FLAG는 무엇일까요?  
<br/>
Now, it's getting more interesting! We need to access the attacker's redirectory as host using leaked key.<br/><br/>

Looking at the history of commits, I soon discovered the serious mistake made by the attacker. The SSH private key is found to be among one of the earliest repositories posted in the Github!  
![Private Key-1.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Private%20Key-1.png)<br/><br/>

In the same repo, we understand the username being used is called "spark." These information alone is enough to access the server as host with ssh command. All I did was copy and pasted the private key into one file and then use the ssh command such that: ```ssh private_key spark@140.238.194.224```.<br/><br/>  

![Private Key-2.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Private%20Key-2.png)<br/>
Oh but wait... it then said that my private_key file is too opened. It's ok, we can easily change the permissions to the file to make it less open such that:  
```
chmod 600 private_key
```
<br/>
This way, only the owner has access towards the file. After that, I was finally able to access the host but oh, it then asked for a passphrase. I looked into the other repos and yup there it is:  
![Passphrase.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Passphrase.png)<br/><br/>

Using the passphrase, we are finally inside the redirectory! Hoorayyy. I tried using ```ls``` at first but there seems to not be that many files in the system.. So I tried ```ls -a``` instead and there's a file called ```flag.txt```.<br/><br/>
![Host.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Host.png)<br/><br/>

Using ```cat``` comamnd on the file and we got the flag!  
**Flag: FLAG{YOUALMOSTGOTME!@#$%@#!#}**<br/><br/>

### #12 Command&Control
> 공격자는 당신이 리다이렉터까지 접근할줄 몰랐을 겁니다.. 이제 리다이렉터에 연결된 미지의 C2 서버의 Public IP를 알아내야합니다! 해당 C2 IP는 무엇일까요?
<br/>
The plan for C2 redirectory was actually mentioned in the previous attack plan:  
![C2-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/C2-1.png)<br/><br/>

It is first important to understand how does port masquerading and C2 pivoting works.  
So basically, if the attacker is running the phishing website directly from their C2 server, it would make it easier to be detected and stopped by TI. To prevent this from happening, the attacker use port masquerading technique so the traffic will look normal, and also pivot their C2 server to hide the real infrastructure from being detected.<br/>

To paint a bigger picture, I imagine it like this: ```victim -> fake website -> some relay server/ssh tunnel -> actual C2 server```  
![C2-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/C2-2.png)<br/><br/>

We can use Linux commands to analyze the network. Since there is a mention about ```socat```(a command line based utility that establishes two bidirectional byte streams and transfers data between them), I first investigate if there are any existing socat services running in the network. Also, I used ```netstat``` command to see established connections.  
![C2-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/C2-3.png)<br/><br/>

Okayy so there are socat services running in the background just like what was mentioned in the attack plan. Also, there are some connections established from port 22. Port 22 is known to signal SSH usage within the network. This is like a direct nod to our initial hypothesis that the attacker uses ssh tunnel to connect to the actual C2 server! The IP address being shown as the destination for our port 22 is the answer for this question :)  

**Flag: 158.180.6.169**<br/><br/>

### #13 Under The "Deep" Sea
> 리다이렉터 호스트를 좀 더 조사하던 중 공격자가 운영하는 숨겨진 공격 어드민 패널 정보가 발견되었습니다. 해당 비밀 서비스에 접속해보니.. 와우! 정말 많은 정보가 있습니다! 해당 서비스에 접속해 파일들을 조사하고, 숨겨진 FLAG를 찾아나세요!
<br/>
The word "Deep" in the challenge name is hinting to the Deep Web that might act as the server for the admin panel. Since deep web URLs often end with ```.onion```, I started with searching for that string first in the directory.  
![Deep-1.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Deep-1.png)<br/><br/>

We see there there's an onion link in the ```.bash``` file. I moved to TOR browser and pasted the link directly. Here's what we got:  
![Deep-3.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Deep-3.png)<br/><br/>

I proceeded to have a walk around the website. After clicking on the "open dashboard", it led me to some files being displayed on the dashboard. By clicking on a file called ```stolen_payment_records.csv```, we immidiately got our flag:  
![Deep-4.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Deep-4.png)<br/><br/>

**Flag: FLAG{R@CC0ONTHEBEST}**<br/><br/>

### #14 Ransomware Artifact
> 공격자가 다음 랜섬웨어 사용할 랜섬웨어 바이너리(예, ransom_loader.exe)를 조사한 후 SHA256 해쉬값을 알아내세요!
<br/>
I was pretty hesitant to download the ```.exe``` file at first because what if it infected my laptop?? I decided not to run it and have a look at the file type first:  
![Exe.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/%EA%B3%B5%EA%B2%A9%EC%9E%90%20%EC%B6%94%EC%A0%81/Part2/Exe.png)<br/><br/>

Lol I was worried for nothing. Ok so moving on to the main point of the question, it is actually straight-forward. All we need to do is hash the file and submit the hash value as our flag.  
```
SHA256 ransom_loader_v2.exe  
```
<br/>
**Flag: e4c1572b153b10ed540f415dc436a87c7b46f0965daaa3ac98df3072925013e8**<br/><br/>

Yayyy all done!
