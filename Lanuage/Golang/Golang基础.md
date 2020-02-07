
# c对比新增特性

1.string boolean map等  
2.闭包，函数同样当做一种变量(func)  
3.struct拥有继承，可以继承所有成员变量和方法  
4.大写开头可以导出，小写开头仅限内部使用  
5.包特性，必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main  
6.类似python的切片操作  
7.支持值传参和指针传参, 不支持引用传递(map/ slice 等表现出引用特征是因为结构体内部指针未变, 而结构体有变化 -> 此为引用的本质, 指针被藏起来了)

```go
func main() {
    m1 := make([]string, 1)
    m1[0] = "test"
    func1(m1) // m1 未变
}

func func1 (a []string) {
    a = append(a, "val1") // a 被重新赋值 变为 [test], [val1]
}
```

# 注意

<https://segmentfault.com/a/1190000013739000>  
1.若多返回值，只需要第二个，可以将前一个值使用 '_' 代替  
2.map，slice，chan是引用类型，不需要传递指针的指针  
3.slice和string区别  
string的指针可以改变，但是指向的内容是只读的，不能改变
slice是数组的抽象  
4.编译器自动选择在栈上还是在堆上分配局部变量,并不是由var还是new声明变量的方式决定的

# 关键字

1. nil 代替 NULL  
2. 由func、var、const、type四种关键字组成  
3. new 和 make 区别  
    - make 只能用于 slice, map, channel  
    - new(T) 返回的是 T 的指针
4. len求切片长度  
5. range
    - 值拷贝
    - range string -> 迭代的是 Unicode 而不是 unit8, 因此返回的是 rune 类型(int); 且 index 可能不是连续的

# 控制结构

if switch for 没有while  
1.if  
if 条件不需要括号  
2.switch  
没有break分支也会结束  
case中可以使用任何值，并且可以放置多个值  
3.for  
表达式不需要括号  
for xx;xx;xx {}  
for i := range m {} == while  // 此时的 i 和 v 是副本拷贝
for {} // 无限循环  

# 类型声明

声明必须以保留字开头，类型位于变量名后  

```go
1.常量
const N = 1024

2.变量 -> 局部变量由编译器决定在堆或者栈上
var x int
var x int = 1
var x = 1 // 若可以推断则省略类型
x := 1 // x必须是没有被使用过
var t1 *T = new(T)
t2 := new(T) // 简易复制，表示左边的变量与右边类型相同
var t3 *T = t2

3.函数
func f (i int) float {}
func f2 (i int) (float, int) {}
func CountWordsAndImages(url string) (words, images int, err error) {
  return
}

4.结构体
type T struct {
  x, y int
}

5.接口
type Write interface {
  
}
```

# 基本数据类型

## 常量

1.常量间的所有算术运算,逻辑运算和比较运算的结果也是常量,对常量的基本函数调用也是常量  
len、cap、real、complex和unsafe.Sizeof  

```go
const (
  _ = 1 << (10 * iota) // 1 << n = 2^n
  KiB // 1024
  MiB // 1048576
)
```

2.常量可以是无类型的,并且可以提供非常高的精度  

## 指针

1.不能对指针(地址)进行加减操作

## 字符串

1.字符串元素不能修改，是一个只读字节数组  
2.使用写时复制  

```go
s := "Hello World" -> 指向 hello world
hello := s[:5] -> 指向s的头 长度为hello
world := s[7:] -> 指向s的world,长度为world

// uft8 遍历
for i := 0; i < len(str); i++ {
    ch := str[i] // byte 类型 <=> int8
}
for _, v := range []byte(data) {
}

// unicode 遍历
for _, ch := range str {
    // ch 类型是 rune <=> int32
}
```

## 切片 - 底层数组的一种引用

1.切片由三部分组成:指针(指向第一个slice元素对应的底层数组元素的地址) + 长度 + 容量(底层类似c++ vector)
2.注意nil 和 空集的区别 -> 在判断一个切片是否为空时，一般通过len获取切片的长度来判断, 因为有合法的空集合存在  

```go
1.定义
var a []int                  // nil切片, 和 nil 相等, 因为是一个指针,所以可以这样用
b = []int{} = make([]int, 0) // 空切片, 和 nil 不相等, 一般用来表示一个空的集合
c = []int{1, 2, 3}           // 有3个元素的切片, len和cap都为3
d = make([]int, 2, 3)        // 有2个元素的切片, len为2, cap为3
e = make([][]int, )
2.添加
a = append(a, 1)     // 有返回值的
a = append([]int{ch}, a...) // 向前插入
3.删除
a = a[:len(a)-N]      // 删除尾部N个元素
a = a[N:]             // 删除开头N个元素,其实是重新赋值
4.清空
a = a[:0]
5.复制
b := a[:]
or copy(new, a) // new 的 len 必须要大于等于 a, 否则会有问题
6.使用
类似python的方式,可以直接使用下标访问
a[3:5]
7.遍历
for i, v := range a {
}
8.二维
dp := make([][]int, len(s) + 1)
for i := 0; i < len(s) + 1; i++ {
  dp[i] = make([]int, len(p) + 1)
}
```

## map - 底层hash的一种引用, 因此无序

```go
1.定义
// 直接定义是 nil, 拿数据没有问题, 但是存数据会出错
var hash map[int]int // hash 为 nil
// 其他方式初始化, 结果为 { }
args := make(map[string]int)
args := map[string]int{}

2.删除
delete(args, key) // 失败时返回查找类型的默认初值,因此查找失败也没关系

3.create
args[key] = value

4.查找
if _, ok := args[key]; ok {

}

5.遍历
for 

6.注意
内存地址可能会变换, 删除或者扩容

```

## struct - 结构体

1. 空结构体 struct{} 大小为 0, 不包含任何信息 -> 可以代替 map 中的 value 从而实现 set 功能, 可以节约空间
2. 结构体可以比较, == 将比较两个结构体的每个成员
3. 匿名嵌入特性, 可以直接访问叶子属性而不需要给出完整路径

# 方法

1. 写法: 函数名之前加上 struct 或者 slice
2. 用法: 调用一个函数时, 会对每个参数进行拷贝, 因此一般使用指针, 并且指针可以使得数据能在函数(方法)内共享使用
3. 其他: 指针时, 编译器会隐式用 &p 去调用方法, 因此只需要写 p.方法即可

```go
// 链表的实现
type IntList struct {
    Value int
    Tail  *IntList
}
// Sum returns the sum of the list elements.
func (list *IntList) Sum() int {
    if list == nil {
        return 0
    }
    return list.Value + list.Tail.Sum()
}
```

# 接口

1. 定义
    - 接口(interface): 描述了方法的集合
    - 实例(implement): 实现了一个接口的所有方法

2. 泛型编程

    ```go
    // 定义接口
    type MyForm interface {
        Len() int
        Less(i, j int) bool
        Swap(i, j int)
    }

    // 泛型编程实现 sort
    func Sort(data MyForm) { // 将 interface 的 MyForm 写在这里
        n := data.Len()
    }

    // 完成实例 实现接口方法
    type ByAge []Person //自定义
    func (a ByAge) Len() int           { return len(a) }
    func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
    func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

    func main() {
       people := []Person{
           {"Bob", 31},
           {"John", 42},
           {"Michael", 17},
           {"Jenny", 26},
       }
       // 绑定
       sort.Sort(ByAge(people))
   }
    ```

# 内存模型

三色标记 gc

## 问题

1. 为什么小对象多会造成 gc压力

# 其他特性

## 匿名函数

```go
func () {
  // 引用
}

func(i int) {
  // 拷贝
}
```

## select

- 如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。
- 若无 case 可以执行
  - 如果有 default 子句，则执行该语句
  - 如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值

## defer

- 出入顺序: 栈 - 后入先出

## '...'

```go
// 1. 函数传参处, 指可以传入多个可变参数
func test1(args ...string) { //可以接受任意个string参数
    for _, v:= range args{
        fmt.Println(v)
    }
}
func main(){
var strss= []string{
        "yui",
        "cvbc",
    }
    test1(strss...) //切片被打散传入
}

// 2. 将slice 打撒传入
var strss= []string{
    "qwr",
}
var strss2= []string{
    "qqq",
    "zzz",
}
strss=append(strss,strss2...) //strss2的元素被打散一个个append进strss
```
