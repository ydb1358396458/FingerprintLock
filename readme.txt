基于51单片机（STC89C52）和指纹识别模块（AS608）的指纹锁项目（V2.0）的全部软硬件资料
This is the second version of a fingerprint lock project based on 51-MCU and AS608, which can be installed on most doors without any conflict.

更新说明：  
1、V1.0版本虽然能正常工作，但开锁和关锁的时间需要在程序中预先写好，对于实际应用可能需要反复修改并烧写以得到合适的时间数据，这不仅极不方便，也无法进行批量的生产使用。为了使本项目进一步产品化，另加了E2PROM(AT24C02)以存储开锁和关锁的时间，程序烧写好了以后，用户可以通过按键修改时间，并写入E2PROM保证掉电不丢失；  
2、V1.0版本液晶背光灯为长开状态，耗电较为严重，V2.0版本通过单稳态按钮开关控制背光灯，按下按钮，背光灯亮，松开即灭；  
3、V1.0版本使用洞洞板和导线进行设计，不适应批量生产，电路整体美观性也较差，V2.0版本使用Altium Designer进行PCB设计，PCB板生产出来后只需要进行元件的焊接；  

文件（夹）说明：
1、AltiumDesignerProject：这是Altium Designer软件绘制的电路原理图和PCB图，其中含有一个FPL.pdf文件，可以预览；
2、Datasheet：内含两个PDF文件和两个文件夹。L298N芯片为电机驱动芯片，由于单片机驱动能力有限，L298N芯片可以增大驱动能力，为减速电机提供足够的电流；STC89C52为宏晶科技公司生产的单片机芯片，本项目中用作主控芯片；AS608文件夹内有两个PDF文件，介绍AS608模块与单片机的通讯方式；AT24C04为AT公司生产的E2PROM芯片，保证数据掉电不丢失；
3、Images：这是一些实物图展示；
4、KeilPproject:这是用keil uvision3软件开发的指纹锁软件项目；
5、Manifest：这是制作整个电路系统所需的元件清单，是用Excel写的*.xlsx格式；
6、SCH&PCB：这是从AltiumDesignerProject文件夹下copy过来的原理图和PCB图；
7、Instruction:其中包含一个框图文件，演示系统使用方式；  

使用说明：
液晶显示主界面有3个功能：search finger（搜索指纹），add（添加指纹），delete（删除全部指纹），还有一个星号表示当前预选中的功能；
单片机P2口连接了4个按钮开关：分别为KEY_Mode=P2^3;KEY_DOWN=P2^2;KEY_OK=P2^1;KEY_CANCEL=P2^0;
1、添加指纹：按KEY_DOWN键，将星号调到“Add”前，按KEY_OK键，屏幕中将显示指纹即将存入的ID号，如果你希望使用该ID号，按KEY_OK键，否则按KEY_DOWN切换ID号，再按KEY_OK键，此后你可以将手指放到指纹识别窗口上，指纹将被读入两次，每读取成功一次蜂鸣器会响一声，两次读取成功后，ID号自动切换到下一个，你可以继续录入指纹或者按KEY_CANCEL键取消；
2、搜索指纹：要执行开锁操作时，按KEY_OK键（为了能在门外操作，指纹识别模块上另有一个按钮开关通过小孔和门内的KEY_OK键并联），将手指放到指纹识别窗口上，若识别成功，单片机将控制减速电机开锁，同时蜂鸣器响1声，若识别失败，蜂鸣器响3声，你可以将手指拿开再重新放上去，将会被自动识别；
3、删除指纹：按KEY_DOWN键，将星号调到“delete”前，按KEY_OK键，屏幕将显示询问是否执行删除操作，按KEY_OK键确认删除，按KEY_CANCEL键取消；
4、在主界面，如果按下KEY_Mode键，进入时间调整模式，此时按KEY_DOWN键可以增加时间，按KEY_CANCEL键可以减小时间，时间调整方式为按位修改，你可以修改开锁时间（OpenTime）和关锁时间（CloseTime）的千位和百位（单位ms），按KEY_OK更改你要调整的位，修改完成后再按一次KEY_Mode键结束修改，回到主界面，这时按一下复位键可使修改生效；
补充：
1、如果当前指纹模块内存储了至少一个指纹模板，你在执行添加指纹、删除指纹或更改时间操作时将需要“主人”授权。例如，我已经添加了一个指纹，ID号为000，现在需要再添加一个指纹，需要按KEY_DOWN键，将星号调到“Add”前，按KEY_OK键，这时需要匹配指纹，我需要将000号指纹放到指纹模块上，若识别成功，则可以执行后续操作添加指纹；
2、在执行任何操作时可以按KEY_CANCEL键退回到主界面；
3、本电路系统使用较为简单，但如果要彻底熟悉相关操作建议查看源代码；

如果你要使用本方案制作一个指纹锁，只需要按以下步骤操作：
1、将PCB文件交付相关的生产厂家进行PCB板制作，得到PCB板后将元件焊接到PCB板上，如果你不想使用PCB板制作本项目，也可以参考我提供的PCB原理图和Layout板图自己用洞洞板制作一个；
2、直接使用stc-isp软件将*.hex文件下载到单片机中（或者你也可以用keil uvision3软件打开上面的keil_project项目抑或使用源码自行创建项目编译得到*.hex文件）；
3、将AS608指纹模块连接到单片机相应管脚上，将L298N驱动芯片输出管脚与减速电机相连，减速电机通过绳子牵引门把手开锁；
补充：
1、程序下载时只需连接VCC,GNC,TX,RX四个管脚；
2、指纹识别模块VCC接3.3V，GND与单片机共地，AS608的TX接单片机RX，AS608的RX接单片机TX；

补充说明：
1、本项目中供电方式为使用锂电池向电路系统供电，同时通过充电器向电池充电，也就是说电池处于边充边放的状态。这样可以保证在家庭电路供电异常的情况下，指纹锁仍能工作一段时间。如果你认为这种情况发生的概率极低，同时希望降低制作成本，可以不使用锂电池，而通过DC接口的充电器直接给电路供电；
2、普通电机所能输出的力矩有限，本系统中采用了减速电机，增大输出力矩，并在机械锁上连接省力杠杆，增加驱动力。两者之间使用绳子软连接。实际应用时可根据机械锁开锁方式及难易程度进行相应调整；
3、到目前为止，本项目已实际使用5月有余（2018.3.18~2018.8.25）。依实际使用来看，电路在绝大多数时候能够正常工作，但V1.0版本也存在因接触故障而不能开锁的情况，V2.0版本去除了所有杜邦线，连接较为稳定。但我仍然建议不要完全依赖于电子锁，出门还是要带上钥匙（虽然大多数情况下你都不用拿出来），由于本项目的实现方式，机械锁原来的功能丝毫不受影响；
4、使用指纹解锁的主要优势是便捷，但不可避免的会降低安全性（如果有“不速之客”，将只需要破解机械锁或指纹锁任意一样即可）。依照指纹模块用户手册相关数据，该指纹识别模块的认假率低于0.001%,拒真率低于1%，这种概率是可以被接受的。但同时实测发现，拒真率要大于1%，分析原因可能是因为录入指纹模板时每个ID号只录了两次，再次识别时存在角度、力度、部位等变量影响，导致识别率下降。解决方案是将多个ID号录入同一个指纹（AS608模块可以录入300枚指纹），录入时使用不同的角度、力度、部位，尽可能录入更多信息。假如实际拒真率高达10%，而你将同一枚指纹录入了10次，并假设是否识别失败是独立随机的，那么拒真率将下降为百亿分之一，这是一个极小的概率。当然，如果你的指纹受损（例如沾了水等），识别失败将不能认为是拒真，你将始终难以识别成功。
