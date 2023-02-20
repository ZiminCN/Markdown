# **ST意法半导体HAl硬件抽象层各类通信方法 使用记录详解**

# SPI篇

SPI （Serial Peripheral Interface）官方介绍

## How to use 

    [..]
      The SPI HAL driver can be used as follows:
    
      (#) Declare a SPI_HandleTypeDef handle structure, for example:
          SPI_HandleTypeDef  hspi;
    
      (#)Initialize the SPI low level resources by implementing the HAL_SPI_MspInit() API:
          (##) Enable the SPIx interface clock
          (##) SPI pins configuration
              (+++) Enable the clock for the SPI GPIOs
              (+++) Configure these SPI pins as alternate function push-pull
          (##) NVIC configuration if you need to use interrupt process
              (+++) Configure the SPIx interrupt priority
              (+++) Enable the NVIC SPI IRQ handle
          (##) DMA Configuration if you need to use DMA process
              (+++) Declare a DMA_HandleTypeDef handle structure for the transmit or receive Stream/Channel
              (+++) Enable the DMAx clock
              (+++) Configure the DMA handle parameters
              (+++) Configure the DMA Tx or Rx Stream/Channel
              (+++) Associate the initialized hdma_tx(or _rx)  handle to the hspi DMA Tx or Rx handle
              (+++) Configure the priority and enable the NVIC for the transfer complete interrupt on the DMA Tx or Rx Stream/Channel
    
      (#) Program the Mode, BidirectionalMode , Data size, Baudrate Prescaler, NSS
          management, Clock polarity and phase, FirstBit and CRC configuration in the hspi Init structure.
    
      (#) Initialize the SPI registers by calling the HAL_SPI_Init() API:
          (++) This API configures also the low level Hardware GPIO, CLOCK, CORTEX...etc)
              by calling the customized HAL_SPI_MspInit() API.
     [..]
       Circular mode restriction:
      (#) The DMA circular mode cannot be used when the SPI is configured in these modes:
          (##) Master 2Lines RxOnly
          (##) Master 1Line Rx
      (#) The CRC feature is not managed when the DMA circular mode is enabled
      (#) When the SPI DMA Pause/Stop features are used, we must use the following APIs
          the HAL_SPI_DMAPause()/ HAL_SPI_DMAStop() only under the SPI callbacks
     [..]
       Master Receive mode restriction:
      (#) In Master unidirectional receive-only mode (MSTR =1, BIDIMODE=0, RXONLY=1) or
          bidirectional receive mode (MSTR=1, BIDIMODE=1, BIDIOE=0), to ensure that the SPI
          does not initiate a new transfer the following procedure has to be respected:
          (##) HAL_SPI_DeInit()
          (##) HAL_SPI_Init()
     [..]
       Callback registration:
    
      (#) The compilation flag USE_HAL_SPI_REGISTER_CALLBACKS when set to 1U
          allows the user to configure dynamically the driver callbacks.
          Use Functions HAL_SPI_RegisterCallback() to register an interrupt callback.
    
          Function HAL_SPI_RegisterCallback() allows to register following callbacks:
            (++) TxCpltCallback        : SPI Tx Completed callback
            (++) RxCpltCallback        : SPI Rx Completed callback
            (++) TxRxCpltCallback      : SPI TxRx Completed callback
            (++) TxHalfCpltCallback    : SPI Tx Half Completed callback
            (++) RxHalfCpltCallback    : SPI Rx Half Completed callback
            (++) TxRxHalfCpltCallback  : SPI TxRx Half Completed callback
            (++) ErrorCallback         : SPI Error callback
            (++) AbortCpltCallback     : SPI Abort callback
            (++) MspInitCallback       : SPI Msp Init callback
            (++) MspDeInitCallback     : SPI Msp DeInit callback
          This function takes as parameters the HAL peripheral handle, the Callback ID
          and a pointer to the user callback function.
       (#) Use function HAL_SPI_UnRegisterCallback to reset a callback to the default
          weak function.
          HAL_SPI_UnRegisterCallback takes as parameters the HAL peripheral handle,
          and the Callback ID.
          This function allows to reset following callbacks:
            (++) TxCpltCallback        : SPI Tx Completed callback
            (++) RxCpltCallback        : SPI Rx Completed callback
            (++) TxRxCpltCallback      : SPI TxRx Completed callback
            (++) TxHalfCpltCallback    : SPI Tx Half Completed callback
            (++) RxHalfCpltCallback    : SPI Rx Half Completed callback
            (++) TxRxHalfCpltCallback  : SPI TxRx Half Completed callback
            (++) ErrorCallback         : SPI Error callback
            (++) AbortCpltCallback     : SPI Abort callback
            (++) MspInitCallback       : SPI Msp Init callback
            (++) MspDeInitCallback     : SPI Msp DeInit callback
    
       [..]
       By default, after the HAL_SPI_Init() and when the state is HAL_SPI_STATE_RESET
       all callbacks are set to the corresponding weak functions:
       examples HAL_SPI_MasterTxCpltCallback(), HAL_SPI_MasterRxCpltCallback().
       Exception done for MspInit and MspDeInit functions that are
       reset to the legacy weak functions in the HAL_SPI_Init()/ HAL_SPI_DeInit() only when
       these callbacks are null (not registered beforehand).
       If MspInit or MspDeInit are not null, the HAL_SPI_Init()/ HAL_SPI_DeInit()
       keep and use the user MspInit/MspDeInit callbacks (registered beforehand) whatever the state.
    
       [..]
       Callbacks can be registered/unregistered in HAL_SPI_STATE_READY state only.
       Exception done MspInit/MspDeInit functions that can be registered/unregistered
       in HAL_SPI_STATE_READY or HAL_SPI_STATE_RESET state,
       thus registered (user) MspInit/DeInit callbacks can be used during the Init/DeInit.
       Then, the user first registers the MspInit/MspDeInit user callbacks
       using HAL_SPI_RegisterCallback() before calling HAL_SPI_DeInit()
       or HAL_SPI_Init() function.
    
       [..]
       When the compilation define USE_HAL_PPP_REGISTER_CALLBACKS is set to 0 or
       not defined, the callback registering feature is not available
       and weak (surcharged) callbacks are used.
    
     [..]
       Using the HAL it is not possible to reach all supported SPI frequency with the different SPI Modes,
       the following table resume the max SPI frequency reached with data size 8bits/16bits,
         according to frequency of the APBx Peripheral Clock (fPCLK) used by the SPI instance.

## 驱动SPI：

总的来说一些常用的SPI的APIs如下：

- HAL_SPI_Init()
- HAL_SPI_DeInit()
- HAL_SPI_MspInit()
- HAL_SPI_MspDeInit()
- HAL_SPI_Transmit()
- HAL_SPI_Receive()
- HAL_SPI_TransmitReceive()
- HAL_SPI_Transmit_IT()
- HAL_SPI_Receive_IT()
- HAL_SPI_TransmitReceive_IT()
- HAL_SPI_Transmit_DMA()
- HAL_SPI_Receive_DMA()
- HAL_SPI_TransmitReceive_DMA()
- HAL_SPI_DMAPause()
- HAL_SPI_DMAResume()
- HAL_SPI_DMAStop()
- HAL_SPI_IRQHandler()
- HAL_SPI_TxCpltCallback()
- HAL_SPI_RxCpltCallback()
- HAL_SPI_TxRxCpltCallback()
- HAL_SPI_TxHalfCpltCallback()
- HAL_SPI_RxHalfCpltCallback()
- HAL_SPI_TxRxHalfCpltCallback()
- HAL_SPI_ErrorCallback()
- HAL_SPI_GetState()
- HAL_SPI_GetError()



（1）声明 SPI_HandleTypeDef 的驱动结构体，例如

```c
SPI_HandleTypeDef  hspi;
```

（2）执行HAL_SPI_MspInit() 以初始化一些SPI的相关低级资源

- 使能SPI的GPIO端口的时钟
- 配置SPI引脚

  1. 使能SPI GPIO的时钟

  2. 配置SPI引脚的推挽复用功能
- 如果需要，配置NVIC中端配置

  1. 配置SPI中断优先级

  2. 使能NVIC SPI IRQ句柄
- 如果需要使用DMA流程，配置DMA
  1. 声明DMA_HandleTypeDef驱动结构，用以传输或者接收信息流
  2. 使能DMAx接口时钟调用
  3. 配置DMA驱动参数
  4. 配置DMA传输或者接收流
  5. 初始化hdma_tx或者hdma_rx驱动以使用hspi DMA的TX或RX
  6. 配置优先级并且使能NVIC为DMA RX或者DMA TX设置传输中断使能
- 配置在hspi的初始化结构体里模式、传输方向、数据大小、波特率分频值、NSS 管理、时钟极性和相位，数据首位和CRC校验。
- 通过唤起HAL_SPI_Init() 初始化SPI寄存器（这个API通过唤起HAL_SPI_MspInit() 来配置低级硬件设备如GPIO、CLOCK、CORTEX等等）

循环模式限制：

1. DMA循环模式无法在以下情况下被使用：当SPI被配置为主模式双线只收和主模式单线接受模式
2. 当DMA循环模式使能后CRC功能不会被管理
3. 当使用SPI DMA暂停功能/停止功能时，我们必须在SPI的回调函数中使用HAL_SPI_DMAPause()或 HAL_SPI_DMAStop()方法



### 初始化和取消初始化功能

本节提供了一组允许初始化和取消初始化的SPIx设备功能：

- 必须配置HAL_SPI_MspInit() 功能，在这个函数中配置所有相关的外围设备资源（时钟、GPIO、DMA、IT和NVIC）
- 调用HAL_SPI_Init()功能来使用选定的配置:模式、传输方向、数据大小、波特率分频值、NSS 管理、时钟极性和相位，数据首位、CRC校验和CRC多项式。
- 调用HAL_SPI_DeInit()功能来恢复所选的SPI外设的默认配置。

这一节总共包含以下API方法：

- HAL_SPI_Init()
- HAL_SPI_DeInit()
- HAL_SPI_MspInit()
- HAL_SPI_MspDeInit()

### IO操作功能

SPI接口提供主模式和从模式：

- 有两种传输模式：
  1. 阻塞模式：此时通信在轮训模式下进行，在第一次传输之后，所有数据的传输过程HAL状态会由相同的方法返回。 
  2. 非阻塞状态：通信以中断或DMA的形式表现，这些API方法会返回HAL状态。当使用中断模式或者DMA模式中开启DMA IRQ时，在数据传输过程的结尾会以SPI IRQ的形式指示。HAL_SPI_TxCpltCallback()、HAL_SPI_RxCpltCallback()和HAL_SPI_TxRxCpltCallback()等回调函数在传输过程或接收过程的结尾将会被分别执行。当通信错误被侦测到时，The HAL_SPI_ErrorCallback()回调将被执行。
- 为这两种传输模式（使用中断或DMA的阻塞模式或非阻塞模式）提供的API适用于1线（单工）和2线（全双工）模式。

这一节总共包含以下API方法：

- HAL_SPI_Transmit()
- HAL_SPI_Receive()
- HAL_SPI_TransmitReceive()
- HAL_SPI_Transmit_IT()
- HAL_SPI_Receive_IT()
- HAL_SPI_TransmitReceive_IT()
- HAL_SPI_Transmit_DMA()
- HAL_SPI_Receive_DMA()
- HAL_SPI_TransmitReceive_DMA()
- HAL_SPI_DMAPause()
- HAL_SPI_DMAResume()
- HAL_SPI_DMAStop()
- HAL_SPI_IRQHandler()
- HAL_SPI_TxCpltCallback()
- HAL_SPI_RxCpltCallback()
- HAL_SPI_TxRxCpltCallback()
- HAL_SPI_TxHalfCpltCallback()
- HAL_SPI_RxHalfCpltCallback()
- HAL_SPI_TxRxHalfCpltCallback()
- HAL_SPI_ErrorCallback()

### 外围设备状态和错误功能

这一节提供一些功能来控制SPI

HAL_SPI_GetState() 功能在运行状态下检查SPI外设的状态很有用。

HAL_SPI_GetError()  功能在运行状态下发生错误是很有用。

这一节总共包含以下API方法：

- HAL_SPI_GetState()
- HAL_SPI_GetError()
