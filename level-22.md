+Source code given.
```
<?php 
ini_set('display_errors', 'on');
ini_set('error_reporting', E_ALL ^ E_DEPRECATED);

if (isset ($_GET['code']) && is_string ($_GET['code'])) {
            $code = substr ($_GET['code'], 0, 21);
} else {
            $code = "'I hate PHP'";
}
?>
```
+ In the above code,when we input anything in the field 'code', it will check if the given input is string or not. Then, only the first 21 characters will be taken using the substr function.
+ if it doesnot follow the above conditions, I hate php message will be displayed.
```
<?php
class A {
    public $pub;
    protected $pro ;
    private $pri;

    function __construct($pub, $pro, $pri) {
        $this->pub = $pub;
        $this->pro = $pro;
        $this->pri = $pri;
    }
}

include 'file_containing_the_flag_parts.php';
$a = new A($f1, $f2, $f3);

unset($f1);
unset($f2);
unset($f3);

$funcs_internal = get_defined_functions()['internal'];

/* lets allow some secure funcs here */
unset ($funcs_internal[array_search('strlen', $funcs_internal)]);
unset ($funcs_internal[array_search('print', $funcs_internal)]);
unset ($funcs_internal[array_search('strcmp', $funcs_internal)]);
unset ($funcs_internal[array_search('strncmp', $funcs_internal)]);

$funcs_extra = array ('eval', 'include', 'require', 'function');
$funny_chars = array ('\.', '\+', '-', '"', ';', '`', '\[', '\]');
$variables = array ('_GET', '_POST', '_COOKIE', '_REQUEST', '_SERVER', '_FILES', '_ENV', 'HTTP_ENV_VARS', '_SESSION', 'GLOBALS');

$blacklist = array_merge($funcs_internal, $funcs_extra, $funny_chars, $variables);

$insecure = false;
foreach ($blacklist as $blacklisted) {
    if (preg_match ('/' . $blacklisted . '/im', $code)) {
        $insecure = true;
        break;
    }
}

if ($insecure) {
    echo 'Insecure code detected!';
} else {
    eval ("echo $code;");
}
?>
```
+ In the above code,creates a blacklist of functions, characters, and variables that are not allowed in the input code. The blacklist includes a list of PHP's internal functions, some extra functions (eval, include, require, and function), some "funny" characters (\, +, -, ", ;, '```', [, and ]), and some PHP superglobals and other variables that could be used to exploit the system.
+ $funcs_internal will contain an array of all internal PHP functions, which is later used to create a blacklist of functions that are not allowed in the input code.
+ FUnctions which are not allowed: eval,include, require,function
+ chars that are not allowed: '\.', '\+', '-', '"', ';', '`', '\[', '\]'
+ variables that are not allowed: '_GET', '_POST', '_COOKIE', '_REQUEST', '_SERVER', '_FILES', '_ENV', 'HTTP_ENV_VARS', '_SESSION', 'GLOBALS'
`$blacklist = array_merge($funcs_internal, $funcs_extra, $funny_chars, $variables);`
By merging these four arrays, $blacklist will contain a complete list of functions, characters, and variables that are not allowed in the input code. The resulting $blacklist array will be used later in the code to check whether the input code contains any of these elements, and to prevent execution of insecure code.
+ So, we can access the $backlist array using the following payload:
payload: $blacklist{1}(since [] is blacklisted, we use the curly braces)
output: func_get_arg
+ So like wise, we can access all the blacklisted chars variables and functions. 
+ We can give parameter to the function using the following payload:
payload: 
