### dpark example
```python

#!/usr/bin/env python2.7
# coding: utf-8

from dpark import DparkContext
from random import randrange

def make_file():
    with open("tmp.txt", "w") as fopen:
        for x in xrange(1, 1000000):
            fopen.write("%s\t%s\t%s\n" % (randrange(1, 10), randrange(1, 10), randrange(100000, 999999)))

if __name__ == "__main__":
    
    """
    9*9=81 key
    """
    
    make_file()
    dpark = DparkContext()

    """
    calc 81 key sum
    """

    rdd = dpark.csvFile("tmp.txt", dialect="excel-tab")\
        .map(lambda line: ((line[0], line[1]), line[2]))\
        .reduceByKey(lambda current, next: int(current) + int(next))\
        .map(lambda line: tuple([x for x in line[0]]) + (line[1],))\
        .saveAsCSVFile("result")

    # result/0000 content like
    # 1,1, 6787766
    # 1,2, 6787766
    # 1,3, 6787766

```