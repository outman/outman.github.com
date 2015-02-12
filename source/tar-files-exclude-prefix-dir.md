### tar 打包并压缩文件，排除文件前缀目录
/exclude/a/file1,2,3 是你要打包的文件

```shell
tar -czvf output.tar.gz -C /exclude_dir/a /exclude_dir/a/file1 /exclude_dir/a/file2 /exclude_dir/a/file3
```

