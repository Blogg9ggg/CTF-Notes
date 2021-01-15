1. `pwnlib.asm()`  
   汇编函数. 将汇编代码转成可直接写入内存的字节序列.  
   **例子:**
   ```python
   >>> asm('mov eax, 0')
    '\xb8\x00\x00\x00\x00'
   ```
   还有反汇编函数 `disasm()`, 将字节序列转成汇编代码:
   ```python
   >>> disasm('\xb8\x0b\x00\x00\x00')
    '   0:   b8 0b 00 00 00          mov    eax,0xb'
   ```

2. `pwnlib.shellcraft`  
   shellcode 生成器. 生成出来的是汇编代码, 常常需要用 `asm()` 函数转成字节序列, 然后才能进行注入.  
   **例子:**
   ```python
   # 最常用, 最简单的 shellcode, 调用 /bin/sh
   shellcraft.sh()
   ```

3. Python ljust() 方法  
   Python ljust() 方法返回一个原字符串左对齐,并使用空格填充至指定长度的新字符串。如果指定的长度小于原字符串的长度则返回原字符串。  
   语法:
   ```python
   # width -- 指定字符串长度
   # fillchar -- 填充字符，默认为空格
   str.ljust(width[, fillchar])
   ```

4. 123