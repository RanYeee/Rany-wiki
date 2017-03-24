# IPAPatch: 免越狱调试、修改第三方App
<image src="http://epo.alicdn.com/image/44apipdgitp0.jpg"/>

###IPAPatch 可以做什么?
和 "HackingFacebook" 类似，"IPAPatch" 主要可以在第三方的 IPA 文件上 "添加" 自己的代码，但过程有很大不同：

<font color="#03A9F4">**过程简单**</font>
提供 IPA 文件和你的代码，配置好签名信息，点击“运行”即可
整个过程在 Xcode 中进行，就像在编写自己的 App
IPA 文件依然需要是解密过的。

![在 Youtube 中弹出自定义窗口](http://epo.alicdn.com/image/d7fbfdmr8m4.jpg)

<font color="#03A9F4">**支持调试**</font>
在 Xcode 中可以直接断点进行调试
可以用 lldb 命令（如 po），输出运行时信息
可以使用 Xcode 的调试功能查看 View Hierarchy、Memory Graph 等信息

<image src="http://epo.alicdn.com/image/44apirqm8360.jpg"/>


<font color="#03A9F4">**支持链接第三方 Framework**</font>
在集成一些第三方服务时很有用
例如之前发微博的 Reveal 调试 Youtube 就是这种方式

<image src="http://epo.alicdn.com/image/44apit7rmio0.jpg"/>

<font color="#03A9F4">**修改过的 App 可以与原始 App 共存，并自动修改名字以作区分**</font>

<image src="http://epo.alicdn.com/image/44apiuk9hmh0.jpg"/>

###怎么实现的？
主要的自动化过程在 patch.sh 这个脚本里，Xcode 会在把你的代码编译成 Framework 后执行这个脚本：
解压 IPA 文件
用 IPA 文件的内容，替换掉 Xcode 生成的 .app 的内容
通过 OPTOOL，将你代码生成的 Framework 及其他外部 Framework，注入到二进制文件中
对这些文件进行重新签名 

完成后，Xcode 会自动将修改过的 .app 安装到 iPhone 上

###具体的例子？
以修改Youtube这个来举例吧。

1). 首先我们需要准备一个解密过的 Youtube IPA 文件，这个文件可以从越狱手机上导出，也可以直接去网站下载，比如我自己常用的是 iphonecake.com

2). 将 IPA 文件命名为 app.ipa，替换模版工程中的 Assets/app.ipa 文件

3). 打开 Reveal，拿到需要集成的 Framework 文件 

<image src="http://epo.alicdn.com/image/44apj0chjau0.jpg"/>
<image src="http://epo.alicdn.com/image/44apj15k2rh0.jpg"/>

4). 将 RevealServer.framework 放置在 Assets/Frameworks/RevealServer.framework

5). 打开 IPAPatch，在 IPAPatch-DummyApp 这个 Target 里，配置好 BundleID 和代码签名。Display Name 会作为前缀添加到原来的 App 上，如图配置的话最后就是 "RevealYoutube" 

<image src="http://epo.alicdn.com/image/44apj1ulga90.jpg"/>

6). 点击 Xcode 左上角的编译运行按钮，修改好的 Youtube 就会安装到手机上，Reveal 中也能找到 

<image src="http://epo.alicdn.com/image/44apj2nhee20.jpg"/>

这个 Demo 打了一个包，传到 GitHub 的 Release 中了
https://github.com/Naituw/IPAPatch/releases

---
**该项目仅用于学习目的，请勿滥用～**

本文转载，文章出处：http://weibo.com/ttarticle/p/show?id=2309404086977153611942 

