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
parse_url: 
