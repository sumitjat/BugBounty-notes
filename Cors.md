# Facebook Bug Bounty 

1 .>  An Instagram account can be reactivated without the 2FA (2FA was enabled in time of deactivation 
         
2 .>  In Business Instagram account 2FA code is not reqiured to login

3 .>  One user Can reply to story in which reply was off

4 .> one can message in live stream because of reaction feature in which we can send emoji in live stream 

5 .>  [Link  ] (https://medium.com/@0xBarakat/broken-session-permanent-access-to-facebook-users-cfed68684113)

In this there was donate link in which you can donate 

> https://m.facebook.com/donation/login/?nonce=xxxxxx&uid={USER_ID}

when go to Facebook.com in your Web Browser even if you didn’t signed in with your Facebook account the Facebook will automatically redirect you to your Facebook-Account without any password or any authentication !!

6 .>  Link :-  https://bugreader.com/jubabaghdad@how-i-found-a-simple-bug-in-facebook-without-any-test-15

There was a misconfiguration in Facebook Events, where blocked users and users who left the group can still get notification from that event after deleting it by the event owner, I found this bug without any test because I got a notification for deleted event from a closed group that I am ex-member in it.

----

7 > There was a misconfiguration in Facebook messenger infrastructure in the parameters that handle the attachments IDs (video,image,file and audio message IDs), where there is no restrictions on these IDs


Link :- https://bugreader.com/jubabaghdad@disclose-private-attachments-in-facebook-messenger-infrastructure-15000-5

- Attacker will upload any file in facebook chat (Facebook main chat, messenger, workplace chat or Portal Facebook chat) it doens't matter since it the same.

- Once he upload a file (let's say he upload an image), then he intercept the request with Burpsuite, the request will be like below:

```
POST /messaging/send/ HTTP/1.1
Host: www.facebook.com

client=mercury&action_type=ma-type%3Auser-generated-message&ephemeral_ttl_mode=0&has_attachment=true&image_ids[0]=123456&message_id=...etc
```

change his value of image_ids[0]= with victim image ID value,

image_id[0] ==> file_id[0]     ====> if he want to disclose files

image_id[0] ==> video_id[0] ====> if he want to disclose videos

image_id[0] ==> audio_id[0] ====> if he want to disclose audio messages


----

8 > an attacker can add friend requests of any facebook user into his own account.

Link :- https://bugreader.com/familyguy@intercept-incoming-friend-requests-of-victim-addaccept-to-your-facebook-17

create a new account with victim no. After that we send friend req to any account .
User was asked to confirm the mobile no. before thr friend req was allowed.

----

9 > Delete any comment on Facebook

Link :- https://bugreader.com/joebalhis@delete-any-comment-on-facebook-18

Remove your comment by usage of the standard function in facebook and intercept it 
Change your comment id with the victim comment id that you and change the legacy id

---

10 > Able to access group plan even after leaving the group

Link :- https://bugreader.com/familyguy@able-to-access-group-plan-even-after-leaving-the-group-19

create group add two user A (you) B and C. B leaves group , A and B changed venue or name C also receive notification.	

----

11 > Admin Disclousre in life event functionality

Link :- https://medium.com/@timpaxerror/page-admin-disclosure-via-an-upgraded-page-post-57863fb02c50

----

12 > Brute Force in Username while password is same

Link :- https://ysamm.com/?p=396

13 > While Commenting from page in fb lite comment was made from admin account

14 > Reply to Story with Allow reply off

https://bugreader.com/majd@158

- Post story with allow reply off
- attacker report that story so we can get story id 
- post your post as story and visit that post from story 
- it will have post id change with the story id
- we can comment in it 

15 > https://medium.com/@ashokcpg/non-technical-write-up-on-my-second-bounty-of-1-000-from-facebook-74daecd6879b

when all the photos from that message were forwarded to my friend

16 > https://thezerohack.com/hack-instagram-again

read it 


17 >  Deleting other video polls

https://bugreader.com/testgrounds@deleting-anyones-video-poll-175

- Upload a video to a Facebook page
- In the video editing page, go to the Polls tab and choose to create a new poll. Then submit the video.
- Go back and edit the video. Delete a poll and before hitting Save, intercept the request with a tool such as Burpsuite
- A POST request will be sent to /video/edit/dialog/save/?v=(VIDEO ID)&av=(PAGE ID)
- The vulnerable parameter in this POST request is:deleted_poll_ids[0] = (POLL ID)
- Replace your (POLL ID) with the victim's video poll id 
 
Victim Video poll will be deleted

----


https://medium.com/@yaala/bypassing-message-request-inbox-cf54f859dd25

18 >  

1- create a job and select to be contacted via message

2 - via job application contact yourself (message thread created)

3 - add anyone to conversation , victim receive message in his inbox instead of (message request or in spam )

----

## 19 > DoS Facebook Messenger Web(prevent chat [from]( https://bugreader.com/kbazzoun@dos-facebook-messenger-webprevent-chat-from-loading-187?fbclid=IwAR3slMk8iTnn6Av4lTGJAxWTqyDq3qzZ3TRsovgD8xGAsKFkEiBljpG6gYM) loading)

In Facebook pages, admins are able to place an order for users who previously contacted the page, this order will be sent to the user through the messenger , if we intercept the request before placing the order and changing the "product_image_url" to a crafted URL the Facebook Messenger on the victim side will fail to load chat and the page will stay showing the load button while its completely down in mobile version(e.g m.facebook.com ) , However user is able to use messenger on the app.

Steps 

1> "Kassem" opened the inbox of his page , then he select any user that is previously contacted the page (Victim who contacted the page)

2> in the right side he chooses "Add Activities " - > "Place order".

3> once he clicked on it , he looked at the conversation with this user , a message is appeared -"You confirmed that "username" placed an order. Send Details")

4>  Clicking on "Send Details" then a box is appeared to fill some information about this order , so "Kassem" filled these information and then intercepting the request and placing the order.

> https://scontent.fbey14-1.fna.fbcdn.net/v/t45.5328-0/c0.0.120.120a/p120x120/95350157_3378619175500882_8353993733280104448_n.jpg?_nc_cat=111&_nc_sid=c48759&_nc_ohc=BAbWYFEjXlQAX8kDMld&_nc_ht=https%3A%2F%2Fkassem.com&oh=7f76f8640eda580ccf82fbad50dc852b&oe=5ED12B9Cwww.kassem.com

[youtube](https://youtu.be/kMtsyzR8Djw)

----

## 20> Finding if Someone invited you to like page [is Admin is not ](https://atom.hackstreetboys.ph/determine-if-someone-who-invited-you-to-like-a-page-is-an-admin-or-not/?fbclid=IwAR0s9M3U1z16YBrnYWs8-H_WCH7MGV_2EV4bVI7LD1p_VKk87U57u6xR-js)

If He is From Page 

![j](https://atom(hackstreetboys.ph/content/images/2020/05/1.png)

if not 

![d](https://atom.hackstreetboys.ph/content/images/2020/05/3.png)

---

## 21> Create hidden comment by [blocking an Admin](https://medium.com/@saugatpokharel/able-to-create-hidden-comment-by-blocking-an-admin-facebook-bug-bounty-2020-c62bd10712f)

I went live on a page and made a comment through another account as a follower. I then realized that the follower user had blocked me (page admin) on Facebook. 

----

## 22> Disclose Page Admins via Facebook Camera [Effects](https://philippeharewood.com/disclose-page-admins-via-facebook-camera-effects/)

Facebook Camera Effects Hub is the area for creating and managing custom picture frames for Facebook. There is data in the hub that allows one to see an admin of a page that used a frame.

```
HTTP POST

/media_effect/create/dialog/?flow_type=basic_hub&step=edit&media_effect_id=MEDIA_ID
```

Once a page has used the frame the admin ID will be disclosed in the response.

----

## 23 > See unpublished jobs of any [page](https://asad0x01.blogspot.com/2018/03/see-unpublished-job-of-any-page.html)

Made a graphql call to the following endpoint and in response got Unpublished JOB of the page.

How I got the graphql call: While browsing Facebook APP I noticed a new option named "Manage Jobs".I clicked on it then I selected my page and it redirected me to my page's JOB list.After selecting page I intercepted the HTTPS request and noticed that it is making graphql call.


----

## 24> Getting other users IP [Address.](https://asad0x01.blogspot.com/2017/05/facebook-buggetting-other-users-ip.html)

1:Create a Shared Photo Album.
2:Add the victim.
3:Let the victim upload a photo in Shared Photo Album.
4;Once the victim uploaded photo in Shared Photo Album request for A Copy Of Your Facebook Data.
5:Once your Copy of Facebook Data is prepared Download it and Extract and Navigate to your Shared Photo Album .

---

##25> Leaking of page store details via [downloaded](http://whitehatstories.blogspot.com/2018/04/hi-this-post-is-regarding-one-of-my.html)

Page Admins can download locations data from Business Locations using the link "https://business.facebook.com/ajax/editpagesx/export_children.php?id=12345&intern_tool=false" where 12345 is the page id.

There were some sensitive data like Franchise, Store Number, Store Visits Measurement,Network Access Type, Location Descriptor etc  in the downloaded excel file which should be accessible only to the page admins. These details were exposed to anyone who exploits this bug.


----

## 26> Disclose the admin_id of any [page](https://medium.com/@sudipshah_66336/the-story-of-my-first-4-digit-bounty-from-facebook-3a29830e03cd)

1. User A goes to his PageX’s inbox through fblite and sees UserB’s message thread
2. UserA messages to User B
3.User B receives the text message done by UserA through page’s id
4. UserA now sends photo to UserB through the page inbox.
5. UserB receives the photo message through UserA’s personal profile id instead of the page id which leads to page admin disclosure.

----

## 27> Admin Discol in [m.fb.com](https://medium.com/@sudipshah_66336/the-story-of-my-first-xxx-bug-bounty-from-facebook-565a212c94ad)

Suppose USerA is admin of a Page . Go to m.facebook.com and login with UserA’s account.
2. Now , Go to your Page menu.Select Acting as [Page_name]
And post a text for example;”Hello World”

3. The text is posted by the page .

4 . Now Again Select Acting as [Page_name] option and try to post a photo or check in or
click on more options.
5. Then upload a photo or do checkin or more and Click on post
6. Then the post will be posted by UserA though we selected the option of
‘Acting as [Page_name]’

----

## 28> No Rate Limit in confirm no endpoint 

https://medium.com/bugbountywriteup/how-i-was-able-to-remove-your-instagram-phone-number-d346515e79c3

----

## 29 > Bypass the 'Allow Message Replies' setting in [Instagram](https://bugreader.com/majd@bypass-the-allow-message-replies-setting-in-instagram-158)

It's possible to bypass the "Allow Message Replies" setting in IG. You can grab the story media id and create a comment on it, even if it does not have a comment sticker.


-----

## 30> Crash Livestream in [Insta](https://bugreader.com/majd@crash-app-notification-section-and-livestream-105)

Find a user thats live streaming and send a question to their live stream
Capture the request and you will find a parameter:

&text=your_question

change the your_question part to a very large text

now forward the request


----

## 31 Admin Disclosure via faq [button](https://medium.com/@ashokcpg/weird-behavior-of-facebook-page-faq-leading-to-bounty-from-facebook-b4984e623b38)

While running ads on Facebook Page post, there is an option to receive messages with the message button embed. When someone clicked, it would greet the user with something like this,

And whenever the admin clicked on the FAQ section, It would go through the admin’s personal inbox to the user.

---

## 32 Cannot Revoke session in Messenger [kids](https://medium.com/@saugatpokharel/cannot-revoke-session-on-messenger-for-kids-facebook-bug-bounty-2020-9505ca201ec7)

The child actually had an active session running on the phone, but the parent user sees no active devices and hence cannot revoke the session of the child user. This is in fact little risky as the parent user would lose control over the child if they cannot revoke the session.

---

## 33 Temp disabling admin from receiving [notification](https://bugreader.com/vivekps143@169?fbclid=IwAR2bxiO0wBOXrT9jUGQYX9rMfrADh7_nFpfzGySA_fMvB1BPPmJHWE22oK8)

There is notification section in the page settings where a page admin can review all the comments, likes posts etc in a go. Admin can keep the page tidy and remove any abusive comments by this and can keep track of activities.
But if a page has only one admin and the admin is blocked by an attacker who interacts on the page, the attacker could temporarily disable admin from receiving the notifications of the page.

---

## 34 Tag Photo to any user’s Scrapbook on [Facebook](https://medium.com/bugbountywriteup/how-could-i-tag-photo-to-any-users-scrapbook-on-facebook-23ab15e6e4b4)

- Open Facebook Profile and Create any Post with photo.
- Now visit any User’s Scrapbook Album.
- Right Click in User’s Scrapbook album and copy Scrapbook ID.
- Now Visit your Post you created and open photo.
- Now Click photo and Click Tag photo option choose any user to tag.
- Now before tagging make sure Burp Suite’s Interceptor is turned on to capture the request.
- Click on “Choose user” now, you will see below kind of request in Burp suite:

```
POST /ajax/photo_tagging_ajax.php?av=100022637353520HTTP/1.1
Host: www.facebook.comConnection: closeContent-Length: 668Origin: https://www.facebook.comUser-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_0)fbid=XXX&id=USERID&subject=XXXX&name=XXXX&action=add&etc
```

8. Now change the subject parameter value to victim’s scrapbook ID and Forward the request. 

---
