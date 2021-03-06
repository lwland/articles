# 一 系统组成结构
1. 计算机系统由硬件和软件组成，他们共同工作运行应用程序，影响程序的正确行和性能
2. 信息是位+上下文，系统中所有的信息包括磁盘文件、内存数据、网络数据都是一串比特表示，区分这些对象的唯一方式是上下文。
3. GCC的过程：hello.c-{cpp(预处理阶段 将#include的头文件直接插入程序中)}->hello.i-{gcl编译器（编译阶段，将程序翻译成汇编语言程序）}->hello.s-as汇编器（汇编2阶段，将汇编语言翻译成机器指令，并将这些指令打包成可重新定位目标程序)->hello.o-ld链接器（连接阶段 合并使用的标准库函数到目标程序）->hello.exe(可执行目标文件)
4. 计算器的硬件组成
   * 总线 贯穿整个系统的一组电子管道，携带信息字节在计算机的各个组件间传递。字节的长度是固定的（32位（4字节）/64位（8字节））
   * 主存 一组临时存储设备，在处理器执行时临时存储程序和程序处理的数据。由一组动态随机存取器（DRAM）芯片组成，逻辑上是一个线性数组，每个字节都有一个地址对应，字节是内存中最小的寻址单位
   * 处理器 解释和执行存储在主存中的指令的引擎。核心是PC（程序计数器）一个字的寄存器，存储下一条执行的指令地址，寄存器文件（一组单字长的存储器） ALU(算术逻辑单元，计算新数据和地址值) 处理器解释执行PC指向的内存地址指令，执行完成后更新PC指向的地址。高速缓存（L1、L2、L3静态随机存储器，存放可能经常访问的数据，高速缓存的局部性原理）
   * I/O 键盘、鼠标、显示器、磁盘、网络等，通过控制器或者适配器和I/O总线相连，负责系统和外界的联系。
5. 存储设备的层次结构 更慢更大 L0(CPU寄存器)->L1(高速缓存SRAM 位于CPU内部)->L2(高速缓存 SRAM)->L3(高速缓存)->主存（DRAM）->磁盘->网路存储
6. 操作系统：位于应用程序和硬件之间的一层软件，应用程序依靠操作系统提供的服务控制硬件，1、防止应用程序对硬件的滥用；2、提供简单一致的机制控制硬件设备。
7. 操作系统的抽象概念：
    * 文件：对I/O设备的抽象
    * 虚拟内存：对主存和I/O设备的抽象。包括程序代码和数据区（代码和C全局变量）、堆（运行时堆 可以通过free和malloc动态扩展和缩小）、共享库（存放C标准库和数学库的代码和数据）、栈（用于函数调用）、内核虚拟内存
    * 进程：对CPU、主存、I/O设备的抽象。对一个正在运行的程序的抽象，给程序提供了独占硬件的假象。上下文切换：当并发执行时，进程的指令交替执行，这种机制称为上下文切换，
  切换时，操作系统负责保存当前进程的上下文（内存、PC、寄存器），并恢复新进程的上下文。是系统资源分配和调度的基本单位
    * 系统内核：操作系统常驻内存的部分 应用程序通过系统调用将控制权传递给内核，内核处理请求并返回结果。
    * 线程：每个进程由多个执行单元构成，这些执行单元运行在进程的上下文环境中，共享进程的数据和状态。比进程更高效，是CPU调度的基本单位
8. 并发和并行 并发是指同时有多个活动的系统，并行是指使用并发使系统运行的更快。
   系统并发由高到低为：线程级并发 指令级并行（处理器可以同时执行多条指令） 单指令多数据并行（单指令）
# 二 信息的表示（整数、浮点数的二进制表示和运算）
是以有限的位完成对无限数字的编码，因此整数会有范围限制 浮点数会有范围和精度限制
####信息的存储
计算机中数据以一串二进制数存储，根据上下文确定其含义。 大多数操作系统以字节作为最小的内存寻址单位，每个字节有唯一对应的地址
字长决定了寻址空间。
多个字节的类型表示：以一字节作为最小的寻址单位，常用的字节顺序有大端法（最高有效字节从高到底）和小端法（最高有效字节从低到高，linux、window都是小端机）
字节顺序的重要性：网络传输、反汇编时阅读字节的顺序
####整数 
整数计算的性质 结合律 交换律 
十六进制:0X开头 两个位表示一个字节
0. 布尔数（
   * 表示 true 1 false 0
   * 运算 
     * 非 ～ ～1=0 ～0=1
     * 与 &   1&1=1 0&1=0 1&0=0 0&0=0
     * 或 |   1|1=1 0|1=1 1|0=1 0|0=0
     * 异或 ^  1^1=0 0^1=1 1^0=1 0^0=0 
   * 规律 a&(b|c)=(a&b)|(a&c) a|(b&c)=(a|b)&(a|c) a^a=0 (a^b)^a=b a^b=(a&~b)|(~a&bxs)
   * 掩码运算 a&0xFF等 留下最后几位
1. 逻辑运算 所有非0都是true 0是false  || && ! 
2. 移位运算 左移<< 左移K位丢弃高K位，后K位补0;逻辑右移>>> 右移K位，低K位舍弃，高K位补0;算术右移>>:右移K位 低K位舍弃，高K位补最高有效位；java中的int 移位是kmod32
3. 无符号数 二进制表示法 表示大于或等于0的数 T表示补码 U表示无符号数 B表示二进制
   * x=2^n 转二进制1+(n个0) 2048=2^11 1000 0000 0000 ; 转十六进制 n=i+4j 0x 2^i+j个0 2048=2^11->11=3+2*4->0x800
   * 求反 -x=2^w-x
   * 加法 x+y (x+y<UMax x+y,x+y>Umax x+y-2^w) 正溢出；减法
   * 乘法 左移 x*(Uw)y=(x*y)mod 2^w
   * 除法 逻辑右移 十进制表示中是向零舍入
4. 有符号数
   * 补码 最高位的权是-1*(2^(w-1)) -x=2^w-x 有符号数的表示方法 既能表示正数也能表示负数 并且提供和无符号数一样的位级表示
   * 反码 最高位的权是-1*(2^(w-1)-1) -x=2^w-1-x
   * 原码 以最高位表示正负，其余位表示数字
   * 补码转有符号数 当x<0 2^w+x 当x>=0是 x;无符号数转补码 当x<TMax x,当x>Tmax x-2^w
   * 扩展一个数字的位使用更多的位表示一个数字，重点是如何保证扩展过程中数字不会发生变化 无符号数采用零扩展（高位补0） 有符号数采用符号扩展（高位补符号位）
   * short转usigned 顺序是先扩展为int 然后转化成usigned
   * 截断数字 无符号数的截断为k位 x'=x mod 2^k 有符号数截断k位 x'=U2T(T2U(x) mod 2^k)
   * 加法 正溢出 -2^w 负溢出 +2^w
   * 补码的非：当x=Tmin x=Tmin 当x>Tmin x=-x -x=~x+1/从左找第一位1将二进制表示分为左右两边，左边取反，右边不变
   * 无符号数和补码的加法乘法都有位级等价性 加法是完全的，乘法是阶段后具有位级等价性 溢出的截断
   * 乘常数 2的幂采用左移位操作代替，另外常数可以采用移位和加法减法代替 如*14 可以使用 *（2^4-2^1） 
        从二进制表示中找到连续的1的位置n->m x*k可以表示为 x<< n+ x<< n-1 +……+x<< m 或者 x<< n+1 - x<< m
   * 除常数 2的幂采用算数右移操作 十进制表示中是向下舍入，通过+偏移量实现 向上舍入   所以补码的向零舍入为(x< 0? x+(1<< k)-1:x)>>k
####浮点数
1. 单浮点数 以2为基数的科学计数法 对形如v=x*2^y形式的有理数进行编码
   表示：符号位 s 表示该浮点数是正数s=0还是负数s=1 使用1位表示、阶码 对浮点数加权 8位、尾数 23位、
      规格化表示：阶码不为0和255，尾数任意  阶码值 e-(2^k-1)-1(-126 - +127) 尾数M=1+f
      非规格化：阶码全是0，尾数任意 阶码值 1-(2^k-1)-1 为了实现非规格化数想规格化数的平滑转变 尾数M=f   表示0和接近0的数
      无穷大:阶码255 尾数全是0
      NaN:阶码255 尾数不是0
   舍入：采用向偶数舍入 （舍入方式包括 向下舍入、向上舍入、向零舍入等）
   计算：浮点数的计算不满足结合律和分配律，只满足交换律，并且浮点数计算具有单调性，如果a>=b,c>0 a*c>=b*c 如果c< 0 a*c<= b*c
  
# 三 程序的机器级表示
* 历史观点 X86机器的发展和摩尔定律
* 程序编码 
    1. 以C语言为例 源码(.cpp)->预处理(#include文件引入#define扩展)->编译器(汇编代码)->汇编器(二进制)->链接器(和库函数合并)->可执行文件
    2. 机器级编程的两层抽象，
    3. 编译器优化 包括指令重排序、使用快速操作代替慢速操作、甚至将递归转化为迭代
* 使用指令集架构定义了对机器程序的格式和行为，包括处理器的状态（程序计数器 记录将要执行的指令地址；整数寄存器 可以记录16个64位值 可以存储指针、整数值 用于保存程序状态和临时数据；条件寄存器 保存最近执行的指令的状态信息 控制数据流种的变化； 一组向量寄存器 保存一个或多个整数或浮点数），指令的格式、指令的影响）
   操作数指令：
      读内存M、寄存器R[]、立即数$  
      址：立即数寻址$Imm-Imm、寄存器寻址 r->R[r]、绝对寻址Imm->M[Imm]、间接寻址(r)-M[R[r]]、基址+偏移量寻址Imm(r)-M[Imm+R[r]]、变址寻址(r1,r2)->M[R[r1]+R[r2]],Imm(r1,r2)->M[Imm+R[r1]+R[r2]]、比例变址寻址(,r1,s)->M[R[r1]*s],Imm(r,s)->M[Imm+R[r]*s],(r1,r2,s)->M[R[r1]+R[r2]*s],Imm(r1,r2,s)->M[Immm+R[r1]+R[r2]*s]
   移动指令（
      mov将数据从源位置移到目的位置 包括movb、movw、movl、movq、movabsq，需要注意的是单字、双字的移动都是只改低字节位置，四字的移动是将低四位修改，高位置0，八字则是全字节修改，movabsq）
      movz（将较小的源复制到较大的目的，采用零扩展） 
      movs（将较小的源复制到较大的目的，采用符号扩展）。移动组合只有五种，立即数->寄存器$Imm->%r，立即数到内存 $Imm->(%r)、寄存器到寄存器%r1->%r2、寄存器到内存%r1->(r1,r2)、内存到寄存器()->r1。
   压栈出栈指令:栈的内存地址是从高向低的 %rsp记录栈顶的位置, 方法的调用通过栈来实现当前运行的方法位于栈顶
      pushq 栈顶位置%rsp先-8然后将值存入栈中
      popq 先从栈顶位置开始读8字节，然后栈顶位置%rsp+8）
   逻辑和算数指令:（加载有效地址、移位、算数运算、特殊运算），和整数寄存器配合
      leaq S D  D=&S 从内存加载有效地址到D中 S指内存地址 D指的是寄存器
      INC  D    D=D+1   自增              D可以是内存也可以是寄存器
      DEC  D    D=D-1   自减
      NEG  D    D=-D    取负
      NOT  D    D=~D    取反
      ADD  S D  D=D+S   加                
      SUB  S D  D=D-S   减
      IMUL S D  D=D*S   乘
      XOR  S D  D=D^S   异或
      OR   S D  D=D|S   或
      AND  S D  D=D&S   与
      SAL  k D  D=D<< k 左移
      SHL  k D  D=D<< k 左移
      SAR  k D  D=D>> k 算数右移
      SHR  k D  D=D>> k 逻辑右移
      特殊的算数操作 16字节的： 
      IMUL S  R[%rdx]:R[%rax]=R[%rax] * S  有符号乘法
      MUL  S  R[%rdx]:R[%rax]=R[%rax] * S  无符号乘法
      clto    R[%rdx]:R[%rax]=R[%rax]      扩展成16字节
      idivq S R[%rdx]=R[%rdx]:R[%rax] mod S R[%rax]=R[%rdx]:R[%rax] / S   有符号除法
      divq  S R[%rax]=R[%rdx]:R[%rax] mod S R[%rax]=R[%rdx]:R[%rax] / S   无符号除法

   控制指令：条件码寄存器（条件寄存器维护的4个1位标志 算数操作+CMP+test都会修改条件码）Set指令（根据条件码设置寄存器值）
      CF 进位标志
      ZF 零标志
      SF 负数标志
      OF 溢出标志
      CMP 根据两个操作数之差设置标志位
      test 类似&操作
      SET 根据条件码组合将一个字节设置位0或1
      JMP JMP Lable（直接跳转） JMP *Operand（间接跳转 和要跳转的地址之差） (je jne js jns jg jge jl jle ja jae jb jbe)条件跳转  基于条件的控制和基于数据的控制
      计算机通过控制指令实现了 if if-else do-while while(juno to middle granded-do) for等语句 switch（通过跳转表实现，跳转表记录了每个case的地址）
   过程控制（method 通过参数和返回值实现某个功能的一种抽象），过程包括函数、子过程、处理函数、方法 等，过程控制的步骤包括
      传递控制（p调用q call,调用时pc指向q方法第一行指令、方法返回 ret时指向p调用q时的下一条指令）、 call Label call *：调用  ret:返回
      传递数据（p调用q p给q提供参数 q给p提供返回值）、首先把参数复制到寄存器中，超过6的部分通过栈传递 7到n的位置必须时8的倍数，p从rax中获取返回值
      分配和释放内存（p栈中存放返回时的地址，为q分配栈桢 栈桢包括局部变量空间、超过6个参数需要在栈桢中提前存放、调用时存储被调用者寄存器保存的值，、q调用结束后栈桢增加释放空间） 208页函数调用
      过程控制通过运行时栈实现
   数组
* 机器级程序使用的内存地址是虚拟地址