# 阿里UC事业部1面  

1、基类中私有函数可以被派生类继承重写  
```c
class Base {
 private:
     virtual void fun() { std::cout << "Base Fun" << std::endl; }
 public:
     virtual void cun() { std::cout << "Base Cun" << std::endl; }
     friend int main();
 };

 class Derived : public Base {
 public:
     void fun() { std::cout << "Derived Fun" << std::endl; }
     void cun() { std::cout << "Derived Cun" << std::endl; }
 };

 int main()
 {
     Base *ptr = new Derived;
     Derived *ptest = new Derived;
     ptr->cun(); // 调用派生类
     ptr->fun(); // 调用派生类
     // 原因：动态束定是运行期间才确定，访问权限是编译时确定的
     return 0;
 }
```  
  
  


# 网易游戏1面  

## 基础  
1.dll与lib的区别  
静态库：静态编译后，会产生一个lib  
动态库：动态编译后，产生一个lib和dll  
1）lib：编译阶段使用，要把内容放到程序中，索引和实现都在其中；  
2）dll+lib：lib索引信息，记录了dll中函数的入口和位置，编译阶段把索引放入程序中，dll中包括了函数具体实现内容，dll在程序运行中被用到；  
2.虚表与虚指针在内存中的位置  
https://www.cnblogs.com/wangxiaobao/p/5850949.html 内存模型  
1）虽然基类中私有的不能使用，但是内存结构中仍然会继承，也就解释了基类中私有的虚函数仍然能够被重写  
2）只有虚函数（包括继承的而没有重写的）地址才会记入虚表  
3）虚指针和类内其他变量相同的位置，栈或者堆  
4）虚表放在常量区（VS中）  
3.虚函数与纯虚函数的区别（在使用上）  
1）纯虚函数后面加=0实现，纯虚函数所在类称为抽象类，抽象类不能创建实例，只能由派生类创建  
2）继承了抽象类的派生类必须实现抽象类的接口  
3）抽象类不是每个虚函数都是纯虚，因此指针被派生类获得后仍然可以调用  
4.inline与define的区别  
1）define在预处理阶段完成，inline在编译阶段  
2）inline是函数，要做类型检查  
3）define是字符串替换，inline是展开代码，直接嵌入，而不需要函数调用跳转  
5.线程通信方式  
1）锁   
2）信号量   
6.关系数据库与非关系数据库有哪些，其区别有哪些？  
1）关系型数据库：  
满足ACID特性，以表的结构进行存储  
2）非关系型数据库：  
a.面向高性能并发读写key-value数据库：Redis  
b.面向海量数据访问的面向文档数据库：在海量的数据中快速查询数据，一般用类似json格式存储，存储内容是文档型的，可以使某些字段建立索引，实现关系数据库的某些功能（MongoDB）  
c.面向可拓展的分布式数据库：  
3）非关系数据库产生的背景：  
a.网页应用中数据的一致性不那么重要，因此可以打破关系数据库中费时的事务操作  
b.系统升级、功能增加，意味着数据结构变化巨大，因此需要新的数据化结构存储  

## 算法    
1.快排  
2.判断单链表是否有环  
3.打家劫舍系列题  

# 网易游戏2/3面  
## 基础  
1.重载和重写的区别以及编译器如何实现重载？  
2.红黑树比AVL树的优势？  
AVL严格按照平衡二叉树去旋转，可能会导致更多旋转操作；  
红黑树牺牲了一定的平衡性，旋转次数不超过3次的条件下保持一个合理的高度；  
3.mysql有几种日志类型  
重做日志：  
保持实务的持久性，方式发生故障时，尚有脏页未写入磁盘，再重启服务后，可以根据日志重做，从而达到持久性特性  
回滚日志：  
保存了事务发生之前的数据的一个版本，可以用于回滚  
二进制日志：  
用于主从复制中，从库利用主库上二进制日志进行重播，实现主从同步  
以上三个与事务操作有关  
错误日志、慢查询日志、一般查询日志、中继日志  
4.数据库语句  
left join  
从左表(table_name1)那里返回所有的行，即使右表(table_name2)中没有匹配的行  
```sql
 SELECT column_name(s)
 FROM table_name1
 LEFT JOIN table_name2 
 ON table_name1.column_name=table_name2.column_name
```
union  
合并多个select语句结果集，select必须拥有  

## 算法  
1.N个字符串，寻找子串相同的字符串（不知道怎么搞，如果是前缀的匹配可以使用前缀树）  
2.智能指针实现  
添加指针的辅助类作为私有成员记录智能指针所指的地址和次数  
不能使用static进行计数，否则会导致只能拥有一种实例  
```c
 class U_Ptr                                  
 {
 private:

     friend class SmartPtr;      
     U_Ptr(Point *ptr) :p(ptr), count(1) { }
     ~U_Ptr() { delete p; }

     int count;   
     Point *p;                                                      
 };
```
https://www.cnblogs.com/QG-whz/p/4777312.html  
3.快排  
4.整型数字转换为字符串  
