# STM32G0B1RET6N
## Output type
+ Push Pull output
+ Open Drain output

### Push Pull 
![](https://hackmd.io/_uploads/SJN9ya7q3.png)


+ When Iuput is High, upper MOSFET will be on state and lower MOSFET will be not.
    + Output will be High.
+ When Input is Low, Both MOSFET will be on the contrary.
    + Output will be Low.

+ Push Pull 可以理解為既可以推也可拉，這樣就無需加上一個上拉電阻
### Open Drain
![](https://hackmd.io/_uploads/SkA5gTmq3.png)

+ When the MOSFET on state, the output will be Low.
+ On the contrary, it will not conduct, so this curcuit need to add a pull-up resistor.

+ Open Drain 雖說要多一個上拉電阻去穩定高電位輸出，但她可以使得從外部上拉的方式提高電位輸出

### Summary
+ 若要轉換速度快，選 Push Pull，但功耗相對大
+ 若需要高輸出、功耗低，選 Open Drain

|  Name  |  Feature  |
| :----: | :-------: |
| Push Pull|1. 可以吸電流<br>2. 可以灌電流<br>3. 輸出電壓由 IC 電源決定|
| Open Drain|1. 可以做電壓轉換<br>2. Vo 由外接提供，IC 內部僅需很小的閘極驅動電流<br>3. 可改變上拉電阻的電壓，調整邏輯 High 的準位|

## GPIO

+ 可以透過軟體配置輸入輸出是否提供上拉電阻，或是設置 Open Drain 或 Push Pull

### Input 
![](https://ithelp.ithome.com.tw/upload/images/20200922/201200932PR7WLQqMS.png)
+ Output buffer 會被禁用
+ 疏密特觸發器的輸入會被啟用
+ 是否要激活上拉或下拉電阻，是依靠 **GPIOx_PUPDR 暫存器**控制
+ 每一個 AHB 周期都會將 I/O pin 上的資訊採樣到 Input data register
+ 可以訪問 Input data register，他會提供你 I/O 狀態

### Output
![](https://ithelp.ithome.com.tw/upload/images/20200922/20120093JLyJBoAZkA.png)
+ Output buffer 會被啟用
    + Open Drain
        + 只會激活 N-MOS，且用 Output register 決定輸出
    + Push Pull
        + P-MOS 以及 N-MOS 都會被激活，一樣用 Output register 決定輸出
+ 同 Input 2-5 點
+ 可訪問 Output data register 得到最後寫入的值

### Alternate function
![](https://hackmd.io/_uploads/S184ThVqn.png)

+ 在初始化 stm32 的時後，原本的腳位設定都是I/O，如果需要選用到TIMER、USART這類的周邊輸出輸入，就需要選擇 alternate function，這樣的話，I/O就會被選擇到相對應的功能。
+ 另外腳位的預設還可以REMAP(重新設定)，這時後也是需要選擇 alternate function，這滿需要注意的
+ 可以配置 Output buffer 是 Open Drain 或是 Push Pull
+ Output buffer 由外部的信號所驅動
+ 同 Input 2-5 點

### Analog 
![](https://hackmd.io/_uploads/rJZJuZB5h.png)

+ Output buffer 會被禁用
+ 疏密特觸發器會**被停用**，每個 I/O Pin 的類比值提供 0 消耗。疏密特觸發器被停用，所以輸出會是 0
+ 內部的上下拉電阻**被停用**
+ 是否要激活上拉或下拉電阻，是依靠**硬體**控制
+ 讀寫訪問 input data register，會得到 0

### Register
+ IDR ( Input Data Register )
    + input 模式下 GPIOx 的讀值
+ ODR ( Output Data Register )
    + Output 模式下 GPIOx 的 data
+ BSRR ( port Bit Set/Reset Register )
    + 可以控制 GPIOx 其中一個位元的狀態為 High 或是 Low
    + 前 16 為 reset，後 16 為 set
+ BRR ( Bit Reset Register )
    + 可以控制 GPIOx 其中一個位元為 Low
    + 前 16 位元可 reset

## GPIO Register by Docs
### Mode 
**Address offset 0x00**
**MODEx[15:0][1:0]**
```
GPIO_InitStruct.Mode
```
+ 00 : Input mode
+ 01 : General purpose output mode
+ 10 : Alternate function mode
+ 11 : Analog mode (reset state)

### Output Type
**Address offset 0x04**
**OTx[15:0]**
```
GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_<Type>
```
+ 0 : Output push-pull (reset state)
+ 1 : Output open-drain

### Output Speed
**Address offset 0x08**
**OSPEED[15:0][1:0]**
```
GPIO_InitStruct.Speed
```
+ 00 : very low 
+ 01 : low 
+ 10 : high
+ 11 : very high


### Pull Up/ Pull Down Register
**Address offset 0x0C**
**PUPD[15:0][1:0]**
```
GPIO_InitStruct.Pull
```
+ 00 : No pull-up, pull-down
+ 01 : Pull-up
+ 10 : Pull-down
+ 11 : Reserved

### Input Data
**Address offset 0x10**
**ID[15:0]**
+ <font color = red>!!</font> Read Only

### Output Data
**Address offset 0x14**
**OD[15:0]**
+ Read and Write by Software

### Bit Set/Reset Register
**Address offset 0x18**
BR = BSRR[31:16]
BS = BSRR[15:0]
**BR[15:0]**
+ Reset the corresponding bit

**BS[15:0]**
+ Set the corresponding bit

### Alternate Function Low
**Address offset 0x20**
**AFSELy[3:0]**
```
GPIO_InitStruct.Alternate
```
Pin 0 ~ 7
+ 0000 : AF0
+ 0001 : AF1
+ 0010 : AF2
+ 0011 : AF3
+ 0100 : AF4
+ 0101 : AF5
+ 0110 : AF6
+ 0111 : AF7
+ Otherwise : Reserved

### Alternate Function High
**Address offset 0x24**
**AFSELy[3:0]**
```
GPIO_InitStruct.Alternate
```
Pin 8 ~ 15
+ 0000 : AF0
+ 0001 : AF1
+ 0010 : AF2
+ 0011 : AF3
+ 0100 : AF4
+ 0101 : AF5
+ 0110 : AF6
+ 0111 : AF7
+ Otherwise : Reserved

### Bit Reset
**Address offset 0x28**
**BR[15:0]**
+ Reset the corresponding bit


## TIME


## ADC 
+ 16 個外部通道，3 個內部通道
+ 可以自我校準
    + ADC_DR register (from bits 6 to 0).
    + 校準資料消失
        + ADC 被拔掉電源
        + ADC 被重製
    + 需要在 Start 前做校準，將 ADCAL 設為 1
+ ADC 轉換
    + 單次
    + 連續
    + 掃描
    + 不連續、間斷
+ 電壓輸入範圍
    + VREF- <= VIN <= VREF+
    + VSSA 和 VREF- 接地，VREF+ 和 VDDA 接 3.3V
    + 所以輸入範圍為 0 ~ 3.3V
+ 規則通道
+ 注入通道
    + 像是中斷一樣
+ 觸發源
    + 直接讀寫暫存器，控制 CR 的 ADEN，調用他觸發轉換
    + 透過內部定時器，利用內部時鐘讓 ADC 週期性的觸發轉換
    + 外部 I/O 觸發，在 ADC 需要時觸發轉換
+ 轉換時間
    + ADC 的每一次信號轉換都要時間，這個時間就是轉換時間
    + 輸入時脈
        + 由於 ADC 在 STM32 中是掛載在 APB2 總線上，所以 ADC 可以得到 PLCK
    + 採樣週期
        + 利用 SMPR 採樣週期
+ 資料暫存器
    + 轉換後的數據會存在暫存器中，存放也有分為規則跟注入通道
    + 規則的資料暫存器，ADC_DR


+ 可以自我校準
    + ADC_DR register (from bits 6 to 0).
    + 校準資料消失
        + ADC 被拔掉電源
        + ADC 被重製
    + 需要在 Start 前做校準，將 ADCAL 設為 1
+ 開關控制器
    + ADEN = 1，啟用 ADC，且 ADRDY 會被 set
    + ADDIS = 1，禁用 ADC 且至於 power down mode
    + 當 ADC 被完全禁用時，ADEN 以及 ADDIS 會被清除


## I2C
+ 3 I2C controllers, I2C1, I2C2, I2C3
https://rexpighj123.pixnet.net/blog/post/219960237
### Mode 
+ Standard-mode
    + up to 100kHz
+ Fast-mode
    + up to 400kHz
+ Fast-mode Plus
    + up to 1Mhz


## SPI 




+ Flash 長度
    + 64M-bit / 8M-byte
+ 最高運行頻率
    + 133MHz Single, Dual/Quad SPI clocks
+ 分塊、扇區擦除大小
    + Uniform Sector/Block Erase (4K/32K/64K-Byte)
+ 寫入數據大小範圍
    + Program 1 to 256 byte per programmable page
+ 電壓工作範圍
    + Full voltage range: 2.3-3.6V
### SPI Pin
+ CS
    + Select device 
+ SCLK
    + System clock is generated by Master
+ MISO
    + Master In, Slave Out
    + Master 接收，Slave 傳送 
+ MOSI
    + Master Out, Slave In
    + Master 傳送，Slave 接收

### Communication mode 
+ CPOL 時鐘極性
    + 0：LOW 為空閒狀態，HIGH 為有效狀態 (平常為 LOW)
    + 1：HIGH 為空閒狀態，LOW 為有效狀態 (平常為 HIGH)
+ CPHA 時鐘相位
    + 0：在第一個邊緣採樣，
    + 1：在第二個邊緣採樣，

| CPOL | CPHA | 資料擷取 |
| :--: | :--: | :------:|
|0|0| H有效、低轉高時採樣(正緣)(第一個邊緣) |
|0|1| H有效、高轉低時採樣(負緣)(第二個邊緣) |
|1|0| L有效、高轉低時採樣(負緣)(第一個邊緣) |
|1|1| L有效、低轉高時採樣(正緣)(第二個邊緣) |

### Inner working
+ Master 跟 Slave 都有一個串列移位暫存器
+ 

### Instructions
+ Standard SPI Instructions
    + SPI bus operation Mode 0 (0,0) and 3 (1,1) are supported.
### Register
+ BUSY
    + Set to 1 when the device is executing a Page Program
    + Set to 0 when the device is ready for futher instructions
+ WELL
    + Set to 1 after the device is executing a Write Enable Instruction
    + Set to 0 when the device is write disabled

### Operate Register
+ Write Enable
    + 0x06
+ Write Disable
    + 0x04
+ Read Data
    + 0x03
    + 24-bit Address
+ Page Program
    + 0x02
    + 24-bit Address
    + Data Byte 1 ~ Data Byte 256
    + WEL would be 0 after Page Program finish
+ Sector Erase
    + 0x20
    + 24-bit Address
+ Block Erase
    + 0x52
+ Chip Erase
    + 0xC7 or 0x60
+ Read Status Register
    + 0x05

### Flash Pin
![](https://hackmd.io/_uploads/B1SCMXCih.png)
+ /CS
    + high is deselected, Output pin would be high impedance 
    + low is selected
+ CLK
+ DI(IO~0~)
+ DO(IO~1~)

### Status Registers
![](https://hackmd.io/_uploads/BJLEaIeh2.png)
