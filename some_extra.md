# Some Extra 

Always Test extension in can contain xss,ssrf and other vulnerability.

1 .> CloudFare bypass 

Link : https://medium.com/bugbountywriteup/bypass-cloudflare-waf-to-pwned-application-2c9e4f862319

x.xyz.com/wp-login.php ?action=register  he was blocked by the cloudfare waf.
bypass their WAF and get Origin IP

he used [CFBYPASS](https://github.com/christophetd/CloudFlair) tool , after running this , i got their Origin IP.  After that he tried x.x.x.x/wp-login.php?action=register  and it is not blocked

for more cloudfare info https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/

-----

2 >  Leak through Referer 

Link :- https://thesecurityexperts.wordpress.com/2018/10/28/journey-through-google-referer-leakage-bugs/

3 > wp-admin unauthenticated can cause DOS too

Link :- https://baraktawily.blogspot.ru/2018/02/how-to-dos-29-of-world-wide-websites.html


4 > Snapchat invalid input validation 

Link :- https://blog.securitybreached.org/2020/01/26/improper-input-validation-add-custom-text-and-urls-in-sms-send-by-snapchat-bug-bounty-poc/

In one of the sub-domain whatis.snapchat.com
In that subdomain we can ask for link than snapchat will send link to download in our text message in mobile.


>https://app.snapchat.com/stories_everywhere/download_sms?phone_number=2133198570&country_code=US&cid=whatis

Whenever we request for link this was http request.
We can see cid was in request and also in our text message link .

He send cid=TES+HACKED+1337+LOL+HacKerOne.com and this lead to link which will open the hackerone.com for that victim.cid=TES+HACKED+1337+LOL+HacKerOne.com and this lead to link which will open the hackerone.com for that victim.

--------

5 > DOS due to long String 

Link :- https://medium.com/@shahjerry33/long-string-dos-6ba8ceab3aa0

create app and put field like username password or address like 1000 character of string . Search A's account from B's account either it will 

- Either it will keeping on searching for long time
- Either the application will crash (500 - Error Code)

----

6 > Long String in Password

Link :- https://hackerone.com/reports/783356


---

7 > profile-picture name parameter with large value lead to DoS for other users 

Link : https://hackerone.com/reports/764434

- Go to https://hackerone.com/settings/profile/edit
- Upload new profile picture and Intercept the request using Burp
- Add the payload text (attached with report payload.txt) at the starting of the filename in the above request e.g <payload>abcd.png

----

8 > Arbitrary cookie set via ;

Link :- https://hackerone.com/reports/806577

>curl -I "https://nordvpn.com/?coupon=HERE;+Cookie1=XXXXX;+Cookie2=WWWWWWW;+Cookie3=EOF"
 
----

9 > Payment Bypass

https://pratikyadav0.blogspot.com/2018/10/hello-everyone-took-some-time-from-my.html?m=1

1> select 

----

10 > SubDomain takeover to Authentication Bypass

Link :- https://hackerone.com/reports/335330

So he takeover devrel.Web.com
since cookie has scope of *.web.com we can do Xss on devrel
we can make cors from devrel to read message of web.com chat room through CORS
reating mail account using GSuite to send and recived emails on behalf of *@devrel.web.com

----

11 > SSRF in sub to calendar 

[link](https://medium.com/bugbountywriteup/bug-bounty-fastmail-feeda67905f5)

In Part SSRF was there in subscribe to calendar functionality 

----

12 > One param to [10k](https://medium.com/@bilalmerokhel/one-param-10k-9d80a33f5eb5)

saw an endpoint where a lot of parameters were there

```http
§CHECK§ /v2/§one§?§two§=§three§ HTTP/1.1
Host: api.reacted.com
Connection: close
Accept: application/json, text/plain, */*
authorization: Bearer {token}
x-reacted-client: web/7.76.2
x-connection-id: 94881378-eassb-4cf4-5569-fb5as334223b61
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.129 Safari/537.36
Origin: https://app.reacted.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
```

![t](https://miro.medium.com/max/1400/1*9TgmT6TCnJQfNvTnsUdpmA.png)

<br>

After bounty he tried One param everywhere but changing requests like changing GET to POST and POST to GET and just like that PUT to POST and GET, not just that let's play around with Content-Type and change the content type as application/JSON to application/x-www-form-urlencoded vice versa.

All of them are disclosing Password 

---------

13 > 2Fa bypass or [DOS ](https://bugreader.com/ahmad_halabi@broken-authentication-in-two-step-verification-112)

>Multiple users can't be able to login to their accounts if the two step verification is enabled on these accounts using a common phone number.


User 1 login and go to Two-step verification section and enter the above phone number.

User 2 login and do the same as step 1.

Two codes sent to my phone number.
User 1 enters the first code received.
User 2 enters the second code received.

The Two step verification is enabled Now.
Both users log out.

User 1 try to login, a code sent to the phone number, user enters it and login ( Login Failed )
User 2 try to login, a code sent to the phone number, user enters it and login ( Login Failed )

----

## > Github Creds to [login in Tesla](https://medium.com/heck-the-packet/how-i-got-my-first-big-bounty-payout-with-tesla-8d28b520162d)

He did recon found nothing used "tesla.com" pwd= 

found base64 encoded creds . He used those creds to login but MFA made it not possible . used the ServiceNow API . No MFA in api 

```
ServiceNow installations. While it is a cloud service, it often includes the use of something they call a “MID Server” which is responsible for pushing information into your cloud instance. This information includes Active Directory data (Users, computers etc.). This is how ServiceNow is able to link an Incident or a Helpdesk Ticket to a particular user. This is also how it allows engineers to plan maintenance on production systems as they’re also imported and managed within ServiceNow’s Configuration Item DB.
```

```
What this means is that in this instance, by querying the right table (and having the right access), someone could effectively build a map of all internal servers, their internal IP addresses, specs and descriptions.
```

----

## > Gain Access to Okta [Instances](https://medium.com/@eldeebxboy/how-i-was-able-to-chain-bugs-and-gain-access-to-internal-okta-instance-f2da9ab71367)

While searching shodan he found one host which redirect to OKTA instance . Did Censys search and found  ip for that which allow him to get directly to web page .

sudo allow admin to control slack invite user deactivate user …etc

found endpoint /slack/invite

```
POST /slack/invite HTTP/1.1
Host: redacted
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/json
Content-Length: 83
Origin: https://redacted
Connection: close
Referer: https://redacted

{"guestEmailId":"myemail","channelName":"the name i got ","guestType":"multi"}
```

He made request and found he was invited to channel In slack chat he found okta cred and which he used for okta login 

---

## > Improrer Auth lead to [takeover](https://twitter.com/th3insp3ct0r/status/1278454393516568577/photo/1)

<br>

![a](https://pbs.twimg.com/media/Eb37dfbXQAInld1?format=jpg&name=large)

<br>

---

as per RFC the maximum length allowed for an email address is 255 characters. However, we don't validate email length, so you can add email addresses that exceed 255 characters. Through this, if you sign up for an email with a length of 1 million or more and log in, withdraw, or change your email, the server may cause DOS due to overload.


---

https://youtu.be/cRDQWRxCr2s 

Amazon s3 takeover 

---

Too many redirect 

example.com/*/ or anything ask me for source 


 
