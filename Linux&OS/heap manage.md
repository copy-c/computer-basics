<https://introspelliam.github.io/2017/09/10/pwn/Linux%E5%A0%86%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86%E6%B7%B1%E5%85%A5%E5%88%86%E6%9E%90%E4%B8%8A/>


## struct 

```c
栈 stack
内存映射区
堆 heap
数据段
代码段
```

![image](https://user-images.githubusercontent.com/19571831/117672737-af7c1c00-b1dc-11eb-9fb0-c6f6394070b8.png)


## mmap & brk

### brk 

1. 从heap底端生长  
2. free 后不释放，要被重新利用  

### mmap

1. 从内存映射区申请  
2. 申请内存 > 128k 才使用mmap  
3. free 直接释放

