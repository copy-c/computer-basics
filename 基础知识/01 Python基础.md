#  语言基础
## 基本数据结构  
1）[] // 可变  
```python
 1.初始化
 multilist = [[1 for col in range(3)] for row in range(2)]
 [[1, 1, 1], [1, 1, 1]]
```
2）() // 不可变  
```python
# 定义
tup2 = (1, 2, 3, 4, 5 )
tup3 = "a", "b", "c", "d" // 任意用逗号隔开，默认元组
tup1 = (50,) // 只有一个元素时，后面使用逗号
```
3）{} // map   
```python
# 定义
dict = { 'abc':123, 98.6:37 }  
# 更新
dict['abc'] = 1
# 新增
dict['new'] = 1
# 删除
del dict  
del dict['abc']
# 清除条目
dict.clear()
# Tips
元素可以是任何类型，包括[] ()等，但是[]不能作为键值
```
4）{} // set  
```python
set() or { }
a = set('abracadabra') // {'a', 'r', 'b', 'c', 'd'}
a = {'a', 'r', 'b', 'c', 'd'}

```
5）字符串  
```python
# 成员运算符 in
"H" in a => True

```
## 切片
```python
1.基本
[start:end:step] // end下标的值访问不到
if step > 1
[start:end] 从start开始从左向右遍历
else 
[start:end:-1] 从start开始从右向左遍历
2.翻转
a[:] = a[::-1] // 翻转并替换
```
## 运算符  
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
## 迭代器
```python
list=[1,2,3,4]
it = iter(list)
next(it)
```
## lambda 
lambda [arg1 [,arg2,.....argn]]:expression  

# 类  
```python
1.命名方法
1）特殊成员
__xxx__ 
  构造函数
  def __init__(self, args1, args2):
2）保护成员
_xxx 
3）私有成员
__xxx 
4）避免与关键字冲突
xx_
2.使用
s = Test()
```
## 

# 概念 
## 拷贝相关
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

# 常用技巧
## 字符串相关　
1.切片转换为字符串  
"x".join(ans) // 使用x作为单词之间的连接符      
2.删除前后的字符  
strip()  
3.分开  
splice()  

