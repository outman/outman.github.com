### mysql通过python导出大数据集

#### 比较重要的属性设置

```python
MySQLdb.cursors.SSDictCursor
MySQLdb.cursors.SSCursor
```

#### 获得cursor以后，设置cursorclass属性
```python
cursor.cursorclass = MySQLdb.cursors.SSDictCursor
```
