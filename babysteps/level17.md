```php
<?php
include "flag.php";

function sleep_rand() {                                 /* I wish php5 had random_int() */
        $range = 100000;
        $bytes = (int) (log($range, 2) / 8) + 1; /*3*/
        do {                                            /* Side effect: more random cpu cycles wasted ;) */
            $rnd = hexdec(bin2hex(openssl_random_pseudo_bytes($bytes)));
        } while ($rnd >= $range);
        usleep($rnd);
}
?>
```
https://github.com/olaayman999/websec.fr_writeups/blob/main/codes/level17_code1.md

```php
 <?php
if (isset ($_POST['flag'])):
        sleep_rand();                  /* This makes timing-attack impractical. */
                        ?>
 ```
 
 https://github.com/olaayman999/websec.fr_writeups/blob/main/codes/level17_code2.md
 
 
 ```php
 <?php
 if (! strcasecmp ($_POST['flag'], $flag))
           echo '<div class="alert alert-success">Here is your flag: <mark>' . $flag . '</mark>.</div>';   
else
            echo '<div class="alert alert-danger">Invalid flag, sorry.</div>';
                                ?>
```
compares the value of the "`flag`" parameter in a POST request with a predefined variable named "`$flag`".

The "`strcasecmp()`" function is used to **compare the two values case-insensitively**, meaning that the comparison is not affected by the case (upper or lower) of the letters in the values being compared.

If the "`flag`" parameter in the POST request = `"$flag"` output "Here is your flag: [flag value].” and the value of "$flag" is displayed, otherwise "Invalid flag, sorry."

by using strcasecmp manupulation, if the flag is of type array then the function is bypassed

https://www.youtube.com/watch?v=athpsjvgkWM


![image](https://user-images.githubusercontent.com/72671239/217106495-eeef3d23-e9e8-450e-b712-d0e0160f5857.png)


_flag => WEBSEC{It_seems_that_php_could_use_a_stricter_typing_system}_
