###git原理 git本质上是一个内容寻址文件系统，在文件系统上提供了版本控制的界面
1. 命令
    1. 高层命令 git init,git clone
    2. 底层命令
       1. git hash-object (-w存储数据对象) 存储数据对象objetc中，生成一个sha-1哈希校验和
       2. git cat-file (-p -t) 获取object
       3. git update-index 将一个文件加入暂存区 
          1. --add 指定加入暂存区 
          2. --cacheinfo指定加入git仓库 
          3. 文件模式 100644普通文件 100755可执行问价 120000符号连接
       4. git write-tree 将暂存区内容生成一个tree
       5. git read-tree --prefix 指定将已有tree加入暂存区
       6. git commit-tree 指定tree提交到仓库
       7. git updete-ref refs/heads/master tree 查看/更新引用
       8. git symbolic-ref HEAD （查看/设置HEAD引用）
       9. git gc git打包 本地存储时对象修改都会生成一个新的对象，当调用git gc或向远程推送时会把提交记录的文件打包，生成一个pack文件和idx文件
2. 文件目录 目录下执行git init 初始化一个仓库 生成了一个.git目录，.git目录下
   1. HEAD 指示目前被检出的分支
   2. refs/ 指向分支的提交对象的指针
   3. objects/ 存储所有数据
   4. index/ 保存暂存区信息
   5. logs/ 提交记录
   6. description 主要是供gitweb使用
   7. config 项目特有的配置 远程仓库位置
   8. hooks/ 客户端或服务端的钩子脚本
   9. info/ 包含一个全局排除文件
3. 内部存储 是一个tree tree的节点可以是blob 和 tree
   1. blob 数据对象 ->object 下的
   2. tree 指向一个tree的指针 代表这一个快照
   3. 提交对象 指明了顶层tree和父提交的对象
   4. 标签对象 指向一个提交对象，包括标签创建者，日期，注释和一个指针
4. 生成内部存储对象
   1. 生成header blob 内容的length 数据对象blob开头 tree对象blob开头 commit对象commit开头
   2. 生成store  header+content
   3. 生成sha1 Digest::SHA1.hexdigest(store)
   4. 存储内容 zlib::Deflate.deflate(store)
   5. 在.git/objects下创建目录 并把文件写入
5. 引用
   1. 标签引用
   2. HEAD引用
   3. 远程引用 refs/remotes/origin/
6. 引用规格 和远程引用有关
   1. git remote add origin会在.git/config种添加远程仓库的位置 url代表远程仓库的位置 fetch代表远程仓库在本地的引用。git获取远程仓库refs/heads/下所有引用并记录到refs/remotes/origin/下。具体配置可以在config中配置
   2. git push origin config push代表本地分支和远程仓库分支的对应
   3. 删除远程分支 git push origin --delete topic/git push origin :topic
7. 传输协议
   1. 哑协议 拉取是一系列Http get请求
   2. 智能协议
   3. 通过update-server-info拉取refs/info文件