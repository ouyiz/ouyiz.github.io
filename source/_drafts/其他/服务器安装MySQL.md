1.下载并解压 MySQL 源码包
使用下面的指令。如果服务器下载慢，可以本地下载后上传，网址是一样的。
```
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-8.0.36.tar.gz
tar -xvzf mysql-8.0.36.tar.gz
cd mysql-8.0.36
```

2.配置编译参数（CMake）
```
mkdir build
cd build
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/mysql \
  -DWITH_BOOST=../boost \
  -DWITH_SSL=system \
  -DWITH_INNODB_MEMCACHED=OFF \
  -DWITH_UNIT_TESTS=OFF \
  -DDOWNLOAD_BOOST=1 \
  -DMYSQL_DATADIR=$HOME/mysql/data
```
运行时报错如下
<img src="file-20250514210843849.png" style="width: 500px; height: auto;">
然后发现那个文件根本不是压缩包
<img src="file-20250514211858411.png" style="width: 500px; height: auto;">
需要访问下述网站下载Boost 源代码，可以下载.tar.gz版本，然后放到mysql-8.0.36/boost中
```
https://sourceforge.net/projects/boost/files/boost/1.77.0/
```
解压压缩包
```
tar -xzvf boost_1_77_0.tar.gz
```
继续跑下述指令
```
cd ..                                   #为了退到mysql-8.0.36中
cd build                                #进入mysql-8.0.36/build
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/mysql \
  -DWITH_BOOST=../boost \
  -DWITH_SSL=system \
  -DWITH_INNODB_MEMCACHED=OFF \
  -DWITH_UNIT_TESTS=OFF \
  -DDOWNLOAD_BOOST=1 \
  -DMYSQL_DATADIR=$HOME/mysql/data
```

3.编译并安装
```
make -j$(nproc)    # 编译（时间较长）
make install       # 安装到 $HOME/mysql 目录下
```

4.初始化数据库
```
cd ~/mysql
mkdir data tmp logs
bin/mysqld --initialize-insecure --basedir=$HOME/mysql --datadir=$HOME/mysql/data
```

5.创建配置文件
创建一个配置文件 `my.cnf`，放在 `~/mysql` 目录下：
```
nano ~/mysql/my.cnf
```
内容如下：
```
[mysqld]
basedir=/home/fx2003/mysql
datadir=/home/fx2003/mysql/data
port=3306
socket=/home/fx2003/mysql/mysql.sock
log-error=/home/fx2003/mysql/data/mysqld.err
pid-file=/home/fx2003/mysql/data/mysqld.pid
skip-networking=0
skip-symbolic-links

[client]
port=3306
socket=/home/fx2003/mysql/mysql.sock
```
编辑完内容后`Ctrl + O → Enter → Ctrl + X`
5.启动 MySQL 服务
```
cd ~/mysql
bin/mysqld_safe --defaults-file=$HOME/mysql/my.cnf &
```

6.连接 MySQL
```
cd ~/mysql
bin/mysql -uroot -S ./mysql.sock
```

7.改密码
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
```
然后使用`exit`退出

8.关闭服务器
```
bin/mysqladmin -uroot -S ./mysql.sock shutdown
```

9.永久添加 PATH
你可以通过修改 `PATH` 环境变量，让系统自动识别你安装在 `$HOME/mysql` 下的 MySQL 命令（如 `mysql`、`mysqld`、`mysqladmin` 等），这样就不用每次都写 `bin/mysql` 了。
```bash
nano ~/.bashrc
```
在文件末尾添加一行：
```bash
export PATH=$HOME/mysql/bin:$PATH
```
保存并退出（`Ctrl+O` → 回车 → `Ctrl+X`） 
让更改生效：
```bash
source ~/.bashrc
```
验证是否成功
输入：
```bash
which mysql
```
应该输出：
```
/home/你的用户名/mysql/bin/mysql
```
以后连接mysql，使用下述指令：
```
 mysql -uroot -S ~/mysql/mysql.sock
```
