# linux

## 网络

1. traceroute  
traceroute ip地址  
查看数据包到某地址经过的网络链路  
2. lsof   
列出当前系统打开的文件，可以用来查看网络连接  
-i 网络连接 :80显示端口 tcp显示  
3. iftop  
网络详细信息  
4. telnet  
远程登录地址  
telnet ip  
5. curl  
下载网页html内容  
curl http://www.baidu.com  
6. IPV6
    - in /etc/sysctl.conf, add following
    - net.ipv6.conf.all.disable_ipv6 = 1
    - net.ipv6.conf.default.disable_ipv6 = 1
    - Run “sudo sysctl -p” to make it effective


## 时间

date -d 'Thu 2020-07-30 09:36:16 UTC' +%s -> 1596101776



## 搜索

1.find  
find ./ -name "*.cc" 搜索文件名  
find ./ -name "*.hpp" -mtime 0 //(-1 昨天修改, 0 今天修改, +1 昨天之前修改的)
2.grep  
grep -nir xx 搜索内容  

## 文本操作

1.head -n 1 获取第一行  
  tail -n 1 获取最后一行  
2.awk '{print $1}' 获取第一列  
3.输出重定向  
stdout(1) stderr(2)  
\>out.txt 2>&1 //实际为1>out.txt 2>&1，其中1>简写为>，2>&1指将2通过管道输出到1的输出中  
其中 >> 是追加, > 是覆盖  

## 系统信息

1. 硬盘相关 

- du 单个或目录 
  - -h // 按MB显示  
  - -s // 只计算总量  
- df 系统总体

2. htop 系统详细信息  

3. env   查看环境变量，可以配合grep进行相关的展示  

4. 查看终端历史命令  
- vi ~/.bash_history  

5. systemctl status 

- systemctl status vboxdrv.service

## 编译相关

1. nm  
查看符号表  
nm libmrtc-base.so | c++filt // 转换为C++风格显示函数名  

## 其他

1. xargs  
获取前面执行的结果

## 应用

1.释放缓存  
echo 1 > /proc/sys/vm/drop_caches  
2.崩溃后继续执行  
sh末尾加上/bin/bash  
3.core 存储位置修改  
echo "/data/core-%e-%t-%p" > /proc/sys/kernel/core_pattern  
4.配置环境变量  
1）全局变量修改  
/etc/profile  
source /etc/profile  
2）用户变量修改  
~/.bashrc  
source ~/.bashrc  
3）参考  
<http://www.mashangxue123.com/Linux/2613038403.html>  
加载顺序  
/etc/profile  
/etc/paths  
~/.bash_profile , ~/.bash_login , ~/.profile 按照从前往后的顺序读取,只会读取一个  
~/.bashrc  
修改  
sudo vi ~/.bashrc  
export PATH=$PATH:/opt/local/bin:/opt/local/sbin  
source ~/.bash_profile // 生效  
echo $PATH // 查看是否生效


