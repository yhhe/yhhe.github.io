# 汇编语言

## 1. 基础
- 汇编语言
    - 面向机器编程
    - 涉及硬件细节
- 指令集简介
    - X86指令集（CISC）
        - 复杂指令集
        - 向下兼容
        - 变长指令 1～15字节，多数为2～3字节
        - 多种寻址方式（可访问不对齐内存地址）
    - MIPS指令集（RISC，只有Load，Store可以访问内存）
        - 频繁指令硬件实现，其余指令软件实现
        - 各个指令流水线分段比较均匀
    - ARM指令集
        - 完成简单操作
        - 大多数定长
        - 大多数指令有条件执行模式
- 整数的机器表示
    - 数制
        - X86 1字2字节
        - MIPS 1字4字节
    - 机器字长
        - 一次运算能处理的二进制数据的位数
    - 无符号数带符号数混合使用，带符号数默认转化为无符号数
    - 
- 



## 汇编语言（王爽著）笔记
1. 基础知识
    - 汇编语言有三类指令：汇编指令（有对应的机器码），伪指令，其他符号
2. 寄存器
    - 通用寄存器
        - AX,BX,CX,DX
        - 存放一般性的数据
        - 可分为()H,()L两个八位寄存器
    - 给出物理地址的方法
        - 段地址，偏移地址
        - 物理地址=段地址*16+偏移地址
    - 段寄存器
        - 提供段地址和偏移地址
        - CS,DS,SS,ES
    - CS和IP
        - 指示了CPU当前要读取指令的地址
        - CS代码段寄存器，IP指令指针寄存器
        - mov不能用于设置CS和IP的值
        - jmp cs:ip
        - jmp ax 仅修改IP
        - 
3. 寄存器（内存访问）
4. 第一个程序
5. [BX]和loop指令
6. 包含多个段的程序
7. 更灵活的定位内存地址的方法
8. 数据处理的两个基本问题
9. 转移指令的原理
10. CALL和RET指令
11. 标志寄存器
12. 内中断
13. int指令
14. 端口
15. 外中断
16. 直接定址表
17. 使用BIOS进行键盘输入和磁盘读写