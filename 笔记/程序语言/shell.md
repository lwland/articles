1. shell:shell是连接系统内核和用户的一层中间代理，对系统内核提供保护，对用户提供封装的命令操作硬件设备
2. 见的shell：sh、bash、ash、csh、tcsh
3. 查看系统可用的shell：cat /etc/shells(记录了系统中所有可用的shell) 一般存放在/bin 或者/user/bin下
4. 查看当前系统shell：echo $SHELL SHELL是指明了当前使用的shell的系统变量
5. 进入终端的两种方式：
   * 1、快捷键 Ctrl + Alt + Fn 
   * 2、系统的终端组件 mac快捷 common+空格 输入ter 
6. 提示符：linux中[mozhiyan@localhost ~]$，mozhiyan表示登录的用户，localhost表示登录主机地址，～表示当前用户目录，/表示根目录，$表示等待用户输入命令
7. 提示符格式：linux采用PS1和PS2两种方式指定，PS1控制最外层提示符的格式，PS2控制第二层命令的提示格式。例如上面的例子中PS1 [\u@\h \W]\$的值是，当需要输入附件的值时 PS2提示通过>表示继续输入
8. 创建第一个shell脚本
    ```
    #!/bin/bash # #!是一个约定标记，告诉系统采用哪一种shell # 表示后面的内容是注释
    echo "Hello World !"  #这是一条语句
    ```
9. 执行脚本：
   * 1、chmod +x *.sh 给脚本赋予执行的权限   2、./*.sh 执行       
   * 使用source命令执行  source *.sh
   * 直接/bin/bash *.sh 执行
10. 查看bash的版本
    * 1、bash --version
    * 2、echo $BASH——VERSION
11. 安装软件的方式：linux 下有三种 rpm yum 源码安装 
12. bash变量：bash变量用于存储数据，不需要表明类型，每个变量都会当作字符串存储
    * 定义变量 variable是变量名，value是变量的值  变量必须使用字母、数字、下划线，不能使用shell的关键字
        ```
        variable=value
        variable='value' #单引号包围时 值是什么就输出什么，即使有命令和变量也会直接输出 适用于不需要解析变量、命令的场景
        variable="value" #输出时解析变量和命令的场景
        ```
    * 使用变量
        ```
        author="value"
        author=$variable
        author=${variable} #推荐
        ```
    * 修改变量 重新赋值即可
    * 将命令的结果赋值给变量
        ```
        variable=$(command)
        variable=`command`
        ```
    * 定义只读变量  `readonly variable `
    * 删除变量 `unset variable`
    * 变量的作用域：
      * 全局变量 shell作用域，只在当前shell中有效 每打开一个shell就是创建了shell会话，即使在sh文件和shell函数中直接定义的变量也是全局的
      * 局部变量 只能在当前函数中使用  函数中使用local 修饰的变量 function fuc(){ local a=99}
      * 环境变量 可以在其他shell中使用 使用 export variable 将全局变量导出为环境变量
13. 位置参数 通过$n n>1的形式
    ```
    #!/bin/bash  #a.sh
    echo "language=${1}"
    echo "URL=${2}"
    ./a.sh shell http://www.baicu.com #调用 shell是第一个参数  http://www.baicu.com是第二个参数
    ```
14. 特殊变量：
    * $0 当前脚本的文件名
    * $# 传递给脚本或者函数的参数个数
    * $* 传递给脚本或函数的所有参数 将接收到的每个参数看做一份数据，彼此之间以空格来分隔 参数会合并到一起形成一份数据
    * $@ 传递给脚本或函数的所有参数 将接收到的每个参数看做一份数据，彼此之间以空格来分隔 参数仍然看作独立的一份
    * $? 上个命令的退出状态或函数的返回值
    * $$ 当前shell的进程ID
15. 字符串
    * ${#variable} 获取variable的长度
    * 当字符串不被""包围时，遇到空格认为是字符串结束
    * 字符串拼接 将两个字符串排放在一起就能实现字符串的拼接
    * ${string:start:length} 字符串截取 ${url:0:10} 
    * 从右边开始计数 ${string:0-start:length}
    * 从指定的字符串开始截取 ${string#*chars} 遇到第一个chars开始  ${string##*chars} 遇到最后一个chars开始 
    * ${string%chars*} 截取chars左边的字符 ${string%%chars*} 截取chars左边的字符
16. 数组 可以存储无限数据，下标从0开始 shell是弱类型语言不要求数组内类型一致
    * 定义 array_name=(ele1  ele2  ele3 ... elen) 
    * 赋值 nums[6]=88 或 ages=([3]=24 [5]=19 [10]=12)
    * 获取数组元素的值 n=${nums[index ]}
    * 获取数组所有元素的值 ${nums[*]} ${nums[@]}
    * 获取数组的长度 ${#nums[*]} 或者 ${#nums[@]}
    * 数组拼接 array_new=(${array1[@]}  ${array2[@]}) 或者array_new=(${array1[*]}  ${array2[*]})
    * 删除数组 unset array   删除数组内的一个元素 unset array[index]
17. 内置命令 alias、echo、exit、ulimit -a -H 查看系统限制、printf、test $[num1] -eq $[num2]
18. 流程控制
    * if `if conditin then c1 else fi` 、`if conditin then c1 elif condition then c2 fi`
    * for `for var in item1 item2 ... itemN; do command1; command2… done`
    * while `while condition do command done`
    * util  `util condition do command done`
    * case
        ```
        case 值 in
            模式1)
                command1
                command2
                ...
                commandN
            ;;
            模式2）
                command1
                command2
                ...
                commandN
            ;;
        esac
        ```
    * break
    * continue
19. 输入输出
    * > 将输入重定向到文件中
    * < 将输入重定向到文件中
    * >> 将输出添加到文件中
    * 
20. 数学运算
    * (()) 就是将数学运算表达式放在((和))之间 表达式可以有多个
    * let 表达式
21. 常用命令 引用变量时前面要加$
    * read 命令用来从标准输入文件（一般终端 System.in）读取用户输入
    * wget 从url下载文件到当前目录
    * pwd 当前目录
    * type 判断命令的类型