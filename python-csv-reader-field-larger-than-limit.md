###python csv.reader field larger than field limit (131072)

一般报这样的错误就是字段太长导致的，而131072 / 1024 = 128k ,正常情况很难超过这个长度的．

本人遇到情况就是因为csv.reader读文件的时候，有个参数叫doublequote=true默认值，而住以\t

分割的文件中，某个字段出现了双引号(doublequote)，所以导致分裂错误，似的从该双引号一直到

结束阶段都是一个字段的值．所以一般遇到这种情况请查看你的数据文件是否正确．

SF[1] 上的解决办法，这个主要是解决真正的字段长度不够才需要的，而大部分时候我们都是不需要的．
```stackoverflow
import sys
import csv

csv.field_size_limit(sys.maxsize)
```

[1]: http://stackoverflow.com/questions/15063936/csv-error-field-larger-than-field-limit-131072

