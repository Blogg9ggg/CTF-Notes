# 格式化字符串利用

### 程序崩溃
利用格式化字符串漏洞使得程序崩溃只需要输入若干个 `%s` 即可, 因为栈上不可能每个值都对应了合法的地址, 所以总是会有某个地址可以使得程序崩溃.

### 泄露内存
1. 泄露栈内存  
   * 获取某个变量的值
   * 获取某个变量对应地址的内存  
   
   基本套路:  
   用 gdb 调试程序: `b printf`(下断点) -> `r`(运行) -> 在栈中找到要泄露的内存的地址`[addr]` -> `fmtarg [addr]`(需要用到 Pwngdb) -> 得到偏移
   ```python
   # payload 模板
   payload = "%n$s" # n 是偏移
   ```

2. 泄露任意地址内存
   * 利用 GOT 表得到 libc 函数地址, 进而获取 libc, 进而获取其它 libc 函数地址
   * 盲打, dump 整个程序, 获取有用信息

3. 123

### 实战
1. hijack GOT  
   **原理**  
   在大部分的 C 程序中, libc 中的函数都是通过 GOT 表来跳转的. 此外, <font color=#FF0000>在没有开启 RELRO 保护的前提下, 每个 libc 的函数对应的 GOT 表项是可以被修改的</font>. 因此, 我们可以修改某个 libc 函数的 GOT 表内容为另一个 libc 函数的地址来实现对程序的控制. 比如说我们可以修改 printf 的 GOT 表项内容为 system 函数的地址. 从而, 程序在执行 printf 的时候实际执行的是 system 函数.  
   假设我们将函数 A 的地址覆盖为函数 B 的地址，那么这一攻击技巧可以分为以下步骤:  
   * 确定函数 A 的 GOT 表地址
     * 这一步我们利用的函数 A 一般在程序中已有，所以可以采用简单的寻找地址的方法来找
   * 确定函数 B 的内存地址
     * 这一步通常来说，需要我们自己想办法来泄露对应函数 B 的地址
   * 将函数 B 的内存地址写入到函数 A 的 GOT 表地址处
     * 这一步一般来说需要我们利用函数的漏洞来进行触发。一般利用方法有如下两种
       * 写入函数：write 函数
       * ROP 
         ```
         pop eax; ret;           # printf@got -> eax
         pop ebx; ret;           # (addr_offset = system_addr - printf_addr) -> ebx
         add [eax] ebx; ret;     # [printf@got] = [printf@got] + addr_offset
         ```
       * 格式化字符串任意地址写  
  
    **例题**  
    ./problem/2016-CCTF-pwn3
2. 123