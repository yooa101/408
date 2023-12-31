# 输入输出系统习题

## 输入输出基本概念

**例题** 在微型机系统中，$I/O$设备通过()与主板的系统总线相连接。

$A.DMA$控制器

$B.$设备控制器

$C.$中断控制器

$D.I/O$端口

解：$B$。$I/O$设备不可能直接与主板总线相连，它总是通过设备控制器来相连的。

## 外部设备

### 输入设备

**例题** 下列关于$I/O$设备的说法中，正确的是()。

Ⅰ.键盘、鼠标、显示器、打印机属于人机交互设备

Ⅱ.在微型计算机中，VGA代表的是视频传输标准

Ⅲ.打印机从打字原理的角度来区分，可分为点阵式打印机和活字式打印机

Ⅳ．鼠标适合于用中断方式来实现输入操作

$A.$Ⅱ、Ⅲ、Ⅳ

$B.$Ⅰ、Ⅱ、Ⅳ

$C.$Ⅰ、Ⅱ、Ⅲ

$D.$Ⅰ、Ⅱ、Ⅲ、Ⅳ

解：$B$。键盘、鼠标、显示器、打印机等都属于机器与人交互的媒介（用户使用键盘、鼠标来控制计算机，计算机使用显示器和打印机来向用户传递信息），因此Ⅰ正确。$VGA$是一个用于显示的视频传输标准，因此Ⅱ正确。打印机从打字原理的角度来分，可分为击打式和非击打式两种，按照能否打出汉字来分，可分为点阵式打印机和活字式打印机，因此Ⅲ错误。键盘、鼠标等输入设备一般都采用中断方式来实现，原因在于$CPU$需要及时响应这些操作，否则容易造成输入的丢失，因此Ⅳ正确。

**例题** $CRT$的分辨率为$1024\times1024$像素，像素的颜色数为$256$，则刷新存储器的每单元字长为()，总容量为()。

$A.8B$，$256MB$

$B.8bit$，$1MB$

$C.8bit$，$256KB$

$D.8B$，$32MB$

解：$B$。刷新存储器中存储单元的字长取决于显示的颜色数，颜色数为$m$，字长为$n$，二者的关系为$2^n=m$。本题中的颜色数为$256=2^8$，因此刷新存储器单元字长为$8$位。刷新存储器的容量是每个像素点的位数和像素点个数的乘积，因此刷新存储器的容量为$1024\times1024\times8bit=1MB$。

### 外存设备

**例题** 若磁盘转速为$7200$转/分，平均寻道时间为$8ms$，每个磁道包含$1000$个扇区，则访问一个扇区的平均存取时间大约是()。

$A.8.1ms$

$B.12.2ms$

$C.16.3ms$

$D.20.5ms$

解：$B$。存取时间=寻道时间＋延迟时间＋传输时间。存取一个扇区的平均延迟时间为旋转半周的时间，即$(60\div7200)\div2=4.17ms$，传输时间为$(60\div7200)\div1000=0.01ms$，因此访问一个扇区的平均存取时间为$4.17+0.01+8=12.18ms$，保留一位小数则为$12.2ms$。

## I/O接口

### I/O总线

**例题** 下列选项中，在$I/O$总线的数据线上传输的信息包括()。

Ⅰ.$I/O$接口中的命令字

Ⅱ.$I/O$接口中的状态字

Ⅲ．中断类型号

$A.$仅Ⅰ、Ⅱ

$B.$仅Ⅰ、Ⅲ

$C.$仅Ⅱ、Ⅲ

$D.$Ⅰ、Ⅱ、Ⅲ

解：$D$。$I/O$总线分为三类：数据线、控制线和地址线。数据缓冲寄存器和命令/状态寄存器的内容都是通过数据线来传送的。地址线用以传送与$CPU$交换数据的端口地址。而控制线用以给$I/O$端口发送读/写信号，只是用来对端口进行读/写控制的。因此Ⅰ、Ⅱ和Ⅲ均正确。

### I/O地址

<!-- #### 统一编制方式 -->

**例题** $I/O$的编址方式采用统一编址方式时，进行输入/输出的操作的指令是()。

$A.$控制指令

$B.$访存指令

$C.$输入/输出指令

$D.$都不对

解：$B$。统一编址时，直接使用指令系统中的访存指令来完成输入/输出操作。独立编址时，则需要使用专门的输入/输出指令来完成输入/输出操作。

### I/O端口

**例题**$I/O$指令实现的数据传送通常发生在()。

$A.I/O$设备和$I/O$端口之间

$B.$通用寄存器和$I/O$设备之间

$C.I/O$端口和$I/O$端口之间

$D.$通用寄存器和$I/O$端口之间

解：$D$。$I/O$端口是指$I/O$接口中用于缓冲信息的寄存器，由于主机和$I/O$设备的工作方式和工作速度有很大差异，$I/O$端口应运而生。在执行一条指令时，$CPU$使用地址总线选择所请求的$I/O$端口，使用数据总线在$CPU$寄存器和端口之间传输数据。

## I/O方式

### 程序查询方式

**例题** 在程序查询方式的输入/输出系统中，假设不考虑处理时间，每一个查询操作需要$100$个时钟周期，$CPU$的时钟频率为$50MHz$。现有鼠标和硬盘两个设备，而且$CPU$必须每秒对鼠标进行$30$次查询，硬盘以$32$位字长为单位传输数据，即每$32$位被$CPU$查询一次，传输率为$2\times2^{20}B/s$。求$CPU$对这两个设备查询所花费的时间比率，由此可得出什么结论？

解：

从时间来看：

因为$CPU$时钟频率为$50MHz$，所以一个时钟周期就是其倒数为$20ns$。

而一个查询操作需要$100$个时钟周期，所以一个查询操作需要$100\times20=2000ns$。

对于鼠标：

每秒需要对鼠标进行$30$次查询，所以一秒内查询鼠标的时间为$30\times2000=60000ns$。

所以查询鼠标所花费的时间比率为$60000ns\div1s=0.006\%$。

从而对鼠标的查询基本上不影响$CPU$性能。

对于硬盘：

因为每$32$位需要查询一次，每秒传送$2\times2^{20}B$，所以每秒需要查询$2\times2^{20}\div4=2^{19}$次。

查询硬盘时间为$2^{19}\times2000=512\times1024\times2000ns\approx1.05\times10^9ns$。

查询硬盘花费时间比率为$105\%$。也就是说即使$CPU$将全部时间都用于对硬盘的查询也不能满足磁盘传输要求。

从频率来看：

$CPU$的时钟频率为$50MHz$，即每秒需要$50\times10^6$个时钟周期。

对于鼠标：

每秒查询鼠标占用时钟周期数为$30×100=3000$，所以查询鼠标花费的时间比率为$3000\div(50\times10^6)=0006\%$。

从而对鼠标的查询基本上不影响$CPU$性能。

对于硬盘：

因为每$32$位需要查询一次，所以每秒需要查询$(2\times2^{20})\div4=2^{19}$次。

所以每秒查询硬盘占用时钟周期数位$2^{19}\times100\approx5.24\times10^7$。

产线硬盘花费时间比例为$(5.24\times10^7)\div(50\times10^6)\approx105\%$。

也就是说即使$CPU$将全部时间都用于对硬盘的查询也不能满足磁盘传输要求。

### 程序中断方式

#### 中断类型

**例题** 当有中断源发出请求时，$CPU$可执行相应的中断服务程序，可以提出中断的有()。

Ⅰ.外部事件

Ⅱ.$Cache$

Ⅲ.浮点数运算下溢

Ⅳ．浮点数运算上溢

$A.$Ⅰ、Ⅱ

$B.$Ⅱ、Ⅱ、Ⅳ

$C.$Ⅰ、Ⅳ

$D.$Ⅰ、Ⅲ、Ⅳ

解：$C$。外部事件是可以提出中断请求的，如通过敲击键盘来终止现在正在运行的程序就可视为一个中断，因此Ⅰ正确。$Cache$属于存储设备，不能提出中断请求，因此Ⅱ错误。浮点数运算下溢，可以当作机器零处理，不需要中断来处理，而浮点数运算上溢时，必须中断来做相应的处理，因此Ⅲ错误、Ⅳ正确。

**例题** 主存故障引起的中断是()。

$A.I/O$中断

$B.$程序性中断

$C.$机器校验中断

$D.$外中断

解：$C$。主存故障引起的中断是机器校验中断，属于内中断，而外中断一般指主存和$CPU$外的中断，如外设引起的中断等。

**例题** 在配有通道的计算机系统中，用户程序需要输入/输出时，引起的中断是()。

$A.$访管中断

$B.I/O$中断

$C.$程序性中断

$D.$外中断

解：$A$。用户程序需要输入/输出时，需要调用操作系统提供的接口（请求操作系统服务)，此时会引起访管中断，系统由用户态转为核心态。$I/O$中断不存在这种中断。

**例题** 下列关于“自陷”的叙述中，错误的是()。

$A.$自陷是通过陷阱指令预先设定的一类外部中断事件

$B.$自陷可用于实现程序调试时的断点设置和单步跟踪
$C.$自陷发生后$CPU$将转去执行操作系统内核相应程序

$D.$自陷处理完成后返回到陷阱指令的下一条指令执行

解：$A$。本题更是操作系统的考点。自陷是一种内部异常，$A$错误。在$80x86$中，用于程序调试的“断点设置”功能是通过“自陷”方式实现的，$B$正确。执行到自陷指令时，无条件或有条件地自动调出操作系统内核程序进行执行，$C$正确。$CPU$执行“陷阱指令”后，会自动地根据不同“陷阱”类型进行相应的处理，然后返回到“陷阱指令”的下一条指令执行，$D$正确。

#### 端口数据类型

**例题** 在采用中断$I/O$方式控制打印输出的情况下，$CPU$和打印控制接口中的$I/O$端口之间交换的信息不可能是()。

$A.$打印字符

$B.$主存地址

$C.$设备状态

$D.$控制命令

解：$B$。在程序中断$I/O$方式中，$CPU$和打印机直接交换，打印字符直接传输到打印机的 $I/O$端口，不会涉及主存地址。而$CPU$和打印机通过$I/O$端口中的状态口和控制口来实现交互。

#### 中断优先级

**例题** 中断响应由高到低的优先次序宜用()。

$A.$访管→程序性→机器故障

$B.$访管→程序性→重新启动

$C.$外部→访管→程序性

$D.$程序性→$I/O$→访管

解：$B$。硬件故障中断属于最高级，其次是软件中断，所以机器故障最高。重新启动应该等待其他任务全部完成，所以优先级最低。访管指令是软件指令中最紧急的，所以软件中断中优先级最高。程序性中断要高于重新启动。所以次序为机器故障→访管→程序性→重新启动。

#### 中断过程

**例题** 下列关于外部$I/O$中断的叙述中，正确的是()。

$A.$中断控制器按所接收中断请求的先后次序进行中断优先级排队

$B.CPU$响应中断时，通过执行中断隐指令完成通用寄存器的保护

$C.CPU$只有在处于中断允许状态时，才能响应外部设备的中断请求

$D.$有中断请求时，$CPU$立即暂停当前指令执行，转去执行中断服务程序

解：$C$。中断优先级由屏蔽字而非请求的先后次序决定，$A$错误。中断隐指令完成的工作有：①关中断；②保存断点；③引出中断服务程序，通用寄存器的保护由中断服务程序完成，$B$错误。中断允许状态即开中断后，才能响应中断请求，$C$正确。有中断请求时，如果是关中断的状态，或新中断请求的优先级较低，则不能响应新的中断请求，$D$错误。

#### 多重中断技术

**例题** 设某机有$4$个中断源$A$、$B$、$C$、$D$，其硬件排队优先次序为$A>B>C>D$，现要求将中断处理次序改为$D>A>C>B$。

1）写出每个中断源对应的屏蔽字。

解：在写当前中断源屏蔽字时，如果当前中断源的优先级大于屏蔽字，则是$1$，否则是$0$：

中断源|A|B|C|D
:-:|:-:|:-:|:-:|:--:
A|1|1|1|0
B|0|1|0|0
C|0|1|1|0
D|1|1|1|1

如若中断源为$A$，因为$A$必须屏蔽它自己，所以$A$的屏蔽字为$1$，而$A$的优先级大于$B$和$C$，所以$BC$屏蔽字也为$1$，$D$的优先级大于$A$，所以$D$的屏蔽字为$0$。

2）若中断源进入程序如表格。设每个中断源的中断服务程序时间均为$20\mu s$。写出$CPU$执行程序的流程。

中断源|进入程序时间点
:----:|:------------:
B|5
D|10
A|35
C|60

解：首先$B$在时刻$5$进入，开始执行，执行到时刻$10$时$D$进入，而$D$的优先级高于$B$，所以时刻$10$开始执行$D$，时刻$30$此时$D$结束，开始重新执行$B$，直到时刻$35A$进入，此时$B$已经执行了$10$个时间单位，$A$优先级高于$B$，所以$A$开始执行，时刻$55$时$A$完成，$B$开始执行，直到时刻$60$时$C$进入，而$C$优先级大于$B$，所以$C$开始执行，$B$此时已经执行了$15$个单位，$80$时$C$结束，$B$开始，时刻$85$时$B$完成，整个结束。

#### 程序中断方式实现

**例题** 假定$CPU$主频为$50MHz$，$CPI$为$4$。设备$D$采用异步串行通信方式向主机传送$7$位$ASCII$字符，通信规程中有$1$位奇校验位和$1$位停止位，从$D$接收启动命令到字符送入$I/O$端口需要$0.5ms$。请回答下列问题，要求说明理由。

1）每传送一个字符，在异步串行通信线上共需传输多少位？在设备$D$持续工作过程中，每秒钟最多可向$I/O$端口送入多少个字符?

解：需要注意的是，在总线那一章说过，数据传输时会隐藏一位起始位，所以一共有$1+7+1+1=10$位数据。

每$0.5ms$送入一个字符，所以每秒送入$1s\div0.5ms=2000$个字符。

2）设备$D$采用中断方式输入输出，$I/O$端口每收到一个字符申请一次中断，中断响应需$10$个时钟周期，中断服务程序共有$20$条指令，其中第$15$条指令启动$D$工作。若$CPU$需从$D$读取$1000$个字符，则完成这一任务所需时间大约是多少个时钟周期？$CPU$用于完成这一任务的时间大约是多少个时钟周期？在中断响应阶段$CPU$进行了哪些操作？

解：因为$I/O$端口每收到一个字符申请一次中断，所以外设$D$每一次工作$0.5ms$。然后中断响应$10$个时钟周期，启动$D$需要$15$条指令，$5$条恢复现场指令。

因为主频$50MHz$，所以时钟周期为$20ns$，工作时间为$0.5ms\div20ns=25000$个时钟周期。

然后中断响应需要$10$个时钟周期，然后启动$D$需要$15$条指令，根据$CPI$是$4$，所以每一条指令需要$4$个时钟周期，从而启动$D$需要$15\times4=60$个时钟周期。由于最后五条指令是用来恢复现场的，所以不算在完成读取字符任务所需的时间内。

从而传输一个字符需要时钟周期$25000+10+60=25070$个。所以传输$1000$个字符需要$25070000$个时钟周期。

而$CPU$完成这个任务的时间就需要加上恢复现场的$5$个时钟周期，且是关于$CPU$所需要的时间，所以同时要去掉外设工作的$0.5ms=25000$个时钟周期。

所以对于一个字符，一共就是请求的$10$个时钟周期，加上$20$条指令所需要$20\times4=80$个时钟周期，从而$CPU$完成任务的时间为$1000\times(10+80)=90000$个时钟周期。

$CPU$进行的操作为：关中断、保存断点、引出中断服务程序。

### DMA方式

**例题** 以下有关$DMA$方式的叙述中，错误的是()。

$A.$在$DMA$方式下，$DMA$控制器向$CPU$请求的是总线使用权

$B.DMA$方式可用于键盘和鼠标的数据输入

$C.$在数据传输阶段，不需要$CPU$介入，完全由$DMA$控制器控制

$D.DMA$方式要用到中断处理

解：$B$。$DMA$方式只能用于数据传输，它不具有对异常事件的处理能力，不能中断现行程序，而键盘和鼠标均要求$CPU$立即响应，因此无法采用$DMA$方式。

**例题** 在$DMA$方式下，数据从内存传送到外设经过的路径是()。

$A.$内存→数据总线→数据通路→外设

$B.$内存→数据总线→$DMAC$→外设

$C.$内存→数据通路→数据总线→外设

$D.$内存→$CPU$→外设

解：$B$。$DMA$方式不经过$CPU$，输出从内存经过数据总线，传送到$DMA$控制器的$DMAC$中，再传送给外设。类似这样的传输路径称为数据通路。

**例题** 某计算机的$CPU$主频为$500MHz$，$CPI$为$5$（即执行每条指令平均需$5$个时钟周期)。假定某外设的数据传输率为$0.5MB/s$，采用中断方式与主机进行数据传送，以$32$位为传输单位，对应的中断服务程序包含$18$条指令，中断服务的其他开销相当于$2$条指令的执行时间。请回答下列问题，要求给出计算过程。

1）在中断方式下，$CPU$用于该外设$I/O$的时间占整个$CPU$时间的百分比是多少？

2）当该外设的数据传输率达到$5MB/s$时，改用$DMA$方式传送数据。假定每次$DMA$传送块大小为$5000B$，且$DMA$预处理和后处理的总开销为$500$个时钟周期，则$CPU$用于该外设$I/O$的时间占整个$CPU$时间的百分比是多少？（假设$DMA$与$CPU$之间没有访存冲突）

解：

1）$CPU$用于该外设$I/O$的时间包括$CPU$响应中断时间与$CPU$处理中断服务程序时间。

所以每传送一次数据，一共需要$18+2=20$条指令，占用$CPU$时间为$20\times5=100$个时钟周期。

外设准备一次$32$位数据的时间为$32bit\div0.5MB/s=8\mu s$。

从而外设每一秒可以准备$1s\div8\mu s=125000$个。从而一秒就需要$125000$次中断。

则$1$秒内$CPU$用于处理中断的时钟周期个数为$125000\times100=12.5M$个。

又主频为$500MHz$，所以$CPU$用于该外设$I/O$的时间占整个$CPU$时间的百分比$=12.5M\div500M=2.5\%$。

2）当外设数据传输率提高到$5MB/s$时改用$DMA$方式传送，每次$DMA$传送一个数据块，大小为$5000B$，则$1s$内需产生的$DMA$次数为$5MB\div5000B=1000$次。

$CPU$用于$DMA$处理的总开销为$1000\times500=0.5M$个时钟周期。

$CPU$用于外设$I/O$的时间占整个$CPU$时间的百分比为$0.5M\div500M=0.1\%$。

**例题** 假定某计算机的$CPU$主频为$80MHz$，$CPI$为$4$，平均每条指令访存$1.5$次，主存与$Cache$之间交换的块大小为$16B$，$Cache$的命中率为$99\%$，存储器总线宽带为$32$位。回答下列问题。

1）该计算机的$MIPS$数是多少？平均每秒$Cache$缺失的次数是多少？在不考虑$DMA$传送的情况下，主存带宽至少达到多少才能满足$CPU$的访存要求？

2）假定在$Cache$缺失的情况下访问主存时，存在$0.0005\%$的缺页率，则$CPU$平均每秒产生多少次缺页异常？若页面大小为$4KB$，每次缺页都需要访问磁盘，访问磁盘时$DMA$传送采用周期挪用方式，磁盘$I/O$接口的数据缓冲寄存器为$32$位，则磁盘$I/O$接口平均每秒发出的$DMA$请求次数至少是多少？

3）$CPU$和$DMA$控制器同时要求使用存储器总线时，哪个优先级更高？为什么？

4）为了提高性能，主存采用$4$体低位交叉存储模式，工作时每$1/4$个存储周期启动一个体。若每个体的存储周期为$50ns$，则该主存能提供的最大带宽是多少？

解：

1）平均每秒$CPU$执行的指令数为$80M/4=20M$，因此 $MIPS$数为$20$。平均每条指令访存$1.5$次，因此平均每秒$Cache$缺失的次数$=20M\times1.5\times(1-99\%)=300K$。当$Cache$缺失时，要补上缺页的数据量，且$CPU$访问主存，主存与$Cache$之间以块为传送单位，所以要用块的大小来计算，此时主存带宽为$16B\times300k/s=4.8MB/s$。

2）题中假定在$Cache$缺失的情况下访问主存，平均每秒产生缺页中断$300000\times0.0005\%=1.5$次。因为存储器总线宽度为$32$位，所以每传送$32$位数据，磁盘控制器发出一次$DMA$请求，因此平均每秒磁盘$DMA$请求的次数至少为$1.5\times4KB\div4B=1.5K=1536$。

3）$CPU$和$DMA$控制器同时要求使用存储器总线时，$DMA$请求的优先级更高。因为$DMA$请求得不到及时响应，I/O传输数据可能会丢失。

4）最大带宽是四个体在一个存储周期内全部循环运行，四个体的总带宽就是$4\times4B$，$4$体交叉存储模式能提供的最大带宽为$4\times4B\div50ns=320MB/s$。

### 程序中断与DMA

**例题** 下列说法中，错误的是()。

Ⅰ.程序中断过程是由硬件和中断服务程序共同完成的

Ⅱ.在每条指令的执行过程中，每个总线周期要检查一次有无中断请求

Ⅲ.检测有无$DMA$请求，一般安排在一条指令执行过程的末尾

Ⅳ.中断服务程序的最后指令是无条件转移指令

$A.$Ⅲ、Ⅳ

$B.$Ⅱ、Ⅲ、Ⅳ

$C.$Ⅱ、Ⅳ

$D.$Ⅰ、Ⅱ、Ⅲ、Ⅳ

解：$B$。程序中断过程是由硬件执行的中断隐指令和中断服务程序共同完成的，因此Ⅰ正确。每条指令周期结束后，$CPU$会统一扫描各个中断源，然后进行判优来决定响应哪个中断源，而不是在每条指令的执行过程中这样做，因此Ⅱ错误。$CPU$会在每个存储周期结束后检查是否有$DMA$请求，而不是在指令执行过程的末尾这样做，因此Ⅲ错误。中断服务程序的最后指令通常是中断返回指令，该指令在中断恢复后，即$CPU$中的所有寄存器都已恢复到了中断前的状态，因此该指令不需要进行无条件转移，因此Ⅳ错误。

**例题** 下列关于中断$I/O$方式和$DMA$方式比较的叙述中，错误的是()。

$A.$中断$I/O$方式请求的是$CPU$处理时间，$DMA$方式请求的是总线使用权

$B.$中断响应发生在一条指令执行结束后，$DMA$响应发生在一个总线事务完成后

$C.$中断$I/O$方式下数据传送通过软件完成，$DMA$方式下数据传送由硬件完成

$D.$中断$I/O$方式适用于所有外部设备，$DMA$方式仅适用于快速外部设备

解：$D$。$DMA$方式适用于不需要中断的数据传输设备。
