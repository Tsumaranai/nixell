A simple substitute for xshell in Linux and Mac

Use shell to organise ssh login information

Make sure that "except","openssl","sed" have been installed in you OS before you use it

Download the nixell and put it into your "bin" directory(different OS, different directory). You would better to save it in your /usr/local/bin/ if you use MAC OS, and change its permission to be executable. 

Usage:
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
        -n 'name' -i 'ip' -u 'username' -p 'password' -o 'port'(optional,defualt 22)
    2.list all items: 
        -l
    3.connect an ssh server:
        -c 'name'
    4.delete an item:
        -d 'name'
    5.modify attributes of an item:
        -m 'name' -iup... 
    5.1 example: -m vps -p '123' -o 25
        (modify password of "vps" to 123 and port to 25)