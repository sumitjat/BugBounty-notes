# Account Takeover , Authentication Bypass , Other Bypass



1> Account Takeover

 [Link ] (https://pwnsec.ninja/2019/09/14/how-i-found-a-simple-and-weird-account-takeover-bug/ "pwnsec ninja how i find simple and weird account takeover")

 in this bug 2fa of two only which is by authenticator app or  Yubikey otp 

 but in recovery time we can set our no. if you cannot login by above method so in this one no. will receive otp in no. 


 this functionality working like if you ask for your otp

 request goes like 

```json
{"loginID":"XXXXX","sendCodeTo":"xxxxxxx"}
```

 so sendcodeto is unique code assigned for each unique mobile and otp will be send to mobile no. assigned to that sendcodeto id 

 he created another acccount req same and copy sendcodeto id for that no. and change in victim req when req for otp  .



2>   __Rest Framework Admin pannel bypass__

[Link] (https://medium.com/@hackerb0y/rest-framework-admin-panel-bypass-and-how-i-recon-for-this-vulnerability-a0ee41b01102 "Rest Framework Admin pannel bypass")
 
by sub domain finding which is for admin 
so first it have some error of code but after directory brute forcing we find site.com/admin 

it required admin login

again dir brute we can **_account_mangr_**

and we can view that he goes to login of that and login with detail of simple user and it accepted id password for admin pannel 


3>**Password Bypass**

[Link] (https://medium.com/@Vibhurushi_Chotaliya/password-bypass-and-something-else-cded0847c9df "Password Bypass")

now date 25/12 i understood it in burp you can change the response too so he changed repsonse when we logged in successfully  in case of wrong password one.

he successfully logged in 100//

i don't understand this but he changed response and he logged in


4>

Link :-  https://medium.com/bugbountywriteup/bypassing-googles-fix-to-access-their-internal-admin-panels-12acd3d821e3


5> 2FA Bypass

Link :- https://medium.com/@lukeberner/how-i-abused-2fa-to-maintain-persistence-after-a-password-change-google-microsoft-instagram-7e3f455b71a1

 
 first he changed password after that still otp work for that account which he recieved before
 password change 
 

6>  Admin Pannel By JS File 

 [Link]( https://blog.yappare.com/2017/06/from-js-to-another-js-files-lead-to.html)

He accessed admin url but when you visit admin pannel you will be redirected . he does not like brute forcing hence he thinks about js file 
``` 
https://bountysite.com/admin/dashboard/js/*attack*.js 
```  
he found login.js

	



 7>  Verifying without otp

1.	Login to https://accounts.shopify.com/account
2.	Click Change Next to email
3.	Enter any new email address
4.	You'll see a message saying:
	Verification email sent
	We sent you an email to verify that you own "email@example.com". We'll change your email once you verify that you own it.
	with a link to resend the verification email or cancel the change.
5. Copy the resend link, it will look like this: https://accounts.shopify.com/email-change/<Confirmation-TOKEN>/resend
6. Go to https://accounts.shopify.com/email-change/<Confirmation-TOKEN>/ and the email will be verified even though you don't own it.




blog have other 2fa bypass :- 

https://gauravnarwani.com/two-factor-authentication-bypass/

9> 

Link :- https://hackerone.com/reports/737169

- Register as a user and signin and try to create a new email using one of the templates.
- you cannot do the above step as you need to verify the email.
- Now logout and again login, but now capture the request in burpsuite and intercept it's response.
- Change the false parameters => true.
- And the verification is bypassed.

10> Account takeover using unverified Fb account

in this he created fb account using phone no and and logged in through now website need email he gave email of already registered account and now his account is no more/\.

12> captcha Misconfig

Link :-  https://medium.com/@abhishake100/bypassing-captcha-17c59d37f459

sign-in button is disabled and is only enabled after we check I’m not a robot. Since it was disabled, i quickly right clicked on the button and clicked Inspect Element and changed the disabled parameter to enabled.
basically it is not even checking for captcha

13> OTP bypass in mobile application

Link :- https://medium.com/@vbharad/full-account-takeover-android-application-78fa922f78c5

He simply request for forgot password and intercepted the response and in response it was leaking the correct otp code which can be used to takeover account.



15 > Zseano account takeover 

Link :- https://medium.com/@zseano/its-all-in-the-detail-email-leak-account-takeover-thanks-to-waybackmachine-extensive-4be365580dd7

In website he found unsubscribe function in which it leaking the email of that id

>https://www.redacted.com/unsubscribe?1=38384&2=ABC12345DDW34W

In this url 1 value is id of user and 2 is encoded of value 1

He found that messaging user which have uid of other user so we can have id of any user .
<br></br>
After that he used waybackmachine in which he found unsubscribe2 in robots.txt in which if you give value of 1 and 2 it will redirect you to main page .
So he try to give value for other account and guess what he is logged in another account without any id password.

-----

16 >  2FA bypass 

Link :- https://medium.com/sourav-sahana/bypass-2fa-in-a-website-d616eaead1e3

POST request and in the body I found ‘reset_key’, ‘_csrf’, ‘email’, ‘password’ and ‘token’ parameters. ‘token’ is my 2FA code.
I deleted token parameter and it’s value. Then I forwarded the request. And Password Reset

----

17 > Account Takeover while Facebook login

- click on login with fb
- intercept req and change reposnse in reponse change email 
- you are logged in victim account 

--------

18 > Account Takeover due to Email Change Oa uth 

Link :- https://medium.com/@godofdarkness.msf/account-takeover-flow-in-mail-ru-s-ext-a-domain-150-8952e8078211

> I change gmail oauth’s email to another email .Even email change,I can login with my old gmail oauth.

first he create email using oauth and then changed to victim . victim reset password but he can still use old gmail auth 


----

19 > 

Link:- https://twitter.com/thedarkwayg/status/1246076729422049280?s=19

Request password reset
Enable 2fa 
Use that password reset link 
You will be redirected to the account

---

20 > 

Link :- https://gerbenjavado.com/the-race-to-the-top-of-a-bug-bounty-program/

```
POST /v2/authenticate.json?isGoogle=true HTTP/1.1
X-PUBLIC-API-Key: 1bc29b36f623ba82aaf6724fd3b16718
Content-Type: application/x-www-form-urlencoded
Content-Length: 21
Host: api.company.com
Connection: close

email=help@company.com
```

login as any user given I knew their email address.

---

21 > PayPal 2FA [bypass](https://henryhoggard.co.uk/blog/Paypal-2FA-Bypass)

- Login with a valid username and password, click on the “Try another way” link.
- Enter any answer for security questions.
- Using a proxy, remove “securityQuestion0” and “securityQuestion1” from the post data.

---

22 > Invitation can lead to change [password](https://medium.com/bugbountywriteup/business-logic-flaw-in-invitation-system-allows-to-takeover-any-account-at-private-company-daaf898966b0)

- Login to your account at https://app.redacted.com
- Now invite any email that you also control under “Users” section
- Click in the invite link and you’ll see a page where it will ask for you to create a new password for the account, intercept the request
- Just change the email to any other user account at your application and forward the request
- You have now set a new password for the “victim” email using an invitation token

---

23 > 5 way to ATO in single [target](https://medium.com/@vasuyadav0786/5-ways-to-do-ato-in-a-single-website-cfe7e5da987e)

### ATO 1-Through JWT Misconfiguration

JWT was there decoded it and it needed user id  tried to login with wrong password and checked the response of the request and it was leaking the user id

![a](https://miro.medium.com/proxy/1*BxFV-n8U1BNVkM4fzkgZtg.jpeg)

<br>

### ATO 2-Through OAUTH Misconfiguration

<br>

![a](https://miro.medium.com/max/875/1*xyEhkQ6YolGnmeYbEps3RA.jpeg)

<br>


### ATO 3- No Rate Limit on OTP Verification

### ATO 4-OTP Bypass Through Response Manipulation

### AT0 5-IDOR on Reset Password 

Basically in response we change the id to that user 

---










