# ⚠️ 安全提示
**注意! 请勿轻易尝试维修AC-DC(交流转直流)供电电路, 其往往存在远超过人体安全电压(36V)数倍的高强度电压. 如果你没有专业的维修知识, 这有可能会将你置于危险之中!**

**请勿尝试拆开交流电源适配器, 交流电源模块等. 因为其内部工作时会产生高达380V甚至更高的高压, 甚至断电后可能依然存在于滤波电容当中. 意外触电轻则会导致严重疼痛, 重则会引发生命危险**

**在这里我们只讨论如何维修DC-DC(直流转直流)的供电电路, 且确定工作电压均在人体安全电压内.**

**如果你不确定你的电子设备是否存在高压电路, 请务必使用搜索引擎搜索相关安全注意事项. 如你的设备贴有 "高压危险" 的安全警示标志, 请务必三思而后行, 尽可能的交给专业人员进行维修.**


# 遇事不决查查供电
查供电非常简单. **万用表打开电压档, 黑表笔接地(电源负), 红表笔戳你想要查的位置**就好了.

如果你观察专职的维修师傅, 你会发现他们都会在正式动手之前先通电测试, 之后一路一路查一下PCB板上的所有供电电路是否正常工作.

或许你可能会觉得这完全是个多余且不相关的操作.

而之所以维修各种电子设备要先查一下供电, 其真正的意义是这样的:

    1. 正确的供电是几乎所有电子元器件能够工作的必须条件. 供电不正常, 一切都免谈;
    2. 检查供电电路是最直接的入手点, 甚至不需要原理图, 通过简单目测就能找出供电电路;
    3. 常见故障(如元器件击穿烧毁, 工作不稳定, 短路故障等)一般均可在对应的供电电路部分体现出异常;
    4. 供电电路很好查, 不需要太多的设备.

这里第一点和第四点显而易见. 而第二点与第三点, 这里会详细说明其操作方式与内在原理.


# 找出供电电路
除了根据原理图找到供电电路, 我们也可以直接目测找到供电电路.

若要目测找到供电电路, 我们就需要了解供电电路的具体结构.

生活中常见的电子设备, 其DC-DC供电电路相关的结构无非是**电荷泵升压**, **buck式供电**; **ldo线性降压**等等. 

    - 对于buck式工作电路, 其原理是**产生pwm信号, 通过pwm的方式降压**. 这就好比你不断地闪烁一个灯泡, 利用其余晖来控制不同的亮度一样. 其特征是往往存在一个大**电感**.
    - 对于电荷泵升压电路, 其工作时往往会生成比输入电压更高的电压. 它的工作原理就像是个小水泵, 把水从低处抽到高处. 其特征往往是存在一个大**电容**.
    - 对于ldo线性降压电路, 其工作时就像是一个大号电阻丝, 帮你把多余的电压自身吃掉(你可以单纯的理解为是通过自身产生热量的方式浪费掉). 其特征是电流过大时自身会严重发热, 因此不能用于承载太大的电流.

而不管什么样的供电电路, 你可能都会在其供电的输入和输出部分发现很多一端是电源, 一端是地的小**电容**. 这样的电容是**滤波电容**, 是保证供电纹波稳定的重要组成部分.

最常见的**滤波电容**, 就是各种芯片背后贴满的那些小颗粒. 其容值大部分为100nF (因经验公式产生).

综上所述, 通过目测**电感**和**电容**的排列, 以及观察芯片上的**丝印型号**, 很快就能找到什么地方是负责供电的电路. 此外, 供电部分就像人体的主动脉, 往往会有着**更粗的线路**. 这也是一个很好的观察点.


# 直接测量供电电路
假如设备没有明显的烧毁痕迹, 在**保证安全**的情况下, 可以直接给设备正常上电. 上电后, 仔细使用**万用表电压档**测量每一组供电电路的输入和输出.

常见的输入和输出电压有12V, 5V, 3V, 1.8V, 1.2V等等, 你需要视情况而判断其电压的数值是否正常.

通常, 你可以根据供电电路背后的负载元器件的说明书手册来确定正确的电压应当是什么. 当然也可以凭借经验, 也可以找到一个正常工作的同款线路板进行比较式测量.

你可能会遇到如下情况:

    - 测量结果显示工作完全正常, 则这一路供电电路不需要做任何处理.
    - 测量结果显示输出电压较低, 请检查电源芯片的**反馈电路**是否正常. 若正常, 请按照**短路**进行排查.
    - 测量结果显示输出电压较高, 请检查电源芯片的**反馈电路**是否正常. 同时需预备好该路电源后面的负载芯片存在大规模高压烧毁的最坏情况.
    - 测量结果显示输出无电压. 请检查电源芯片的**工作条件**是否满足, 如电源芯片是否正确接收到启动信号.
    - 测量结果异常的情况下, 若排查周边器件无法确定故障点, 则请果断考虑更换新的电源芯片.

同时如果你的输入电源具备电流表的功能, 请你注意电流表的示数. 该示数可根据与正常工作的同款线路板进行对比, 或者根据经验进行判断, 从而确定电路的工作状态:

    - 若电流表示数明显异常, 数值较大, 考虑按照**短路**进行排查.
    - 若电流表示数极低或无示数, 则考虑该设备无法正常上电, 请检查电源芯片是否能够正常工作, 以及是否有可能的线路断线存在.


# 进一步确诊
假如供电电路存在**明显短路**现象(电源无法正常输出; 有元器件异常发热; 上电即保护; 电流表示数明显异常等); 或者你想顺手查一下:

则请你将**万用表设定到电阻档**, 按照如下的方式排查.

**断开被测设备的电源**, 使用**红表笔接地, 黑表笔去探测需要的位置**. 你可以**反复调整电阻档的量程, 使用不同的量程测量同一个点**, 从而确定其示数的大概范围.

这个示数**没什么意义, 无需纠结其数字**, 你只需要判断其相对正常的情况**高了还是低了**即可.

你可能会遇到如下情况:

    - 示数相比同款正常的PCB线路板没有太大区别, 则大概率是正常情况, 不需要做任何处理.
    - 示数无论任何档位均直接显示0或数字极小. 则考虑你所测量的位置直接存在严重短路. 请根据负载进一步判断. 假若负载不是一个功耗很大的设备(如CPU或显卡的核心供电, 阻值一般只有个位数是正常的), 则可直接确诊短路.
    - 示数无穷大, 万用表戳上去没任何反应. 则请调万用表的高量程档位, 并用万用表的笔尖打磨一下被测位置, 以防误测. 若依旧没有任何反应, 或测到的示数非常大, 则考虑这里可能存在断线.

其实, 这里所述的**阻值法**测量方式也**同样适用于供电电路以外的电路**.

不过值得一提的是, 若供电电路存在大短路, 则因为其后面所挂载的元器件可能较多, 你无法简单的通过判断来确定到底是什么东西短路.

那么请休息一下, 接下来马上就会讲解如何解决供电电路上存在的对地大短.
