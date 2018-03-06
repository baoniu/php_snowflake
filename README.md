# php_snowflake
改自 https://github.com/Sxdd/php_snowflake
将原来的32位机器码变成了26位（两个long字段存下）

## Requires
* PHP >= 5.6  (Below 5.5 self-testing)
* Linux

### Non-thread-safe version (NTS)
```
0　0　　　　　　    13　　　　　　 　15   23　　　   26
---+----------------+--------------+----+----------+
   |timestamp(ms)  | service_no 　 |pid | sequence |
---+----------------+--------------+----+----------+
```

### Thread-safe version (TS)
```
0　0　　　　　 　   13　　　　　　 　15   23　　　   26
---+----------------+--------------+----+----------+
   |timestamp(ms)  | service_no 　 |tid | sequence |
---+----------------+--------------+----+----------+
```

## Installation
```
phpize
./configure --with-php-config=/usr/bin/php-config7.1 
make 
sudo make install 
sudo service php7.1-fpm restart
```

###

php.ini添加扩展

    extension = php_snowflake.so
    
## Example
Attention： Interval of $service_no in the range of 10-99. Beyond that scope, PHP will report a fatal mistake.
```
$service_no = 99;
for ($i=0; $i < 10; $i++) { 
        echo PhpSnowFlake::nextId($service_no)."\n";
}
/*

15098671568796600005457001
15098671512126600005457001
15098671509876600005308001

*/
```
