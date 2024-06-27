---
title: 计算机组成原理期末复习
date: 2024-06-27 22:58:25
tags: "CO"
categories: "Note"
top_img: https://apr-akina.online/blog/61ac5e440dac4d97a6624e05044c4074.jpg
cover: https://apr-akina.online/blog/61ac5e440dac4d97a6624e05044c4074.jpg
---
### Notes

# 填空题

- 计算机系统中采用补码运算的目的是（**简化计算机设计**）
- 程序计数器PC在（**控制器中**）
- 计算机主频周期是指（**时钟周期**）
- I/O接口中数据缓冲器的作用是（**解决CPU与外设之间的速度不匹配问题**）
- 直接转移指令的功能是将指令中的地址代码送入（**PC程序计数器**），以实现程序的跳转或分支
- 计算机系统的软件可分为（**系统软件**）和（**应用软件**）
- 微程序设计技术是利用（**软件**）方法设计（**硬件**）的一门技术，特点是（**规整性，可维护性，灵活性**）
- 储存系统的（**Cache一主存**）和（ **主存—辅存** ）都用到了局部性原理
- 完整的指令周期包括取指周期，间指周期，（**执行周期和中断周期**）
- DMA技术的出现使得外围设备可通过（**DMA控制器**）直接访问（**内存**）

# 简答题

解释时钟周期，机器周期，指令周期，主机，主存，存储字长，机器字长，指令字长

- 时钟周期，也称为节拍脉冲，是CPU最小的时间单位
- 机器周期，又称CPU周期，是CPU访问一次内存所花的时间较长，因此用从内存读取一条指令字的最短时间来定义
- 指令周期是CPU从主存中取出并执行一条指令的时间
- 主机通常指的是计算机系统中的中央处理单元CPU及其相关的控制电路，它负责执行程序指令和处理数据
- 主存，也称为主存储器，是计算机系统中用于暂时存放数据和指令的部件
- 存储字长是指一个存储单元的二进制代码位数，通常与MDR（存储器数据寄存器）的位数相同
- 机器字长是指CPU进行一次整数运算时能处理的二进制数据的位数
- 指令字长是指一条指令的总长度

总线仲裁是指在多个主设备同时竞争总线控制权时，通过某种方式选择一个主设备优先获得总线控制权的过程

集中式仲裁的方法：

1. 链式查询方式
2. 计数器定时查询方式
3. 独立请求方式

提高访存速度可采取哪些措施？

- 采用高速器件：选择存取周期短的芯片，这样可以直接提高存储器的速度。
- 采用层次结构Cache：Cache位于存储器和CPU之间，如果CPU需要的数据已经存在于Cache中，那么访问速度将大大提高
- 调整主存结构：采用单体字系统，在一个存取周期内可以取出多个存储字，这样可以增加存储器的带宽。
- 使用大容量内存：大容量内存可以更好的容纳数据，减少内存碎片，从而提高访存速度。
- 使用虚拟内存：虚拟内存可以将物理内存与硬盘空间结合起来，模拟更大的内存容量，从而提高访存速度。
- 优化内存管理：合理的内存管理可以有效提高访存速度，例如通过减少内存碎片和优化内存分配策略。

简述CPU访问IO端口的过程。

1. CPU通过地址总线发送I/O端口的地址
2. 系统中的译码器接收到地址信息后，确定具体的I/O端口地址。
3. CPU通过数据总线将数据发送到选定的I/O端口。
4. I/O设备接收到数据后，根据指令进行相应的操作。

# 计算题

求下列各种码对应的真值（正负数原反补移的计算）

*先求正数，原反补都一样，再求移码。接着负数原码取负即可（同一个数的话），剩下的按规则转为原码计算即可。本质得记住转换方法，这里只针对本题提高效率，一定不要死板，看好题目要求，记好每一步转换后得到的到底是什么码，只有原码才转数字（真值）*

```java
[x]原 = 10001101----------[x]原 = 00001101
[x]反 = 10001101----------[x]反 = 00001101
[x]补 = 10001101----------[x]补 = 00001101
[x]移 = 10001101----------[x]移 = 00001101
符号位除外：
	按位取反，原反
	加一，反补
	从右到左第一个1左边的取反（按位取反加1）原补
符号位：
	取反，补移
[-13]-------[13]
[-114]------[13]
[-115]======[13]
[13]--------[-115]
```

*补充一点：转换规则就三种方式；取反，加一，按位取反。没必要记什么码转什么码用什么方式，自己转完了就知道是什么码了，原反补对数值操作，移码对符号位操作。*

### 总线

![总线计算题](https://apr-akina.online/blog/20240627160148.png)

### 微程序

某计算机的控制器采用微程序控制方式，微指令中的操作控制字段采用字段直接编码，共有33个微命令，构成5个互斥类，分别包含：7、3、12、5、6个微命令，则操作控制字段至少有多少位？

*取对数求和秒了，不懂，,没啥好说*

![微程序指令计算](https://apr-akina.online/blog/20240627161801.png)

*为啥取2的对数？想想也知道7得用3个二进制位表示。*

### 计算机性能计算

两台计算机A和B采用不同主频的CPU，而片内逻辑电路相同。

（1）若A机的主频为8MHz，B机为12MHz，则两机的CPU时钟周期各是多少？

（2）如果A机的平均指令执行速度为0.4MIPS，那个A机的平均指令执行时间是多少？

（3）B机的平均指令执行速度MIPS是多少？

**答案：**

（1）A机的CPU时钟周期=1/主频=1/8=0.125us，B机的CPU时钟周期=1/主频=1/12=0.083us

（2）这里指定MIPS=0.4，即每秒执行0.4百万条指令，所以平均指令执行时间为1/MIPS=1/0.4=2.5us

（3）A机的MIPS=0.4，所以其CPI=主频/MIPS=8/0.4=20，由于A机和B机的片内逻辑电路完全相同，所以两者的CPI也相同，即B机的CPI=20。因此B机的MIPS=主频/CPI=12/20=0.6MIPS

# 应用题（指令流）

除了第一题，剩余题答案都是代码框中的内容

M代表内存，（）括号代表某个寄存器内的内容，（PC）代表pc寄存器内的内容

![指令流](https://apr-akina.online/blog/20240627170059.png)

1. **请写出图中a、b、c、d 4个寄存器的名称。**
    
    从图中可以看到，a、b、c、d 4个寄存器的名称分别是：
    
    - a: MAR（主存地址寄存器）
    - b: MDR（主存数据寄存器）
    - c: IR（指令寄存器）
    - d: PC（程序计数器）
2. **简述图中取指令的数据通路。**
    
    ```pascal
    (PC)→ MAR
    M(MAR)→ MDR
    (MDR)→IR
    ```
    
    取指令的数据通路大致如下：
    
    - PC 中存储着下一条指令的地址。
    - 这个地址通过地址总线送到 MAR。
    - MAR 通过地址总线从主存储器（M）中取出指令，存入 MDR。
    - MDR 中的指令通过数据总线送到 IR。
3. **简述数据在运算器和主存之间进行存/取访问的数据通路。**
    
    ```pascal
    存/取的数据放到ACC中
    设数据地址已放入MAR
    取:
    	 M(MAR)→MDR
    	(MDR)→ALU→ ACC
    存:
    	(ACC)→ MDR
    	(MDR)→ M(MAR)
    ```
    
    数据在运算器（ALU）和主存之间进行存/取访问的数据通路如下：
    
    - 取数据：
        - 从主存储器（M）中的数据通过地址总线到 MAR，然后数据通过数据总线到 MDR，最后从 MDR 送到 ACC（累加器）或者直接送到 ALU 进行运算。
    - 存数据：
        - 从 ACC 或 ALU 送出数据，通过数据总线到 MDR，然后通过地址总线和数据总线送到 MAR，再存入主存储器（M）。
4. **简述完成指令 LDA X 的数据通路（X 为主存地址）。**
    
    ```pascal
    X → MAR
    M(MAR) → MDR
    (MDR)→ALU → ACC
    ```
    
    指令 LDA X（装载数据到累加器）的数据通路如下：
    
    - PC 将指令地址送到 MAR。
    - 从主存（M）中取出指令送到 MDR，然后送到 IR。
    - 解释指令 LDA X 后，X 地址送到 MAR。
    - 从主存（M）中取出 X 地址的数据送到 MDR，然后送到 ACC。
5. **简述完成指令 ADD Y 的数据通路（Y 为主存地址）。**
    
    ```pascal
    Y → MAR
    M(MAR) → MDR
    (MDR)→ ALU,(ACC) →ALU
    ALU→ ACC
    ```
    
    指令 ADD Y（将主存地址 Y 的数据加到累加器）的数据通路如下：
    
    - PC 将指令地址送到 MAR。
    - 从主存（M）中取出指令送到 MDR，然后送到 IR。
    - 解释指令 ADD Y 后，Y 地址送到 MAR。
    - 从主存（M）中取出 Y 地址的数据送到 MDR。
    - MDR 中的数据送到 ALU，与 ACC 中的数据进行加法运算，结果存回 ACC。
6. **简述完成指令 STA Z 的数据通路（Z 为主存地址）。**
    
    ```pascal
    Z → MAR
    (ACC)→ MDR
    (MDR)→ M(MAR)
    ```
    
    指令 STA Z（将累加器的数据存入主存地址 Z）的数据通路如下：
    
    - PC 将指令地址送到 MAR。
    - 从主存（M）中取出指令送到 MDR，然后送到 IR。
    - 解释指令 STA Z 后，Z 地址送到 MAR。
    - ACC 中的数据送到 MDR。
    - MDR 中的数据通过数据总线存到主存（M）的 Z 地址。

# 设计题

某机器字长为8位，试用如下给出的芯片设计一个存储器来增加主存的存储字数。（芯片：8K*8位）

![主存容量扩展-字扩展](https://apr-akina.online/blog/20240627160218.png)

*我不会，记吧，记两个就行，后面也能画出来*

# 整点彩蛋

<style>
.custom-image-container {
    text-align: center;
    margin: 20px 0;
}

.custom-image-container img {
    max-width: 100%;
    height: auto;
    border: 2px solid #ddd;
    border-radius: 5px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.custom-image-caption {
    margin-top: 10px;
    font-style: italic;
    color: #555;
}
</style>

<div class="custom-image-container">
    <img src="https://apr-akina.online/blog/03.jpg" alt="主存容量扩展-字扩展">
    <div class="custom-image-caption">这将是人生长达四年的夏季</div>
</div>