## **SlimeVR_BMI270**
![SlimeVR_Server.png](https://image.lceda.cn/oshwhub/f2e35224d06b4dde9bd27ad48a2b8369.png)

### **1. 什么是SlimeVR？**
SlimeVR是一组开放的硬件传感器和开源软件，有助于在虚拟现实中实现全身跟踪（FBT）。
- [官方文档](https://docs.slimevr.dev/)
- [l0ud/SlimeVR-Tracker-ESP-BMI270分支](https://github.com/l0ud/SlimeVR-Tracker-ESP-BMI270)

### **2. 介绍**
本项目由 ESP-12F + IP5306-CK + BMI270 组成，CH340K实现自动下载，采用LDO降压，提供3.3V供电

**官网对BMI270的介绍如下**：BMI270 是一款相对较新的实验性 IMU，用于 DIY SlimeVR。 它的性能似乎明显优于 BMI160，同时价格实惠。
外壳使用Fusion360设计![外壳](./IMG/Assembly.gif)

### **3. 固件**
SlimeVR 固件的主分支尚不支持此BMI270，因此本项目使用[``` l0ud/SlimeVR-Tracker-ESP-BMI270 ```](https://github.com/l0ud/SlimeVR-Tracker-ESP-BMI270)分支

#### 3.1 **构建 SlimeVR 固件**
- 安装[CH340驱动](https://cdn.sparkfun.com/assets/learn_tutorials/8/4/4/CH341SER.EXE)
- VSCode安装PlatformIO IDE扩展

- 克隆[``` l0ud/SlimeVR-Tracker-ESP-BMI270 ```](https://github.com/l0ud/SlimeVR-Tracker-ESP-BMI270)分支并通过VSCode打开(在此期间VSCode会自动配置环境，请保持网络畅通)
- 在 ```platformio.ini``` 设置```DWIFI_CREDS_SSID```和```DWIFI_CREDS_PASSWD```
- 替换```src```/```defines.h```

```
#define IMU IMU_BMI270
#define SECOND_IMU IMU
#define BOARD BOARD_CUSTOM
#define IMU_ROTATION DEG_90
#define SECOND_IMU_ROTATION DEG_0

#define PRIMARY_IMU_OPTIONAL true
#define SECONDARY_IMU_OPTIONAL true

#define MAX_IMU_COUNT 1

#ifndef IMU_DESC_LIST
#define IMU_DESC_LIST \
    IMU_DESC_ENTRY(IMU,        PRIMARY_IMU_ADDRESS_ONE,   IMU_ROTATION,        PIN_IMU_SCL, PIN_IMU_SDA, PRIMARY_IMU_OPTIONAL,   PIN_IMU_INT) 

#endif

#define BATTERY_MONITOR BAT_EXTERNAL
#define BATTERY_SHIELD_RESISTANCE 180

#define PIN_IMU_SDA D2
#define PIN_IMU_SCL D1
#define PIN_IMU_INT D0
#define PIN_IMU_INT_2 D5
#define PIN_BATTERY_LEVEL A0
```
#### 3.2 **上传固件**
- 使用数据线连接
- 点击```PlatformIO:Upload```,等待烧录完成

### 4. **使用**
- 打开SlimeVR服务端，按提示操作

### 5. **其他**
BMI270的i2c地址是0x68(当SDO脚接地)/0x69(当SDO脚拉高)，SlimeVR源码中```MPU9250```/```BMI160```/```MPU6500```/```ICM20948```/```ICM42688```默认作为主传感器的i2c地址为0x68，我在画第一版的时候是将SDO脚拉高了，所以把```src```/```sensors```/```sensoraddresses.h```文件中的0x68与0x69做了替换，修改过的版本无需注意

###  6. **感谢**
- 主要电路参考[SlimeVR_BNO085](https://oshwhub.com/myzhazha/slimevr)，感谢