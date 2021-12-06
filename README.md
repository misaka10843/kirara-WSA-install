## 如何在Windows Subsystem for Android上游玩你游

![](https://docimg3.docs.qq.com/image/IKkuZMZogCUzJYDVdp936Q.png?w=711&h=432)

---
## 文中所需内容的相关链接(带下划线的蓝字为超链接)：

基础所需内容

【[MagiskOnWSA](https://github.com/LSPosed/MagiskOnWSA)】

【[你游安装包](https://apkcombo.com/zh/kirarafantajia/com.aniplex.kirarafantasia/download/apk)】

懒人工具，与进阶工具选其一

【[子系统助手](https://url54.ctfile.com/d/29434354-43505884-32e560)】(用于快速安装apk) by晨钟酱(位于目录Windows工具/子系统助手)，访问密码1143，

【[子系统助手使用教程](http://www.bilibili.com/video/BV1X34y1o7a4)】

【[FTPshare](https://github.com/ghmxr/ftpshare/releases/)】 (用于传输文件) by ghmxr

【[FTPshare使用教程](https://www.bilibili.com/video/BV1GS4y1d723)】

进阶工具，与懒人工具选其一

【[Android调试工具/SDK Platform Tools](https://developer.android.google.cn/studio/releases/platform-tools)】建议把调试工具加入到Path，此处不详解。（在解压目录中就有`adb.exe`，其实理论来说不需要）

---

## 一、前提操作

(1) Windows系统操作

①开启开发人员模式以从第三方部署WSA

方法：设置→隐私和安全性→开发者选项→开发人员模式

②开启“虚拟机平台”功能以提供对WSA的运行支持，开启“适用于Linux的Windows子系统”以调用GPU提高性能

方法：

设置→应用→可选功能→(滚到最下方)相关设置-更多Windows功能

或者是搜索→输入“启用或关闭Windows功能”

启用功能后点击确定，系统将添加所需文件，稍作等待后将出现重启提示，重启后即可正式启用功能。

(2) Android环境条件

在Android端运行你游需满足以下基本条件：无Root /Root未被检测、USB调试模式已被关闭（不检测网络调试功能所以可以开启网络调试）、谷歌服务框架（Google Service Framework）完整且正常可工作。

由于官方版本WSA无GMS，因此需要[MagiskOnWSA](https://github.com/LSPosed/MagiskOnWSA)等第三方方案修改的WSA进行游玩。

某些玄学情况下还可能因为底层Linux的SELinux模式触发奇奇怪怪的bug导致游戏无法正常游玩，则需要设置SELinux为permissive（宽容模式）。

---

## 二、WSA部署步骤

（1）、下载WSA的安装包

按照下面的gif操作来下载WSA的APPX包
![](https://s2.loli.net/2021/12/06/E5x9YaeO2tPhWkj.gif)

（2）、打开PS权限

为了防止无法安装，所以先用 `管理员运行`的方式打开powershell，并输入 `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`
![](https://s2.loli.net/2021/12/06/QVwTc9DKfFX24UY.png)

（5）、部署

解压下载好的构建版本，找到“Install.ps1”文件并右键用PowerShell运行，片刻之后将部署成功，可在开始菜单中看见WSA设置面板。
（如果看不到也可以在PowerShell命令行中运行`.\Install.ps1`(但是您应该首先先切换到解压目录。如`cd [您的解压目录]`)）

![](https://docimg2.docs.qq.com/image/3ZxTulA0-8ClzNrwWy49LQ.png?w=380&h=52)

---

## 三、WSA实际使用

（1）、启用无线调试功能

![](https://docimg8.docs.qq.com/image/tzKcjSewzAtOGJhilItNFQ.png?w=784&h=884)
如下图所示，首先启用开发者模式，然后点击Manage developer setting进入开发者设置。（在开发者模式那里）

确保关闭USB调试并启用无线调试，然后打开WSA设置面板点击“刷新网络地址”，而后WSA设置面板的开发者模式描述中将会出现【ADB can be connected on 127.0.0.1:58526】字样。

![](https://docimg7.docs.qq.com/image/rqoZ3HIkG-bsOO81sKhyJg.png?w=464&h=339)

（2）、安装apk

注：每次从外部安装apk时都需保证子系统正在运行且无线调试功能为启用状态。

懒人方式：

双击启动晨钟酱的[子系统助手](https://url54.ctfile.com/d/29434354-43505884-32e560)，点击安装APK栏的【选择并安装】，选择apk并安装完成后将在开始菜单中显示已安装的Android应用，点击即可启动。

点击【添加快捷安装】后可对.apk后缀的文件进行关联，后续即可双击apk启动子系统助手进行安装。

![](https://docimg10.docs.qq.com/image/5ANPYa8ztOLBfo5A2uQrxA.png?w=505&h=321)

进阶方式：

进入您解压WSA的目录(如：`D:\WSA-with-magisk-NoGApps_1.8.32828.0_x64_Release-Nightly`)

在文件夹空白处按住shift打开右键菜单，选择【在Windows终端中打开】，在终端中输入`adb connect 127.0.0.1:58526`连接子系统。(如果连接不上请将IP换成刷新网络地址下的IP)

（子系统在每次重启后需要重新连接，输入`adb devices`则可查询已连接Android设备，如果多余一个则可用`adb disconnect 某个设备:端口号`来断开多余设备的连接）

输入`adb install apk文件的绝对路径`(若apk文件在调试工具目录内则可直接输入相对路径)进行安装。

示例：`adb install D:\Download\kirarafantasia.apk`

---

## 四、构建带Root的WSA需进行的额外操作

（1）、修复环境使Magisk工作

![](https://docimg6.docs.qq.com/image/jlnekUjHkAwiZJ4PN_TG9g.png?w=579&h=479)
在WSA首次启动后，点击开始菜单的Magisk，会在界面加载后片刻提示下载，给予安装权限并完成下载安装后重新启动Magisk，将会提示【修复环境】，点击确认后片刻WSA将会重启（若不重启可以手动启动Magisk启动WSA），此时Magisk将正常工作。

（2）、启用Zygisk并排除你游

点击右上角齿轮按钮进入设置并启用【Zygisk】与【遵守排除列表】，而后返回上一级点击右上角环形箭头按钮选择【重启】。重启完成后Zygisk功能将会正常工作，此时点击设置中【配置排除列表】将你游添加至列表中。

（3）、关闭SELinux（可选）

WSA中SELinux默认为强制模式（enforcing），由于某些玄学问题，你游在WSA上的运行可能会触发SELinux导致被强制停止，在主页面前的Loading页闪退。倘若出现这种情况，可以通过Root将SELinux修改成宽容模式（permissive）。

使用调试工具并连接设备，不再赘述，在终端中依次输入
`adb shell`
`su`（此时Magisk可能会弹出shell申请Root授权，需允许授权）
`setenforce permissive`

---

## 五、引继，文件传输，注意事项

（1）、引继/文件传输

倘若不想用引继码引继则可复制a.d和a.d2两个文件（登陆凭证），从Windows中向WSA传输文件可以使用[FTPshare](https://github.com/ghmxr/ftpshare/releases/)（[使用教程](https://www.bilibili.com/video/BV1GS4y1d723)），或是使用`adb push Windows绝对路径 WSA绝对路径`

示例：`adb push D:\Download\a.d sdcard:\Download`

建议搭配[MT管理器](https://www.coolapk.com/apk/bin.mt.plus)等文件管理器进行方便的文件管理。

（2）、注意事项

当Magisk的Zygisk功能启用时，Riru及基于Riru api的模块比如HideMyApplist或正式版Lsposed将不会工作，若需使用LSPosed请安装其在Telegram发布的Zygisk兼容测试版本。[https://t.me/LSPosedArchives](https://t.me/LSPosedArchives)

你游有几种不兼容的表现：（仅供参考）

1. 打开闪退：

谷歌框架不完整（可能不弹网页）

一般由Root被检测到、usb调试开启所导致（弹网页通知)

额外的玄学问题

2. 打开，加载几秒后闪退：

被停止运行，可能是由于内存不足、系统限制导致

在WSA的SELinux强制停止会被你游触发

USB调试开启

额外的玄学问题

3. 标题主界面卡加载等待：

谷歌验证超时导致，可能是使用了不支持谷歌验证的加速器，或网络质量差。

（3）、网络代理

科学网络：

在WSA外爬梯需要使用支持TUN/TAP全局代理的客户端(SSTap/Netch等)，在WSA内可通过VirtWifi从外部代理网络。

如果您在Windows系统中开启梯子的话，您可以打开全局代理（需要可以直接代理网卡的类似的功能），
或者先打开浏览器，随便输入一个域名（如:`asasasasasasasasasasa.com`），然后有可能会出现此图
![orRikR.md.png](https://s4.ax1x.com/2021/12/06/orRikR.md.png)
记住端口号，进入您解压WSA的目录(如：`D:\WSA-with-magisk-NoGApps_1.8.32828.0_x64_Release-Nightly`)
在文件夹空白处按住shift打开右键菜单打开终端，输入`ipconfig`,会看见类似下面的输出
```bash
以太网适配器 以太网:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 本地连接* 1:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 本地连接* 2:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 WLAN:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::d457:1173:d63:97b3%15
   IPv4 地址 . . . . . . . . . . . . : 192.168.1.101
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : 192.168.1.1
```
例如我是使用的WIFI连接，所以记住`192.168.1.101`这个IP

然后再输入`adb devices`查找到WSA的设备名称后

输入`adb -s [WSA的名称] shell settings put global http_proxy [IP]:[端口]`即可
如果需要关闭代理的话只需要输入`adb -s [WSA的名称] shell settings put global http_proxy 0.0.0.0`即可

---
## 备注：
此教程是按照贴吧大佬[晓星](https://tieba.baidu.com/p/7609268378)改编而来，理论来说改编后更为具体（？），至少可以不自己fork了qwq
