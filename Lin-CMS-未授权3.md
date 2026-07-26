# There is an unauthorized access vulnerability in version 0.2.1 of Lin-CMS Spring Boot. This vulnerability allows remote attackers to query books without authorization by exploiting the book query method in the `BookController.java` component. By directly accessing the `/v1/book/search` path, they can obtain detailed information about all books.

Vulnerable Class File: src/main/java/io/github/talelin/latticy/controller/v1/BookController.java



Line 58 of the file uses the GET request method to access the /v1/book/search route. Without any permission verification, the searchBook() method is triggered, directly calling the database to query all book details.

![image-20260726231134499](https://github.com/hhhh333/CVE/blob/picture/image-20260726231134499.png)

On line 58 of the file, a GET request is used to access the route `/v1/book/search`. It is worth noting that this endpoint lacks any form of access control or permission verification. As a result, it triggers the `searchBook` method. Therefore, an attacker can directly access this path, thereby gaining access to all book-related information.



POC for Sending a Request Without Any Permissions:

```
GET /v1/book/search HTTP/1.1
Host: localhost:5000
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-site
Priority: u=0


```

![image-20260726230518161](https://github.com/hhhh333/CVE/blob/picture/image-20260726230518161.png)

