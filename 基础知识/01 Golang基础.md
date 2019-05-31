https://chai2010.cn/advanced-go-programming-book/ch2-cgo/ch2-01-hello-cgo.html  

# c对比新增特性
1.string boolean map等  
2.闭包，函数同样当做一种变量(func)  
3.struct拥有继承，可以继承所有成员变量和方法  
4.大写开头可以导出，小写开头仅限内部使用  
5.包特性，必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main  
6.类似python的切片操作  
7.slice

# 关键字
1.nil 代替 NULL  
2.由func、var、const、type四种关键字组成  
3.new 和 make 区别  
make 只能用于 slice, map, channel  
new(T) 返回的是 T 的指针  

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
for i := range m {} == while   
for {} // 无限循环   
## 类型声明
声明必须以保留字开头，类型位于变量名后   
```go
const N = 1024 //常量

type T struct {
	x,y int
} //新类型
var t1 *T = new(T)
t2 := new(T) // 简易复制，表示左边的变量与右边类型相同
var t3 *T = t2

var x int = 1
var x = 1 // 若可以推断则省略类型
x := 1 // x必须是没有被使用过

var a []int = make([]int, len) // []int len capacity
a := []int{0,1,2,3,4,5,6,7,8} // 切片 类似python操作
a := make([]int,0,5) 

func f (i int) float {}
func f2 (i int)(float,int) {}
```

# 泛型
```go
var a[5] string //字符串数组
var s[] string //字符串切片
var m map[int]string //map
var c chan int //管道

f := stacj.get.(float) //cast
```   

# 基本数据结构
## 数组
1.数组是值语义，一个数组变量代表整个数组，并不是指向第一个元素的指针，而是完整的值  
2.使用时为了避免整个值的复制，传递指向数组的指针  
```go
var s1 = [2]string{"hello", "world"}

var a = [...]int{1, 2, 3} // a 是一个数组
var b = &a                // b 是指向数组的指针
fmt.Println(a[0], a[1])   // 打印数组的前2个元素
fmt.Println(b[0], b[1])   // 通过数组指针访问数组元素的方式和数组类似

for i := range a
for i, v := range a // i是 index、v是 value
```
## 字符串
1.字符串元素不能修改，是一个只读字节数组    
2.字符串虽然不是切片，但是支持切片操作  
3.使用写时复制  
```go
s := "Hello World" -> 指向 hello world
hello := s[:5] -> 指向s的头 长度为hello
world := s[7:] -> 指向s的world,长度为world
```
## 切片
1.在判断一个切片是否为空时，一般通过len获取切片的长度来判断，一般很少将切片和nil值做直接的比较。  
```go
1.定义
var a []int           // nil切片, 和 nil 相等, 一般用来表示一个不存在的切片
b = []int{}           // 空切片, 和 nil 不相等, 一般用来表示一个空的集合
c = []int{1, 2, 3}    // 有3个元素的切片, len和cap都为3
d = make([]int, 2, 3) // 有2个元素的切片, len为2, cap为3
2.添加
a = append(a[:i], append([]int{x}, a[i:]...)...)     // 在第i个位置插入x
3.删除
a = a[:len(a)-N]      // 删除尾部N个元素
a = a[N:]             // 删除开头N个元素,其实是重新赋值
4.清空
a = a[:0]
5.引用
a = a[:]
6.使用
类似python的方式,可以直接使用下标访问
a[3:5]
```

# 多线程
## goroutine 
## channel - 类似消息队列
1.创建  
ch := make(chan int, 64) // 引用类型  
通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值  
如果缓冲区已满，需要等待直到某个接收方获取到一个值  
2.方向  
1）定义  
c chan<- int // 向管道输入int类型  
c <-chan int // 管道向外输出int类型
2）使用  
i := <-c  输出  
c <- 2  输入  
3）关闭  
close(c)  
## 锁
## 信号量
sync.WaitGroup  
## 例子
```go
// 生产者: 生成 factor 整数倍的序列
func Producer(factor int, out chan<- int) {
    for i := 0; ; i++ {
        out <- i*factor
    }
}
// 消费者
func Consumer(in <-chan int) {
    for v := range in {
        fmt.Println(v)
    }
}
func main() {
    ch := make(chan int, 64) // 成果队列

    go Producer(3, ch) // 生成 3 的倍数的序列
    go Producer(5, ch) // 生成 5 的倍数的序列
    go Consumer(ch)    // 消费 生成的队列

    // 运行一定时间后退出
    time.Sleep(5 * time.Second)
}
```

# cgo
## 设置
1.import "C"  
import "C"语句则表示使用了CGO特性，紧跟在这行语句前面的注释是正常的C语言代码。 —— 注释的大小全部加入了“C”这个包中  
2.#cgo
注释中加入#cgo  
设置编译阶段和链接阶段的相关参数  
// #cgo CFLAGS: -DPNG_DEBUG=1 -I./include  // 定义相关宏和指定头文件检索路径
// #cgo LDFLAGS: -L/usr/local/lib -lpng  // 指定库文件检索路径和要链接的库文件
## 类型转换
1.传参：需要将go语言中的类型强制转换为c语言中的类型 v:= 1 C.int(v)  
2.指针: *C.char  
3.结构体: C.struct_xxx，若成员名字是go的关键词，那么可以通过名字前面加下划线进行访问  
4.字符串:  
```go
func C.CString(string) *C.char 	  // Go string to C string
func C.GoString(*C.char) string   // C string to Go string
```
5.指针  
1）指针到指针  
unsafe.Pointer指针类型 == C语言中的void*类型的指针  
任何类型的指针都可以通过强制转换为unsafe.Pointer指针类型去掉原有的类型信息，然后再重新赋予新的指针类型而达到指针间的转换的目的。  
2）数值到指针，反之也成立  
a.int32 -> uintptr  
b.uintprt -> unsafe.Pointer  
c.unsafe.Pointer -> *C.char  
6.枚举  
若枚举有别名，则直接使用C.别名  
若枚举没有别名，则使用C.enum_名称  

# 标准包
## 字符串相关
### strings - 查询、替换、比较、截断、拆分和合并等  
1.由于字符串只读不能改变，修改会导致大量内存操作,使用缓存    
strings.Builder  

### bytes - 针对bytes[],提供strings相同的功能
1.同上第一条  
bytes.Builder  

### strconv - 提供布尔型 整数型 浮点数 与对应字符串之间的转换 

### unicode - 提供字符串类型(大小\数字)的判断和转换 


## fmt  
格式化输出、接收输入等  
Println 输出一行  
## os
len(os.Args)  
os.Args[0]  

 
## sort
1.sort.Search(n int, f func(int) bool) int  
返回在0<=i<n范围内，让func为true的最小值，若不存在则返回n

# 常用
## 注意
1.若多返回值，只需要第二个，可以将前一个值使用 '_' 代替  
2.map，slice，chan是引用类型，不需要传递指针的指针  
3.slice和string区别  
string的指针可以改变，但是指向的内容是只读的，不能改变    
slice是数组的抽象  
4.编译器自动选择在栈上还是在堆上分配局部变量,并不是由var还是new声明变量的方式决定的
## 关键字
len  
range  
## 遍历
for i : xx // python c11  
## 实例
```go
func strWithout3a3b(A int, B int) string {
	var res strings.Builder
	res.Grow(A+B)

	a, b := byte('a'), byte('b')

	if A < B {
		A, B = B, A
		a, b = b, a
	}

	for A > 0 {
		res.WriteByte(a)
		A--

		if A > B {
			res.WriteByte(a)
			A--
		}

		if B > 0 {
			res.WriteByte(b)
			B--
		}
	}

	return res.String()
}
```
