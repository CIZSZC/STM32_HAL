# STM32_HAL
记录了STM32的HAL库开发，一些工程，希望对你有用

使用的芯片：STM32F103C8T6

1.gpio_out
描述：
GPIO输出测试
功能：
1.GPIOA1和GPIOA2闪烁

2.gpio_in_where
描述：
GPIO输入测试，轮询检查电平
功能：
1.GPIOA3上拉输入，where循环一直轮询检测
2.当检测到低电平时将点亮GPIOA1和GPIOA2（输出高电平）
3.当检测到高电平时熄灭GPIOA1和GPIOA2（输出低电平）

3.gpio_in_interrupt
描述：
GPIO输入测试，中断
功能：
1.GPIOA3上拉输入，下降沿中断触发
2.中断触发取反GPIOA1和GPIOA2的输出电平

4.usart_recv_where
描述：串口1延时循环接收和发送
功能：
主循环一直查询串口1的数据接收，接收到了就发送
存在问题：
主循环有其它阻塞任务时将很影响数据接收

5.usart_recv_interrupt_1
描述：串口1库函数中断接收和延时发送
功能：
串口逐个字节接收，然后调用发送函数发送
存在问题：
可能会卡死中断接收，当外面向STM32发送数据量过大够快时，而STM32也一直在发送数据时，很容易造成中断开启失败，导致一直都不能接收数据。
通常做法是在主循环判断接收中断标志位并再次开启

6.usart_recv_interrupt_2
描述：串口1修改版中断接收，可以有效的解决接收卡死问题
功能：
串口逐个字节接收，然后调用发送函数发送
说明：
已经经过了项目测试，这个很稳定

7.usart_recv_interrupt_rx_dma_tx
描述：串口1库函数中断接收和DMA发送
说明：
虽然使用的是库函数中断数据接收，但是暂时未发现卡死问题

8.usart_dma_tx_circular_mode
描述：DMA发送，循环搬运模式测试
功能：
DMA将一直向串口发送UART1_DMA_TX_BUF数组的内容，改变数组即可改变发送的内容，my_uart1_send（）发送函数只需要调用一次即可。主循环里1秒改变一次数组的首元素内容。
 
9.usart_dma_tx_rx
描述：实现DMA数据收发，接收部分实现环形缓冲区效果
注意：主循环使用DMA数据发送，容易卡死

10.usart_dma_tx_rx
描述：实现DMA数据收发，接收部分实现了标准的逐字节接收方式
说明：收发未出现卡死现象

