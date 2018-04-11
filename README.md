# 使用STM32 Cube创建工程

所使用的芯片为STM32F334，开发板为NUCLEO-64pin。在模板选项里选择所用的板子，NUCLEO F334R8，并选择初始化默认的外设配置。

##芯片设置
除了基本的下载功能，前期只需要一个按键和LED灯作为测试用，所以可以将把USART的两个脚去掉。GPIO输出引脚PA5:LD2，外部中断引脚为PC13:PushButton。可以在Configuration中进一步配置。比如在GPIO配置成Output的情况下，能分为Push Pull和Open Drain，配置成EXIT时，能分为上升沿触发和下降沿触发等。这里我们默认即可。在时钟树里面设置72MHz的频率，即最大频率。

##工程设置
芯片外设和时钟配置完后，为了更加合适后续添加功能，我们还要在生成项目代码前做些具体的设置。在Project选项卡里选择配置编译环境为MDK-ARM，在Code Generation中Generated Files的第一个框勾上，这样能针对不同外设生成相应的.c和.h文件，可读性会强些。在最后一个选项卡中选择使用LL库，这样编译速度会快些。

#设置微秒定时器
使用CUBE的最方便的就仅是初始化，我们还是以点亮LD2和按键控制LD2为例开始进行。点亮LD2只需要一条语句，对GPIO的输出寄存器GPIOx_ODR进行相应的操作即可：GPIOA->ODR|=1<<5；//PA5输出高电平
GPIOA->ODR&=~(1<<5);//PA5输出低电平
这是对位操作的方法，以前用F4时老师就是这样用的。而所用的LL库里，它用的是BRR和BSRR寄存器


虽说你也可以在相应的USER CODE框内添加自己的代码，下次再CUBE里添加引脚，也不会覆盖，但是还是不宜多写。因为你越往后你的代
码量增加，会和初始化文件混合在一起。所以在工程树中添加一个文件夹《mylib》，里面放置自己调好的.c和.h文件，最后在根据所要测试的功能，编写USER.c和.h文件。


#配置滴答定时器
我们使用IO口模拟I2C,就是
