# 凌鸥45系列的 GCC 平台搭建


<!--more-->
# 凌鸥45系列的 GCC 平台搭建

## 一、使用官方 LKS_CONFIG 配置底层代码（简化框架搭建）

### 1. 选择 MCU 型号

打开 LKS_CONFIG 工具，在 MCU 筛选环节选择对应型号：

- MCU 系列：LKS45x

- MCU 型号：LKS32MC453RCT8

  

### 2. 配置时钟

在时钟配置界面进行参数设定：

- HSI@12MHz

### 3. 配置 IO 输出

依据硬件原理图，在工具中对对应 IO 引脚进行输出配置，匹配硬件上的电源灯等外设的引脚需求。

### 4. 开启运放和 ADC

在**外设配置**界面，启用对应的运放模块和 ADC 通道，为后续模拟信号采集和处理预留硬件接口。

## 二、生成底层代码

在 LKS_CONFIG 工具的**代码生成**界面，完成以下配置并生成工程：

- 工程名称：LKS453_Pump
- 工程目录：E:/EmIDE/LKS453_EIDE/
- IDE 选型：gcc
- 点击**生成代码**完成工程创建

## 三、使用自带工具编译

### 1. 安装依赖组件 mingw32

编译前需安装 mingw32

### 2. 编译报错及解决方案

#### 报错 1：外部晶振变量配置错误

- **报错信息**：`source/hardware_init.c:14:33: error: 'SYS_PLLSRSEL_CRYSTAL' undeclared`
- **原因**：时钟配置的 PLL 源选择宏定义不匹配
- **解决方法**：将`hardware_init.c`中`SYS_InitStruct.PLL_SrcSel = SYS_PLLSRSEL_CRYSTAL;`修改为`SYS_PLLSRSEL_CRYSTAL_12M`；或直接改用 HSI 时钟，避免重复报错

#### 报错 2：无法找到 TRIM_Read

- **报错信息**：`undefined reference to TRIM_Read`
- **原因**：makefile 未引用`lks32mc45x_periph_driver/Source/liblks32mc45x_trim.a`库文件
- **解决方法**：在 makefile 中添加该库文件路径，修改后关键配置如下：

makefile

```akefile
# libraries
LIBDIR =lks32mc45x_periph_driver/Source/liblks32mc45x_trim.a
LDFLAGS = $(MCU) -specs=nano.specs -T$(LDSCRIPT) $(LIBDIR) $(LIBS) -Wl,-Map=$(BUILD_DIR)/$(TARGET).map,--cref -Wl,--gc-sections
```

> 建议修改 LKS_CONFIG 目录下的模块，避免后续生成代码重复触发该报错

### 3. 编译成功验证

编译完成后会生成`build/LKS453_Pump.elf`等文件

```plaintext
filename              text    data     bss     dec     hex
build/LKS453_Pump.elf  5344    4836       8   10188    27cc
```

同时会生成`hex`和`bin`格式的可下载程序文件

## 四、点灯程序调试

在`main.c`的 while 循环中添加点灯逻辑，即可通过 Jlink 的 RTT 工具进行调试，核心代码示例：

```c
while(1)
{
    /* USER CODE BEGIN MainWhile */
    GPIO_ResetBits(GPIO1, GPIO_Pin_1);
    GPIO_ResetBits(GPIO1, GPIO_Pin_2);
    GPIO_ResetBits(GPIO1, GPIO_Pin_3);
    PRINTF("flash\r\n");
    delay_ms(1000);
    GPIO_SetBits(GPIO1, GPIO_Pin_1);
    GPIO_SetBits(GPIO1, GPIO_Pin_2);
    GPIO_SetBits(GPIO1, GPIO_Pin_3);
    /* USER CODE END MainWhile */
    PRINTF("run main.while()...\r\n");
    delay_ms(1000);
}
```

RTT Viewer 终端会输出调试日志，验证程序正常运行。

## 五、后续计划：切换到 EIDE 环境

由于凌鸥默认配套环境功能简陋，可将工程移植到 vscode 搭载的 EIDE 框架中，结合各类 AI 插件提升开发效率。



---

> 作者: eMotor  
> URL: https://www.eMotor.top/posts/lks45_1/  

