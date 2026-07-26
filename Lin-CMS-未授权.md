# There is an unauthorized access vulnerability in version 0.2.1 of Lin-CMS Spring Boot. This vulnerability allows remote attackers to query books without authorization by exploiting the book query method in the `BookController.java` component; by directly accessing the `/v1/book/{id}` path, they can directly obtain the relevant book details.

Vulnerable Class File: src/main/java/io/github/talelin/latticy/controller/v1/BookController.java



Line 41 of the file uses the GET request method to access the /v1/book/{id} route. Without any permission verification, the getBook() method is triggered, directly calling the database to query book details.

![image-20260726224723874](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260726224723874.png)

On line 41 of the file, a GET request is used to access the route `/v1/book/{id}`. It is worth noting that this endpoint lacks any form of access control or permission verification. Therefore, it triggers the `getBook` method, which queries the details of each book based on the `id`. Additionally, the `id` parameter follows a predictable and enumerable pattern. Thus, an attacker can traverse the `id` values to target every book currently stored in the database, thereby querying information related to each book.



POC for Sending a Request Without Any Permissions:

```
GET /v1/book/1 HTTP/1.1
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

![image-20260726225351613](C:/Users/86133/AppData/Roaming/Typora/typora-user-images/image-20260726225351613.png)





