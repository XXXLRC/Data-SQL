# Data-SQL
Data SQL



## Database


## MySQL
MySQL是一个开源的关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

**MySQL的逻辑架构**

![](https://github.com/XXXLRC/Data-SQL/blob/d677d97a31c57c265185200a6f51214ea6d96ee7/images/20211009001.png)

**MySQL查询执行路径**

![](https://github.com/XXXLRC/Data-SQL/blob/92e3e7f3981d1ab6b30ade0a7566786059a11400/images/2021100910002.png)

**MySQL的索引**
定义和特点
- 索引是帮助数据库高效获取数据的排好序的数据结构。
- 索引存储在文件中。
- 索引建多了会影响增删改效率 - 针对改变会做顺序调整。

分类：MySQL建索引可使用的数据结构有B+树和Hash两种。

Hash用得少， 优点是可以快速定位到某一行，缺点是不能解决范围查询问题。对于如果不需要使用范围查询、只需要精准查询的场景，可以使用Hash索引方法。

什么是B+树结构，为什么MySQL使用B+树结构作为索引？
