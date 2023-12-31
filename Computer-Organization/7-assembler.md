# 汇编程序

## 概念

### 定义

在计算机组成原理中会涉及汇编语言的基本指令，需要对基本命令有基本的掌握。

机器语言是机器指令的集合。这些机器指令本质上就是由一组0和1组成的命令，是CPU唯一能解释执行的命令。

机器语言和高级语言之间的就是汇编语言，汇编语言是人类可以理解的语言。

汇编语言的主体是汇编指令，汇编指令和机器指令的差别在于指令的表示方法上，汇编指令是机器指令的助记符，汇编指令是更便于记忆的一种书写格式。它较为有效地解决了机器指令编写程序难度大的问题，汇编语言与人类语言更接近，便于阅读和记忆。使用编译器，可以把汇编程序转译成机器指令程序。

```txt
1000100111011000
MOV AX,BX
```

汇编语言不区分大小写。

表示把寄存器BX的内容移动到寄存器$AX$中。

### 运行流程

这就需要对汇编程序进行编译，即将其翻译成由机器指令组成的机器码，如此计算机就能执行汇编程序上。本质上，计算执行的仍然是机器指令，而非汇编指令。对更多高级的编程语言（比如$C++$等），使用高级语言编写的程序同样需要先转译成汇编程序，再编译成机器码，从而进一步地在机器上运行。这个过程，即编译过程。

把机器码程序（机器指令程序）转译成汇编指令，即反编译过程。

### 汇编语言组成

1. 汇编指令：与机器指令一一对应，它是机器码的助记符，汇编后变成机器语言可以被计算机$CPU$执行。
2. 伪指令：由编译器识别执行，用于指示汇编程序如何汇编源程序，也称为命令语句，编译结束后伪指令失去作用，计算机$CPU$不执行。
3. 其它符号：如+、-等，由编译器识别执行，没有对应机器码。

汇编语言的核心是汇编指令，汇编指令决定了汇编程序的特性。

## 8086CPU

是一个由$Intel$于$1978$年所设计的$16$位微处理器芯片，是$x86$架构的鼻祖。

### 寄存器

$8086CPU$有$14$个$16$位寄存器，既能处理$16$位数据，也能处理$8$位数据，分别为：

通用寄存器，用来存放一般性数据：

+ 数据寄存器：一般用于存放参与运算的操作数或运算结果。分为两个字节，高位和低位，其中高位为$\cdots H$，低位为$\cdots L$，如$AX$分为两个八位的$AH$和$AL$：
  + $AX$（$Accumulator$）：累加器。用该寄存器存放运算结果，可提高指令的执行速度。此外，所有的$I/O$指令都使用该寄存器与外设端口交换信息。
  + $BX$（$Base$）：基址寄存器。$8086/8088CPU$中有两个基址寄存器$BX$和$BP$。$BX$用来存放操作数在内存中数据段内的偏移地址，$BP$用来存放操作数在堆栈段内的偏移地址。
  + $CX$（$Counter$）：计数器。在设计循环程序时，使用该寄存器存放循环次数，可使程序指令简化，有利于提高程序的运行速度。
  + $DX$（$Data$）：数据寄存器。在寄存器间接寻址的$I/O$指令中存放$I/O$端口地址；在做双字长乘除法运算时，$DX$与$AX$一起存放一个双字长操作数，其中$DX$存放高$16$位数。
+ 地址指针寄存器：
  + $SP$（$Stack\,Pointer$）：堆栈指针寄存器。在使用堆栈操作指令（$PUSH$或$POP$）对堆栈进行操作时，每执行一次进栈或出栈操作，系统会自动将$SP$的内容减$2$或加$2$，以使其始终指向栈顶。其中$ESP$（$Extended Stack Pointer$）为扩展栈指针寄存器，是$32$位寄存器，用于存放函数栈顶指针。
  + $BP$（$Base\,Pointer$）：基址寄存器。作为通用寄存器，它可以用来存放数据，但更经常更重要的用途是存放操作数在堆栈段内的偏移地址。$EBP$（$Extended Base Pointer$），扩展基址指针寄存器，也被称为帧指针寄存器，是$32$位寄存器，用于存放函数栈底指针。
+ 变址寄存器：
  + $SI$（$Source\,Index$）：源变址寄存器。存放源串在数据段内的偏移地址。
  + $DI$（$Destination\,Index$）：目的变址寄存器。存放目的串在附加数据段内的偏移地址。

段寄存器，为了对$1M$个存储单元进行管理，$8086/8088CPU$对存储器进行分段管理，即将程序代码或数据分别放在代码段、数据段、堆栈段或附加数据段中，每个段最多可达$2^{16}=64K$个存储单元。段地址分别放在对应的段寄存器中，代码或数据在段内的偏移地址由有关寄存器或立即数给出：

+ $CS$（$Code\,Segment$）：代码段寄存器。用来存储程序当前使用的代码段的段地址。$CS$的内容左移四位再加上指令指针寄存器$IP$的内容就是下一条要读取的指令在存储器中的物理地址。
+ $SS$（$Stack\,Segment$）：堆栈段寄存器。用来存储程序当前使用的堆栈段的段地址。堆栈是存储器中开辟的按先进后出原则组织的一个特殊存储区，主要用于调用子程序或执行中断服务程序时保护断点和现场。
+ $DS$（$Data\,Segment$）：数据段寄存器。用来存储程序当前使用的数据段的段地址。$DS$的内容左移四位再加上按指令中存储器寻址方式给出的偏移地址即得到对数据段指定单元进行读写的物理地址。
+ $ES$（$Extra\,Segment$）：附加数据段寄存器。用来存储程序当前使用的附加数据段段的段地址。附加数据段用来存放字符串操作时的目的字符串。

段寄存器|提供段内偏移地址的寄存器
:------:|:----------------------:
CS|IP
DS|BX、SI、DI或一个16位数
SS|SP或BP
ES|DI（用于字符串操作指令）

控制寄存器：用于存放控制数据：

+ $IP$（$Instruction\,Pointer$）：指令指针寄存器。用来存放下一条要读取的指令在代码段内的偏移地址。用户程序不能直接访问$IP$。
+ $FLAGS$：标志寄存器，也称为程序状态寄存器（$PSW$）。它是一个$16$位的寄存器，但只用了其中的$9$位，包括$6$个状态标志位和$3$个控制标志位。

$FLAGS$标志位的索引从0到15，则：

状态标志位，用于查看运算数据状态：

简写|英文名称|中文名称|索引|描述|用途
:--:|:------:|:------:|:--:|:--:|:--:
CF|Carry Flag|进位标志位|0|当进行加减运算时 ，若最高位发生进位或借位，则CF为1，否则为0|判断十六位无符号数运算结果是否超出了计算机所能表示的无符号数的范围
PF|Parity Flag|奇偶标志位|2|当指令执行结果的低8位中含有偶数个1时，PF为1，否则为0|奇偶校验
AF|Auxiliary Flag|辅助进位标志位|4|当执行一条加法或减法运算指令时，若结果的低字节的低4位向低字节的高4位（0-3向4-7）有进位或借位，则AF为1，否则为0|判断四位无符号数运算结果是否超出了计算机所能表示的无符号数的范围，如BCD码
ZF|Zero Flag|零标志位|6|若当前的运算结果为0，则ZF为1，否则为0|快速判断0
SF|Sign Flag|符号标志位|7|若运算结果的最高位为1，SF=1，否则为0|快速获取符号
OF|Overflow Flag|溢出标志位|11|当运算结果超出了带符号数所能表示的数值范围，即溢出时，OF=1，否则为0|该判断带符号数运算结果是否溢出

控制标志位有三个，用于控制CPU的操作，由程序设置或清除：

简写|英文名称|中文名称|索引|描述|用途
:--:|:------:|:------:|:--:|:--:|:--:
TF|Trap Flag|跟踪（陷阱）标志位|8|若将TF置1，CPU处于单步工作方式，每执行一条指令后产生一次单步中断|简化测试程序
IF|Interrupt Flag|中断允许标志位|9|若将IF置1，表示允许CPU接受外部从INTR引脚上发来的可屏蔽中断请求；若用CLI指令将IF清0，则禁止CPU接受可屏蔽中断请求信号|控制CPU是否可以响应CPU外部的可屏蔽状态中断请求
DF|Direction Flag|方向标志位|10|若将DF置为1，串操作按减地址方式进行，也就是说，从高地址开始，每操作一次地址自动递减；否则按增地址方式进行|串操作时控制数据计算的方向

### 寻址

有$16$根数据线和$20$根地址线。

由于$20$根地址线，所以可寻址的内存空间为$2^{20}=1MB$。但是由于数据线只有$16$位，所以$CPU$内部只能传输$16$位的地址。所以为了解决这个问题，$8086CPU$使用了两个十六位地址通过地址加法器合成一个二十位物理地址的方式。

其中定义第一个地址为段地址，第二个地址为偏移地址，即物理地址=段地址$\times16+$偏移地址。

如传入两个十六位地址，$1230H$和$00C8H$，然后$1230H\times16=12300H$（乘其对应的进制数等价于左移一位），然后$12300H+00C8=123C8$得到最终地址。

所以给定一个段地址，仅通过变化$n$位的偏移地址来寻址最多可以定位$2^n$个内存单元。

### 工作流程

在$8086CPU$加电启动或复位后（即$CPU$刚开始工作时）$CS$被设置为$FFFFH$，而$IP$被设置为$0000H$，即在$8086PC$机刚启动时，$CPU$使用地址加法器从内存$FFFF\times10H+0000H=FFFF0H$单元中读取指令执行，$FFFF0H$单元中的指令是$8086PC$机开机后执行的第一条指令。

+ 在$CPU$中，程序员能够用指令读写的部件只有寄存器，程序员可以通过改变寄存器中的内容实现对$CPU$的控制。
+ $CPU$从何处执行指令是由$CS$、$IP$中的内容决定的，程序员可以通过改变$CS$、$IP$中的内容来控制$CPU$执行目标指令。
+ 但是不能直接改变$CS$、$IP$寄存器的值，所以$8086CPU$使用转移指令来进行设置`JMP 段地址:偏移地址`。
+ 其中段地址用来修改$CS$，偏移地址用来修改$IP$。如果仅修改$IP$内容，可以使用`JMP 寄存器地址`，如`JMP AX`类似于`MOV IP,AX`。

## 汇编指令

### 数据传输指令

通用数据传送指令：

+ $MOV$：
  + 格式：`MOV 目的,源`。
  + 功能：把源操作数赋值传送给目的操作数，源操作数不变。
  + 要求：目的和源可以为数据也可以为地址。
  + 禁止操作：向段寄存器写入立即数（必须通过通用寄存器中转）；$CS$作为目标寄存器；目的地址与源地址同时为存储单元或段寄存器。
+ $MOVSX$：
  + 格式：`MOVSX 目的寄存器,源操作数`。
  + 功能：把源操作数先进行符号拓展（为正数则高位补0，负数则高位补1），再赋值传送给目的操作数，源操作数不变。
  + 要求：源操作数字长要小于或等于目标寄存器字长。
+ $MOVZX$：
  + 格式：`MOVZX 目的寄存器,源操作数`。
  + 功能：把源操作数先进行零拓展（高位补0），再赋值传送给目的操作数，源操作数不变。
  + 要求：源操作数字长要小于或等于目标寄存器字长。
+ $LEA$：
  + 格式：`LEA 目的寄存器,源操作数`。
  + 功能：计算内存单元的有效地址（不是其中的操作数）并送到目的寄存器中。
  + 说明：有效地址就是偏移地址，LEA指令等效于OFFSET运算符，计算DS段地址加上偏移地址的地址。
+ $XCHG$：
  + 格式：`XCHG 操作数1,操作数2`。
  + 功能：对两个操作数进行位置互换。
  + 要求：操作数类型需要一致；至少一个为寄存器。
  + 禁止操作：操作数为段寄存器或立即数或内存操作数。
+ $CMPXCHG$：
  + 格式：`CMPXCHG 操作数1,操作数2`。
  + 功能：比较并对两个操作数进行位置互换。
  + 要求：第二个操作数必须为累加器$AL/AX/EAX$。
  + 禁止操作：操作数为段寄存器或立即数或内存操作数。

堆栈传输指令：

堆栈操作指令中的操作数类型必须是字操作数，即16位操作数。

+ $PUSH$：
  + 格式：`PUSH 源地址`。
  + 功能：将字压入堆栈。
  + 说明：一个字进栈，系统自动完成两步操作：$SP\leftarrow SP-2$，$(SP)\leftarrow$操作数。
  + 说明：一个双字进栈，系统自动完成两步操作：$ESP\leftarrow ESP-4$，$(ESP)\leftarrow$操作数。
+ $POP$：
  + 格式：`POP 目的地址`。
  + 功能：将字弹出堆栈。
  + 说明：弹出一个字，系统自动完成两步操作：操作数$\leftarrow(SP)$，$SP\leftarrow SP+2$。
  + 说明：弹出一个双字，系统自动完成两步操作：操作数$\leftarrow(ESP)$，$ESP\leftarrow SP+4$。
+ $PUSHA$：
  + 格式：`PUSHA`。
  + 功能：把通用寄存器$AX,CX,DX,BX,SP,BP,SI,DI$依次全部压入堆栈。
  + 要求：为$16$位字。
+ $POPA$：
  + 格式：`POPA`。
  + 功能：把通用寄存器$DI,SI,BP,SP,BX,DX,CX,AX$依次全部弹出堆栈。
  + 要求：为$16$位字。
+ $PUSHAD$：
  + 格式：`PUSHAD`。
  + 功能：把拓展通用寄存器$AX,CX,DX,BX,SP,BP,SI,DI$依次全部压入堆栈。
  + 要求：为$32$位字。
+ $POPAD$：
  + 格式：`POPAD`。
  + 功能：把拓展通用寄存器$DI,SI,BP,SP,BX,DX,CX,AX$依次全部弹出堆栈。
  + 要求：为$32$位字。

## 环境

### 下载

首先下载[DOSBox](https://www.dosbox.com/download.php?main=1)，下载后点击安装。DOSBox即通过软件让在不同的系统如Windows、Linux、MacOS等运行DOS程序，直接操作硬件。

然后下载[MASM5.0](http://www.winwin7.com/soft/27172.html#xiazai)，将压缩包解压并把对应文件夹放到指定位置。宏汇编程序$MASM$是具有宏加工功能的汇编程序。可以用它定义含参数的程序段，在使用的位置上调用它们，汇编时将进行宏指令展开，把宏定义所预先定义的指令目标代码插在该位置上。

### 挂载

完成后打开$DOSBox$的$DOSBox.exe$程序。

由于这个软件的起始目录是$Z$，并没有我们电脑上的任何本地目录，所以我们需要首先将我们自己的目录给挂载到$DOSBox$系统中。

需要将$MASM$目录挂载在$DOSBox$中，输入`MOUNT 挂载虚拟盘符 MASM5.0安装地址`（命令不区分大小写）。如我的$MASM5.0$安装在$D$盘的$MASM$目录下，要挂载到$D$盘中运行，就要在$DOSBox$控制台中输入`MOUNT D: D://MASM`。如果挂载成功会显示$Drive\,D\,is\,mounted\,as\,local\,directory\,D://MASM\\$

然后查看部署是否成功，`D:`、`dir`，如果显示列表中出现了$DEBUG$就代表MASM挂载成功，可以使用里面的$DEBUG$进行操作。

然后挂载成功后输入`D:`进入挂载地址，输入`DEBUG`就可以开始使用$DEBUG$了（汇编程序执行器）。

此后每次使用$DOSBox$就必须挂载$MASM$，非常麻烦，可以在配置文件中配置挂载命令。

找到$DOSBox\,0.74-3,Options.bat$，双击打开配置文件（注意不要直接修改bat文件），在末尾添加：

```bat
MOUNT D: D://MASM
D:
```

就相当于默认挂载，并转到虚拟盘符地址，点击$DOSBox.exe$可以直接使用相关工具了。

+ $EDIT$编辑器。
+ $MASM$编译器。
+ $LINK$连接器。
+ $DEBUG$调试器。

### DEBUG

+ $R$命令查看、改变$CPU$寄存器的内容。
  + `R`查看所有寄存器内容。
  + `R 寄存器名`查看指定寄存器内容。然后下一行弹出一个:，后面可以添加值，从而能修改指定寄存器内容。
+ $D$命令查看内存中的内容。其中左边为十六进制值，右边为对应ASCII值。
  + `D 段地址:偏移地址`：查看指定段地址与偏移地址所指向的内存后128个内存单元中内容。
  + `D 段地址:偏移地址 数量`：查看指定段地址与偏移地址所指向的内存后指定数量的内存单元中的内容。
+ $E$命令改写内存中的内容。
  + `E 段地址:偏移地址 数据列表`：从指定段地址与偏移地址所指向的内存开始修改内容为数据列表。数据列表中间以空格分隔。
  + `E 段地址:偏移地址`：不表明数据列表，则控制台会主动给你提供从该地址开始的原数据，可以在后面输入新数据，输入空格后会继续弹出下个地址的原数据，回车结束输入执行修改。
  + `E 段地址:偏移地址 "字符串"`：从指定段地址与偏移地址所指向的内存开始保存字符串值，对应的ASCII码自动转换并保存到内容中。
+ $U$命令将内存中的机器指令翻译成汇编指令。`U 段地址:偏移地址`。结果分别为地址、机器指令、汇编指令、说明。
+ $A$命令以汇编指令的格式在内存中写入一条机器指令。`A 段地址:偏移地址`。从该地址开始输入汇编指令保存到对应内存。
+ $T$命令执行一条机器指令。`T 段地址:偏移地址`。从该地址执行保存的汇编指令。

## 操作

### 数据操作

如要读取$10000H$单元的内容可以使用下面的汇编程序：

```asm
MOV BX,1000H
MOV DS,BX
MOV AL,[0]
```

首先我们要读取$10000H$单元的内容，则必须把这个内容移动到段寄存器中然后读取。但是段寄存器不能直接被赋值，必须通过通用寄存器。所以第一步就是将$1000H$移动到基址通用寄存器$BX$中，因为是基址所以将$10000H$右移四位变为$1000H$为基址（$10000H$表示欸$1000:0$）。然后将$1000H$基址从基址寄存器中移动到数据段寄存器$DS$中，此时可以通过$DS$读取数据了。其中$[X]$表示内存单元，$X$表示相对于段寄存器地址的偏移量，所以$[0]$的计算后的基址就是$1000H\times16+0=10000H$，然后通过$MOV$命令移动到$AL$累加器寄存器中。移动到$AL$而不是$AX$是因为一个内存单位为一个字节，为八比特，而$8086CPU$一个字为两个字节，所以只用累加器的低位就可以了，即$AL$即可。

所以针对数据传输必然是：数据→通用寄存器→段寄存器。

### 字操作

由于$8086CPU$数据线为$16$根，所以其字大小为十六位即两个字节。之前数据传输时只是传输一个字节，所以通用寄存器使用$AL$而不是$AX$，而这里要传输字，所以使用一整个通用寄存器。

假如内存情况为：

地址|数据
:--:|:--:
10000H|23
10001H|11
10002H|22
10003H|66

执行下面的汇编程序，请问执行后寄存器会变成什么样：

```asm
MOV AX,1000H
MOV DS,AX
MOV AX,[0]
MOV BX,[2]
MOV CX,[1]
ADD BX,[1]
ADD CX,[2]
```

计算机默认为小端模式，所以高位在高地址，低位在地地址，所以比如要读取10000H指向的字，不能只读一个$23$，因为只有八位而$8086CPU$字长为十六位，所以还要向高位读一个字节，即为$1123$，高地址的在高位，所以$11$在$23$前面。

指令|寄存器内容变化|说明
:--:|:------------:|:--:
MOV AX,1000H|AX=1000H|将基址1000H送到累加器中
MOV DS,AX|DS=1000H|将基址1000H赋值给数据段寄存器
MOV AX,[0]|AX=1123H|将偏移量为0的数据读取到AX中，多读一个[1]位置
MOV BX,[2]|BX=6622H|将偏移量为2的数据读取到BX中，多读一个[3]位置
MOV CX,[1]|CX=2211H|将偏移量为1的数据读取到CX中，多读取一个[2]位置
ADD BX,[1]|BX=8833H|即将[1]位置的数据与BX数据相加，[1]位置的数据不能是半字，所以必须为十六位，即[2][1]=2211H，相加得到6622H+2211H=8833H
ADD CX,[2]|CX=8833H|同理将[2]位置的数据扩展为[3][2]=6622H，相加2211H+6622H=8833H

### 段操作

对于$8086CPU$可以将数据根据需要让一组内存单元定义为一个段，以后操作根据段来进行。

+ 组的最大长度为$64K$，因为偏移量为$2^{16}=64K$。
+ 地址连续。
+ 起始地址为$16$倍数。（即地址的最后一个数字为$0$，如$123B0H$）

将$123B0H\sim123BAH$的内存单元定义为数据段，累加这个数据段中的前三个单元中的数据：

```asm
MOV AX,123BH
MOV DS,AX
MOV AL,0
ADD AL,[0]
ADD AL,[1]
ADD AL,[2]
```

### 栈操作

$8086CPU$的栈操作都是以字，即十六比特为单位进行。高地址单元放高八位，低地址单元放低八位。

其中$SS$指向栈顶的段地址，$SP$指向栈顶的偏移地址，所以$SS:SP$指向栈顶元素。

如将$10000H\sim1000FH$作为栈，执行下面指令：

```asm
MOV AX,0123H
PUSH AX
MOV BX,2266H
PUSH BX
MOV CX,1122H
PUSH CX
POP AX
POP BX
POP CX
```

$10000H\sim1000FH$作为栈，则栈底为高位$1000FH$，栈底为低位$10000H$，由于需要压入栈一共三个数据六个字节，所以只有$1000AH\sim1000FH$被使用到。

指令|MOV AX,0123H|PUSH AX|MOV BX,2266H|PUSH BX|MOV CX,1122H|PUSH CX|POP AX|POP BX|POP CX
:--:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:
1000AH||||||22
1000BH||||||11
1000CH||||66|66|66|66
1000DH||||22|22|22|22
1000EH||23|23|23|23|23|23|23|
1000FH||01|01|01|01|01|01|01|

$SP$在入栈和出站是会加减二。

又将$10000H\sim1000FH$这段空间当作栈，初始状态是空的。设置$AX=001AH$，$BX=001BH$。将$AX$、$BX$中的数据入栈。然后将$AX$、$BX$清零。最后从栈中恢复$AX$、$BX$原来的内容。

由于$10000H\sim1000FH$为栈，栈底在高位，所以栈为空时栈顶应该初始化赋值为最大值$1000FH$的下一位$SP=0010H$。

```asm
MOV AX,1000H
MOV SS,AX
MOV SP,0010H
MOV AX,001AH
MOV BX,001BH
PUSH AX
PUSH BX
SUB AX,AX
SUB BX,BX
POP BX
POP AX
```

由于$8086CPU$栈空间没有保护，所以可能存在栈顶越界。

### 栈段操作

可以将字段内存作为栈段，这是用户配置的，CPU并不会把它当作栈，所以我们要让$SS:SP$指向自定义栈段，从而能让栈操作指令能访问我们自己定义的栈段。

由于栈段操作时只需要移动$SP$，所以$SP$的大小决定栈段的最大长度，所以最大长度为$2^{16}=64K$。

如果将$10000H\sim1FFFFH$这段空间作为栈段，初始空间为空，则$SS=1000H$，求$SP$。

由于空间高位为$1FFFFH$作为栈底，所以如果为空，则栈顶应该在$1FFFFH$的更高一位$1FFFFH+1=20000H$。但是给定了栈段空间$10000H\sim1FFFFH$，所以不能取到$SS=2000H$，否则越界。

所以向栈中填充一个元素，所以任意时刻，$SS:SP$指向栈顶，当栈中只有一个元素的时候，$SS=1000H$，$SP=FFFEH$。

如果栈为空，就相当于栈中唯一的元素出栈，出栈后，$SP=SP+2$。$SP$原来为$FFFEH$，SP=FFFEH+2=0$，所以，当栈为空的时候，$SS=1000H$，$SP=0$。

所以$CS:IP$指向哪里，$CPU$就将该段内存当作代码，$SS:IP$指向哪里，$CPU$就将该段内存当作栈。

## 程序定义

可以将汇编文件使用编译器编译为可执行文件。编写->编译->连接->执行。

如下面就是一个简单的汇编源程序：

```asm
ASSUME CS:codeseg
codeseg SEGMENT
START: MOV AX,0123H
       MOV BX,0456H
       ADD AX,BX
       ADD AX,AX
       MOV AX,4C00H
       INT 21H
codeseg ENDS
END START
```

### 定义段

+ $SEGMENT$和$ENDS$是一对成对使用的伪指令，这是在写可被编译器编译的汇编程序时，必须要用到的一对伪指令。
+ $SEGMENT$和$ENDS$的功能是定义一个段，$SEGMENT$说明一个段开始，$ENDS$说明一个段结束。（$END$可以视为$END\,SEGMENT$的简写）
+ 一个段必须有一个名称来标识，使用格式为：`段名 SEGMENT`、`段名 ENDS`。
+ 一个汇编程序是由多个段组成的，这些段被用来存放代码、数据或当作栈空间来使用。
+ 一个有意义的汇编程序中至少要有一个段，这个段用来存放代码。
+ 段名称最终会被编译、连接程序处理为一个段的段地址。

### 程序框架

$END$是一个汇编程序的结束标记，编译器在编译汇编程序的过程中，如果碰到了伪指令$END$，就结束对源程序的编译。

如果程序写完了，要在结尾处加上伪指令$END$。否则，编译器在编译程序时，无法知道程序在何处结束。

$END$除了通知编译器程序结束外，还可以通知编译器程序的入口在什么地方。由$START$后面加一个冒号，此后就是汇编指令，即基本的代码段。$END START$即指明入口在$START$位置。

### 假设

$ASSUME$表示为假设的伪代码，假设某一段寄存器与程序中的某个用$SEGMENT$和$ENDS$定义的段相关联，这样就不用再使用$MOV$指令将段通过通用寄存器移动到段寄存器，从而简化了代码。

如`ASSUME CS:codeseg`将$codeseg$代码段放到$CS$代码段寄存器中。

### 程序返回

我们的程序最先以汇编指令的形式存在源程序中，经编译、连接后转变为机器码，存储在可执行文件中。

而$DOS$是一个单任务操作系统，所以只能有一个程序运行，所以一个程序结束后，将$CPU$的控制枚交还给使它得以运行的程序，这个过程为程序返回。

返回应该在程序的末尾添加返回的程序段。

如代码的`MOV AX,4C00H`和`INT 21H`。这两句话的意思是$DOS$系统功能调$INT 21H$功能中的一种，表示带返回码结束用户程序。$INT 21H$指令中，寄存器$AX$分为$AH$和$AL$两个部分，$AH$中存入指令码$4C$表示带返回码结束，$AL$为程序返回码。用这个指令返回是不需任何条件，还可顺便关闭打开后忘记关闭的文件，并返回寄存器$AL$的值。

AH|功能|调用参数
:-:|:-:|:------:
00|程序终止（同INT 20H）|CS=程序段前缀。
01|键盘输入并回显|AL=输入字符。
02|显示输出|DL=输出字符。
03|异步通迅输入|AL=输入数据。
04|异步通迅输出|DL=输出数据

### 错误

+ 语法错误：程序在编译时被编译器发现的错误，容易被发现。
+ 逻辑错误：程序在编译时不能表现出来，在运行时发生的错误，不容易被发现。

### 编辑

打开DOSBox，挂载后直接运行`EDIT`命令，打开编辑器，编写：

```asm
ASSUME CS:codeseg
codeseg SEGMENT
MOV AX,0123H
MOV BX,0456H
ADD AX,BX
ADD AX,AX
MOV AX,4C00H
INT 21H
codeseg ENDS
END
```

编写完成偶点击File，选择Save然后输入文件名进行保存。然后点击File的Exit退出编辑。

### 编译

然后运行`MASM`，然后需要输入源程序名。如果源程序文件不是以$asm$为扩展名的话，就要输入它的全名，比如$test.txt$。在输入源程序文件名的时候一定要指明它所在的路径，如`D:\MASM\test.asm`（但是注意此时你已经挂载了$MASM$，所以必须使用`dir`查看当前文件路径，文件路径直接为$test.asm$）。如果文件就在当前路径下，只输入文件名就可以。

然后出现$Object\,filename$，默认是后面括号的内容，如果要修改就输入新名字。

然后是$Source\,listing$，表示列表文件，是编译器将源程序编译为目标文件的过程中产生的中间结果。

最后是$Cross-reference$，表示交叉引用文件，这个文件同列表文件一样，是编译器将源程序编译为目标文件过程中产生的中间结果。

也可以直接加上文件地址输入，如`masm test.asm`。

### 连接

将生成的$TEST.OBJ$文件连接为可执行的$TEST.exe$文件。

控制台中输入$LINK$进行连接。如果目标文件不是以$obj$为扩展名的话，就要输入它的全名，同理要指明文件路径。也可以直接输入`LINK TEST.OBJ`。

$Run\,file$即表示要生成的可执行文件的名字。$List\,file$映像文件是连接程序将目标文件连接为可执行文件过程中产生的中间结果。$Libraries$即库文件，包含了一些可以调用的子程序，如果我们的程序中调用了某一个库文件中的子程序，就需要在连接的时候，将这个库文件和我们的自标文件连接到一起，生成可执行文件。

此时会提示一个警告：$waning L4021: no stack segment$没有栈段，可以不用理会。得到一个TEST.EXE。

### 运行

由于是可执行文件，所以可以直接在控制台中输入文件名进行执行。

操作系统是由多个功能模块组成的庞大、复杂的软件系统。任何通用的操作系统﹐都要提供一个称为$shell$外壳）的程序，用户（操作人员）使用这个程序来操作计算机系统工作。

$DOS$中有一个程序$command.com$，这个程序在$DOS$中称为命令解释器，也就是$DOS$系统的$shell$。

所以就是这个运行的$command$将汇编文件的程序加载进入内存，并设置$CPU$的$CS:IP$指向程序的第一条指令，即程序入口，从而使得程序得以运行。程序运行结束后返回$command$中，$CPU$继续运行$command$。

可以使用`DEBUG 程序名`的方式对程序进行监控。使用`R`指令对内存进行查看。可以看到$CX=000F$，即汇编程序一共十五个字节。

## 高级使用

### [BX]

类似于$[0]$表示内存单元的偏移地址，所以$[BX]$也表示一个内存单元，但是偏移地址在$BX$中。

### (X)

$(X)$用于表示一个地址或寄存器中的内容。如$(AX)=0010H$，即寄存器$AX$中的内容为$0010H$。如$MOV AX,[2]$等价于$(AX)=((DS)*16+2)$，$ADD AX,2$等价于$(AX)=(AX)+2$。

其中定义$idata$表示常量值的占位，如$MOV AX,[idata]$代表$MOV AX,[$常量$]$。

### LOOP

为循环指令，格式：`LOOP 标号`。$CPU$执行该指令时会进行两步操作：

1. $(CX)=(CX)-1$。
2. 判断$CX$中的值，里面保存循环次数，不为零则转至标号处执行程序，如果为零打破循环向下执行。

计算$2^{12}$：

```asm
ASSUME CS:CODE
CODE SEGMENT
MOV AX,2
MOV CX,11
S: ADD AX,AX
LOOP S
MOV AX,4C00H
INT 21H
CODE ENDS
END
```

这里的标号$S$就指向的是`ADD AX,AX`指令。循环执行的语句要在标号和$LOOP$指令之间。

在实际编程中，经常会遇到，用同一种方法处理地址连续的内存单元中的数据的问题。
我们需要用循环来解决这类问题，同时我们必须能够在每次循环的时候按照同一种方法来改变要访问的内存单元的地址。这时，我们就不能用常量来给出内存单元的地址（比如$[0]$、$[1]$是常量），而应用变量。`MOV AL,[BX]`中的$BX$就可以看作一个代表内存单元地址的变量，我们可以不写新的指令，仅通过改变bx中的数值，改变指令访问的内存单元。

默认地址的段前缀都是放在$DS$中，可以设置不同的段寄存器从而设置不同的段前缀来简化。

## 多段程序

一个程序不可以只有一个代码段，所以必然存在多个段进行不同的数据操作。

由于$END$和$START$是一组关键字，$END$可以指明程序。

```asm
ASSUME 段寄存器名:段名
段名 SEGMENT
……
定义数据
……
START:
……
数据代码
……
段名 ENDS
END START
```
