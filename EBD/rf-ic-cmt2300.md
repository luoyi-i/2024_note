
#### cmt2300 例子程序和测试结果
直接用之前接收和发送

egcode: \esp32\rf-ic\esp32_rf\src
接收
1， 开始接收
2， 接收结束，等待

state: RF_Process
1， 开始接收
if state == idle:
    RF_StartRx()
2， 接收结束，等待
if state == RF_ERROR || RF_RX_DONE || RF_RX_TIMEOUT
    RF ended
RF_STATE_ERROR: RF_ERROR
    log
RF_STATE_RX_TIMEOUT: RF_RX_TIMEOUT
    log
RF_STATE_RX_DONE: RF_RX_DONE
    handle received data
3， state == busy
    do nothing
4, unknown error

-- test result
测试板间隔 60 ms 收到两组
收到的数据是对的， 但是只收到两组，少收了几组
检查程序，减少间隔时间---> 删掉 5ms delay， 解决

----------------------------------------
packet 模式下可以收到正确的数据
正确的数据是传感器会发送 6 次

-------------------------------
试一下 packet 下用了编码的方式
syn  曼彻斯特编码 
设为 4 字节， 接收失败
设为 2 字节， 接收失败
设置 0 为 high， 1 为 low  --- > 成功

现在 data 也使能 曼彻斯特编码，
当前 01=1，10=0, 接收失败， 因为未按键开始接收， 接收成功
当前 01=0，10=1, 接收失败


现在 packet 加上编码也可以，fifo 读到的数据就是解码出来的数据
波特率 19.2 = 9.6*2
**不管是否加上编码，波特率都是 19.2， 即 sensor 的波特率*2**

---------
波特率是  19.2*2
sync 是  0xFE, 编码后 0xAAA9
上面已经验证加上编码后可用，为了方便，加上编码，

---- 现在就是 sync 有问题，直接的原因是值有问题， 计算得到编码值是  aaaaaaa9
现在改 data representation  0=low, 1=high， 可以了。
用编码的时候，  0=high, 1=low

总结 packet 模式下参数的设置
data mode: packet
manchester: disable
deviation， 
传感器波特率为 9.6， cmt3200 里设为 19.2， deviation 设为 60(测出来的)
传感器波特率为 19.2， cmt3200 里设为 38.4， deviation 设为
preamble size 设为 0， 目前只测了接收
不使用编码时
FSK data representation: 0=f-low, 1=f-high
sync 4 bytes, eg 同步头 为 FE 时， sync 值设为  aaaaaaa9
data len 等于实际的数据长度 * 2（包括传感器的 CRC，不包括 sync）
使用编码时
Manchest 设为 enable, Manchester type 设为 01=one， 10=0
FSK data representation: 0=f-high, 1=f-low
sync 2 bytes, eg 同步头 为 FE 时， sync 值设为 FFFE
data len 等于实际的数据长度(包括传感器的 CRC， 不包括 sync)

其他
// Packet Type               = Fixed Length
// Payload Bit Order         = Start from msb


#### spec



_____ 寄存器分区
对CMT区，频率区，数据率区，以及发射区来说，用户无需理解具体的寄存器含义，只需要将导出的内容写入到对应的寄存器地址就可以了。
对于系统区和基带区，用户或许有需要在应用程序中实时更改某些配置，因此用户需要理解每一个寄存器位的含义。
同时，对于配置区1和配置区2来说，用户也需要理解每一个寄存器位的含义。

对3个区来说，地址是连续的，操作方式无本质区别，都是使用SPI按照访问寄存器的时序进行直接读写操作
-- 配置区 （由 app 配置）
[CMT Bank] 0 - 0x0b
[System Bank] 0x0c - 0x17
[Frequency Bank] 0x18 - 0x1f
[Data Rate Bank] 0x20 - 0x37
[Baseband Bank] 0x38 - 0x54
[Tx Bank] 0x55 - 0x5f

---控制区由用户根据应用需求配置
-- 控制区：0x60-0x6A 该区域配置工作状态、跳频配置、GPIO配置、中断源开关等
-- 控制区: 0x6B-0x71 该区域用于读取中断源标志，进行FIFO控制和RSSI测量等

cmt2300 datasheet

// __prog__ CC11xx_REG const r9600Fsk


4.3.5 接收信号强度指示器 (RSSI)
用户可以通过读取寄存器获得相应的RSSI 码值（RSSI_CODE<7:0>）或dBm 值（RSSI_DBM<7:0>）
对于相同的对象，根据接收信号强度可以判断其位置

对 SPI 协议的原理更熟悉了
片选信号使能后，在串口时钟信号的下降沿送出数据，在上升沿采集数据（根据不同芯片说明书)， SDIO 是一个双向的脚，用于输入和输出数据。

5.3 启动时序：
上电后需要 十几 ms 才能达到稳定状态

6.1 直通模式
Direct – 直通模式，在RX 模式下仅支持preamble 和sync 检测，FIFO 不工作；在TX 模式下仅支持对GPIO 输入的数据进行透传
RX, 在直通模式中，数据从解调器的输出直接通过DOUT 发送到外部MCU,
1. 通过CUS_IO_SEL寄存器配置GPIOs。
2. 配置DATA_MODE = 0。
3. 发送go_rx命令。
4. 连续地从DOUT捕获接收数据。
5. 发送go_sleep/go_stby/go_rfs命令来完成接收，并节省功耗。

#### 芯片的 dataesheet 是最详细最重要的

6.2 数据包模式
RX 
在包模式中，从解调器输出的数据会先被移送至包处理机中进行解码，然后填入FIFO。
包处理机提供多种解码引擎和判断据有效性的选项，这些可以减轻用户的 MCU 资源。

1. 通过CUS_IO_SEL配置GPIO。
2. 通过CUS_INT1_CTL, CUS_INT2_CTL和CUS_INT_EN设置中断。
3. 发送go_rx 命令。
4. 根据相关的中断状态读取RX FIFO。
5. 发送go_sleep/go_stby/go_rfs命令以节省功耗。
6. 通过CUS_ INT_CLR1和CUS_ INT_CLR1清除中断状态。


现有的项目是通过 spi 去读接收到的数据的，所以它不是用的直通模式。

_____ 直连模式
直通发射时需要配置0x55、0x69寄存器
连接配置为 DIN 的 2300A_GPIO 管脚的 MCU IO 口要配置为输出口


直通模式是直接从 IO 口采样， 采样几个点，去判断出电平的变换。
这个比较麻烦。
所以用 packet 模式最方便。
end

---------------- 写寄存器 ----------------
_____ 寄存器分区
对CMT区，频率区，数据率区，以及发射区来说，用户无需理解具体的寄存器含义，只需要将导出的内容写入到对应的寄存器地址就可以了。
对于系统区和基带区，用户或许有需要在应用程序中实时更改某些配置，因此用户需要理解每一个寄存器位的含义。
同时，对于配置区1和配置区2来说，用户也需要理解每一个寄存器位的含义。

对3个区来说，地址是连续的，操作方式无本质区别，都是使用SPI按照访问寄存器的时序进行直接读写操作
-- 配置区 （由 app 配置）
-- 直接用配置文件生成的
[CMT Bank] 0 - 0x0b 
[Frequency Bank] 0x18 - 0x1f
[Data Rate Bank] 0x20 - 0x37
[Tx Bank] 0x55 - 0x5f
-- 需要知道如何配置
[System Bank] 0x0c - 0x17
[Baseband Bank] 0x38 - 0x54

---控制区由用户根据应用需求配置
-- 控制区：0x60-0x6A 该区域配置工作状态、跳频配置、GPIO配置、中断源开关等
-- 控制区: 0x6B-0x71 该区域用于读取中断源标志，进行FIFO控制和RSSI测量等

配置 sync value
共 8 个字节 from PKT6 - PKT13
sync from byte l to h   [b0, b1, b2, b3]
if 1 字节， pkt13 = b0
if 2 字节， pkt13 = b1,  pkt12 = b0
if 3 字节， 
拷贝是从低字节开始
把 sync 值转换为



#### 特别的点
sync最少要 3 字节， 波特率越大，需要的 sync 字节越多


