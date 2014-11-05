### pdo(php5.2.8) 多次 query 后, fetch 失败的 bug .
```php
$pdo = $this->pdoInstance("hp_slave"); 
$stmt = $pdo->query($sql);
if ($stmt) {
    $mx = $stmt->fetch(PDO::FETCH_ASSOC);
    $stmt->closeCursor(); // 注意这里，问题在下面的 while 处。
    // $stmt = null ; // 修复方案，其他方案见参考链接
    if ($this->debugStep) {
        $this->debug(json_encode($mx));
    }

    if ($mx && $mx["minId"] && $mx["maxId"]) {

        while ($mx["minId"] <= $mx["maxId"]) {
            $stop = $mx["minId"] + 100000;
            $sql = "SELECT CreatorId, OperationType, Message, Type 
                FROM house_source_operation 
                WHERE OperationId BETWEEN '{$mx["minId"]}' AND '{$stop}'; ";

            if ($this->debugStep) {$this->debug($sql); }
            $stmt = $pdo->query($sql);
            if ($stmt) {

                // 注意下面这个 while 中的fetch ，在没有上面修复方案的时候，$val 是 false
                // 采用 while 取值目的是通过 PDO::MYSQL_ATTR_USE_BUFFERED_QUERY 快速 dump 数据 ，而没有采用 fetchAll() 
                while ($val = $stmt->fetch(PDO::FETCH_ASSOC)) {

                    if (isset($this->opt[$val["OperationType"]], $accounts[$val["CreatorId"]])) {

#### 相似问题参考链接：
[1] http://amiteshkumar.wordpress.com/2010/08/24/php-pdo-error-fix-sqlstatehy000-general-error-2050/
[2] http://www.justskins.com/forums/35793-com-general-error-25815.html