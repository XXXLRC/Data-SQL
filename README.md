# Data-SQL
Data SQL



## Database


## MySQL
MySQL是一个开源的关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

![](https://github.com/XXXLRC/Data-SQL/blob/095170c0118dd02330bcc876745ee4d3b0f3eb65/images/2021110800003.png)

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

1.什么是B+树结构，为什么MySQL常使用B+树结构作为索引？

二叉查找树也称为有序二叉查找树，满足二叉查找树的一般性质，是指一棵空树具有如下性质：
- 只有一个根节点，每个非根节点最多两个子节点；
- 任意节点左子树不为空,则左子树的值均小于根节点的值；任意节点右子树不为空,则右子树的值均大于于根节点的值；
- 任意节点的左右子树也分别是二叉查找树；

![]()
上图为一个普通的二叉查找树，按照中序遍历的方式可以从小到大的顺序排序输出：2、3、5、6、7、8。（中序遍历逻辑：先访问左节点，再访问根节点，最后访问右节点）

查找：
1. 从根节点查起，比较键值和根节点的大小，小于根节点从左子树查起，大于根节点从右子树查起；
2. 比较键值和子树的根节点大小，小于子树根节点从左子树查起，大于子树根节点从右子树查起，直到找到数据返回，没有找到返回空

局限性：
一个二叉查找树是由n个节点随机构成，顺序存放情况下二叉查找树会退化成一个有n个节点的线性链。

平衡二叉树（AVL Tree）
平衡二叉树（AVL 树）在符合二叉查找树的条件下，还满足任何节点的两个子树的高度最大差为 1。它也被称为高度平衡树。查找、插入和删除在平均和最坏情况下都是O（log n）。
增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。这样的平衡可以降低树的高度并且提高查找的效率。

数据库查询瓶颈来自于磁盘IO，一次性从磁盘IO拿1条数据到内存和拿10条数据的消耗几乎时一样的。而二叉搜索数每个节点只存储一条数据，现有硬件条件下，可以进行优化提高效率：尽量在结点上存储较多相关的信息来保证层数尽量少，使得在查找时磁盘的I/O操作少一些，可以加快查找速度。

B树是为了磁盘或其它存储设备而设计的一种平衡多路查找树(相对于二叉，B树每个内节点有多个分支)，与红黑树相比，在相同的的节点的情况下，一颗B树的高度远远小于红黑树的高度（二叉树节点中保存的数据只有一个，而B树的节点中保存的是线性表）。B树存储的查找时间通常由存取磁盘的时间和CPU计算时间这两部分构成，而CPU的速度非常快，所以B树的操作效率取决于访问磁盘的次数，关键字总数相同的情况下B树的高度越小，磁盘I/O所花的时间越少（B树的每个节点的元素存储量可以视为一次I/O读取，树的高度表示最多的I/O次数，在相同数量的总元素个数下，每个节点的元素存储个数越多，树的高度越低，查询所需的I/O次数越少。）。

B树的特点
1. 定义任意非叶子结点最多只有M个儿子，且M>2； 
2. 根结点的子树数为[2, M]； 
3. 除根结点以外的非叶子结点的子树数为[M/2, M]； 
4. 每个结点存放至少M/2-1（取上整）和至多M-1个关键字；（至少2个关键字） 
5. 非叶子结点的关键字个数=指向子树的指针个数-1； 
6. 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] < K[i+1]； 
7. 非叶子结点的指针：P[1], P[2], …, P[M]；其中P[1]指向关键字小于K[1]的子树，P[M]指向关键字大于K[M-1]的子树，其它P[i]指向关键字属于(K[i-1], K[i])的子树； 
8. 所有叶子结点位于同一层

B+树是B树的变体，也是一种多路查找树，跟B树不同的是，B+树的非叶子节点只存储索引数据不保存原记录数据，这样非叶子节点存储的索引数量会大大增加，提高查找效率。B+叶子结点中包含了全部数据记录的信息，及指向含有这些记录的指针，且叶子结点本身依关键字的大小自小而大的顺序链接（双向链表）。

MySQL使用B+树作为索引的数据结构，有如下特点：
- B+树的非叶子结点只存储索引信息，相对B树更小，节点所能容纳的关键字索引数量也越多，一次性读入内存中的需要查找的关键字也就越多，层级也越少，相对来说IO次数降低了，提高了查询效率；
- B+树查询效率更加稳定：任何基于索引的查找必须走一条从根结点到叶子结点的路径，导致每一个数据的查询效率相当；
- B+树便于遍历和范围查询：B树在提高了IO性能的同时并没有解决元素遍历效率低下的问题。B+树只需要去遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询非常频繁，B+树基于索引的顺序存储便于范围查找。

从平衡二叉树、B树、B+树总体来看它们的贯彻的思想是相同的，都是采用二分法和数据平衡策略来提升查找数据的速度；不同点是他们在演变的过程中通过IO从磁盘读取数据的原理进行一步步的演变，每一次演变都是为了让节点的空间更合理的运用起来，从而使树的层级减少达到快速查找数据的目的。

2.MySQL是怎样用B+树来存储数据的

MySQL常见的存储引擎InnoDB，MyISAM，存储引擎适用范围是表级别的。

MyISAM存储的数据有三个文件分别是：.frm表结构文件，.MYD数据文件（MyISAM Data），.MYI索引文件（MyISAM Index）。

特点：
索引文件和数据文件是分离的，索引结构的叶子节点value存储的是文件指针。非聚集索引。

查找：
MyISAM主键索引查找流程：先通过.MYI文件找到对应索引的文件指针，再根据文件指针去.MYD文件中定位对应的数据记录。
MyISAM普通索引查找流程：和主键索引查找流程一致。

![]()

InnoDB存储的数据有两个文件：.frm是表结构文件，.ibd是数据和索引文件（InnoDB Data）
特点：
数据文件本身就是索引文件
表数据文件本身就是按B+树组织的一个索引结构文件
聚集索引的叶子节点包含了完整的数据记录
表必须有主键，且推荐使用整型的自增主键
普通索引结构叶子节点存储的是主键值

查找：
InnoDB主键索引查找流程：通过.ibd文件找到对应的索引，索引的value即为那行对应的完整数据。
InnoDB普通索引查找流程：通过.ibd文件找到对应的索引，索引的value即为那行对应的主键的值，再根据主键值去主键索引树中找到对应的行数据。
