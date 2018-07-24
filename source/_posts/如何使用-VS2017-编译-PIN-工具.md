---
title: 如何使用 VS2017 编译 PIN 工具
date: 2018-07-17 14:08:05
tags: PIN
---

```
翻译自：https://hshrzd.wordpress.com/2018/07/16/how-to-compile-a-pin-tool-using-visual-studio-2017/
```
PIN（intel）是动态二进制插桩的绝佳平台。我日常都是用它进行跟踪和反混淆恶意软件，也经常把它推荐给其他人。不幸的是，确定如何弄好它，并不是那么简单。如果你想编译在包中分发的默认的功能，你会遇到很多的错误。

我从未见过完全修复这些错误的整个文档。我自己也在努力解决这个问题，不时也有人向我请求帮助。所以我决定进行一次尝试，描述一下成功编译PIN工具的完整过程。

- 需要的PIN包
  - [pin-3.7-97619-g0d0c92f4f-msvc-windows](https://software.intel.com/en-us/articles/pin-a-binary-instrumentation-tool-downloads)
- 环境
  - Microsoft Visual Studio Community 2017 (Version 15.6.5)
  - Windows 8.1 64bit

第一步，下载PIN包，并解压到C:\pin\
![](https://hshrzd.files.wordpress.com/2018/07/c_pin.png)

我将要编译MyPinMyPinTool，这个工具是PIN包的一部分。
![](https://hshrzd.files.wordpress.com/2018/07/my_pin_tool.png)

第二步，在Visual Studio中打开这个工具并编译
![](https://hshrzd.files.wordpress.com/2018/07/32bit.png)

出现了错误
![](https://hshrzd.files.wordpress.com/2018/07/error1.png)

所以，我搜索PIN包的主目录，找到这个文件的位置，“C:\pin\extras\xed-ia32\include\xed”，在VS的设置中包含该目录
![](https://hshrzd.files.wordpress.com/2018/07/step1.png)

第三步，再次编译，出现了另一个错误
![](https://hshrzd.files.wordpress.com/2018/07/safeseh.png)

需要禁用SAFESEH
![](https://hshrzd.files.wordpress.com/2018/07/option.png)

我们切换到配置页：
![](https://hshrzd.files.wordpress.com/2018/07/disable.png)

第四步，我们再次编译，然后又出现了一系列linking leve的错误：
![](https://hshrzd.files.wordpress.com/2018/07/unresolved_externals.png)

我google这个错误，找到了这个[blog](http://zubcic.re/blog/fixing-intel-pin-visual-studio-project-files)，根据建议，通过在额外依赖中加入“crtbeginS.obj”，解决了这个问题：
![](https://hshrzd.files.wordpress.com/2018/07/dependencies.png)

最后终于编译通过了：
![](https://hshrzd.files.wordpress.com/2018/07/result.png)

只能说，它是PIN中最糟糕的一部分了，不过，现在变得容易多了。在这个包里面有很多简单的工程用来学习PIN的功能。

为了使它更容易使用，我写了一些脚本，来把PIN工具加入到跟踪菜单的上下文中，有了这些脚本，我可以只点击一次就可以跟踪任意的可执行文件了，你可以在[这里](https://github.com/hasherezade/MyPinTools/tree/master/SectionsTracer/Pin_Tools)找到这些脚本。
![](https://hshrzd.files.wordpress.com/2018/07/run_with_pin.png)