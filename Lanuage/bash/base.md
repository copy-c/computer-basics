
# 特殊符号

```bash
1. 命令替换  
`` == $()

2. 使用变量
${xxx} == $xxx  

3. 整数运算
$[] == $(())
eg:
a=5; b=7; c=2
echo $(( a+b*c ))

```

# 控制

```bash
1. if 

if [ -n "$x" ]; then ... fi == if (!x.empty()) {}
if [ con1 -o con2 ]; == if (con1 || con2)
if founction ; == if (fountion())

2. case

case value in
  "a") xx ;;
  "b") xx ;;
  *) xx ;;

3. for 

for v in value; do
  xxx
done
```

# 函数

```bash

1. 入参

$1: 第一个
$@: 所有值

```
