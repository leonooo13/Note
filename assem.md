## 编译过程

  `assembling`：文字指令→二进制

  asm语言→汇编语言

### 寄存器

  CPU 本身只负责运算，不负责储存数据。数据一般都储存在内存之中，CPU 要用的时候就去内存读写数据。但是，CPU 的运算速度远高于内存的读写速度，为了避免被拖慢，CPU 都自带一级缓存和二级缓存。基本上，CPU 缓存可以看作是读写速度较快的内存。

  CPU 缓存还是不够快，另外数据在缓存里面的地址是不固定的，CPU 每次读写都要寻址也会拖慢速度。因此，除了缓存之外，CPU 还自带了寄存器（register），用来储存最常用的数据。也就是说，那些最频繁读写的数据（比如循环变量），都会放在寄存器里面，CPU 优先读写寄存器，再由寄存器跟内存交换数据

### 寄存器类型reg

32位通用寄存器

16位通用寄存器

8位通用寄存器

### 存储器mem

### 立即数imm

## 字符的编码

## 变量

变量名→数据类型→初值表

数据类型：先定义后使用

1. 【byte】字节类型，每个数据8位
    1. 字符串，字符
    2. 对应c语言中的char
    3. 不区别有无符号
    4. 访问下一个数据要+1
2. 【 word】字类型，每个数据16位
    1. 访问下一个数据+2
    2. c语言中short
3.  【 dword】双字类型，每个数据32位
    1. 

初值表：

数据的对齐方式：小端对齐

## 通用数据处理指令

### 传送类指令

1. mov:move
    1. 操作数与原操作数类型必须一致
    2. `mov al ,ah` 寄存器之间传送
    3. `mov bvar,cl`
    4. 段寄存器都是16位

        ![](https://secure2.wostatic.cn/static/5ncV9qcSfktTzKota2XrwH/image.png?auth_key=1687158323-kQ1x6fDjRM3hA6kgUS86Zo-0-2e2253ae1fc65edc94927a66f6d8ef66)
2. XCHG:exchange
数据交换指令，交换原操作数和目的操作数的内容

    ## 堆栈操作指令

    ![](https://secure2.wostatic.cn/static/iUbMD7811oDSDiA27y1qBh/image.png?auth_key=1687158323-iScfusByiJnw3kirDNtyYU-0-616c138bd76c02de9de31367145585e3)

    先进后出、压入数据栈顶减小，弹出数据栈顶增大

    1. 进栈pusn
    2. 出栈pop

    ## 算术运算类指令

    状态标志

    加法指令add adc inc

    减法指令 sub sbb dec neg cmp

    乘法指令 mul

    除法指令 div