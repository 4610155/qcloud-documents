用户可以使用 CREATE VIEW 语句创建视图。视图是一个虚拟表，基于某条 SELECT 语句。视图可以用作定义新的虚拟数据源（类型转换、列变换和虚拟列等），拆分过长代码等用途。

#### 语法
```
CREATE VIEW 视图名 AS
SELECT 子句
```
#### 示例 1
创建一个名为 MyView 的视图：
```
CREATE VIEW MyView AS
SELECT s1.time_, s1.client_ip, s1.uri, s1.protocol_version, s2.status_code, s2.date_
FROM KafkaSource1 AS s1, KafkaSource2 AS s2
WHERE s1.time_ = s2.time_ AND s1.client_ip = s2.client_ip;
```

#### 示例 2
在计算中由于数据量较大、函数方法类型匹配要求等原因，必须使用 TINYINT、SMALLINT、REAL 等类型，但 CDP 输入类型不符合需求时，可以通过 CREATE VIEW 语句配合 CAST() 类型转换函数（见 4.6 小节）实现定义虚拟视图作为新的数据源。

下面是一个示例，它通过定义了一个名为 KafkaSource2 的视图，实现了将 KafkaSource1 数据源中的BIGINT 类型的 status_code 列转为 VARCHAR 类型的列：
```
CREATE VIEW KafkaSource2 AS 
SELECT 
  `time_`,
  `client_ip`,
  `method`,
  CAST(`status_code` AS VARCHAR) AS status_code,
FROM KafkaSource1;
```
>**注意：**
>不当的数据转换 CAST() 可能会导致精度损失，例如由 BIGINT 转为 INTEGER 或 TINYINT 等，请谨慎使用。
