1. **Linux nc**  
   目前知道的一种使用方法:
   `nc -nv [IP地址] [端口号]`  
   (详情: https://blog.csdn.net/zhangxiao93/article/details/52705642)
   
2. **编译指令**  
   1. 32位程序  
   `gcc -m32 -fno-stack-protector -no-pie -o [file_name] [file_name].c`  
   gcc 编译指令中, `-m32` 指的是生成 32 位程序; `-fno-stack-protector` 指的是不开启堆栈溢出保护, 即不生成 canary; `-no-pie` 指的是关闭 PIE(Position Independent Executable), 避免加载基址被打乱.  
   Linux 平台下还有地址空间分布随机化 (ASLR) 的机制. 简单来说即使可执行文件开启了 PIE 保护, 还需要系统开启 ASLR 才会真正打乱基址, 否则程序运行时依旧会在加载一个固定的基址上 (不过和 No PIE 时基址不同). 我们可以通过修改 /proc/sys/kernel/randomize_va_space 来控制 ASLR 启动与否, 具体的选项有:
   * 0, 关闭 ASLR, 没有随机化. 栈, 堆, .so 的基地址每次都相同.
   * 1, 普通的 ASLR. 栈基地址, mmap 基地址, .so 加载基地址都将被随机化, 但是堆基地址没有随机化.  
   * 2, 增强的 ASLR, 在 1 的基础上, 增加了堆基地址随机化.  
  我们可以使用 `echo 0 > /proc/sys/kernel/randomize_va_space` 关闭 Linux 系统的 ASLR, 类似的, 也可以配置相应的参数.  

   2. 123
   
3. 快速找溢出点  
   `cyclic 200` ->  
   将输出的长度为 200 的字符串输入到程序中, 记住无效地址(如 `0x62616164`) ->  
   `cyclic -l 0x62616164`  

4. 123