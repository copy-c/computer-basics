# STL 基础

## 定义

### 分类

- 容器 (container) class temple
- 算法 (algorithm) function temple
- 迭代器 (lterator)
- 仿函数 (functor)
  - 给算法提供策略
- 配接器 (adaptor)
  - 修饰容器/ 仿函数/ 迭代器的东西, 类似 queue 和 stack
- 配置器 (allocator)
  - 内存分配等

### 关系

1 container 通过 allocator 获取空间  
2 algorithm 通过 lterator 访问 container  
3 functor 帮助 algorithm 实现不同策略变化  
4 adaptor 可以修饰或者套接其他

## 容器

### 分类

序列式容器

- array - 直接存储对象本身，而不是像vector直接存储指针
- vector - 数组(增导致resize、删导致后续会往前移、插会导致后续往后移都是导致迭代器失效）
- heap - 内含 vector(数组实现二叉树)
- priority_queue - 内含 heap

- list - 双向链表
- deque - 双端队列（数组+链表结合，同vector导致迭代器失效）

- stack - list  // 配接器
- queue - list  // 配接器

注：仍然可以使用erase，会返回新迭代器的下一个新地址  

关联容器

- map —— 红黑树  
- set —— 红黑树  
- unordered_map —— hash  

### vector

#### 初始化

```c++
1 
vector<int> list
2 
vector<int> list(7,3) // 7 个值为 3 的 int
3 
vector<int> list1(list2) = vector<int> list1 = list2
4 
vector<int> list1(list2.begin()+2, list2.end()-2);
int a[2] = {1, 2};
vector<int> list1(a, a+2); // 指针的意思
5 
vector<int> list1{0, 1, 2} = vector<int> list1{0, 1, 2}
6 


```

#### 注意
1.释放内存 shrink_to_fit  
2.几个成员函数辨析  
size() 返回当前n的大小  
resize(int n) 若 n < size: 多出来的尾部元素将被析构 size < n < capacity: 调用构造函数填充 n >capacity: 扩容并填充  
capacity() 返回当前capacity大小  
reserve(int n) 直接将capacity扩充到n，避免自动多次分配，推荐一开始就做  

### set

### map

1.插入效率问题  
新建：insert，因为[]会先构造一个空的类，再赋值，这个过程中就多了一步  
更新：operator[]  

## 算法

### std::sort()

<https://blog.csdn.net/ihadl/article/details/7400929>  

1）仿函数

```c
less<int>()  
```

2）自定义仿函数

```c
struct cmp {
  bool operator()(const Interval& a, const Interval& b) //重载了() {
    return a.start < b.start; // 一定要用大于或者小于号
  }
};

```

3）全局函数

```c
bool less_second(const myclass& m1, const myclass& m2) {
  return m1.second < m2.second; 
}
```

4）类内重载符号  

```c
class myclass {
  bool operator < (const myclass& m)const {
    return first < m.first;
  }
};
```

### find() / find_if()

find —— 值  
find_if —— 函数

### for_each() —— 遍历函数

```c
std::for_each(nums.begin(), nums.end(), [](int &n){ n++; });
```

### copy —— 拷贝

```c
std::copy(from_vector.begin(), from_vector.end(), std::back_inserter(to_vector)); // 复制到另一个容器

// 输入和输出
std::istream_iterator<int> is(cin); //绑定标准输入作为开始
std::istream_iterator<int> eof; //定义输入结束位置
std::copy(is, eof, back_inserter(to_vector));
std::copy(to_vector.begin(), to_vector.end(), std::ostream_iterator<int>(std::cout, " ")); // 利用ostream的迭代器输出到cout中
```

### transform —— 拷贝 + 遍历函数

将容器内的元素通过函数后，存入新的容器中  

```c++
std::string s("hello"); // 第一个完全可以被for_each取代
std::transform(s.begin(), s.end(), s.begin(), // 若想使用begin(), 长度必须大于等于要copy过来的元素大小
	   [](unsigned char c) -> unsigned char { return std::toupper(c); }); // int toupper(int c) 转换大写 

std::vector<std::size_t> ordinals;
std::transform(s.begin(), s.end(), std::back_inserter(ordinals), // 否则新的必须使用插入，或者通过resize()扩充大小
	   [](unsigned char c) -> std::size_t { return c; });

std::cout << s // HELLO
for (auto ord : ordinals) {
  std::cout << ' ' << ord; // 72 69 76 76 79
}
```

### back_inserter() / inserter()

## 迭代器

### 定义

提供某种方法, 能够不暴露 container 内部的情况下访问它

### 输入和输出

```c
// 输入和输出
std::istream_iterator<int> is(cin); //绑定标准输入作为开始
std::istream_iterator<int> eof; //定义输入结束位置, 没法结束, 需要加强制结束
std::copy(is, eof, back_inserter(to_vector));
std::copy(to_vector.begin(), to_vector.end(), std::ostream_iterator<int>(std::cout, " ")); // 利用ostream的迭代器输出到cout中
```

## 仿函数

对()的重载 -> 对一系列操作的重载 -> 目的:能够使其高可用

```c++
temple<T>
struct cmd {
  T operator() (const T& x, const T& y){ return x + y; }
}
cmd<int> cmdObj;
cout << cmdObj(3, 4); // 单独拿出来也可以搞
cout << cmd<int>(3, 4);
```

## 空间配置器

``` c++
class Foo{};
Foo* pf = new Foo;
delete pf;
```

两个过程

- 配置内存
  - alloc::allocate() -> #include<stl_alloc.h>
    - operator::new() 等价于 c 中 malloc()
  - alloc::deallocate()
- Foo:foo 构造内容
  - ::construct() -> #include<stl_construct.h>
    - 将初值构造到所指内存上
  - ::destory()


## 应用

### 删除元素的方法

1.内存连续分配

```c
1) 借助std::remove()函数
c.erase(std::remove(c.begin(), c.end(), value), c.end()) // remove()将==value的值都放到后面，但是不删除
c.erase(std::remove_if(c.begin(), c.end(), function), c.end()) // remove_if() value替换成function
2) 使用迭代器
for (iter = vector.begin(); iter != vector.end(); /**/)
{
  if (ok) iter = vector.erase(iter); // 内存连续分配的erase后，后面的元素会往前移动，因此迭代器会改变，这里需要重新设置
  else iter++;
}
```

2.list

```c
1）
c.remove(value)
c.remove_if(function) //
2）使用迭代器
同map，删除后迭代器会保持原来的值
```

3.关联容器

```c
1）
map.erase(value);
2）使用迭代器
for (iter = map.begin(); iter != map.end(); /**/)
{
  if (ok) map.erase(iter++); // 删除后迭代器会失效，在失效前先缓存好++
  else iter++;
}

全部删除时，可以用while
map::iterator iter = map.begin();
while (iter != map.end())
{
  map.erase(iter++);
}
```
