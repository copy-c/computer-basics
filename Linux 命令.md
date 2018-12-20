1.配置环境变量
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


2.traceroute ip地址
查看数据包到某地址经过的网络链路  
