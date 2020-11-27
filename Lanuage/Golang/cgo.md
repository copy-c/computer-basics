# cgo

参考  
<https://chai2010.cn/advanced-go-programming-book/ch2-cgo/ch2-01-hello-cgo.html>

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
3)二维数组字符串指针转换  

```go
typedef struct SeiContent {
    char** contents;
    int count;
} SeiContent;

ptr := *(**C.char)(unsafe.Pointer(uintptr(unsafe.Pointer(c.contents)) + uintptr(i) * unsafe.Sizeof(*c.contents)))
str := C.GoString(ptr)
```

6.枚举  
若枚举有别名，则直接使用C.别名  
若枚举没有别名，则使用C.enum_名称  
