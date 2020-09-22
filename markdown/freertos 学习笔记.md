# freertos 学习笔记

备注：均以 stm32F4 (contex - M4)为例；

### 1、临界区

### 2、disable interrupt

### 3、suspend task

### 4、VFP

### 5、FPU

### 6、SVC



https://www.jianshu.com/p/52841b514868

​	system service call

​	首次进入任务时调用了SVC异常，且只使用了一次。

### 7、pendSV

​	PendSV是可悬起异常，如果我们把它配置最低优先级，那么如果同时有多个异常被触发，它会在其他异常执行完毕后再执行，而且任何异常都可以中断它。

​	PendSV的典型使用场合是在上下文切换时（在不同任务之间切换）。例如，一个系统中有两个就绪的任务，上下文切换被触发的场合可以是：

1. 执行一个系统调用
2. 系统滴答定时器（SYSTICK）中断，（轮转调度中需要）

### 8、taskYIELD

### 9、MSP & PSP

“主堆栈指针MSP”和“进程堆栈指针PSP”，对于Cortex-M3硬件，当系统复位后，默认使用MSP指针。MSP指针用于操作系统内核以及处理异常（也就是说中断服务程序中默认强制使用MSP指针，这是硬件自动设置的）。任务（进程）使用PSP指针，操作系统负责从MSP指针切换到PSP指针。这个过程在《FreeRTOS高级篇3---启动调度器》一文的最后部分中进行了讲解：在SVC中断服务程序中启动第一个任务，当从SVC中断服务退出前，通过向r14寄存器最后4位按位或上0x0D，使得硬件在退出时使用进程堆栈指针PSP完成出栈操作并返回后进入线程模式、返回Thumb状态。

### 10、任务调度

#### 10.1、抢占式调度

​		任务可以有不同的优先级，任务会一直运行知道被更高优先级的任务抢占或者是主动进入阻塞;

#### 10.2、非抢占式（时间片调度）

​		每个任务有相同的优先级，任务会运行固定的时间片或者是主动进入阻塞状态，才会执行同优先级之间的任务调度；

​		8128 上面，抢占式和时间片调度都有开启，如果线程优先级相同，采用时间片调度；如果不同，采用抢占式调度；

### 11、CLI 系统

​	功能分为三个模块：

​	1、创建一个链表（list item），里面记录了每一个command 的cmd 字符串，参数个数，执行函数等等信息；由application 中task 去注册；（rtos/cli/FreeRtos_CLI.c）

​	2、从终端获取cmd，其实就是调用getc() api，以”\r“或 ”\n“ 作为结束符；（rtos/console/console_init.c）

​	3、解析cmd 字符串，获取参数个数（以” “作为分割），找到相应的执行函数执行

​	4、task 指令解析：

​		4.1：”task“ 指令的注册于执行函数在application/commands/task.c 中；



### 12、SysTick (M4)

![1563348215894](C:/Users/yuki/Desktop/pc/freertos%20%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/1563348215894.png)



### 13、tickless 模式

需要设置宏 `configUSE_TICKLESS_IDLE` = 1

调用顺序：

```
portTASK_FUNCTION ->
portSUPPRESS_TICKS_AND_SLEEP ->
vPortSuppressTicksAndSleep ->
vTaskStepTick
```

#### portTASK_FUNCTION:

​	freertos 提供的 IDLE task，在启用线程调度（vTaskStartScheduler）时创建；

​	当os 中没有其他更高优先级的task 运行时，会调用此task；

​	1、判断此时是否有自我销毁的线程，如果有，回收资源；

​	2、判断xExpectedIdleTime 是否大于宏`configEXPECTED_IDLE_TIME_BEFORE_SLEEP`（即系统下一次task 的调度时间是否大于预设进入tickless 模式的时间）；如果是，则进入该模式；

#### vPortSuppressTicksAndSleep

​	1、根据进入tickless 的时长，重新设置system tick timer;

​	2、判断此时是否会有调度出现，如果有，则不进入tickless 模式；

​	2、进入wfi 模式；直到有中断到来；

​	正常情况下：freertos 设置了system tick timer，其reload 值为`CPU_HZ/rate_hz`，即一个tick 的timer 的count 的值；理论上，timer 没触发一次中断，则将全局的tickcount ++；时间也过了`1/rate_hz` 秒；进入tickless  模式时，会重设 system tick timer 的值，reload 值为期待唤醒的值（当然不会超过最大值，所以tickless 最大时长是有限制的）；当没有其他中断触发时，system tick timer 会唤醒系统，当然此时不会去调用system timer handlder 去使tickcount ++；

#### vTaskStepTick

​	因为在tickless 模式下，tickcount 没有变化，所以需要在系统唤醒之后将其补偿回去；