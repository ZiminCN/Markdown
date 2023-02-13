# 嵌入式软件工作记录

lastest 

以HAL库（硬件抽象层库）为主

## GPIO方面

PullUp与PullDown主要在InputPin上有体现，当引脚没有输出信号时，PullUp会使引脚自动拉高、PullDown会使引脚自动拉低，如果不做这个设置，当没有输入引脚没有输入信号时，引脚将悬空，此时电平是未知的。

开漏与推挽的一个区别是，开漏只能输出低电平，如果需要输出高电平则需要外部电路的上拉电阻，而不需要IC电路内部去驱使它拉高；推挽高低电平都能输出，依赖于IC电路内部去驱使它拉高拉低。对比来看，推挽的电平转换响应速度一定更快，但是会增加IC的功耗；开漏除了功耗比推挽低，还具有“线与”的功能。



## TIM定时器

htim.Init.Period可以设置一个duty的总时间

htim.Init.Prescaler表示一个duty的占空比，其中定时器中断也需要通过Prescaler来设置



## 中断问题









模拟IO引脚输入输出

```
/对应波特率的1个电平持续时间
//(1/2400) = 416us
#define IO_USART_SENDDELAY_TIME 	416
 
void uart_tx_bit(uint8_t bit) {
	if (bit == 1) {
		GPIO_SetBits(GPIOD, GPIO_Pin_12);
	} else {
		GPIO_ResetBits(GPIOD, GPIO_Pin_12);
	}
}
 
void uart_tx_delayus(uint32_t nTime) {
	uint16_t tmp;
	//获得 TIM4 计数器的值
	tmp = TIM_GetCounter(TIM4);
	if (tmp + nTime <= 65535)
		while ((TIM_GetCounter(TIM4) - tmp) < nTime);
	else {
		TIM_SetCounter(TIM4, 0);
		//设置 TIM4 计数器寄存器值为0
		while (TIM_GetCounter(TIM4) < nTime);
	}
}
 
void uart_tx_byte(uint8_t data) {
	uint8_t i, tmp;
 
	// 开始位
	uart_tx_bit(0);	//将TXD的引脚的电平置低
	uart_tx_delayus(IO_USART_SENDDELAY_TIME);
 
	for (i = 0; i < 8; i++) {
		tmp = (data >> i) & 0x01;
		if (tmp == 0) {
			uart_tx_bit(0);
			uart_tx_delayus(IO_USART_SENDDELAY_TIME);	//0
		} else {
			uart_tx_bit(1);
			uart_tx_delayus(IO_USART_SENDDELAY_TIME);	//1
		}
	}
	// 结束位
	uart_tx_bit(1);	//将TXD的引脚的电平置高
	uart_tx_delayus(IO_USART_SENDDELAY_TIME);
}
 
static void hw_init(void) {
	GPIO_InitTypeDef GPIO_InitStructure;
	TIM_TimeBaseInitTypeDef TIM_TimeBaseStruct;
 
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOD, ENABLE);
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_OUT;
	GPIO_InitStructure.GPIO_OType = GPIO_OType_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_2MHz;
	GPIO_InitStructure.GPIO_PuPd = GPIO_PuPd_UP;
	GPIO_Init(GPIOD, &GPIO_InitStructure);
	GPIO_SetBits(GPIOD, GPIO_Pin_12);
 
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM4, ENABLE);
	TIM_TimeBaseStruct.TIM_Period = 65535 - 1;
	TIM_TimeBaseStruct.TIM_Prescaler = 84 - 1;  
	TIM_TimeBaseStruct.TIM_ClockDivision = TIM_CKD_DIV1;
	TIM_TimeBaseStruct.TIM_CounterMode = TIM_CounterMode_Up;
	TIM_TimeBaseInit(TIM4, &TIM_TimeBaseStruct);
	TIM_Cmd(TIM4, ENABLE);
}
 
void uart_tx_init(void) {
	hw_init();
}
```

