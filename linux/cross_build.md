# linux平台交叉编译相关知识

>常用相关编译有Autotools、cmake、meson

* Autotools
如果不存在configure文件
依次执行
`autoscan`
`aclocal`
`autoconf`
`autoheader`
`automake --add-missing`
生成configure
配置CC CXX等交叉编译工具链执行后执行 CC=... CXX=... AR=... ./configure

* meson
cross-build配置 [官网教程](https://mesonbuild.com/Cross-compilation.html)
执行步骤
**cross_profile:** 交叉编译配置文件
**builddir：** 编译路径
`meson setup builddir/ --cross-file cross_profile` 
`meson compile builddir` 如果使用ninja构建 等价于`ninja -C builddir`
`meson install` 如果使用ninja构建 等价 `ninja install`

* cmake
官方文档:<https://cmake.org/cmake/help/book/mastering-cmake/chapter/Cross%20Compiling%20With%20CMake.html>

## 如何编译arm版本的Python2.7

可以参照这个脚本 <https://github.com/sjkingo/python27-arm-xcompile/blob/master/build.sh>

步骤：
下载python2.7源码 <http://www.python.org/ftp/python/2.7.3/Python-2.7.3.tar.xz>
下载patch <https://github.com/sjkingo/python27-arm-xcompile/blob/master/files/Python-2.7.3-xcompile.patch>
进入源码目录 执行 `patch -p0 < ../files/Python-2.7.3-xcompile.patch`

首先编译当前机器的python和pgen
`./configure`
`make`
生成python和pgen后移动到指定目录

``` shell
mkdir host
mv python Parser/pgen host
```

编译arm版本python

```shell 
CC=arm-linux-gnueabi-gcc CXX=arm-linux-gnueabi-g++ AR=arm-linux-gnu
eabi-ar OBJDUMP=arm-linux-gnueabi-objdump STRIP=arm-linux-gnueabi-strip RANLIB=arm-linux-gnueabi-ranlib \
CONFIG_SITE=config.site ./configure --host=arm-linux-gnueabi --prefix=$HOME/usr/python2.7
make HOSTPYTHON=host/python HOSTPGEN=host/Parser/pgen HOSTARCH=arm-linux-gnueabi BUILDARCH=86_64-linux-gnu CROSS_COMPILE_TARGET=yes 
make HOSTPYTHON=host/python HOSTPGEN=host/Parser/pgen CROSS_COMPILE_TARGET=yes install
```
编译后会提示缺少的module 需要自己编译对应依赖库

拷贝$HOME/usr/python2.7至arm平台