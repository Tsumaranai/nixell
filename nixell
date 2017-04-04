#!/bin/bash

cipherkey="nixell_cipher"
home_dir=$HOME"/.nixell/"
line=""


function usage () {
    echo "Usage:
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
        -n 'name' -i 'ip' -u 'username' -p 'password' -o 'port(optional,defualt 22)'
    2.list all items: 
        -l
    3.connect an ssh server:
        -c 'name'
    4.delete an item:
        -d 'name'
    5.modify attributes of an item:
        -m 'name' -iup... 
    5.1 example: -m vps -p '123' -o 25
        (modify password of \"vps\" to 123 and port to 25)
    "
    exit 1
}

function is_exits () {
    #arg = $filepath 1 $name 2
    #echo -n $2
    line=$(grep -w $2 $filepath)
    if [ -z "$line" ] ;
    then   
        return 0
    else
        return 1
    fi
}

function write_into_list () {
    #arg = $name 1 $ip 2 $user 3 $passwd 4 $port 5

    is_exits $filepath $name

    if [ $? -eq 1 ] ; then
        echo "There is already a item named \"$name\" please change another"
        exit 1
    fi
    pwd_cipher=$(echo -n $4 | openssl enc -aes-256-cbc -a -pass pass:$cipherkey)
    
    
    
    echo "$1|$2|$3|$pwd_cipher|$5" >> $filepath
    echo "Infomation is writed into list"
    ssh_connection $1
    exit 0

}


function ssh_connection () {
    #arg = $name
    #ssh $user@$ip:$port
    if [ -z "$name" ] ; then
        echo "pay attention on Usage 3"
        usage
    fi
    is_exits $filepath $name
    if [ ! $? -eq 1 ]; then
        echo "There is not a item named "$name" please check it"
        exit 1
    fi

    info=(${line//|/ })
    #echo ${info[3]}
    pwd_plain=$(echo ${info[3]} | openssl enc -aes-256-cbc -a -d -pass pass:$cipherkey)
    expect -c "
        spawn ssh ${info[2]}@${info[1]} -p ${info[4]};
        expect {
            *yes/no* {send \"yes\r\";exp_continue} 
            *word* {send \"$pwd_plain\r\"}
            };
        interact;"
    exit 0

}

function ssh_list () {

    while read line
    do
        arr=(${line//|/ })
        echo name:${arr[0]} ip:${arr[1]}  username:${arr[2]}
    done < $filepath

    exit 0
}

function ssh_modify () {

    #arg = $origin_name 1 $name 2 $ip 3 $user 4 $passwd 5 $port 6
    is_exits $filepath $1
    if [ ! $? -eq 1 ]; then
        echo "There is not a item named \"$origin_name\" please check it"
        exit 1
    fi
    arr=(${line//|/ })

    if [ -z "$name" ]; then
        name=${arr[0]}
    fi
    if [ -z "$ip" ]; then 
        ip=${arr[1]}
    fi
    if [ -z "$user" ]; then 
        user=${arr[2]}
    fi
    if [ -z "$passwd" ]; then   
        passwd=${arr[3]}
    else
        passwd=$(echo -n $passwd | openssl enc -aes-256-cbc -a -pass pass:$cipherkey)
    fi
    if [ -z "$port" ]; then
        port=${arr[4]}
    fi

    change_content="$name|$ip|$user|$passwd|$port"
    c="/^$origin_name\|/d"
    #echo $c
    sed -i "" $c $filepath
    echo $change_content >> $filepath
    echo "\"$origin_name\" has been modified!"
    exit 0
    
}

function del_item () {

    if [ -z "$name" ] ; then
        echo "pay attention on Usage 4"
        usage
    fi
    is_exits $filepath $name
    if [ ! $? -eq 1 ]; then
        echo "There is not a item named \"$name\" please check it"
        exit 1
    fi
    c="/^$name\|/d"
    sed -i "" $c $filepath
    echo "\"$name\" has been deleted!"
    exit 0
}

if [ ! -d "$home_dir" ]; then
    #echo "directory not exist"
    mkdir -p $home_dir
fi

filepath=$home_dir"list.xsh"
if [ ! -f "$filepath" ]; then
    touch $filepath
fi
port=22
ssh_modify_flag=0


while getopts "n:i:u:p:c:o:lm:d:h" arg
do
    case $arg in

    n)
        name=$(echo -n $OPTARG)
        #echo $OPTARG
        ;;
    i)
        ip=$(echo -n $OPTARG)
        #echo $OPTARG
        ;;
    u)
        user=$(echo -n $OPTARG)
        #echo $OPTARG
        ;;
    p)
        passwd=$(echo -n $OPTARG)
        #echo $OPTARG
        ;;
    c)
        name=$(echo -n $OPTARG)
        ssh_connection $filepath $name
        exit 0
        ;;
    o)
        port=$(echo -n $OPTARG)
        #echo $OPTARG
        ;;
    l)
        ssh_list
        ;;
    m)  
        origin_name=$(echo -n $OPTARG)
        if [ -z "$origin_name" ] ; then
            echo "modify item name cannot be empty, pay attention to usage 5"
            usage
        fi
        ssh_modify_flag=1
        ;;
    d)
        name=$(echo -n $OPTARG)
        del_item $name
        ;;
    h)
        usage
        ;;
    *)
        usage
        ;;
    esac
done

if [ $ssh_modify_flag -eq 1 ]; then
    #echo $origin_name
    ssh_modify $origin_name $name $ip $user $passwd $port
fi

if [ -z "$user" ]||[ -z "$passwd" ]||[ -z "$ip" ]||[ -z "$name" ];
then
    echo "pay attention to usage 1"
    usage
fi

write_into_list $name $ip $user $passwd $port
#echo $name:$ip:$user:$passwd >> list.xsh
