1. 开始指令  
   `gdb ./name` (name: 可执行文件名)
2. disas  
   `disas function` (反汇编函数function)
3. 设置断点  
   `b *[addr]`
4. 显示变量值(运行到相应得断点处)  
   `r` (如果用的是 peda 的话, 可以显示 registers, code, stack)
5. `vmmap`  
   分析应用程序使用虚拟和物理内存的情况. 可以显示内存中各段的情况(例如, 是否可读, 可写, 可执行).
   ```
   权限字段: rwxsp
   r -> read
   w -> write
   x -> execute
   s -> shared
   p -> private (copy on write)
   ```

6. gc