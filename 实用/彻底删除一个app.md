> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_71246590/article/details/139046741)

写在前面：超级难搞，sxf...！教程来源于网络的五湖四海＋[gpt](https://so.csdn.net/so/search?q=gpt&spm=1001.2101.3001.7020) 解答。用到了好几个教程和方法，遂这篇总结一下。会标明出处！我的方法会有一些麻烦，你们可以先收藏着，先试一下别人，搞不定再来看我的方法。

先上一下战绩：

  ![](https://i-blog.csdnimg.cn/blog_migrate/84a6798c91548c677601b266354a216a.png)

![](https://i-blog.csdnimg.cn/blog_migrate/dbc5ee84473ef1a8c86e29248344da5b.png)

终于把 sangfor 扔进回收站里了！！！哈哈哈那一刻真的好爽

**目录**

[一、问题来源：easyconnect 这个流氓软件！文件绑定系统进程](#t0)

[二、真正解决方法：](#t1)

[三、关于残留文件的删除](#t2)

[四、删除 sangfor 后网络坏了、某些 app 无法正常启动怎么解决？](#t3)

一、问题来源：easyconnect 这个流氓软件！文件绑定系统进程
----------------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/7d99c4a19cb3a4e8ec392821480a3788.png)

打开****任务管理器****，****进程****里能看到两个 easyconnect 开头的在活跃，

选中 -> 右键 -> 打开文件所在位置

找到路径：C:\Program Files (x86)\Sangfor\SSL 

这时候**把进程里跟 easyconnect 有关的******结束任务。再删除**** Sangfor 文件夹, 还是显示该文件已被打开。只要把相关进程都关闭掉，就可以删掉 sangfor 文件。但是我们找不到是哪个进程占用着这个文件啊啊？？？

于是，我们要找到哪个进程在用这个文件，方法：

> ****资源监视器****：
> 
> 1. 按 ****Win + R**** 键，输入 ****resmon**** 并按回车键，打开资源监视器。
> 
> 2. 在资源监视器中，点击 **“CPU” 选项卡**。
> 
> 在右侧的 “关联的句柄” 栏中，输入文件的名称（或部分名称）进行搜索。（这里把 sangfor 的路径复制过去, 右键文件属性可以找到路径）**C:\Program Files (x86)\Sangfor**
> 
> 3. 这时候资源监视器会显示哪些进程正在使用该文件。

你会看到这个进程叫做 explorer.exe。！！**千万不要结束这个进程！！**（因为我结束过，桌面崩了，重启就行。）

这时候，为什么 sangfor 如此难以删除，**原因**找到了：

    **流氓的 sangfor 跟系统本身的进程绑定在一起了！！！所以才难以删除**

 ****explorer.exe**** 是 **Windows 操作系统****中的一个重要进程**，主要负责提供图形用户界面（GUI） 负责以下几个关键功能：****文件资源管理器****：这是用户与文件系统交互的主要工具。通过文件资源管理器，你可以浏览、复制、移动、重命名和删除文件和文件夹；****桌面环境****：****explorer.exe**** 也负责显示桌面图标、任务栏和开始菜单。如果这个进程被终止，桌面图标和任务栏会消失；​​​​​​****任务栏和开始菜单****：任务栏和开始菜单是 Windows 桌面环境的一部分，用于启动程序、查看时间和系统状态，以及切换打开的应用程序窗口；

二、真正解决方法：
---------

**方法 1：**

> ******1. 任务管理器（Ctrl+Shift+Esc）-> 服务******![](https://i-blog.csdnimg.cn/blog_migrate/b56accdcbb7e8c4a88664fe76d8fc0be.png)

> **2. 随便选中一个服务 -> 右键选择打开服务**
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/b394e70cf30c090f86f48e085ffa12e7.png)

> 3. 这时候找到两个 sangfor 开头的服务。
> 
> **选中 -> 右键 -> 属性**
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/3cce5ee8b920311a4b5837ed3fd90508.png)
> 
> 把**启动类型**搞成**手动**（两个服务都要搞哦）
> 
> **然后重启，再尝试上述的删除，不行的话，就接着按照方法 2。**

**方法 2. 使用安全模式**

****2.1 进入安全模式****：

> 1. 按 ****Win + R**** 键，输入 ****msconfig**** 并按回车键，打开系统配置实用程序。
> 
> 2. 在 “引导” 选项卡中，勾选“安全引导”，然后选择“最小”。
> 
> 3. 重启计算机，系统会进入安全模式。

这时候你的操作系统会看起来有点诡异，图标很大，壁纸黑色，别担心，这都是可以恢复的！！ 

****2.2 删除流氓软件****

> 1. 在安全模式下，流氓软件不会启动或绑定到 ****explorer.exe****。
> 
> 2. 尝试删除相关文件和目录，或者通过控制面板的 “程序和功能” 卸载流氓软件。
> 
> 3. 就是这一刻！Sangfor 被扔进回收站哈哈哈！！

****2.3 恢复正常模式****：

> 完成文件删除后，再次打开 ****msconfig****，取消 “安全引导” 的勾选，然后重启计算机。

三、关于残留文件的删除
-----------

这时候，服务里还是有两个 sangfor 开头的服务存在。

> **用管理员权限下**的 **cmd** 采用代码：
> 
> ```
> ​​​​​​​sc delete SangforPWEx 
> 
> ```
> 
> 删除的这个服务。// **另一个同理删掉。**

       这时候，我发现电脑上已经有一些 app 开始出问题了，出了网络问题或者闪退问题，这个问题真的让我沮丧很久，但是我还是决定先把 sangfor 有关的删掉。顺便说一下，这里即使重装别的 app 也没用哦。

**解决方法：**

（对了，我是在把所有残留文件删完后才用这个操作的。你们不急的话可以先看下文章后面内容）

> **管理员权限 cmd 执行命令：**
> 
> ```
> netsh winsock reset
> 
> ```
> 
>  **重启计算机，完工。**

[https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235^v43^pc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11](https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235%5Ev43%5Epc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11 "https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235^v43^pc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11")

感谢 csdn 上的这位老哥，你们可以点进去看一下会出现这样问题原因。

    **  此时！！大框架已经删完了，但是很坏很坏的 sangfor 还有很多残留文件留在你的电脑里。并且我发现这些文件还会一直繁殖！一直有新日期的 sangfor 文件出现。（这让我下决心把残留的都清掉）**

**下载一个软件 Everything**

![](https://i-blog.csdnimg.cn/blog_migrate/b739e69cc4855e78a0be56aa2884434b.png)

搜索 sangfor，发现有很多文件，一个个右键删除。

**此时的删除操作又会遇到各种问题。**

我参考了这几个解决方法，轮流用。我当时没截图，已经有点忘记怎么删的了（糟糕）... 这里杂糅使用了多个方法，我只能把我当时用到的都说出来，使用顺序已经忘了啊啊啊因为真的很多，你们多试试，能删掉的！！

对了，这个过程中，好像后面哪一步我**还把连接着的 wifi 都忘记掉**，**重启**，**在没网的环境操作**。

**用到的方法 1**：

> 按照这篇文章**后面**的**修改权限的方法**已经把 sangfor 有关的删掉得 7788
> 
> [删除流氓软件 easyconnect sangfor_sangfor 怎么彻底删除 - CSDN 博客](https://blog.csdn.net/Oblivion_L/article/details/133278904 "删除流氓软件easyconnect sangfor_sangfor怎么彻底删除-CSDN博客")

方法 2：

> 把删不掉的剪切到桌面，改后缀名，重启
> 
> [easy connect 无法卸载干净，后台 sangfor 文件一直在运行的卸载方法 - 哔哩哔哩](https://www.bilibili.com/read/cv23928303/ "easy connect无法卸载干净，后台sangfor文件一直在运行的卸载方法 - 哔哩哔哩")

方法 3：

> 但是最后还有一个删不掉！（.sys 结尾的）且无法通过右键直接跳转到文件里，说我没权限？？
> 
> 后面好像是采用断网、重启？？（maybe）
> 
> **然后复制这个文件的路径，找一个地方粘贴看着，一点点跟着路径找到这个文件，就直接删掉了！**

这时候就不会产生新的 sangfor 开头文件了！！

Sangfor 文件拜拜。Sangfor 目录下的自带的 remove.exe，点了之后根本没反应。不知道为什么要把软件设计成一个非常难以删除的程序？在我们电脑里还是自启动进程。是想干嘛？

四、删除 sangfor 后网络坏了、某些 app 无法正常启动怎么解决？
-------------------------------------

这时候由于强行卸载 sangfor，我发现我的 app 都用不了了，连不上网。互联网上还是好人多，根据 csdn 上一位老哥教的方法，终于修好了我的电脑。

解决方法：

（对了，我是在把所有残留文件删完后才用这个操作的。你们可以先看下文章后面内容）

> **管理员权限 cmd 执行命令：**
> 
> ```
> netsh winsock reset
> 
> ```
> 
>  **重启计算机，完工。**
> 
> [https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235^v43^pc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11](https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235%5Ev43%5Epc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11 "https://blog.csdn.net/counsellor/article/details/115197152?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-8-115197152-blog-134767448.235^v43^pc_blog_bottom_relevance_base9&spm=1001.2101.3001.4242.5&utm_relevant_index=11")
> 
> 感谢 csdn 上的这位老哥，你们可以点进去看一下会出现这样问题原因。