# Race Condition 

----

1 > Race condition in voucher applying

Link :- https://hackerone.com/reports/759247

He bought gv and use it for redeem and send that req to turbo-intruder and after that set x-request:%s for race condition 

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=30,
                           requestsPerConnection=30,
                           pipeline=False
                           )

   for i in range(30):
    engine.queue(target.req, i)
        engine.queue(target.req, target.baseInput, gate='race1')


    engine.start(timeout=5)
   engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```


----

-> Race Condition bypassing team limit

Link :-  https://medium.com/@arbazhussain/race-condition-bypassing-team-limit-b162e777ca3b

There was one functionality in which we can invite the  5 user to connect send that req to intruder and increase the no of thread 15+ .

Increasing Threading to ~10 will send 10 request’s at the same time. this will generate a type confusion which bypassed their team limit.

---

-> https://josipfranjkovic.blogspot.com/2015/04/race-conditions-on-facebook.html 

Good One read it 

----

->  Race Condition lead to multiple creation on [API](https://medium.com/@Alone_Wwolf/rare-race-condition-p3-2adb4eb6b715)

1 — First you should make a new account from https://example.com/signup page
2 — after you create a new account go to this page https://example.com/account/edit and then update your email but just change your email not confirm it which mean just write your password and new email any email you want
3 — after you change your email but not confirm it go to this page https://example.com/account/api_keys/add and then fire up your brup and catch the request when you click on create API
1 —send the request to intruder and then drop the request from interceptor
2 — the option of intruder should
  1 — options -> Number of threaded = 50
  2 — positions- >clear
  3 — payloads → payloads type = Null
4 — payloads -> options -> generate = 20
6 — after you set the options above correct click on start attack
7 — you will see the intruder got 2 request as 200
8 — go back to https://example.com/account/api_keys page you will see there is two API has been created

----

