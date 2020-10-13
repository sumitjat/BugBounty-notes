# IDOR



  https://api.example.com/api/user/139349 - in which you supply the endpoint with a userid/guid,

  Features to test

 ---------------------------------------------------------


- **Opt out links**

 These sometimes just contain a userid arguement to opt out and will usually reveal the users emails.
 These can be found in emails they send you. Signup to all newsletters!

- File upload for oher user  through IDOR.
- Mobile Apps

  80% of IDOR findings are from mobile apps. Most mobile apps use a simple API system to log the user in,
  display their information etc. A lot of API's just take a userid parameter and will usually reveal all their
  information to you, as shown above in the example.

- Updating account settings

 Sometimes when updating your account settings, they'll send your userid as a parameter. Manipulating this can 
 sometimes result in another users profile being edited. Don't forget that if one feature is vulnerable to 
 IDOR then it may be a site-wide issue (and don't forget to check mobile site!)

- Reset password

  The same as above. Mongobug was able to do this on Uber but instead of a userID, 
  he had to supply the users number. Not hard to obtain though right and earned himself a nice $10k,
  check it out here: https://hackerone.com/reports/143717

- Anytime you see some sort of indentifer

  This can mean a simple ID (1), or a guid (425d1126-4139-4067-ac68-d9caafdf2b46), or some other value. The key is to look for 
  values which identifies you when interacting with the site/API, then to check if you can provide another users ID 
  (your second testing account), and if yes, you have a bug. If it's something like a GUID then you should look for 
  potential places it may be leaked on the site (one key tip is it's sometimes in in their photo image name. When a user uploads
  a profile photo and it's saved, they use the guid as an identifier :D)

- IDOR in adding items to the list

## Reports 

1 > Information DIsclousre via idor in the activate endpoint of website API

Link :- https://hackerone.com/reports/95552

In this he found the activate link we are using to activate our account is using some api to complete our request and in response it is giving us api key of our account .

```
Request Body 

POST /api/v3/organizations/5460d2394b793294df01104a/mopub/activate HTTP/1.1
```

```
Response Body 

{"mopub_identity":{"id":"5496c76e8b15dabe9c0006d7","confirmed":true,"primary":false,"service":"mopub","token":"35592"},"organization":{"id":"5460d2394b793294df01104a","name":"\u003Ca href=\"javascript:alert(1);\"\u003Es\u003C/a\u003E\u003Ch1\u003Etest\u003C/h1\u003E","alias":"img-srcx-onerrorprompt1s-projects2","api_key":"8590313c7382375063c2fe279a4487a98387767a","enrollments":{"beta_distribution":"true"},"accounts_count":3,"apps_counts":{"android":2},"sdk_organization":true,"build_secret":"5ef0323f62d71c475611a635ea09a3132f037557d801503573b643ef8ad82054","mopub_id":"33525"}}

```

Response will have api-key of that user and this is vulnerable to  IDOR 

2 --> Read Message using the IDOR 

Link :- https://medium.com/bugbountywriteup/bugbounty-how-i-was-able-to-read-chat-of-users-in-an-online-travel-portal-c55a1787f999


He Found end point in which can chat online in Forum . 

![Link ](https://miro.medium.com/max/1046/1*ILi8Z5ZesWD_lCWYsXFHHg.png) 

This was the response  

![Response](https://miro.medium.com/max/1043/1*XK5Z0yRDuSqik238WCbvMQ.png) 

 first value i.e hari013903158 was the customer id and the second value FL-132756 comes out to be the chat id which on further analysis found to be incremental. 

Then problem was that customerid is not incremental because it is alpha-numeric then “blog section” in rescue. In blog post of user customerid was there in the url . Now we can read chat history of any user.

3 --> IDOR in web socket

Link :- https://footstep.ninja/posts/idor-via-websockets/

A HTTP PUT request was first sent which notifies the other users of the comment (this was also vulnerable and I would write about it in an upcomming post!).

And a WebSocket request with the following body.

```
{ "command": { "operation": [ { "p": [ "slides", 0, "comments", 12 ], "li": { "author": "authorID", "content": " My Comment", "createdAt": "creationDate" } } ], "uuid": "commentUUID", "version": 1 }, "type": "operation", "channel": { "type": "Presentation", "uuid": "slideUUID" } }
```

replaced the authorID with that of another account I have control over. But that didn’t work 😩.

**Phir se padh samjh nhi aaya kya bol rha .**

3 --> IDOR in sending notification to user 

Link :- https://footstep.ninja/posts/idor-via-http/

----

4 > IDOR in adding secondary user in PayPal

Link :- https://hackerone.com/reports/415081

----

5 > IDOR in replying to comment

Link :- https://bugreader.com/jubabaghdad@add-comment-on-a-private-oculus-developer-bug-report-6

> I created public bug and added comment to my bug after that I replied to my comment and Intercepted the request with burpsuite to see what kind of parameters we have in this option, the request was like below:



```
POST /graphql?locale=user HTTP/1.1
Host: graph.oculus.com 

access_token=My-Acces-Token&variables={“input”:
{“client_mutation_id”:”1",”comment_parent_id”:”556190998150906",
”external_post_id”:”548709645565708",”message”:”what ever”}}&blablabla
```

----

6> IDOR in Hidden Place 

Link :- https://medium.com/@aseem.shrey/attention-to-details-a-curious-case-of-multiple-idors-5a4417ba8848

- IDOR in Downloading the pdf  
 http://api.whereIDORsLive.com/XYZService/EticketPdf/bookingId.pdf

- Second One :- 

http://cloud.whereIDORsLive.in/XYZService/dboperation.svc

this was leaking all endpoint 

![](https://miro.medium.com/max/2333/1*xeEM_6Lmm8SrJX7ejhmQdA.png)

<br>
/GetETicket/{TransactionscreenID}/{UserName}/{Password}/{ProcessType}

he tried with his own trans id , username  and password in url info was there of his account
he changed id only and it was showing data for other user. first  he tried processType  =1 
and after that he tried 2 and it was leaking another info 


-----

7> IDOR Via Parameter [Pollution ](https://blog.mert.ninja/twitter-hpp-vulnerability/)

you can unsubscribe the twitter notifications by clicking the "unsubscribe" button in footer of mail

```url
https://twitter.com/i/u?t=1&cn=bwvzc2fnzq%3d%3d&sig=647192e86e28fb6691db2502c5ef6cf3xxx&iid=f6529edf-322d-xxx-b99a-067876dfe799&uid=1134885524&nid=22+26
```

changed uid but no success

> https://twitter.com/i/u?t=1&cn=bwvzc2fnzq%3d%3d&sig=647192e86e28fb6691db2502c5ef6cf3xxx&iid=f6529edf-322d-xxx-b99a-067876dfe799&uid=2321301342&uid=1134885524&nid=22+26

Success 


----

## How Source code reading helped me [FIND AN IDOR](http://hack4bounty.com/how-source-code-reading-helped-me-find-an-idor/)

```http
POST /php/filter_user_tab_content.php HTTP/1.1`
Host: www.example.com`
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://www.example.com/
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 92
Connection: close
Cookie: XXXXXXXXXXXXX


user_id=1234&tab=address
```

Changed user_id but didnot work for other.


```http
POST /php/filter_user_tab_content.php HTTP/1.1`
Host: www.example.com`
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://www.example.com/
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 92
Connection: close
Cookie: XXXXXXXXXXXXX


user_id=1234&tab=address
```

checked for address also . Again Found Nothing in response 

From the profile page, I already know that , it can have , orders,address,credit cards etc but none of them are vulnerable.

He Found in Source code of page 

```html
<a href="x" data="orders">orders</a>
<a href="x" data="address">address</a>
....
....
....
....
....
<!-- //commented
<a href="x" data="secret">secret</a>
-->
```

Something that was commented . He changed secret in tab and changed user_id found secret info 

----

## PII Leak in [Winni](https://addictivehackers.blogspot.com/2020/06/buying-gift-can-cost-you-your-pii-data.html)

While selecting the address one POST request made to fetch the address in reference to  addressId.


```
 POST /checkout/adv/address/select-previous HTTP/1.1
Host: www.winni.in
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:77.0) Gecko/20100101 Firefox/77.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 16
Origin: https://www.winni.in
Connection: close
Referer: https://www.winni.in/checkout/adv/address
Cookie: AWSALBTG=XXX

addressId=685945
```

----

## Some [IDOR](https://medium.com/bugbountywriteup/stories-of-idor-4966369e6d82)

Part 1: IDOR Can Able To view Other User Account Details
Part-2: IDOR : Can Unsubscribe Anyone User Email From Subscription list
Part-3: Open Mail Relay Identified: Can Send Spoof Email To Victim From Authentic Whocare.com Mail Server

----

## IDOR can take company [down](https://ninadmathpati.com/how-critical-is-idor-vulnerability-can-it-take-down-a-whole-company/)

![s](https://ninadmathpati.com/wp-content/uploads/2019/05/kangaroo1.png)

<br>
We tried DELETE method 
<br>
![s](https://ninadmathpati.com/wp-content/uploads/2019/05/kangaroo2.png)
<br>
Now if we are able to fetch information by GET and erase the information by DELETE, then why not give a try to PUT method.

<br>
Here if we use the PUT method, we are able to edit/change the customer’s information, like first name, last name, dob…etc, or it can be anything, so how can it lead to full account takeover

There are two ways we can do that,
1) Using the PUT method change the password
2) Using the PUT method change the email and contact no




----

## IDOR + [Dork](https://medium.com/@totmukesh/hack-till-your-last-breath-3e58f4fb1738)

<br>
![e](https://miro.medium.com/max/875/1*dG87sNDhN59couZlWLyOrA.jpeg)

Changing userid with victim lead to change of email in the victim id but userid is not predictable and later he used google dorking 

site:redacted.com inurl:”xxxxxxxx”


By Using above dork he can use the uid to change email and that will lead to the Full ATO.

---

## Idor in google [product](https://medium.com/@balook/idor-in-google-datastudio-google-com-f2fa51b763de)

```
POST /u/4/deleteShareable?appVersion=20190926_020020 HTTP/1.1
Host: datastudio.google.com
Connection: close
Content-Length: 54
Sec-Fetch-Mode: cors
Origin: https://datastudio.google.com
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Content-Type: application/json
Accept: application/json, text/plain, */*
encoding: null
Sec-Fetch-Site: same-origin
Referer: https://datastudio.google.com/u/4/navigation/reporting

Cookie: RAP_XSRF_TOKEN=ACQ5uE-fZxoHyJIMJ6I9fWifDGZzjTeHCw:1569756166600; gh_7510439=;
{“id”:”9c491b49-a2f7–49fe-bd91-c4783657781",”type”:0}
```

id parameter was vuln . 


----

IDOR to expose full name belonging to another [account](https://hackerone.com/reports/332381)

Create an account
As an admin, go to create a new user (https://account.newrelic.com/accounts/1523936/users/new)
Create a new user with this email: newrelic@gmail.com
Notice that when you create the account, the name of the account owner is hidden:


Now, navigate to the internal API endpoint here: https://alerts.newrelic.com/internal_api/1/accounts/%7BYOURACCOUNTNUMBER%7D/users/
Notice that the full name is exposed at this JSON endpoint:

---

IDOR through traditional JSON wrapping method {“appid”:{“appid”:1998}}

---

http://site.com/v1/user/aditya.bug?action=view_key

change aditya to username IDOR 

---








These IDOR lub ( understand the app first ) 
https://medium.com/@pratyush1337/the-art-of-idor-7-idors-in-edm0d0-b86d683c8de9


Link :- https://medium.com/@corneacristian/top-25-idor-bug-bounty-reports-ba8cd59ad331s

