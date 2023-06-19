### 1. 概述
  1. 实验设计目的：了解RISC-V的基本指令及其使用方法，会用Verilog设计支持该指令集的CPU
  2. 任务要求：用硬件描述语言设计支持该指令集的CPU，基本要求是完成5条指令的单周期微架构设计
  3. 实验环境：windows10，Quartus 32-bit，welab实验平台

### 2. RISC-V概述

  RISC-V是基于已建立的精简指令集-----RISC（相较于复杂指令集CISC,如英特尔的X86架构，应该是应用最广的复杂指令集，没有之一）原则的一个开源指令集（ISA）。

  与大多数指令集相比，RISC-V指令集可以自由地用于任何目的，允许任何人设计、制造和销售RISC-V芯片及软件。RISC-V于2010年始于加州大学伯克利分校，2015年基金会成立，但许多贡献者是该大学以外的志愿者和行业工作者。它并不是第一个开源指令集，但为什么说觉得意义非凡，那些过往的，出身名门且高贵的架构啊指令集比比皆是，如：SPARC作为SUN公司经典的RISC微处理器架构之一（其痛点在庞大的寄存器族群（庞大数量的专门类寄存器可以带来高性能，但是弊端也不会少），并不是任何场景条件下需要使用到如此多资源，这样做不仅仅是浪费了晶片面积！

  RISC-V指令集的设计考虑了小型、快速、低功耗的现实要求实现。其设计使其适用于现代计算设备（计算机云计算、移动电话和微小嵌入式）。设计之初考虑到了这些用途中的性能与功率效率。指令集还具有众多支持的软件，从而解决了新指令集通常的弱点。

## 3. 单周期RISC-V设计与验证

### 1. 起点：只有一条addi指令的RISC-V

  assign aluOut = regReadData1 + immData;

  生成一个立即数与RD1相加送入ALU寄存器中

  实验验证：addi指令如下

    `0：  00600913    addi x18 x0 6`

    `4:   fff90a13    addi x20 x18 -1`

    `8:   000a0993    addi x19 x20 0`

    实验结果：

    ![](https://secure2.wostatic.cn/static/moXD5FdHUXSht2ymHRtQm/image.png?auth_key=1687159683-vWzXftncta1tfD78eTT1ch-0-a1aa4241071336853f2c605d4d3f4fb0)

    1. 将6送入x18寄存器
    2. x18寄存器与-1相加送入x20寄存器
    3. x20寄存器与0相加送入x19寄存器

### 2.  支持I型的运算指令
  1. I型的数据通路，无需增加数据通路

      ![](https://secure2.wostatic.cn/static/rRKjnVRZNz9ofFpNVSJTzS/image.png?auth_key=1687159683-xhpg697JUED5DqRKWtogkD-0-569b6619786cfcceea0c613339d7001f)

      1. MainDecoder中

```Verilog
case(iOpcode)
7'b0010011: 
begin // I-Type运算类指令
  oImm_type[4:0] <= 5'b00001;//I类型立即数
  oRegWrite <= 1'b1;//寄存器写信号允许
  ALUop<=2'b11;//ALU选择器
  memtoreg<=1'b0;
  end 
```

```Verilog
always_latch
if (iALUop==2'b11)
begin
case(iFunct3)//ifunct3来确定alu选择哪一种运算方式
3'b000:oALUctrl=4'b0001;//加法
3'b111:oALUctrl=4'b0011;//和
3'b110:oALUctrl=4'b0100;//OR
3'b100:oALUctrl=4'b0101;//XOR
endcase
end
```

@00000001  FFF90A13  // addi x20, x18, -1 

@00000002  000A0993  // addi x19, x20, 0

@00000003  0039e193   //ori x3 x19 3`  5（101）和3（11）进行或运算7（111）`

@00000004  0029c293  //xori x5 x19 2`  5(101)和2(10)进行异或操作7（111）`

@00000005  00197213  //andi x4 x18 1`  6(110)和1(1)进行与操作0（000）

  6.实验结果

    ![](https://secure2.wostatic.cn/static/k1PyvWd3dnU1G4PVBuq3H3/yjzy.gif?auth_key=1687159684-8pMVWFM8NQ8SPL4af2Fiwc-0-7e92ae7718e220632bbc87b896618931)

### 3. 实现Store指令
  1. store数据通路

  ![](https://secure2.wostatic.cn/static/afG7gRqMytGXCpkkmqgB33/image.png?auth_key=1687159683-aNNdY54hx1QnHYsSWfFfSS-0-9e542add575209d0d1bbc30584ffe4ed)

  1. 需要在RD2和MemToReg之间添加数据存储器RAM可读可写
  2. MainDecoder中

```Verilog
case(iOpcode)
    7'b0100011: begin  
    oImm_type[4:0]<=5'b0000010;//s型立即数
    oRegWrite<=1'b0;
    ALUop<=2'b00;
    memtoreg<=1'bx;
    MemWrite<=1'b1;//存储器写允许
    end
```

```Verilog
if(iALUop==2'b00)//day2
begin
case(iFunct3)
3'b000:oALUctrl=4'b0001;//SB
3'b001:oALUctrl=4'b0001;//SH
3'b010:oALUctrl=4'b0001;//SW，选择加法操作
endcase
end
```

@00000001  FFF90A13  // addi x20, x18, -1 5

@00000002  000A0993  // addi x19, x20, 05

@00000003  01202023  //sw x18 0(x0)//`将x18寄存器内容存入(x0)+0中`

@00000004  01402223  //sw x20 4(x0)//`将寄存器x20的内容(x0)+4中
  1. 实验结果

  ![](https://secure2.wostatic.cn/static/cj8YMuEmNUoxfk5yGBqkr9/store.gif?auth_key=1687159683-hpJyzxCtCiGBwNDzruw4kg-0-f450fa90ff789f4db18a9b1bca8c5b79)

### 4.  实现load指令

数据通路、控制器，核心设计代码阐释，验证结果分析等。
  1. load数据通路

      ![](https://secure2.wostatic.cn/static/pyNyXcasKQq8cvNX98GX3d/image.png?auth_key=1687159683-dyZPv1GsPXXJNaSGRtAnUe-0-28a04260987cde1b1b76503889d0dd80)
  2. 增加选择器MemToReg为1是选择RAM读出数据outdata
  3. miandecoder

```Verilog
case(iOpcode)
7'b0000011:
 begin  
   oImm_type[4:0]<=5'b00001;//I类型立即数
   oRegWrite<=1'b1;
   ALUop<=2'b00;
   memtoreg<=1'b1;
   MemWrite<=1'b0;
   end 
```
  4. aludecoder

```Verilog
if(iALUop==2'b00)//sw
begin
case(iFunct3)//
3'b000:oALUctrl=4'b0001;//SB
3'b001:oALUctrl=4'b0001;//SH
3'b010:oALUctrl=4'b0001;//SW和lw同一个,借助sw中的指令
endcase
end
 
```

@00000001  FFF90A13  // addi x20, x18, -1 |5

@00000002  000A0993  // addi x19, x20, 0|5

@00000003  01202023  //sw x18 0(x0)

@00000004  01402223  //sw x20 4(x0) 20送入4+0存器

@00000005  00402203  //lw x4 4(x0) 4+(x0)送入X4寄存器`
  1. 实验结果

      ![](https://secure2.wostatic.cn/static/uQGaZU4rR9Zm8DB3d6hvs2/load.gif?auth_key=1687159683-cNpFq5UEJZJQGQE3dXVG82-0-5031216c5e0c989475376f88b82383b2)

### 5.  实现R型运算指令
  1. R型数据通路

      ![](https://secure2.wostatic.cn/static/6xiFui29o1ZRcLD7gwYZJ5/image.png?auth_key=1687159683-gwaLqQeP9CXCa2JhWwMwEk-0-b27afaf00c14f1161e4faffb86fa1de6)

      数据通路、控制器，核心设计代码阐释，验证结果分析等。
  2. 增加ImmToAlu控制器为1选择RD2输出，MemToReg为0选择aluout输出
  3. maindecoder

```Verilog
case(iOpcode)
7'b0110011:
    begin  
   oImm_type[4:0]<=5'bx;//R型立即数
   oRegWrite<=1'b1;
   ALUop<=2'b10;//根据fun3和fun7
   memtoreg<=1'b0;
   MemWrite<=1'b0;
   ImmtoAlu<=1'b0;
   end
```
  4. aludecoder

```Verilog
if(iALUop==2'b10)//R型运算指令
begin
case({iFunct7,iFunct3})//
10'b0000000000:oALUctrl=4'b0001;//add
10'b0100000000:oALUctrl=4'b0010;//sub
10'b0000000100:oALUctrl=4'b0101;//xor
10'b0000000110:oALUctrl=4'b0100;//or
10'b0000000111:oALUctrl=4'b0011;//and
endcase
end 
```

@00000001  FFF90A13  // addi x20, x18, -1 |5

@00000002  000A0993  // addi x19, x20, 0|5

@00000003  014981b3 //add x3 x19 x20`
  1. 实验结果

      ![](https://secure2.wostatic.cn/static/aDpLzdFoVkaQnF9Xcx55TX/R型add.gif?auth_key=1687159683-3UiWSveyFG9BoTQFrszjwT-0-0372da68c4fed4489589cd6021482856)

### 6. 分支指令
  1. beq指令

      ![](https://secure2.wostatic.cn/static/5r32hyKfEBDUMxikuBU5SM/image.png?auth_key=1687159683-cc6bxtXCvhiqun8t95sLt1-0-4677cbdc5c45a920cb40a1cc57c74632)
  2. 将立即数与pcjump连接
  3. miandecoder

```Verilog
case(iOpcode)
7'b1100011: begin  
    oImm_type[4:0]<=5'b00100;//分支转移指令
    oRegWrite<=1'b0;
    ALUop<=2'b01;//根据fun3和fun7
    memtoreg<=1'bX;
    MemWrite<=1'b0;
    ImmtoAlu<=1'b0;
    end
```

```Verilog
if(iALUop==2'b01)//beq
begin
oALUctrl=4'b0010;//减法
end
```

```Verilog
assign nextPC=aluOut?pc+4:pc+immData;
```

@00000001  FFF90A13  // addi x20, x18, -1 |5

@00000002  000A0993  // addi x19, x20, 0|5

@00000003  014981b3 //add x3 x19 x20

@00000004  ff498ae3//beq x19 x20 -12 <3>

@00000005  ff4908e3  //  beq x18 x20 -16 <3>`
  1. 实验结果

  ![](https://secure2.wostatic.cn/static/ckmgoKr53QUJ6Pk16hsGc6/beq.gif?auth_key=1687159683-vQdLZgkjM6s7zwxHcpLHGt-0-a594f9b5a386d7bc7ea96bd175eec82e)

### 1.7 遇到的问题与解决方法

查找相关资料，访问相关网站

### 1.8 设计的创新性

  ## bne指令

  aludecoder

```Verilog
if(iALUop==2'b01)//beq
begin
//oALUctrl=4'b0010;
case(iFunct3)
3'b000:oALUctrl=4'b0010;//beq
3'b001:oALUctrl=4'b0010;//bne
//3'b100:oALUctrl=4'b0001;//blt
endcase
end
```

```Verilog
always_latch
begin
  if(funct3==3'b000)
  nextPC=aluOut?pc+4:pc+immData;
  else if(funct3==3'b001)
  nextPC=aluOut?pc+immData:pc+4;
end
```

@00000001  FFF90A13  // addi x20, x18, -1 |5

@00000002  000A0993  // addi x19, x20, 0|5

@00000003  014981b3 //add x3 x19 x20|  10

//@00000004  ff498ae3//beq x19 x20 -12 <3>|  0

@00000004  ff4908e3  //  beq x18 x20 -16 <3>6-5

@00000005  fe0a16e3//  bne x20 x0 -20 <3>0和5不相等jialijihshu

@00000006  ff4994e3  //bne x19 x20 -24 <3>`

  实验结果

  ![](https://secure2.wostatic.cn/static/vA7kMsXwrYYcEtnHaav6pn/bne.gif?auth_key=1687159684-4jMFrEwa5tNzFA5TeUmbbY-0-23a30137fd5e56edf16bb2eeca287992)

assign nextPC=aluOut?pc+4:pc+immData;

assign nextPC=aluOut?pc+4:pc+immData;

指令集

`@00000000   00600913  // addi x18, x0, 6  |6

maindecoder与beq相同

虚拟面板

1. 指令验证

    `@00000000   00600913  // addi x18, x0, 6  |6

5.虚拟面板

1. aludecoder
2. 虚拟面板

```Verilog
assign alu_y=cImmtoAlu?immData:regReadData2;
 assign WD = {
  alu_y,
    regWriteData,
    outdata,
    regReadData2,
      aluOut,
      zero,   // 1
      immData,
      regReadData1,
      ra2,    // 5
      ra1,    // 5
      wa,     // 5
      instr_code,
      pc,
      nextPC
  };
```
3. 指令验证

    `@00000000   00600913  // addi x18, x0, 6  |6
4. 虚拟面板

```Verilog
assign regWriteData=cMemtoReg?outdata:aluOut;//load数据通路
assign WD = {
     regWriteData,//增加寄存器写数据进行观察
     outdata,
     regReadData2,
       aluOut,
       zero,   // 1
       immData,
       regReadData1,
       ra2,    // 5
       ra1,    // 5
       wa,     // 5
       instr_code,
       pc,
       nextPC
   }; 
```
5. 指令验证

    `@00000000   00600913  // addi x18, x0, 6  |6
6. ImmGen：增加S型立即数的生成

    `wire [31:0]  s_imm =  { {20{instr[31]}}, instr[31:25], instr[11:7] };`
7. 虚拟面板

`RAMinstrMem1(.iClk(clk),.iWR(cMemWrite),.iAddress(aluOut),.iWriteData(regReadData2),.oReadData(outdata));`//调用RAM模块读出的数据赋给outdata

1. 

`@00000000   00600913  // addi x18, x0, 6  6

1. AluDecoder中
2. 虚拟面板中

```Verilog
ALU #(32) alu(
 .iX(regReadData1), 
 .iY(alu_y), //立即数
 .iCtrl(cALUctrl), 
 .oF(aluOut), //运算结果
 .oFlag(flag)
);
```
3. 指令验证

    `@00000000   00600913  // addi x18, x0, 6
4. ALUDecoder中