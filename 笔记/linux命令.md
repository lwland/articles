#### Linux下SSH远程连接断开后让程序继续运行解决办法
一、screen安装
　　yum  install screen   #CentOS安装
　　sudo apt-get install screen #ubuntu安装
二、screen常用命令
　　screen -S mariadb   #新建一个叫mariadb的session
　　screen -r mariadb   #回到mariadb   这个session
　　screen -X -S mariadb  quit # 删除叫mariadb的session
　　screen -ls #列出当前所有的session
　　screen -d  mariadb  #远程detach叫mariadb的session
三、操作示例
　　1.创建会话窗口
　　　　screen -S mariadb
　　2.在会话窗口中执行命令
　　　　yum install mariadb-server mariadb-client
　　3.此时可以关闭ssh窗口或离开会话窗口了
　　　　离开会话窗口是先执行ctrl + a ,后ctrl + d
　　4.重新登录ssh查看mariadb窗口运行状态
　　　　screen -r mariadb