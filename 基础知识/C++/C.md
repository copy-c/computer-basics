# 内存操作

|函数|返回|功能|
|------|------|------|
|void *malloc(unsigned int size)|成功：返回所开辟空间首地址, 失败：返回空指针|向系统申请size字节的堆空间|
|void *calloc(unsigned int num, unsigned int size)|成功：返回所开辟空间首地址, 失败：返回空指针|按类型申请num个size字|
|void *realloc(void *p,unsigned int size)|成功：返回所开辟空间首地址, 失败：返回空指针|将p指向的堆空间变为size|
|void free(void *p)|无返回值|释放p指向的堆空间|

```c
1.申请一个动态字符串数组
char **strings = NULL;
strings = realloc(strings, sizeof(char*));

strings[0] = (char*)calloc(10+1, sizeof(char));
memset(strings[0], '1', 10);
strings[0][10] = '\0';

strings = realloc(strings, sizeof(char*)*2);
strings[1] = (char*)calloc(10+1, sizeof(char));
memset(strings[1], '2', 10+1);
strings[1][10] = '\0';
```
