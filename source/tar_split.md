### tar file split and merge
```shell
tar zcvpf - source_file_name | split -d -b 100m

# result
# x01 x02 x03
```

```shell
# merge
cat x**** >> new.tar.gz
```

