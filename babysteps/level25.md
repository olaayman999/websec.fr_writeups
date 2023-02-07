```php
<?php
if (!isset($_GET['page'])) {
  header('Location: http://websec.fr/level25/index.php?page=main');
  die();
}
?>
```
```html
 <!--Yeah, the webserver is configured so that you can't directly access .txt files :)
     And no, PHP wrappers aren't the only way to have fun!
  -->
```

A PHP wrapper is a script that serves as an interface between a PHP application and a specific external library or service. It allows the application to access the functionality of the library or service in a convenient and consistent manner, without having to worry about the underlying implementation details. Wrappers are used to simplify the process of integrating third-party services into a PHP application, and can provide a more user-friendly and intuitive interface for accessing these services.

The die() function in PHP is a language construct that immediately terminates the execution of the script and outputs a message. It's used as a quick and simple way to stop the execution of a script if a certain condition is not met. For example, if a function needs a specific argument to be passed and the argument is not provided, you could use die() to stop the script and output an error message:

```php
if (!isset($arg)) {
    die("Error: argument is not set");
}
```
Note that die() is similar to the exit() function in PHP, but exit() is a function and die() is a language construct. They both stop the execution of the script and output a message, but die() is slightly faster because it's a language construct.

```php
 <p class='well'>
                  <?php
                  parse_str(parse_url($_SERVER['REQUEST_URI'])['query'], $query);
                  foreach ($query as $k => $v) { /*loop through the elements of the $query array*/
                      if (stripos($v, 'flag') !== false)
                          die('You are not allowed to get the flag, sorry :/');
                  }

                  include $_GET['page'] . '.txt';
                  ?>
        </p>
```
This PHP script performs the following actions:

Parse the query string from the URL of the current request using the` parse_url()` function. The result is stored in the `$query` array.

Loop through the elements of the `$query` array and check if any of the values contain the string "`flag`". If a value contains "`flag`", the script outputs a message "You are not allowed to get the flag, sorry :/".

Include a file specified in the page parameter from the query string. The file name is constructed by concatenating the value of the page parameter with the string ".txt".

This code is a simple security measure to prevent unauthorized access to sensitive information contained in the included files. By checking if the query string contains the string "flag", the script attempts to prevent an attacker from accessing the contents of a file that might contain the flag.   

`stripos()` is a PHP function that performs a case-insensitive search for a needle (specified string) within a haystack (specified string). If the needle is found, the function returns the **position** of the first occurrence of the needle in the haystack as an integer. If the needle is not found, the function returns **false**.

```php
int stripos ( string $haystack , string $needle [, int $offset = 0 ] )
```
The function takes two mandatory parameters: `$haystack` and `$needle`. The `$offset` parameter is optional and specifies the position in the haystack to start the search.

Here's an example of using the `stripos()` function:

```php
$haystack = "Hello World";
$needle = "world";

$pos = stripos($haystack, $needle);
if ($pos !== false) {
    echo "The needle was found at position $pos";
} else {
    echo "The needle was not found in the haystack";
}
```
In this example, the function will return the position of the first occurrence of the needle "world" in the haystack "Hello World", which is 6.

Here are a few possible ways to bypass the stripos() function:

URL encoding: If the input is being passed in the URL, you could try URL encoding the string "flag". For example, the URL-encoded representation of the string "flag" is "%66%6C%61%67".

Case sensitivity: By default, stripos() performs a case-insensitive search, so you could try using a different case for the string "flag". For example, "Flag", "FLAG", etc.

Null bytes: Null bytes can sometimes be used to bypass string checks. For example, you could try appending a null byte to the string "flag". The PHP string "flag\0" would not match the string "flag" when searched using stripos().


```php
Copy code
foreach ($array as $key => $value) {
    // code to be executed
}
```
In this specific code, the $query array contains the key-value pairs of the query string from the URL of the current request. For each iteration of the loop, $k will be set to the key of the current array element, and $v will be set to the value of the current array element.

The purpose of the loop is to check if any of the values in the $query array contain the string "flag". If a value contains "flag", the script outputs a message "You are not allowed to get the flag, sorry :/".

the solution is in parse_url()
https://www.php.net/manual/en/function.parse-url.php

In versions prior to 7.10, putting ':' makes parse_url return false

![image](https://user-images.githubusercontent.com/72671239/217126741-b305c1ae-0d94-4f31-a4c4-737e46c942f6.png)

_flag=> WEBSEC{How_am_I_supposed_to_parse_uri_when_everything_is_so_broooken}_
