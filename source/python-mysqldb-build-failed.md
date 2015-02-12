####MySQLdb build failed
```
building '_mysql' extension
gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -Dversion_info=(1,2,4,'beta',4) -D__version__=1.2.4b4 -I/usr/include/mysql -I/usr/local/webserver/python-2.7.6/include/python2.7 -c _mysql.c -o build/temp.linux-x86_64-2.7/_mysql.o -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -fno-strict-aliasing -fwrapv
_mysql.c: In function ‘_mysql_ConnectionObject_Initialize’:
_mysql.c:602: 错误：expected expression before ‘)’ token
error: command 'gcc' failed with exit status 1
```

找到 _mysql.c 的602行附近，将下面代码的逗号位置，放到下一个 #ifdef 里面

```c
    if (!PyArg_ParseTupleAndKeywords(args, kwargs,
    #ifdef HAVE_MYSQL_OPT_READ_TIMEOUT
        "|ssssisOiiisssiOii:connect",
    #else
        "|ssssisOiiisssiOi:connect",
    #endif
        kwlist,
        &host, &user, &passwd, &db,
        &port, &unix_socket, &conv,
        &connect_timeout,
        &compress, &named_pipe,
        &init_command, &read_default_file,
        &read_default_group,
        &client_flag, &ssl,
        &local_infile, // 注意这里
    #ifdef HAVE_MYSQL_OPT_READ_TIMEOUT
        &read_timeout
    #endif
    ))
    return -1;
```

改到
```c
    if (!PyArg_ParseTupleAndKeywords(args, kwargs,
    #ifdef HAVE_MYSQL_OPT_READ_TIMEOUT
        "|ssssisOiiisssiOii:connect",
    #else
        "|ssssisOiiisssiOi:connect",
    #endif
        kwlist,
        &host, &user, &passwd, &db,
        &port, &unix_socket, &conv,
        &connect_timeout,
        &compress, &named_pipe,
        &init_command, &read_default_file,
        &read_default_group,
        &client_flag, &ssl,
        &local_infile
    #ifdef HAVE_MYSQL_OPT_READ_TIMEOUT
        ,&read_timeout // 注意这里
    #endif
    ))
    return -1;
```