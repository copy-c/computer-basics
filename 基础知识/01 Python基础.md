#  语言基础
## 基本数据结构  
1）[] // 可变  

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
