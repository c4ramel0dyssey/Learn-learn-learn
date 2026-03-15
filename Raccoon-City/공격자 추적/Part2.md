### #10 공격자의 Github 리포를 조사해봅시다.. 재밌는걸 찾으셨나요? 혹시 ... 공격자의 위치를 특정할 무언가를 찾을 수 있나요? 공격자의 머물렀던 지역을 특정하려 합니다. 특히 한국시간 KST 2025년 11월 13일 오전 11시 45분 25초에 공격자가 상주했을 "가능성"이 있는 나라는 어디일까요?  

I first started off by looking into the contributions made on the said date.  
![Geolocation-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/Geolocation-1.png)<br/><br/>

There seems to be 8 commits made in total. I reviewed the commits done to get a little insights into past actions, as well as the date for each one. By clicking on one commit and adding ```.patch``` at the end of its URL, the raw data of the commit will be displayed. This includes specific date and time of the action being done in the past. One of the commits bring us to this information:  
![Geolocation-1](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/Geolocation-2.png)<br/><br/>

Hmm the time is similar, and we have ```-0500``` at the end. This indicates the timezone when the action was committed. I asked gemini for the UTC time system being used for each of the answer options to know which one matches what we have here.  
1. Columbia: UTC -0500
2. United States West Coast: UTC -0800
3. South Korea: UTC +0900
4. Portugal: UTC +0000Armenia: UTC +0400<br/><br/>

Boom! Looks like we have our answer!  
**Flag: Columbia**<br/><br/>

### #11 공격자의 Github 리포를 조사해봅시다.. 혹시 재밌는걸 찾으셨나요? 이런! 공격자는 아주 "치명적인" OPSEC 실수를 저질렀습니다! 해당 정보를 사용하여 공격자의 리다이렉터 호스트로 접근 (Initial Access)하세요! 찾아낸 KEY를 통해 호스트 접근한 후 에 "숨겨진" FLAG를 읽어내세요. FLAG는 무엇일까요?  

Now, it's getting more interesting! We need to access the attacker's redirectory as host using leaked key.<br/><br/>

Looking at the history of commits, I soon discovered the serious mistake made by the attacker. The SSH private key is found to be among one of the earliest repositories posted in the Github!  
![Private Key-1.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/Private%20Key-1.png)<br/><br/>

In the same repo, we understand the username being used is called "spark." These information alone is enough to access the server as host with ssh command. All I did was copy and pasted the private key into one file and then use the ssh command such that: ```ssh private_key spark@140.238.194.224```.<br/><br/>  

![Private Key-2.png](https://github.com/c4ramel0dyssey/Learn-learn-learn/blob/main/Raccoon-City/Private%20Key-2.png)<br/>
Oh but wait... it then said that my private_key file is too opened. It's ok, we can easily change the permissions to the file to make it less open such that:  
```
chmod 600 private_key
```
<br/>
This way, only the owner has access towards the file. After that, I was finally able to access the host but oh, it then asked for a passphrase. I looked into the other repos and yup there it is:  


Using the passphrase, we are finally inside the redirectory! Hoorayyy. I tried using ```ls``` at first but there seems to not be that many files in the system.. So I tried ```ls -a``` instead and there's a file called ```flag.txt```.<br/><br/>

