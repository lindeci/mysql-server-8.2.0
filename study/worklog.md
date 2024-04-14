# mysql 脚本
```sql
启动 MySQL 服务
/data/mysql-server/build/runtime_output_directory/mysqld-debug --user=mysql --datadir=/data/mysql-server/data --socket=/data/mysql-server/data/mysql.sock.lock &

# 6、在容器中使用 MySQL 客户端进行操作
/data/mysql-server/build/runtime_output_directory/mysql -uroot -p'步骤4中日志倒数第二行的密码' --socket=/data/mysql-server/data/mysql.sock.lock 
#    修改 MySQL 密码，以后直接使用 root 这个简单密码登录数据库
ALTER USER root@localhost IDENTIFIED BY 'root';


/data/mysql-server/build/runtime_output_directory/mysql -uroot -p'root' --socket=/data/mysql-server/data/mysql.sock.lock



mkdir -p /data/mysql-server/build /data/mysql-server/data
chown -R mysql.mysql /data/mysql-server/data

cmake --no-warn-unused-cli -DWITH_BOOST=/data/boost_1_77_0 -DWITH_DEBUG=1 -DBUILD_CONFIG=mysql_release -DWITH_RTTI=ON -DWITH_SHOW_PARSE_TREE=ON -DCMAKE_BUILD_TYPE:STRING=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -S/data/mysql-server -B/data/mysql-server/build -G "Unix Makefiles"

cmake --build /data/mysql-server/build --config Debug --target component_reference_cache -j 32
cmake --build /data/mysql-server/build --config Debug --target mysqld -j 32
cmake --build /data/mysql-server/build --config Debug --target mysql -j 32

rm -rf /data/mysql-server/data/*
/data/mysql-server/build/runtime_output_directory/mysqld-debug --initialize --user=mysql --basedir=/data/mysql-server/build --datadir=/data/mysql-server/data
```

# 初始化 SQL
```sql
drop table if exists t1;
drop table if exists t2;
drop table if exists t3;
drop table if exists t4;
drop table if exists t5;
drop table if exists t6;

create table t1 (id int, a int);
create table t2 (id int, a int);
create table t3 (id int, a int);
create table t4 (id int, a int);
create table t5 (id int, a int);
create table t6 (id int, a int);
insert into t1 (id,a) values (0,0),(1,1),(8,8);
insert into t2 (id,a) values (0,0),(1,1),(2,2);
insert into t3 (id,a) values (1,1),(2,2),(3,3),(8,8);
insert into t4 (id,a) values (2,2),(3,3),(4,4);
insert into t5 (id,a) values (2,2),(3,3),(4,4),(5,5);
insert into t6 (id,a) values (2,2),(3,3),(4,4),(5,5),(6,6);


((SELECT * FROM t1 UNION SELECT * FROM t2 UNION ALL SELECT * FROM t3
             ORDER BY a LIMIT 5) INTERSECT
            (((SELECT * FROM t3 ORDER BY a LIMIT 4) ) EXCEPT SELECT * FROM t4)
            ORDER BY a LIMIT 4) ORDER BY -a LIMIT 3;
```

# 调试脚本
‵‵`sh
-exec source /data/mysql-server/study/object.py
-exec mysql expr thd->lex->unit
```




# 草稿
```sh
select t1.* from t1 nature join t2 where t1.id>0;
```