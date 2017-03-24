
# iOS证书及ipa包重签名探究


##证书ID是什么？
![](http://olinone.qiniudn.com/certifier%20ID.png)

图中方框里字符串就是证书ID，升级后的ipa标识就是证书ID+BundleID，<font color="#03A9F4">只有两者完全匹配，安装包才能覆盖安装</font>，否则就会提示安装失败。解决办法就是卸载安装包，重新安装！

>The entitlements specified in your application’s Code Signing Entitlements file do not match those specified in your provisioning profile

目前，重签名主要用于企业证书重签名个人证书发布的ipa包，包括各种助手及企业内测包的发布等。在重签名前，让我们先看看一个完整的ipa包有哪些与证书相关的东西！打开ipa包，会发现_CodeSignature和embedded.mobileprovision两个文件

![](http://olinone.qiniudn.com/resign_file1.png)

* _CodeSignature：ipa包签名文件

* embedded.mobileprovision：证书配置文件

因此，替换上面两个文件就解决了ipa重签名的主要问题。此外，代码签名探析文中还提到entitlements.plist授权文件，重签名时也需要处理。按照下图内容创建plist文件，输入相关信息。

![](http://olinone.qiniudn.com/resign_entitlement.png)
 

##签名过程如下（文件路径自定义）

**1、解压ipa安装包**


```
cp olinone.ipa olinone.zip
```

**2、替换证书配置文件（文件名必须为embedded，不得自定义）**

```
cp embedded.mobileprovision Payload/olinone.app
```

**3、重签名（certifierName为重签名证书文件名，可以加证书ID后缀）**

```
codesign -f -s "iPhone Distribution: olinone Information Technology Limited(6a5TVN58SY)" --entitlements
entitlements.plist Payload/olinone.app
```

**4、打包**

```
zip -r olinone.ipa Payload
```

很多朋友在重签名时会忽略第二步或者没有指定entitlements.plist，都会造成ipa包安装失败
 

其次，有些朋友希望修改ipa包里的素材，然后再签名，以我所知，这个貌似也行不通

