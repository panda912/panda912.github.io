---
title: 反编译 jar 包修改代码后重新打包
date: 2017-05-10 20:27:04
tags:
---

定位要修改的类，新建一个工程，包名与 jar 包名一致，新建同名的类，将 class 中的代码拷贝进去，然后将 jar 包中要修改的类删除，将删除后的类的 jar 包添加到新的工程中，重新编译，将编译后的 class 文件拷贝到 jar 包中，over！

举个栗子：

![](1.png)

现在如果要修改 udesk jar 包中的 e、f、g、h、l、UdeskHttpFacade.class 文件，新建一个工程，包名一致，新建需要修改的同名的类文件，将代码拷贝进去

![](2.png)

删除原 jar 包中需要修改的 class 文件，将新的 jar 包放进新的工程 libs 下

![](3.png)

修改需要修改的代码，使其能够正确编译，编译之后拷贝生成对应的 class 文件，放进新的 jar 包中

![](4.png)

PS: mac 下没有找到可以直接删除 jar 中的 class 文件的工具，在 windows 下可以直接用 winrar 打开，直接删除或者添加 class 文件。

PS: 如果反编译修改类似 Activity 这样涉及到资源文件的 class 文件，需要在原工程中新建包名去修改，如果在新的工程中修改，生成的资源 id 与在原工程中不一致，导致发生错误。

参考链接：http://blog.csdn.net/yy4040/article/details/6641688

