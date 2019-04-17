# linux 设置
## 配置环境变量  
http://www.mashangxue123.com/Linux/2613038403.html    
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


# linux 命令
## 网络
1.traceroute  
traceroute ip地址  
查看数据包到某地址经过的网络链路  
2.lsof   
列出当前系统打开的文件，可以用来查看网络连接  
-i 网络连接 :80显示端口 tcp显示  
3.iftop  
网络详细信息  
4.telnet  
远程登录地址  
telnet ip  
## 搜索
1.find  
find ./ -name "*.cc" 搜索文件名  
2.grep  
grep -nir xx 搜索内容  

## 系统信息
1.du   
-h // 按MB显示  
-s // 只计算总量  
2.htop  
系统详细信息  
3.env  
查看环境变量，可以配合grep进行相关的展示  
## 编译相关
1.nm  
查看符号表  
nm libmrtc-base.so | c++filt // 转换为C++风格显示函数名  


## 应用
1.释放缓存  
echo 1 > /proc/sys/vm/drop_caches  
2.崩溃后继续执行  
sh末尾加上/bin/bash  
3.core 存储位置修改  
echo "/data/core-%e-%t-%p" > /proc/sys/kernel/core_pattern  


