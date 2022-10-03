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
parse_url: parses the url and return its components.
parse_str: parses the string to variables.
stripos: finds at what position a particular string is present(case insensitive)
`$_SERVER['REQUEST_URI']` will return the URI of the webpage.
Eg: if the url is : https://www/google.com/example/example.php, then `$_SERVER['REQUEST_UR1']` will contain /example/example.php.
clue given in source code:  Yeah, the webserver is configured so that you can't directly access .txt files :)
                        And no, PHP wrappers aren't the only way to have fun!




