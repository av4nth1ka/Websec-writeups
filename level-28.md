```
<?php
if(isset($_POST['submit'])) {
  if ($_FILES['flag_file']['size'] > 4096) {
    die('Your file is too heavy.');
  }
  $filename = './tmp/' . md5($_SERVER['REMOTE_ADDR']) . '.php';

  $fp = fopen($_FILES['flag_file']['tmp_name'], 'r');
  $flagfilecontent = fread($fp, filesize($_FILES['flag_file']['tmp_name']));
  @fclose($fp);

    file_put_contents($filename, $flagfilecontent);
  if (md5_file($filename) === md5_file('flag.php') && $_POST['checksum'] == crc32($_POST['checksum'])) {
    include($filename);  // it contains the `$flag` variable
    } else {
        $flag = "Nope, $filename is not the right file, sorry.";
        sleep(1);  // Deter bruteforce
    }

  unlink($filename);
}
?>
```
+ $_SERVER['REMOTE_ADDR']gives the IP address from which the request was sent to the web server.
+ Here, the fopen function opens the file for read only,and real the file contents and write the file contents to $filename.
+ The code checks if the md5 hash of the file we uploaded is equal to the flag.php file which has our flag(strict comparison) and also checks if the checksum we entered is equal to the crc32 of the checksum.




References:
+ https://www.php.net/manual/en/function.fopen.php
+ https://www.php.net/manual/en/function.fread.php
+ https://www.php.net/manual/en/function.file-put-contents.php
+ https://www.php.net/manual/en/function.md5-file.php
+ https://www.w3schools.com/php/func_string_crc32.asp
+ 
