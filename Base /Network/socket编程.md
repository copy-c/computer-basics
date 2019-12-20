# socket编程  

## 基本函数  

socket()  
bind()  
listen()  
connect()  
accept()  
read()  
write()  
close()  

## 与I/O复用的配合  

bind() -> listen -> select/poll/epoll(帮助监听和阻塞) -> accept()  
