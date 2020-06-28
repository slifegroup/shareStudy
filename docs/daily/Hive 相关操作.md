# Hive 相关操作

- 创建表

```java
CREATE TABLE opt_data(
    id INT comment '编号',
    name STRING comment '运营商名称'
)
PARTITIONED BY (collect_date STRING) // 设置分区字段
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '&' // 字段数据的分割符
STORED AS TEXTFILE;
```

- 修改表

```java
// 添加分区
ALTER TABLE ADD opt_data PARTITION (collect_date='2020-2-29')
// 添加字段
ALTER TABLE opt_data ADD COLUMNS (remark String comment '备注')
// 删除字段目前用不了
ALTER TABLE opt_data DROP [COLUMN] column_name 
使用 REPLACE 语法代替
// 变换字段名
ALTER TABLE opt_data CHANGE column_name new_name new_type 
ALTER TABLE test CHANGE month mon INT 
// 修改表字段
ALTER TABLE opt_data REPLACE COLUMNS (col_spec[, col_spec ...])
```

- 插入数据

load data local inpath '/home/centos/files/opt_data.txt' into table opt_data;

* 复制表

create table t2 like t1;

- 同步函数

reload function

* 显示分区

  ```
  show partitions base_data_success 表名;
  ```

- 设置执行参数（执行并行操作）
```java
// 开启任务并行执行
set hive.exec.parallel=true;
// 同一个sql允许并行任务的最大线程数
set hive.exec.parallel.thread.number=20;
// 指定内存
SET mapreduce.reduce.memory.mb=8192;
// 设置执行的方式 （mr，spark）
SET hive.execution.engine=spark;
```
设置执行参数前确认执行需要多少Stage，使用 EXPLAIN 关键字

例子1：
```java
EXPLAIN SELECT * FROM base_data_clean
```

得到以下结果：

![](https://img.mupaie.com/1583391711937-191dd0c7-96c6-4cf7-9537-625055127f6e.png)

例子2：
```java
EXPLAIN INSERT overwrite table abd.base_data_succes_province
SELECT DISTINCT a.cell_id,a.lgt,a.ltt,a.grid_code,a.collect_date,
split(polygonname,'_')[0],split(polygonname,'_')[1],split(polygonname,'_')[2],null
from abd.base_data_succes_temp a LEFT JOIN abd.gx_city_poi b on a.grid_code = b.grid_code
```

得到以下结果：

![](https://img.mupaie.com/1583392009911-7ffd3039-eb49-43e6-99ae-90e530f721b9.png)

结论：如果执行EXPLAIN后发现只有一个Stage，执行上述参数无意义。
记住需要配置并行执行前使用EXPLAIN检验

* 创建自定义函数

  1. 将打包后的函数jar包通过 `XFtp`工具上传到服务器上，上传到 hdfs 路径上

     ```shell
     [rjtx@rjtx221 ~]$ hadoop fs -put /home/rjtxudf.jar /udf
     hadoop fs -put 当前文件路径（path） 上传到指定路径下
     ```

  2. 进入hive命令行添加jar包

     ```shell
     [rjtx@rjtx221 ~]$ hive
     hive> add jar hdfs:/udf/rjtxudf.jar; // 添加指定目录的jar
     hive> delete jar hdfs:/udf/rjtxudf.jar; // 删除指定目录的jar
     ```

  3. 自定义临时函数

     ```shell
     hive> create temporary function numtoip12 as 'StSql.NumToIp12';
     hive> create temporary function IpHome  as 'StSql.IpHome';
     
     ```
  
 create temporary function xulatest as com.xula.hivedemo.ReplaceEarFcnUDF
     ```
  
  4. 自定义永久函数
  
     ```shell
     hive> create function numtoip12  as 'StSql.NumToIp12' using jar 'hdfs:///udf/rjtxudf.jar';
     解释 -------
     numtoip12 (自定义函数名)  
     'StSql.NumToIp12' （方法的全路径 包名 + 方法名）
 'hdfs:///udf/rjtxudf.jar';（记得hdfs路径前面必须加hdfs://才可以执行）
     ```
  
  5. 删除自定义函数
  
     ```shell
     hive> drop function NumToIp12--永久函数
 hive> drop temporary function NumToIp12--临时函数
     ```

  **注意**：
  
是jar包里面嵌入txt含中文记得要把文件另存为utf-8 bom格式，否则乱码
  
  
  
  ### 使用 `impala-shell` 导出数据
  
  ```
  impala-shell -q "SELECT village_id,province,city,county,towns,natural_village,radius,number,lgt,ltt,lonlat from abd.data_village where city like '%百色市%'" -B --output_delimiter="," -o /data2/base_fat.csv
  
  impala-shell -q "SELECT cell_id,enbid,rsrp,opt_name,lgt,ltt,dynamic_network_type,clisite FROM abd.base_data_clear2 where collect_date = 'nanning_20200616' and cell_id in ( SELECT cell_id FROM abd.base_data_clear2 a WHERE a.collect_date='nanning_20200616' GROUP BY a.cell_id HAVING count(*) > 1000) and lgt REGEXP '^[1-9][0-9]{1,2}\\.([0-9]{4,})*$' and ltt REGEXP '^[1-9][0-9]\\.([0-9]{4,})*$'
  " -B --output_delimiter="," -o /data2/base_data.csv
  ```
  
  
  
  
  


