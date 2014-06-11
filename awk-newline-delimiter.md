###awk 没有正确使用换行符导致的一个 BUG

```awk
type[1]=A
type[2]=B

#lineStr是从文件读取的行数据的某列，且是最后一列例如 hello@word
split(lineStr, element, "@")

print type[element[2]] #取值失败

```
因为程序开始没有设定 RS，而且文本文件写入的时候换行符写的是 \r\n, 这样导致 element[2] 的结果是 word\r
所以取值失败。