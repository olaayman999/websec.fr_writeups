## Introduction of Is_numberic function

Domestic part of the CMS program has been useful to the Is_numberic function, we first look at the structure of this function
```php
BOOL Is_numeric (mixed $var)
```
Returns TRUE if Var is a numeric and numeric string, otherwise FALSE is returned.


Second, the function is safe

Let's look at an example to see if this function is safe.
```php
$s = is_numeric ($_get[' s ')? $_get[' s ']:0;
$sql = "INSERT into test (type) values ($s);"; VALUES ($s) are not values (' $s ')
mysql_query ($sql);
```
https://github.com/olaayman999/websec.fr_writeups/blob/main/codes/level04_php_notes.md


The above fragment program is to determine whether the parameter S is a number, is to return a number, not to return 0, and then brought into the database query. (This will not construct the SQL statement)
We convert ' 1 or 1 ' to the value of the ' s ' parameter to base16 0x31206f722031
After the program runs, we query the database to see, such as:

If you re-query the field of this table out, do not filter into another SQL statement, will cause 2 injections.

Third. Summary
Try not to use this function, if you want to use this function, it is recommended to use the canonical SQL statement, conditional add single quotation marks, so that the 16 binary 0x31206f722031 will be displayed in the database. No 1 or 1 will appear.

------------------------------------------------------------------------------------------------------------------------

The is_numeric function in PHP can return true for a variety of numeric formats, including:

Decimal numbers (e.g. 42, -123, 3.14)
Exponential notation (e.g. 6.62607e-34, -1.6e5)
Octal numbers (e.g. 0755, 0377)
Hexadecimal numbers (e.g. 0x10, 0xFF)
Additionally, the is_numeric function can return true for numeric strings that contain leading and trailing whitespace, as well as numeric strings that include thousands separators (e.g. 1,000,000).

It's important to note that the is_numeric function returns true for any string that represents a valid numeric value, regardless of the data type used to store the number.

------------------------------------------------------------------------------------------------------------------------

## what is php's garbage collector
The garbage collector in PHP is a mechanism that automatically frees up memory that is no longer being used by the program.

In PHP, variables are dynamically allocated and automatically deallocated when they are no longer referenced by the program. However, in some cases, the memory occupied by a variable cannot be immediately released because it is still referenced by other parts of the program. The garbage collector in PHP is responsible for detecting and freeing up this unused memory.

The garbage collector runs periodically and frees up memory occupied by objects that are no longer referenced by any part of the program. This helps to prevent memory leaks, which can cause the program to run out of memory and crash.

The PHP garbage collector is enabled by default and runs automatically. However, it is possible to configure the garbage collector, for example to change the frequency with which it runs, or to disable it entirely. However, this is not recommended, as the garbage collector is a crucial part of the PHP memory management system and helps to ensure that the program runs efficiently and does not run out of memory.

------------------------------------------------------------------------------------------------------------------------

## what is serialization in php

Serialization in PHP is the process of converting a PHP data structure, like an array or an object, into a string representation that can be stored or transmitted and later reconstructed back into the original data structure. This is useful when you want to persist complex data structures across multiple page loads or transmit data between different scripts.

The serialization process can be performed with the `serialize` function, which takes a variable as an argument and **returns its string representation**, and the reverse process can be performed with the `unserialize` function. The format of the string representation depends on the data structure being serialized and can contain information about the type and structure of the data.
