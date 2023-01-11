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
+ Anither thing to notice it is using loose comparison, so now it is easy to bypass
+ 
