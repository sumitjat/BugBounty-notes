# Information Disclosure 

One  Found Database.txt    
Found xmlrpc.php  
/admin/WS_FTP.LOG  
WS_FTP.LOG  
/errors/errors.log  
/server-status was leaking data    
 /dir --> 403   tried /dir/ --> found directory as index 
/Api/somedir --> nothing   
/Api/somedir/?filter=all --> information Leaking  
dboperation.svc

403 on /upload_panel
Bypassed by /upload_panel?stats=1
<br>
----
some good writeup for recon

For Shodan :- https://github.com/jakejarvis/awesome-shodan-queries
----
<br>
Link :- https://blog.usejournal.com/how-recon-helped-samsung-protect-their-production-repositories-of-samsungtv-ecommerce-estores-4c51d6ec4fdd
<br>
![This](https://miro.medium.com/max/550/1*N9W6DfGA6wxgKTiywV9aUA.png)

he found one in scrib something 


-----

## Elastic Search Exposed 

http://ghostlulz.com/elastic-search/

 try to find all the indexes(Databases) that are available. This can be done by hitting the **“/_cat/indices?v”** endpoint with a GET request.

/_stats/?pretty=1” endpoint.  This can be also used

To perform a full text search on the database you can use the following command “/_all/_search?q=email”

----

## GraphQL Exposed 

http://ghostlulz.com/api-hacking-graphql/

/graphql
/graphiql
/graphql.php
/graphql/console

Issuing the following requests will show you all the queries that are available on the endpoint.

>example.com/graphql?query={__schema{types{name,fields{name}}}}

use this according to previous 

>example.com/graphql?query={TYPE_1{FIELD_1,FIELD_2}} 

-----

### 1 -> 

  Link :- https://medium.com/@logicbomb_1/bugbounty-from-finding-jenkins-instance-to-command-execution-secure-your-jenkins-instance-9bd1e75c2288

 Every hack, every pentesting starts with recon (information gathering) so this - finding subdomain, open ports/services, public server IPs are some parts of
 it. In order to find public server IPs, one of the tools I generally rely on is https://censys.io/.

 server name here was Jetty:8080 which gave me a glimpse that it might have Jenkins running on it (usually because it’s running on a 
 standard port 80/443/8080/8443 on Jetty)

 he opened it and it was publicly accessible  

---------

### 2 ->  Leaking Info of other user when it collect activity

Link :- https://hackerone.com/reports/321249

- sign in the forum and send a comment on your dashboard/activities . the request should look like this:
```
POST /dashboard/activity/comment HTTP/1.1
Host: 9thsecurity.vanillaforums.com
User-Agent:
Accept: application/json, text/javascript, /; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Referer:
Content-Length: 132
Cookie:
Connection: close

TransientKey=LgFniB9ii5sAgDbG&hpt=&ActivityID=1021&Return=activity%2Fpost%2F127'&Body=anything&DeliveryType=VIEW&DeliveryMethod=JSON
```

- In this we can change value of activityID and that leads to leak info like the  email ip address of some  other user .

-----------------
### 3 ->  Million user Information Leaks

Link :- https://medium.com/bugbountywriteup/million-users-pii-leak-attack-288c5e37b283

He found out that they are storing image file in this url https://d3ez8in977xyz.cloudfront.net/avatars/009afs8253c47248886d8ba021fd411f.jpg

he visited this  _https://d3ez8in977xyz.cloudfront.net/_ and found the data of other in it.

------------

### 4> Finding customer data even password in plain text

Link :- https://medium.com/@saurabh5392/how-i-earned-by-finding-confidential-customer-data-including-plain-text-passwords-f93c4ce2631

used assetfinder tool by tomnomnom and fed to httprob found one subdomain 
Did dirsearch found url/application/logs 
 Found this log-09–09–2019.php download using log-09–09–2019.php.gz 

------------------------

### 5->  Information Disclosure through google dorking

Link :- https://medium.com/bugbountywriteup/a-simple-bypass-of-registration-activation-that-lead-to-many-bug-a-story-about-how-my-friend-5df0889f1062

 “github.com subdomain.targetname.com” From this he found one pattern 

>subdomain.targetname.com/Detail?trNum=${TRANSACTION_NUMBER}&e=${EMAIL}.

after that he did 

>site:subdomain.targetname.com AND inurl:trNum=

then try different value of e and trnum and give us information about transactions.

----------------------
### 6->  Access to all User

Link :- https://medium.com/@cc1h2e1/unauthorized-access-to-all-user-information-leaks-5db95746aecf

Found subdomain **api.redacted.com** run dirsearch found  system (JavaMelody)  found out it was leaking accesskey but not imp

console.redacted.com he done same to this domain  found endpoint in that data which redirect us to admin login page  after searching he found onsole.redacted.com/monitoring?part=sessions… 
and this was admin sessionid 

-----------------------------

### 7 ->  IP address disclosing in vine 

Link :- https://medium.com/bugbountywriteup/how-i-earned-5040-from-twitter-by-showing-a-way-to-harvest-other-users-ip-address-e9f43c931e9a

 I noticed a Endpoint what uses in Vine Repost mechanism have a Parameter Named “ipAddress” with some plain Number value Like :- 2130706433 . and this is ip address converted in decimal format

to reproduce you have repost the video after that copy reposted post id 
https://vine.co/api/timelines/users/1293308695089926144
now visit it. 

```
“repost”: { “username”: “██████”, “verified”: 0, “vanityUrls”: [], “created”: “█████”, “repostId”: ████████, “avatarUrl”: “██████”, 
“userId”: ████, “user”: { “username”: “█████████”, “verified”: 0, “vanityUrls”: [], “avatarUrl”: “█████████”, “userId”: ████, “private”: 0, 
“location”: █████████ }, “flags|platform_lo”: 1, “postId”: ███, “ipAddress”: 2130706433 , “flags|platform_hi”: 1 }
```
----------------------------------

### 8 -> leaking Sensitive Information

Link :- https://hackerone.com/reports/268888

https://cards-dev.twitter.com/keys/.

The application downloads a file json.json which discloses the following information
"customer_key":"████"
"customer_secret":"█████████"
"jira_password":"██████"


---

### 9 -> Information Disclosure from js file

Link :- https://medium.com/@Skylinearafat/how-to-look-for-js-files-vulnerability-for-fun-and-profit-78bfdfbd6731

In this linkfinder was not much of use he read js file by himself.
he found this //# sourceMappingURL=app.js.map

found this 

- //# sourceMappingURL=seller-join.js.map
- //# sourceMappingURL=done.js.map
- https://xyz.com/dist/seller-join.js.map
- https://xyz.com/dist/company-account/done.js.map --> Info was leaking
 
-----

### 10 -> From Github to Big issue

Link :- https://blog.securitybreached.org/2018/11/03/p1-like-a-boss-information-disclosure-via-github-leads-to-employee-account-takeover/

In this he found id password in github so when he try to login it is successful after that he logged in help.example.com he can see token and all of other user . id passwd is of high priv user {admin maybe}.

### 11 -> Info leaking through api endpoint

Link :- https://medium.com/@Skylinearafat/how-misconfigured-api-leaked-user-private-information-e3e8c13e52e4

He understood the docs.redacted.com and found there is url https://api.redact.io/service/<userID>
It is sensitive information .

He changed user ID to uid it shows error
He tried with username of user and it worked 
It was leaking information about that user.

----

12 > Travis Log 

Link :-  https://hackerone.com/reports/638401

Private key is printed in Travis Console log https://travis-ci.org/tronprotocol/java-tron/builds/361945077#L4101

----

13 > Hackerone Disclosing email 

Link :- https://hackerone.com/reports/792927

HackerOne has an invitation system that allows program owners to send invitations to users for various purposes, such as invitations to hack on private programs. This system allow us to invite by username or  email. If user is invited by username we must not know about email of that user

for reproduce create one program and one report and invite someone it disclosing the email

----

14 > Exif Geo Location 

----

15 > Email Disclousre using password reset link 

Link : https://medium.com/@godofdarkness.msf/users-email-disclosure-via-invalid-password-reset-link-250-c431ed46680e

> https://auth.reacted.com/account/password-reset?userid=1234&key=abcdef

In json response include email parameter and email address

```
{"valid":false,"email":"godofdarkness_msf@gmail.com"}
```

change userid see the response 

---

16 > IP Address Disclousre 

Link :- https://medium.com/@iframe_h1/a-picture-that-steals-data-ff604ba1012


---

17 > JS File Containing API

Link :- https://medium.com/@spade.com/api-secret-key-leakage-leads-to-disclosure-of-employees-information-5ca4ce17e1ce

Found one subdomain visited source code js file is there visited have bamboohr api key with auth key 

----

18 > Enumeration of username and userid 

Link :- https://hackerone.com/reports/752443

[Razer Pay] Broken Access Control at /v1/verifyPhone/ allows enumeration of usernames and ID information
The tester discovered an API endpoint with insufficient access control that could allow an adversary to obtain user name and phone number information.


----

## 19 > Information Disclos Via [GraphQL](https://blog.usejournal.com/graphql-bug-to-steal-anyones-address-fc34f0374417)

If you Send money to any user first backend will check whether that user exist or not  for that they are using status 


<br>
![image](https://miro.medium.com/max/1400/1*eySZuREyqkKX6G3DQWx7Zg.jpeg)
<br>

__typename:- Auth_User

https://api.stg.target.com/graphq?query={__schema{types{name,fields{name}}}} he Used this to find all the fields

<br>
![te](https://miro.medium.com/max/1400/1*4VY5yTtO90e0Jv5IgdBKUQ.jpeg)
<br>

He replaced status with address field and it returned the address for that user 

for more GraphQL 

for more GraphQL https://medium.com/@the.bilal.rizwan/graphql-common-vulnerabilities-how-to-exploit-them-464f9fdce696

----

## 20> PII using [bing](https://hackerone.com/reports/877598)

Perform a search on Bing: site: ███ AND "https://██████████Portals/22/Documents/Meetings/"
OR navigate to the provided links:
Detailed resume: https://███████Portals/22/Documents/Meetings/m14/█████.pdf


