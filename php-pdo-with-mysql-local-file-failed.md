### pdo(php 5.2) with mysql (5.1) load data local infile not work

```php (not work)
1. $pdo->setAttribute(PDO::MYSQL_ATTR_LOCAL_INFILE, true);
2. $pdo->prepare($sql, array(PDO::MYSQL_ATTR_LOCAL_INFILE => true));
```

```php (√)
$pdo = new PDO($dsn, $username, $passsword, array(
    PDO::MYSQL_ATTR_LOCAL_INFILE => true,
));
```
