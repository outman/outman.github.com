### mysql command line option quick

```shell
mysql -utest -p****** -htest.db.com -P33060 test --quick -Bse "SELECT CONNECTION_ID();"
```
 |
 v
```shell
Default options are read from the following files in the given order:
/etc/my.cnf ~/.my.cnf
The following groups are read: mysql client
The following options may be given as the first argument:
--print-defaults	Print the program argument list and exit
--no-defaults		Don't read default options from any options file
--defaults-file=#	Only read default options from the given file #
--defaults-extra-file=# Read this file after the global files are read

Variables (--variable-name=value)
and boolean options {FALSE|TRUE}  Value (after reading options)
--------------------------------- -----------------------------
auto-rehash                       TRUE
character-sets-dir                (No default value)
default-character-set             latin1
comments                          FALSE
compress                          FALSE
database                          (No default value)
delimiter                         ;
vertical                          FALSE
force                             FALSE
named-commands                    FALSE
local-infile                      FALSE
no-beep                           FALSE
host                              g1-off-ku-real.dns.ganji.com
html                              FALSE
xml                               FALSE
line-numbers                      TRUE
unbuffered                        FALSE
column-names                      TRUE
sigint-ignore                     FALSE
port                              3320
prompt                            mysql>
quick                             FALSE
......
```

#### config ~/.my.cnf
```ini
[client]
quick
```

```shell
mysql -utest -p****** -htest.db.com -P33060 test --quick -Bse "SELECT CONNECTION_ID();"
```
 |
 v
4547014 # your connection id.

NOTICE: --quick beofore -Bse
