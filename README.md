## 如何在Windows Subsystem for Android上游玩你游


![](https://docimg3.docs.qq.com/image/IKkuZMZogCUzJYDVdp936Q.png?w=711&h=432)

Tips：

1. Windows Subsystem for Android（Windows的安卓子系统，以下简称WSA）在Windows10系统上也可运行，但需要额外的操作步骤且不保证使用体验，本文不提供方案，此教程仅适用于Windows11系统。
2. 由于WSA在启用了“虚拟机平台”功能后将使用Hyper-V api，将导致其他Android模拟器性能下降或不可用（目前已有兼容Hyper-V的Android模拟器比如[BlueStacks5的64位版](https://www.bluestacks.com/download.html)），因此在使用WSA之前请斟酌取舍，当然可以选择在试玩后卸载WSA再关掉本功能。
3. 目前WSA的机制不存在关闭实例窗口后主动结束进程回收内存，换言之你开了多少个app只要不主动退出就都会一直在后台运行直至TurnOff，建议在使用完毕后手动在WSA面板里TurnOff(关闭WSA)。
4. 目前微软对WSA采用与亚马逊应用商店合作的形式，常规流程下是通过亚马逊应用商店在WSA内部安装应用，因此短期内并不会给予更简易的方案（比如双击.apk文件）从外部安装apk，仅可通过侧载（sideload）方式安装apk。
5. WSA将随着进一步使用而逐渐增加硬盘占用，因此部署WSA时请仔细斟酌自己的硬盘空间，若卸载WSA则将一并卸载所有已安装Android应用。
6. 特别鸣谢[MagiskOnWSA](https://github.com/LSPosed/MagiskOnWSA)项目及[Lsposed开发组](https://github.com/LSPosed)对WSA做出的root适配与Lsposed兼容，以及[WSA-Community](https://github.com/WSA-Community)的GAPPS嵌入脚本项目[WSAGAscript](https://github.com/WSA-Community/WSAGAScript)。
7. 本文默认读者具有“良好的、开放的网络环境”，若相关链接无法访问或下载不提供解决方案。

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

【[Android调试工具/SDK Platform Tools](https://developer.android.google.cn/studio/releases/platform-tools)】建议把调试工具加入到Path，此处不详解。

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


## 二、WSA部署步骤

[LSPosed](https://github.com/LSPosed)团队在[WSAGAscript](https://github.com/WSA-Community/WSAGAScript)的基础上制作了方便的集成脚本[MagiskOnWSA](https://github.com/LSPosed/MagiskOnWSA)与[视频教程](https://user-images.githubusercontent.com/5022927/139580565-35971031-7258-40bf-93e2-49a0750156f3.mp4)，以下步骤可以在项目中阅读readme页或观看[视频教程](https://user-images.githubusercontent.com/5022927/139580565-35971031-7258-40bf-93e2-49a0750156f3.mp4)代替。

注意：以下步骤需要使用GitHub帐号，没有则建议注册使用。

可选：此处推荐一个[GitHub汉化脚本](https://greasyfork.org/scripts/407485)，需使用拓展[Tampermonkey](https://www.tampermonkey.net/)[](https://www.tampermonkey.net/)

（1）、Fork

将项目Fork（复刻）到自己项目，而后会跳转到自己的Fork项目页面

![](https://docimg6.docs.qq.com/image/A-s7FZwKqZJGR-ZEgif_Aw.png?w=1641&h=874)
  
![](https://docimg9.docs.qq.com/image/E4CeDCxlMvEyVWiE08JMmQ.png?w=746&h=535)
（2）、获取上游


若Fork的仓库在将来有新提交，可在自己的仓库页面点击Fetch upstream获取上游更新。


（3）、构建

切换到Actions栏目，选择Magisk工作流并配置好预设，按下运行工作流按钮后将开始进行构建，稍作等待后（一般情况下在4分钟内）将会输出构建好的WSA。

此处进行简单解释一下预设：

Variants of gapps即为Google套件的各种变体规格，其中none为不配置Google套件，super为完整的Google Service Framework以及全套Google系应用，pico是最低可运行Google Play Store的规格，其他变体规格不过多讲解，感兴趣可以自行查阅，此处以笔者喜好的pico为例。

Root soluction则为Root方案，提供magisk（面具）和none（无Root）两种选择，根据喜好选择，此处笔者使用magisk，若读者选择不进行root，则可跳过后续的[步骤四](_Toc87565976)。

![](https://docimg1.docs.qq.com/image/ef_GF0h20zY3UK-k2NIoxg.png?w=1442&h=765)


（4）、工作流运行完成后点击对应工作获取输出。

注：

后缀为arm64的版本适用于arm架构64位处理器的设备，即骁龙/苹果M1设备

后缀为x64的版本适用于x86架构64位处理器的设备，即英特尔/AMD设备

![](https://docimg3.docs.qq.com/image/63nIu33aEkJ3iWRTOCBCsw.png?w=796&h=359)
  
![](https://docimg5.docs.qq.com/image/neAR4dBD81tjFA14iuhKxA.png?w=1088&h=731)


（5）、部署

解压下载好的构建版本，找到“Install.ps1”文件并右键用PowerShell运行，片刻之后将部署成功，可在开始菜单中看见WSA设置面板。

![](https://docimg2.docs.qq.com/image/3ZxTulA0-8ClzNrwWy49LQ.png?w=380&h=52)



## 三、WSA实际使用

（1）、启用无线调试功能

![](https://docimg8.docs.qq.com/image/tzKcjSewzAtOGJhilItNFQ.png?w=784&h=884)
    如下图所示，首先启用开发者模式，然后点击Manage developer setting进入开发者设置。

确保关闭USB调试并启用无线调试，然后打开WSA设置面板点击“刷新网络地址”，而后WSA设置面板的开发者模式描述中将会出现【ADB can be connected on 127.0.0.1:58526】字样。

![](https://docimg7.docs.qq.com/image/rqoZ3HIkG-bsOO81sKhyJg.png?w=464&h=339)


（2）、安装apk

注：每次从外部安装apk时都需保证子系统正在运行且无线调试功能为启用状态。

懒人方式：

双击启动晨钟酱的[子系统助手](https://url54.ctfile.com/d/29434354-43505884-32e560)，点击安装APK栏的【选择并安装】，选择apk并安装完成后将在开始菜单中显示已安装的Android应用，点击即可启动。

点击【添加快捷安装】后可对.apk后缀的文件进行关联，后续即可双击apk启动子系统助手进行安装。

![](https://docimg10.docs.qq.com/image/5ANPYa8ztOLBfo5A2uQrxA.png?w=505&h=321)


进阶方式：


在解压后的[Android调试工具/SDK Platform Tools](https://developer.android.google.cn/studio/releases/platform-tools)文件夹空白处按住shift打开右键菜单，选择【在Windows终端中打开】，在终端中输入【adb connect 127.0.0.1:58526】连接子系统。


（子系统在每次重启后需要重新连接，输入【adb devices】则可查询已连接Android设备，如果多余一个则可用【adb disconnect 某个设备:端口号】来断开多余设备的连接）


输入adb install apk文件的绝对路径(若apk文件在调试工具目录内则可直接输入相对路径)进行安装。


示例：【adb install D:\Download\kirarafantasia.apk】

## 四、构建带Root的WSA需进行的额外操作

（1）、修复环境使Magisk工作

![](https://docimg6.docs.qq.com/image/jlnekUjHkAwiZJ4PN_TG9g.png?w=579&h=479)
    在WSA首次启动后，点击开始菜单的Magisk，会在界面加载后片刻提示下载，给予安装权限并完成下载安装后重新启动Magisk，将会提示【修复环境】，点击确认后片刻WSA将会重启（若不重启可以手动启动Magisk启动WSA），此时Magisk将正常工作。


（2）、启用Zygisk并排除你游

点击右上角齿轮按钮进入设置并启用【Zygisk】与【遵守排除列表】，而后返回上一级点击右上角环形箭头按钮选择【重启】。重启完成后Zygisk功能将会正常工作，此时点击设置中【配置排除列表】将你游添加至列表中。

（3）、关闭SELinux（可选）


WSA中SELinux默认为强制模式（enforcing），由于某些玄学问题，你游在WSA上的运行可能会触发SELinux导致被强制停止，在主页面前的Loading页闪退。倘若出现这种情况，可以通过Root将SELinux修改成宽容模式（permissive）。


使用调试工具并连接设备，不再赘述，在终端中依次输入【adb shell】【su】（此时Magisk可能会弹出shell申请Root授权，需允许授权）【setenforce permissive】


## 五、引继，文件传输，注意事项

（1）、引继/文件传输


倘若不想用引继码引继则可复制a.d和a.d2两个文件（登陆凭证），从Windows中向WSA传输文件可以使用[FTPshare](https://github.com/ghmxr/ftpshare/releases/)（[使用教程](https://www.bilibili.com/video/BV1GS4y1d723)），或是使用【adb push Windows绝对路径 WSA绝对路径】

示例：【adb push D:\Download\a.d sdcard:\Download】


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
