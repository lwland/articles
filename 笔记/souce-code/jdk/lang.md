### java.lang
1. Object 是所有类的父类，所有类包括数组都实现了该类的所有方法
    * registerNatives() native
        通过静态代码块默认调用，加载需要的其他原生方法 类似System.loadlibrary
    * getClass() native
        返回运行时的对象类型，返回的是类型擦除后的实际类型
    * finalize() 
        1. 垃圾回收器检测到没有指向该对象的引用时调用，或者没有被任何活跃的线程使用
        2. 通常子类重写该方法用来回收系统资源或者清理工作，有时用来重新建立引用
        3. 最多执行一次
    * wait()
        1. 使当前线程等待，等待notify/notifyAll唤醒，传入时间时最多等待时间
        2. 调用wait方法必须持有监视器
        3. wait方法把当前线程加到对象的等待队列中，线程休眠，放弃同步锁，直到notify/notifyAll/interrupt方法调用
        4. 线程唤醒时重新竞争锁，获取锁的先执行，所以wait方法应该在loop中使用，如果不满足条件继续wait
        5. wait方法只会对当前对象生效，对临界区的其他锁不生效
    * notify()
        1. 随机唤醒一个等待对象监听器的线程
        2. 唤醒的线程需要获取锁才能继续执行
        3. 只有在同步方法或者同步块中才能调用
    * notufyAll()
        1. 唤醒所有等待对象监听器的线程
    * equals() 
        1. return this==obj 通过==比较
        2. 比较两个对象是否相等，x.equals(x)=true;x.equals(y)=true 则y.equals(x)=true;x.equals(y)=true,y.equals(z)=true,则x.equals(z)=true;x.equals(null)=false
        3. 对于俩个非空引用，x==y表示两个是否是同一个对象
        4. 重写equals()方法必须重写hashcode方法，保证相等对象的hashcode值一致
    * hashcode() native
        为每个对象返回一个独一无二的int值，默认实现返回的是内存地址。应用生命周期中，同一个对象无论在何时调用都返回相同的值；相等的两个对象返回一致的int值
    * clone() native
        1. clone一个对象，y=x.clone(),则必须y!=x,非必须x.getClass()==y.getClass,x.equals(y)
        2. clone和对象的class有关 默认实现调用的super.clone(),如果所有父类遵循这个规则，最终调用的是object的clone方法
        3. 深拷贝和浅拷贝, Object的clone是浅拷贝
        4. 对象必须实现Cloneable才能调用clone，所有的数组默认实现类Cloneable
    * toString()
        1. getClass().getName()+@+Integer.toHexString(hashCode())
2. String 字符序列、不可变、特殊的+其他对象会被+tostring()的值 cache char[] 和 hash
    * 构造方法：
        1. String(),
        2. String(String str)创建一个新对象,
        3. String(char[] old)创建一个新对象，对oldchar[]的改变不会影响string;
        4. String(int[])
        5. String(byte[])
        6. String(StringBuffer()),同步方法 从stringbuffer中获取
    * 普通方法
        1. charAt(int index) indexOf()单字节字符 indexOfSupplementary()两字节字符 lastIndexOf lastIndexOfSupplementary
        2. codePointAt(int index)：返回指定索引处的int值 codePointBefore(int index)：返回（指定索引-1）处的int值  codePointCount(int begin,int end):返回范围内字符的长度 string有时候采用utf-16两个字节才能表示一个字符
        2. getBytes/getChars(char dst[], int dstBegin):复制指定范围的字符到数组中
        3. equals(Object anObject):比较两个字符串是否相等，先==比较 比较引用地址，在比较每个字符是否一致
        4. contentEquals()和Stringbuffer/stringbuilder/string等比较内容是否相等
        5. equalsIgnoreCase() compareTo() compareToIgnoreCase() regionMatches()
        6. startsWith endsWith
        7.  hashcode h = 31 * h + val[i];
