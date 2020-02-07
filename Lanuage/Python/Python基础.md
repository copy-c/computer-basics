# 特性

1.一切皆为对象, 变量则为引用

```python
a = [1,2,3]
def foo(b):
    b.append(4)
foo(a)
print(a)  #  [1,2,3,4]

def bar(c):
    c = [0,0,0] # c 被重新给予对象, 此时的 c 这个变量指向变了, 而不是原来的对象改变了
bar(a)
print(a)  # [1,2,3,4]

def add(num):
    num = num + 10 # num 的引用对象改变了
d = 2
add(d) # d = 2
```

1.拷贝与引用

```python
m = [1,2,[3]]
1.引用
n = m
2.浅拷贝 - 只拷贝了第一层，第二层[3]没有拷贝
n = m[:] or
n = copy(m)
3.深拷贝
n = deepcopy(m)
```

# 基本数据结构

1.[] // 可变

```python
# 1.初始化
multilist = [[1 for col in range(3)] for row in range(2)]

# 2.切片操作  
[start:end:step] // end下标的值访问不到
if step > 1 :
  [start:end] 从start开始从左向右遍历
else :
  [start:end:-1] 从start开始从右向左遍历

# 3.翻转
a[:] = a[::-1] // 翻转并替换
```

2.() // 不可变  

```python
# 定义
tup2 = (1, 2, 3, 4, 5 )
tup3 = "a", "b", "c", "d" // 任意用逗号隔开，默认元组
tup1 = (50,) // 只有一个元素时，后面使用逗号
```

3.{} // map

```python
# 定义
dict = { 'abc':123, 98.6:37 }  
# 更新
dict['abc'] = 1
# 新增
dict['new'] = 1
// 新增和更新要分开，不能使用自增去新增
dicts = {}
for i in nums1:
  if i in dicts:
    dicts[i] += 1
  else:
    dicts[i] = 1
// 或者使用指定默认值
ab_map[i] = ab_map.get(i, 0) + 1 // 若不存在map[i]的值,那么返回默认值
# 删除
del dict  
del dict['abc']
# 清除条目
dict.clear()
# Tips
元素可以是任何类型，包括[] ()等，但是[]不能作为键值
# 键值
map_.keys()  
# value
map_.values()
# 键值对
map_.items()
```

4.{} // set  

```python
# 定义
set() or { }
a = set('abracadabra') // {'a', 'r', 'b', 'c', 'd'}
a = {'a', 'r', 'b', 'c', 'd'}
# 交集
x & y
# 并集
x | y
# 差集
x - y
```

5.字符串  

```python
# 成员运算符 in
"H" in a => True
```

# 运算符

1.算数运算符  
1）// 整数除  
2）** 次幂  
2.逻辑运算符  
not  
and  
or  
3.成员运算符  
in  
not in
 
# 迭代器

```python
list=[1,2,3,4]
it = iter(list)
next(it)
```

# lambda

lambda [arg1 [,arg2,.....argn]]:expression  


# 常用技巧

## 字符串相关

1.切片转换为字符串  
"x".join(ans) // 使用x作为单词之间的连接符  
2.删除前后的字符  
strip()  
3.分开  
splice()  
