###jni->jna4
##### 资料
1. 博客 https://blog.csdn.net/xyang81/article/details/41759643
2. JNI文档 https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/jniTOC.html
3. gcc编译：https://blog.csdn.net/arackethis/article/details/43340065
4. 搜索目录 https://blog.csdn.net/erliang20088/article/details/45790047
5. fpic https://stackoverflow.com/questions/13812185/how-to-recompile-with-fpic
#### jni
1. jni允许JVM中运行的java代码和c/c++编写的程序互相调用

#### 实际的例子
1. 
   1. mac
      1. g++ -std=c++11  -L. libfvad_int.a  org_jitsi_webrtcvadwrapper_WebRTCVad.cpp -framework JavaVM -I/$JAVA_HOME/include -I/$JAVA_HOME/include/darwin -dynamiclib -o libwebrtcvadwrapper.dylib
      2. g++ -std=c++11  -L. -lfvad  org_jitsi_webrtcvadwrapper_WebRTCVad.cpp -framework JavaVM -I/$JAVA_HOME/include -I/$JAVA_HOME/include/darwin -dynamiclib -o libwebrtcvadwrapper.dylib
   2. linux g++ -std=c++11  -I/$JAVA_HOME/include -I/$JAVA_HOME/include/linux  -Wl,--whole-archive -L./ -lfvad_int -Wl,--no-whole-archive org_jitsi_webrtcvadwrapper_WebRTCVad.cpp  -fPIC -shared -o libwebrtcvadwrapper.so
   3. c++ -std=c++11  -Wl,--whole-archive libfvad_int.a org_jitsi_webrtcvadwrapper_WebRTCVad.cpp  -L/usr/lib/gcc/x86_64-redhat-linux/4.8.5 -I/$JAVA_HOME/include -fPIC -shared -o libwebrtcvadwrapper.so
   4. gcc --whole-archive -shared -Wl,-soname,libmylib.so -o libmylib.so mylib.o mylib.a
2. ar -t XXX.a查看文件的.o文件
3. gcc,ar,automake
基于jni的库调用使用其他库的问题有两种方式
1. 使用动态库链接，通过命令指定共享库位置
2. 使用静态链接
g++ -Wall -std=c++11  -I/$JAVA_HOME/include -I/$JAVA_HOME/include/linux  -I. -L. -static -lfvad_int  org_jitsi_webrtcvadwrapper_WebRTCVad.cpp -shared  -o libwebrtcvadwrapper.so -fpic 
clang -lstdc++  -L. -lfvad  org_jitsi_webrtcvadwrapper_WebRTCVad.cpp -framework JavaVM -I/$JAVA_HOME/include -I/$JAVA_HOME/include/darwin -dynamiclib -o libwebrtcvadwrapper.dylib


./ffmpeg -y  -f s16le -ac 1 -ar 8000  -acodec pcm_s16le -i /home/work/10561875/5e5d76ff6e04782c-0101290010-1001-9.pcm -ar 8000 -ac 1 -acodec libmp3lame /home/work/10561875/5e5d76ff6e04782c-0101290010-1001-9.mp3
ffmpeg -f s16le -ar 8000 -ac 2 -i out.pcm -ar 44100 -ac 2 out.wav
ffmpeg -y  -f s16be -ac 1 -ar 8000 -acodec pcm_s16le  -i /home/work/10567069/5e5d783c1604eef2-0101290010-1001-75.pcm /home/work/10567069/5e5d783c1604eef2-0101290010-1001-75.mp3
ffmpeg -y  -f s16be -ac 1 -ar 8000 -acodec pcm_s16le  -i 1.pcm 1.mp3
./kafka-console-consumer.sh --topic hyzx-data-entry-all --bootstrap-server 10.16.79.170:9092,10.16.79.174:9092,10.16.79.175:9092,10.16.79.176:9092,10.16.79.177:9092 --from-beginning
./kafka-console-producer.sh --topic hyzx-data-entry-all --broker-list 10.16.79.170:9092,10.16.79.174:9092,10.16.79.175:9092,10.16.79.176:9092,10.16.79.177:9092 --from-beginning