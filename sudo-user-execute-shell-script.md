#### www账号ls脚本权限如下
```shell
-rwxr--r-- 1 www www  test.sh
```

#### 通过这样的方式无法执行
```shell
/bin/sh -c {sudo -H -u zzz /bin/bash -c "/bin/bash /server/www/test.sh"}
```

#### 查看相关账号的权限配置如下
```shell
/etc/passwd
www:x:48:48::/home/www:/sbin/nologin
zzz:x:48:48::/home/zzz:/bin/bash

# 用户名:密码(加密后为x):用户ID:组ID:用户ID信息(可以省略):用户目录:使用的shell
```

虽然www和zzz指向的是相同的用户ID和组ID,但是通过sudo -u zzz指定用户运行的时候,test.sh依然没有执行权限

在www用户下修改test.sh的权限
```shell
chmod 0755 test.sh
```
