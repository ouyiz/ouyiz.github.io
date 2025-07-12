## 一.安装TPC-H
1.克隆开源版本
```
git clone https://github.com/electrum/tpch-dbgen.git
cd tpch-dbgen
```

2.修改 `makefile`
```
nano makefile
```
找到对应行，改成下述
```
MACHINE = LINUX
```
编辑完内容后`Ctrl + O → Enter → Ctrl + X`

3.编译
```
make
```
成功后生成`dbgen`与`qgen`


## 二.使用TPC-H
### 1.选择数据规模
比如生成 1GB 的测试数据
```
./dbgen -s 1
```
- `-s` 指定数据规模（单位是 GB，`1` 表示 1GB 数据）
- 默认会在当前目录生成 8 个表的 `.tbl` 文件，如：
可以将它们移到一个文件夹中
```
mkdir tpch-data
mv *.tbl tpch-data/
```

### 2.把 `customer.tbl` 文件导入到 MySQL 中(其他同理)

> 注意
> 此处需要`~/mysql/my.cnf`中有`local_infile=1`，`[mysqld]`和`[client]`都要有

启动mysql
```
cd ~/mysql
bin/mysqld_safe --defaults-file=$HOME/mysql/my.cnf &
```
连接mysql
```
 mysql -uroot -S ~/mysql/mysql.sock --local-infile=1
 或者
 bin/mysql -uroot -p -S ./mysql.sock
```
建表，下述在MYSQL中操作：
```
CREATE DATABASE tpch;
USE tpch;
CREATE TABLE customer (
  c_custkey     INT,
  c_name        VARCHAR(25),
  c_address     VARCHAR(40),
  c_nationkey   INT,
  c_phone       CHAR(15),
  c_acctbal     DECIMAL(15,2),
  c_mktsegment  CHAR(10),
  c_comment     VARCHAR(117)
);
```
将 `customer.tbl` 导入 `customer` 表，假设`customer.tbl` 数据在`/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/customer.tbl`。下述在MYSQL中操作：
```
LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/customer.tbl'
INTO TABLE customer
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;
```
可以使用下述指令看是否成功
```
SELECT COUNT(*) FROM customer;
```
<img src="file-20250514231247268.png" style="width: 500px; height: auto;">

其他的
```
CREATE TABLE lineitem (
    l_orderkey INT,
    l_partkey INT,
    l_suppkey INT,
    l_linenumber INT,
    l_quantity DECIMAL(15,2),
    l_extendedprice DECIMAL(15,2),
    l_discount DECIMAL(15,2),
    l_tax DECIMAL(15,2),
    l_returnflag CHAR(1),
    l_linestatus CHAR(1),
    l_shipdate DATE,
    l_commitdate DATE,
    l_receiptdate DATE,
    l_shipinstruct CHAR(25),
    l_shipmode CHAR(10),
    l_comment VARCHAR(44)
);

CREATE TABLE nation (
    n_nationkey INT,
    n_name CHAR(25),
    n_regionkey INT,
    n_comment VARCHAR(152)
);

CREATE TABLE orders (
    o_orderkey INT,
    o_custkey INT,
    o_orderstatus CHAR(1),
    o_totalprice DECIMAL(15,2),
    o_orderdate DATE,
    o_orderpriority CHAR(15),
    o_clerk CHAR(15),
    o_shippriority INT,
    o_comment VARCHAR(79)
);

CREATE TABLE part (
    p_partkey INT,
    p_name VARCHAR(55),
    p_mfgr CHAR(25),
    p_brand CHAR(10),
    p_type VARCHAR(25),
    p_size INT,
    p_container CHAR(10),
    p_retailprice DECIMAL(15,2),
    p_comment VARCHAR(23)
);

CREATE TABLE partsupp (
    ps_partkey INT,
    ps_suppkey INT,
    ps_availqty INT,
    ps_supplycost DECIMAL(15,2),
    ps_comment VARCHAR(199)
);

CREATE TABLE region (
    r_regionkey INT,
    r_name CHAR(25),
    r_comment VARCHAR(152)
);

CREATE TABLE supplier (
    s_suppkey INT,
    s_name CHAR(25),
    s_address VARCHAR(40),
    s_nationkey INT,
    s_phone CHAR(15),
    s_acctbal DECIMAL(15,2),
    s_comment VARCHAR(101)
);

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/lineitem.tbl'
INTO TABLE lineitem
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/nation.tbl'
INTO TABLE nation
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/orders.tbl'
INTO TABLE orders
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/part.tbl'
INTO TABLE part
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/partsupp.tbl'
INTO TABLE partsupp
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/region.tbl'
INTO TABLE region
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';

LOAD DATA LOCAL INFILE '/home/fx2003/fx/SQL/sqlEdgeCloud/tpch-data/supplier.tbl'
INTO TABLE supplier
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n';
```

### 3.生成查询
```
export DSS_QUERY=./queries
# 创建保存输出查询的目录
mkdir -p ./queries_generated
for i in {1..22}
do
  ./qgen -d -s 1 $i > ./queries_generated/q${i}.sql
done
```