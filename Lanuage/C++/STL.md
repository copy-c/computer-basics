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

## 1 容器

### 底层数据结构

- array (c11)
  - 直接存储对象本身，而不是像vector直接存储指针
  - 代替普通固定数组，可以使用容器的配套设施

- vector
  - 数组(增导致resize、删、插都会导致迭代器失效）

- list
  - 闭环双向链表
- forward_list
  - 单向链表，比起list效率更高

- deque
  - 双端队列（数组+类 map 结构 = 多组缓冲区连接）
  - 头尾两段都可以插入和取出

- stack // 配接器, 拓展

  ```c
  stack<int> // 默认底部为 deque
  stack<int, vector<int>>
  stack<int, list<int>>
  ```

- queue - 队列 // 配接器, 拓展

  ```c
  queue<int> // 默认底部为 deque
  queue<int, list<int>>
  ```

- heap
  - 扮演 priority_queue 的助手
  - 内含 vector(数组实现二叉树)
- priority_queue
  - 缺省状态是 heap 以 vector 为底
  - 缺省转态是 大顶堆(less<int>), 小顶堆可用(greater<int>)

- 元组
  - pair 只有二元
  - make_tuple 构造元组
  - get 获得元组某个位置的值
  - tie 元祖拆包

关联容器

- map
  - 红黑树
  - 按照键值排序
- set
  - 红黑树  
  - 值唯一
  - 按某种规则自动排序
- unordered_map
  - hash  

### array

```c++
void foo(int *p, int len) {}
std::array<int, 4> arr = {1,2,3,4};

// C 风格接口传参
// foo(arr, arr.size()); // 非法, 无法隐式转换
foo(&arr[0], arr.size());
foo(arr.data(), arr.size());
```

### vector

特性

- 支持高速随机访问
- 高效尾端插入与删除 O(1)
- 头部插入与删除 O(n)
- push_back() 平均复杂度为 O(1)

#### 初始化

```c++
1 // 初始化表
vector<int> list{3} // 1 个赋值为 3 的值
2 // 按数目 初始化
vector<int> list(3) // 3个初值为 0 的数组
vector<int> list(7,3) // 7 个值为 3 的 int
vector<vector<int>> dp(2, vector<int>(3)); //[2][3]
3 
vector<int> list1(list2) = vector<int> list1 = list2
4 
vector<int> list1(list2.begin()+2, list2.end()-2);
vector<int> list1(a, a+2); // 指针的意思 int a[2] = {1, 2};
5
vector<int> list1{0, 1, 2} = vector<int> list1 = {0, 1, 2} //c11
```

#### 常用成员函数

读取

- front() // 第一个
- back() // 最后一个

存取

- pop_back() // 弹出最后一个 -> list 中是双向的, pop_front() / pop_back()
- push_back() // 从后面放入

删除

- erase() // 删导致后续会往前移
  - (first, end)
  - (position)

插入 -> 扩容可能会导致迭代器失效

- insert() // 插会导致后续往后移
  - 会插入 position 之前

释放内存

- shrink_to_fit()
- vector<int>().swap(需要释放的 vector)

大小

- size() 返回当前n的大小  
  - 返回值: size_type // 无符号整型, 那么和 int 作对比的时候小心反转
  - ``` auto a = io.size()(若为空) - 1; 那么 auto 后的 a 是无符号的, 导致反转 ```
- capacity() 返回当前capacity大小

扩容 -> 导致迭代器失效

- resize(int n)
  - 若 n < size: 多出来的尾部元素将被析构
  - size < n < capacity: 调用构造函数填充, 或 int 往上填 0
  - n > capacity: 扩容并用填充  
- reserve(int n)
  - 直接将capacity扩充到n，避免自动多次分配，推荐一开始就做  

#### 常见问题

1. vector.push_back() 平均复杂度为 O(1)
    - 每次扩容+拷贝需要 O(N) 的时间复杂度
    - 平摊开销

2. 扩容方式
    - 一般为 2 倍, 在 vs 下貌似 1.5 倍
    - 若 2 倍仍然不够, 则继续增长

3. at(i) 与 [i]
    - 前者加入了越界检测, 为空则会抛出异常

4. 当扩容时，会调用复制构造函数进行复制，C11下使用移动语义   

5. push_back() vs emplace_back()  
push_back(): 将已经存在的项目拷贝或者移动(move)到最后一项中  
emplace_back(): 重新构造一个新的项目

### list

特性

- 高效前/后插入与删除操作
- 随机访问效率低
- 内存开销比 vector 大

#### 构造

环状双向list, 定义 node 指向尾部(空的)指针

信息

- begin() -> (*node)->next;
- end() -> node

加入

- push_front()
- push_back()

清除

- easer() // 清除一个
  - pop_back()
  - pop_front()
- clear() // 全部销毁
- remove(T) // if == T, 那么删除

重构造

- .sort() // 迭代器不同, 不能使用 stl 中的 sort 算法
- .reverse() // 反转
- list1.splice(pos, list2) // 把 list2 添加到 list1 的 pos 之前

### heap

vector 配合以下算法可以转换成heap

```c
vector<int> vec = {4, 8, 9, 3, 5}
make_heap(vec.begin(), vec.end()) // 9 5 8 3 4
vec.push_back(7) // 此时放在最后 9 5 8 3 4 7
push_heap(vec.begin(), vec.end()) // 9 7 5 8 3 4
pop_heap(vec.begin(), end) // 7 5 8 3 4 9 移到最后, 但仍存在
sort_heap(vec.begin(), end) // 全部排序
```

### map

特性

- 底层采用 RB-tree
- 自动 sort

插入

```c
map<string, int> map_ = {1,2};
map_[string("a")] = 1;
map_.insert({"1", 10});
```

读取

for (auto& [key, val] : map) {}

复杂度

- 插入: O(logN)
- 查看: O(logN)
- 删除: O(logN)

### hash_table

结构

- 采用 vector 作为表格, 易拓展
- 采用 list(非 stl) 作为链
    ![avatar](picture/hash_table.png)

删除

- clear()
  - 只删除了 list, 而 vector 的 capacity 仍然存在

### unordered_map

特性

- 底层采用 hash_table
- 没有自动 sort

判断

- if (hash1 == hash2) return true;

### unordered_set

初始化

- unordered_set<int> good(words.begin(), words.end()); // vector<int> words;

### 元组

```c++
// get
auto student = std::make_tuple(3.8, 'A', "张三");
std::get<0>(student); // 使用下标取值
std::get<double>(student); // 使用类型取值

// 拆包
tie(gpa, grade, name) = std::make_tuple(3.8, 'A', "张三");
double a = gpa;
char b = grade;
string c = name;
```

## 2 算法

### sort()

#### 范围

仅支持 RandomAccessIterator 类, 也既vector 和 deque

#### 内部

- 数据量大 -> quick sort (递归分段)
- 数据量小 -> insert sort
- 递归过深 -> heap sort

### count() / count_if()

计数

### find() / find_if()

- 效率: 顺序查找, O(n)
find —— 值  
find_if —— 函数

### for_each() —— 遍历函数

- 注意: 静态算法, 元素内容不会变

```c
for_each(nums.begin(), nums.end(), [](int &n){ n++; }); // 此时是引用, 因此会变
```

### transform()

```c
transform(v.begin(), v.end(), )
```

### copy()

```c
std::copy(from_vector.begin(), from_vector.end(), std::back_inserter(to_vector)); // 复制到另一个容器
```

### remove()

remove() // 返回值指向被删除的第一个  
删除某值, 但会放到最后, 产生冗余, 需要再使用 erase 删除  

```c
vector.erase(remove(vector.begin(), vector.end(), x), vector.end())
```

### replace()

替换某值

### back_inserter() / front_inserter()

### int count = accumulate(v.begin(), v.end(), value_begin); // value_begin + 累计和

## 3 迭代器

### 定义

提供某种方法, 能够不暴露 container 内部的情况下访问它

### 输入和输出迭代器

```c
// 都可以单独当做对象使用
std::ostream_iterator<int> out(std::cout, " ")) // 每接受一个值, 后面跟 " "
std::istream_iterator<int>

// 输入和输出
std::istream_iterator<int> is(cin); //绑定标准输入作为开始
std::istream_iterator<int> eof; //定义输入结束位置, 没法结束, 需要加强制结束
std::copy(is, eof, back_inserter(to_vector));
std::copy(to_vector.begin(), to_vector.end(), std::ostream_iterator<int>(std::cout, " ")); // 利用ostream的迭代器输出到cout中
```

## 4 仿函数

对()的重载 -> 对一系列操作的重载 -> 目的:能够使其高可用

```c++
template<typename T>
struct cmd {
  auto operator() (const T& x, const T& y) { return x + y; }
}
cmd<int> cmdObj; // 也可以被当做单独的对象使用
cout << cmdObj(3, 4); // 单独拿出来也可以搞
cout << cmd<int>(3, 4);
```

### 常见仿函数

**for_each 调用这些函数的时候并不会改变原来的值,自己写的匿名函数可以**

```c
// 计算
less<int>() // less<int>() (a,b) -> a < b 是否成立
less_equal<int>()
greater<int>()
equal_to<int>()
not_equal_to<int>()

// 逻辑
logical_and<int>() (true, true)
logical_or
logical_not
```

### 用法

#### 利用 bind 进行组合

```c
bind(multiplies<int>(), 2, std::placeholders::_2); // x * 2 = ?
```

#### sort 规则定义

1)内置仿函数

```c
less<int>() // less<int>() (9,3)
less_equal<int>()
greater<int>()
equal_to<int>()
not_equal_to<int>()
```

2)自定义仿函数

```c
template <class T>
struct cmp {
  bool operator()(const T& a, const T& b) //重载了() {
    return a < b; // 一定要用大于或者小于号
  }
};

```

3)全局函数 -> 写成仿函数可以更具拓展性

```c
bool less_second(const int& a, const int& b) {
  return a < b;
}
```

4)直接写匿名函数

## 5 空间配置器

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

## 6 应用

### 删除元素的方法

1.内存连续分配

```c
1) 借助std::remove()函数
c.erase(std::remove(c.begin(), c.end(), value)) // remove()将==value的值都放到后面，但是不删除
c.erase(std::remove_if(c.begin(), c.end(), function) // remove_if() value替换成function
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
  if (ok) map.erase(iter++); // 删除后迭代器会保持原来的值, 因此需要缓存并++
  else iter++;
}

全部删除时，可以用while
map::iterator iter = map.begin();
while (iter != map.end())
{
  map.erase(iter++);
}
```
