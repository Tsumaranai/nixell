# nixell
### A simple substitute for xshell in Linux and Mac
Linux 与 Mac 平台简单的Xshell替代品

### Use shell to organise ssh login information
用于记录与管理ssh登录的信息

### Make sure that "except","openssl","sed" have been installed in you OS before you use it
使用之前确保在你的操作系统中已经安装了 except openssl sed

### Download the nixell and put it into your "bin" directory(different OS, different directory). You would better to save it in your /usr/local/bin/ if you use MAC OS, and change its permission to be executable. 
下载“nixell”文件 放入你的可执行文件目录（不同操作系统目录也不太一样）MAC OS 的话我放在了/usr/local/bin中，确保其权限能够执行

### Usage:
    -h help
    -n item name
    -i server ip
    -u server username
    -p server password
    -o server port
    -c connection
    -l item list
    -m modify list
    -d delete an item from list
    
    1.create an item:
    创建一个连接信息
        -n 'name' -i 'ip' -u 'username' -p 'password' -o 'port'(optional,defualt 22)
    2.list all items: 
    显示以保存的ssh连接信息
        -l
    3.connect an ssh server:
    连接登录ssh服务器
        -c 'name'
    4.delete an item:
    删除其中一项
        -d 'name'
    5.modify attributes of an item:
    修改其中一项
        -m 'name' -iup... 
    5.1 example: -m vps -p '123' -o 25
        (modify password of "vps" to 123 and port to 25)


