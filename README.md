The PHP Interpreter
===================

This is the github mirror of the official PHP repository located at
http://git.php.net.

[![Build Status](https://secure.travis-ci.org/php/php-src.svg?branch=master)](http://travis-ci.org/php/php-src)

Pull Requests
=============
PHP accepts pull requests via github. Discussions are done on github, but
depending on the topic can also be relayed to the official PHP developer
mailinglist internals@lists.php.net.

New features require an RFC and must be accepted by the developers.
See https://wiki.php.net/rfc and https://wiki.php.net/rfc/voting for more
information on the process.

Bug fixes **do not** require an RFC, but require a bugtracker ticket. Always
open a ticket at https://bugs.php.net and reference the bug id using #NNNNNN.

    Fix #55371: get_magic_quotes_gpc() throws deprecation warning

    After removing magic quotes, the get_magic_quotes_gpc function caused
    a deprecate warning. get_magic_quotes_gpc can be used to detected
    the magic_quotes behavior and therefore should not raise a warning at any
    time. The patch removes this warning

We do not merge pull requests directly on github. All PRs will be
pulled and pushed through http://git.php.net.


Guidelines for contributors
===========================
- [CODING_STANDARDS](/CODING_STANDARDS)
- [README.GIT-RULES](/README.GIT-RULES)
- [README.MAILINGLIST_RULES](/README.MAILINGLIST_RULES)
- [README.RELEASE_PROCESS](/README.RELEASE_PROCESS)

#Install PHP

## 创建目录
```shell
mkdir -p /var/soft/
cd /var/soft/
```
## 升级bison
```shell
wget http://ftp.gnu.org/gnu/bison/bison-2.6.4.tar.gz
tar -xvzf bison-2.6.4.tar.gz 
cd bison-2.6.4
./configure
make && make install
cd ../
## 升级re2c
#### 解决You will need re2c 0.13.4 or later if you want to regenerate PHP
```shell
wget http://sourceforge.net/projects/re2c/files/re2c/0.13.5/re2c-0.13.5.tar.gz/download
tar zxf re2c-0.13.5.tar.gz && cd re2c-0.13.5
./configure
make && make install
cd ../
```

## 安装 libmcrypt mhash mcrypt
#### CentOS 7 默认不包含这三个模块，所以得手动安装，注意mcrypt依赖前两者
```shell
wget http://sourceforge.net/projects/mcrypt/files/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz
tar -zxvf libmcrypt-2.5.8.tar.gz
cd libmcrypt-2.5.8
./configure
make && make install
cd ../
wget http://sourceforge.net/projects/mhash/files/mhash/0.9.9.9/mhash-0.9.9.9.tar.gz
tar -zxvf mhash-0.9.9.9.tar.gz
cd mhash-0.9.9.9
./configure
make && make install
cd ../
wget http://sourceforge.net/projects/mcrypt/files/MCrypt/2.6.8/mcrypt-2.6.8.tar.gz
tar -zxvf mcrypt-2.6.8.tar.gz
cd mcrypt-2.6.8
LD_LIBRARY_PATH=/usr/local/lib ./configure
make && make install
cd ../
```

## 安装基础扩展
```shell
yum install -y gcc gcc-c++ make cmake automake autoconf gd file bison patch mlocate flex \
diffutils zlib zlib-devel pcre pcre-devel \
libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel \
glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel \
ncurses ncurses-devel curl curl-devel libcurl libcurl-devel e2fsprogs e2fsprogs-devel \
krb5 krb5-devel openssl openssl-devel \
openldap openldap-devel nss_ldap openldap-clients openldap-servers \
openldap-devellibxslt-devel kernel-devel libtool-libs \
readline-devel gettext-devel libcap-devel php-mcrypt libmcrypt libmcrypt-devel recode-devel
```

## 创建临时安装目录
```shell
mkdir php7
cd php7
git clone http://git.php.net/repository/php-src.git
cd php-src
./buildconf
##### onfigure: WARNING: unrecognized options: --with-mysql, --enable-gd-native-ttf
./configure --prefix=/usr/local/php7 \
--with-config-file-path=/usr/local/php7/etc \
--with-mcrypt=/usr/include \
--disable-fileinfo \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-gd \
--with-iconv \
--with-zlib \
--enable-xml \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--enable-ftp \
--with-openssl \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--enable-zip \
--enable-soap \
--without-pear \
--with-gettext \
--enable-session \
--with-curl \
--with-jpeg-dir \
--with-freetype-dir \
--enable-opcache
make && make install
```

## 配置
```shell
cp php.ini-production /usr/local/php7/etc/php.ini
cp sapi/fpm/init.d.php-fpm /etc/init.d/php7-fpm
chmod +x /etc/init.d/php7-fpm
cp /usr/local/php7/etc/php-fpm.conf.default /usr/local/php7/etc/php-fpm.conf
cp /usr/local/php7/etc/php-fpm.d/www.conf.default /usr/local/php7/etc/php-fpm.d/www.conf
```
## 配置opcache
```shell
vim /usr/local/php7/etc/php.ini
zend_extension=/usr/local/php7/lib/php/extensions/no-debug-non-zts-20141001/opcache.so
```
#### 启动
```shell
/etc/init.d/php7-fpm start
```
#### 查看PHP版本
```shell
/usr/local/php7/bin/php -v

PHP 7.2.0-dev (cli) (built: Oct 21 2016 20:08:15) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.1.0-dev, Copyright (c) 1998-2016 Zend Technologies
```

## 修改环境变量
```shell
vim /etc/profile
```
####### 在文件末尾加上如下两行代码

```shell
PATH=$PATH:/usr/local/php7/bin
export PATH

PATH=$PAHT:/usr/server/php/bin 
```
 
####### 执行命令
```
source /etc/profile
echo $PATH
```
