vagrant up	启动本地环境  
vagrant halt	关闭本地环境  
vagrant suspend	暂停本地环境  
vagrant resume	恢复本地环境  
vagrant reload	修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up）  
vagrant ssh	通过 ssh 登录本地环境所在虚拟机  
vagrant destroy	彻底移除本地环境  


vagrant provision 加载更新后的的yml  

vagrant global-status 查询  

传文件  
vagrant plugin install vagrant-scp  
vagrant scp <some_local_file_or_dir> [vm_name]:<somewhere_on_the_vm>  
