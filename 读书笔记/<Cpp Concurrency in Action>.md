# 2 线程管理基础
1.启动  
std::thread对象创建(为线程指定任务)时启动  
2.停止  
myThread.detach() // 不等待线程结束（线程仍然在执行中）
myThread.join() // 等待线程结束
