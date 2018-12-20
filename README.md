# DAQ电路板
1、电源
外部输入VCC   ->  防浪涌、防短接  ->  XL6009输出24V  -> TPS5430输出6V  -> LP3871-5V（模拟电源，给ADG和AD7606供电）
                                                    -> LC滤波  ->  输出24V（外部）

2、开关量测量电路


3、ADC采集
GND：接AGND
CVA/CVB，控制ADC采集
RAGE，0：量程±5V，1：量程±10V
RD/SCLK，读数据  <-  SPI总线时钟SCK
RST，复位信号  
BUSY，忙信号，正在转换
CS，片选信号
FRST，第一个通道样本的指示信号
VIO，通信接口电平，用控制/通信电路板的3.3V
DB7(DOUTA)，SPI总线数据线MISO
其余DB，不接
FRST，不接


|PGND   |DGND（用于开关量的采集）  |开关量测量（向下，数字）   |3.3V（数字，向上，用于开关量采集）  |VCC6V（向下）  |SPI_MISO,SPI_SCK,（与ADC通信）  

|CVA/CVB（向上，控制ADC采集）  |RAGE(这里直接用控制/通信电路板的GND拉低)   |CS（向上，AD7606片选） |VIO（向上，连控制/通信电路板的3.3V）


# 控制/通信电路板
1、电源
外部输入VCC6V  ->  LM2596-3.3V(用于STM32和通信模组供电)
               ->  LM2596-5V（用于CAN芯片TJA1050供电）

2、主控
STM32

3、Wi-Fi模组
RS9001

4、SW下载

5、CAN通信
TJA1050

# 注意点
1、接口类型

电源(6芯)

输入：8-32V电源(对应PGND)

输出：24V(对应AGND)、5V(对应DGND)

地：PGND、AGND、DGND

模拟信号(6芯)

输入：CH1、CH2、CH3、CH4

输出：24V（用于传感器供电）

地：AGND

数字信号(3芯)

输入：Digital1、Digital2

地：DGND

通信(5芯)

输入/输出：CAN1+、CAN1-、CAN2+、CAN2+

地：DGND

2、外壳类型

*组件*

通风口

天线接口

信号借口

启动/复位按钮

工作指示灯


*晶振*

采用有源晶振，0.1PPM，老化率1PPM/Y

*数据回放功能*

数据存储在本地（EXCEL文件）

随时可以读取EXCEL

以本次测试名称+时间戳的方式

*场景*

* 测试人员在测试地点测试，可以通过局域网查看数据（也需要联网设置数据库中本次数据命名）

* 远程调试人员（陈亮），通过调取服务器数据，可以不用跟着去

* 远程调试人员（陈亮）远程修改代码，通过FOTA更新制动器代码。（刘波所要做的工作）

*方案*

* 测试开始前上位机连接服务器，设置本次测试的名称

设置名称后，本地和服务器端的数据都可以以“名称+时间戳”的方式存储数据

本地：excel
服务器：document（时间戳也由上位机发送过来）

所要做的工作：
服务器：修改MongoDB数据插入方法，每次修改名称后，就重新建立一个doc
上位机：每次修改名称后，就建立一个excel

* 增加服务器的数据转发使能功能，只有上位机要求服务器转发才转发

不会导致上位机连接服务器后马上接收到数据（只能通过局域网或者服务器，两者不能同时开）

*操作流程*

**现场**

a.上位机连接局域网和远程服务器

选择连接局域网/远程服务器
如果是现场调试，需要全部连接（如果服务器崩了，那么只存储在本地也可以）；如果是远程调试，只需要连接服务器

b.选择传感器数据通过局域网还是服务器接收

c.上位机设置本次测试名称+时间戳

设置框+确定按钮，服务器和本地都成功创建了存储文件

d.开始试验

**远程**

a.上位机连接服务器

b.查看服务器数据的doc名称

c.下载所需的doc到本地excel

d.上位机导入excel，查看数据图像

我的工作

服务器:

1、可以创建不同名称的doc

2、PC连上服务器后，还需要指令来使能实时发送。

方案：连上服务器后，加入已连接的map，申请实时发送后再加入实时发送的map

5、TVS管
SMDJ33CA,smc  

或者SMDJ36CA



