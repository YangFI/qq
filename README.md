此仓库用于“2020清华大学计算机考研”qq群（120277543）每日问题收集，使得群内高价值的问题得以长久保存。
提问册与解答册均以.txt格式文件给出。  
同学们提出的高价值问题，群主会整理相关同学的正确回答作为答案收录。希望大家多多踊跃提出在学习过程中遇到的问题；2.群主作为默认选项提出的问题均有标准答案给出（不过当天的问题次日才会整理上传github）。  
群主289212182每天会整理QQ群里高价值的问题进行收录，分别在早上8：00点，中午1：00点，下午6：00点，对各位同学的优秀问题进行二次索引以方便所有同学可以及时看到（不至于被其他发言水屏）。如果没有值得索引的同学提问，则群主在上述时间点会（作为默认选项）提出1个912问题供大家讨论。群主的体温范围仅限于[注1]。


注[1]：  
912参考教材注[2]、课堂ppt、教材课后题、实验指导书、期中期末试卷、王道天勤练习、912真题。

注[2]：912参考教材：  
1.数据结构：《数据结构(第三版)》（邓俊辉老师）；  
2.计算机组成原理：《计算机组成与设计——软硬件接口（第五版）》；  
3.操作系统：《操作系统概念第七版》、《操作系统精髓与设计》、《ucore实验指导书》；  
4.计算机网络：《计算机网络——自顶向下(第七版)》  


![avatar](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=347139529,1934829869&fm=200&gp=0.jpg)

## 0001【4月5日◆】简答题——by：20-912只聊学习(3272662938) && nullptr
## 问题：各位大佬，页目录表中保存的二级页表地址是线性地址还是虚拟地址呢？

讨论1：物理地址（——by：Crowerly）

讨论2：[pic]这是 entry.s 的代码；这是系统头一次初始化页目录表和二级页表的地方；[pic]这是我在pmm_init中的测试 ；最新的代码对于entry.s有所修改 但是基本一样；@nullptr ；二级页表的物理地址基本是通过循环迭代算出来的；那个_boot_pt1 就是代表了一个二级页表的地址；抱歉 是我的问题 他调用了realloc 就是物理地址（——by：&你的心link我的心）

讨论3：@&你的心link我的心  同学 页目录如果存的是虚拟地址，那还怎么定位二级页表的页啊（——by：Crowerly）

讨论4★：“CPU可以看到的地址都是虚拟地址，经过MMU后才会有物理地址。@Crowerly 定位二级页表的事由MMU做。”（——by：向老师）

讨论5：@向老师 老师 页表就是用来给MMU做地址翻译的啊，如果一级页表里头存的是虚拟地址，MMU就无法定位二级页表的位置了吧；我再回去确认一下吧；（——by：Crowerly）

讨论6：其实 你们说的就是对的；代码是通过realloc一个简单的减法；实现了物理地址和虚拟地址的转换；老师的意思是强调CPU拿到的地址是虚拟地址；我们讨论的地址是mmu看到的地址；页表也是由mmu来实现的；不冲突；我声明我观点的错误 因为没有看到realloc；页目录表和二级页表都是存储的物理地址包括cr3 代码中均有体现；所站视角不同；（——by：&你的心link我的心）

讨论7：@某普通大学的数学汪  仔细捋了捋代码，确实是物理地址，书上也写了是物理地址（——by：nullptr）

讨论8★：也就是说根据线性地址生成最终物理地址的工作由MMU完成。cpu拿到的第一手地址都是虚拟地址，由分段机制生成线性地址，若不启动分页则线性地址就是最终物理地址，若启用分页机制则再对线性地址进行加工。 所以页表的地址是物理地址还是虚拟地址哇？（——by：群主289212182）

讨论9：cr3有页目录表物理地址，然后查找到存放的页表物理地址，然后从页表查到物理页号和offset拼起来，应该是这样吧?（——by：null）

讨论10★：我突然想起来一件事情；咱们访问必须是32位地址。页目录项给的所谓"物理地址"是这种可以直接拿来不做任何变化访存的地址吗？；如果不是，mmu必须对它进行变换，那所谓的"物理地址"也就不是真正的物理地址鸭？；能这样理解吗？轻喷。（——by：群主289212182）



讨论11：页目录存的是物理页框号；确实不是真的物理地址；真的物理地址需要做12位左移位。（——by：Crowerly）

讨论12★：是吧？我觉得所有的访存工作都是mmu在做，cpu并不知情，它俩谁也不认识谁；个人粗俗理解（——by：群主289212182）

讨论13：...（若干）

讨论14★：“我注意到大家对虚拟地址和物理地址的讨论。这是操作系统课的重要概念，需要仔细理解代码才能有准确的了解。有必要进行深入的讨论。
我先说说我的理解。
1. 在保护模式下CPU可以看到的地址都是虚拟地址，经过MMU后才会有物理地址。定位二级页表的事由MMU做。所以，CPU不能直接用物理地址来访问内存，而必须使用虚拟地址来访问。这时才有，CPU要修改页表项内容时，也是通过虚拟地址来访问的。
2. 在X86-32 CPU上，物理地址可能不是32位的。如在使用物理地址扩展（PAE）时，物理地址会是36位，使用4KB页面大小时物理页号也就变成了24位，于是一个页表项就占了8字节。
3. CR3寄存器中保存的是页目录的起始物理地址，CPU只在地址转换中使用它的内容。
请同学们继续置疑和修正描述，并补充相关的代码例证。希望有同学来整理大家的交流结果，并放到Piazza上，以方便以后的同学。谁有兴趣来做此事？”（——by：向老师）




## 0002【4月6日◆3/3】简答题——by：群主289212182
## 问题：KMP算法对蛮力算法的优势在什么条件足够明显？为什么？

（出处2019年912真题 数据结构 简答题第8小题）  
有思路的同学在一条消息中给出答案，其他勿刷屏




## 0003【4月7日◆1/3】简答题——by：群主289212182
## 问题：一个被系统认为符合规范的硬盘主引导扇区的特征是什么？

（问题出处：操作系统ucore实验指导书LAB1_练习1_第二小问  pdf版p91）  
有思路的同学在一条消息中给出答案，其他勿刷屏
