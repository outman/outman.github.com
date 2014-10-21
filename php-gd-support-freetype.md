### php gd lib support freetype in mac os x

- 因为已经支持自带了gd库，不支持freetype，又没办法移除，所以只能重新编译php，进行覆盖。
```php
brew install jpeg
brew install gd
brew install freetype
brew install libpng
```

phpinfo() 获得默认php的编译参数，在原来参数基础上增加下面的参数重新编译安装进行覆盖.
```
--with-freetype-dir
```
