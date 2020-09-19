# BugBounty Tip

> While Directory bruteforcing you may get 403 error so instead of url/FUZZ try url/__%2e__/FUZZ this may return the 200  and try for %0d also 

>  Whenever you see content type JSON always try to change content to XML and if accepted you know what to do .

> when try to test for web cache poisoning always try multiple directory it may not work in one directory

> Testing Ruby on Rails application ? Append .json to url endpoint this may return alot of info

>  when you’re looking for IDORs in a model that references another model, try storing IDs that don’t exists yet. I’ve seen a number of times now that, because the model can’t be found, the system will save the ID. (1/2)

> Use Round bracket to inject something in email like yourmail(./%<>)@gmail.com  or youtmail@(./%<>)gmail.com 

> FUZZ non printable character in input field like %00 to %FF  it may give is high information, memory leakage or other things .

> Use long value in id or password SQL might throw error for us

> Found app which embed the youtube video try xss by youtube link  
![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LbWrDBBrbM1WtGeIKRO%2F-LyYnOs7BvMFV3UbVbtf%2F-LymA6D4y4crwxfpmUWp%2Fimage.png?alt=media&token=6aef2334-592e-488c-b106-755c3d96cf39)

> Need to find uuid for some specific user try to register with that user email it may result uuid in error response 

> ![](https://pbs.twimg.com/media/EHpPwPnWsAA6XZ2?format=png&name=900x900)

<br>

> ![](https://pbs.twimg.com/media/EHKJEJMWoAAh2_c?format=png&name=900x900)

<br>

> ![](https://pbs.twimg.com/media/DwjSw48X0AAIxgg?format=jpg&name=900x900)

<br>

> Intercept the request and put email:victim mail%0d%0acc:hacker mail id. The server sends an email with CC attacker email.

<br>

> {“email”:[“victim@gmail.com”,”attacker@gmail.com”],”token”:”some random token”} Try this 

> scenario where password needed for confirmation 

- Check whether the password is properly validated?
- Try removing the old password parameter through burp suite
- Try providing different user’s password.
- Response manipulation.
- SQL injection.

> Never Forget Parameter Pollution can be Used in IDOR , XSS , SSRF ,access control and bypassing other things 

> Just by putting a '%c0' character in the URL, you get to know the name of the bucket.

> If upload is of png try to upload png file upload with svg content image/svg+xml

> site.com/sec 403    
site.com/sec/ 200
site.com/sec/.
site.com/sec//


