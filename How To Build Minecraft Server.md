# 快速搭建 Minecraft 服务器 #

## #1 安装Java运行环境 ##

 - 刷新安装缓存并更新软件包

```bash
sudo apt update && sudo apt upgrade -y
```

 - 检查当前是否有Java运行环境

```bash
java -version
```

如果返回版本信息请跳至 [#2](https://github.com/Mashiro-qwq/Minecraft/blob/main/How%20To%20Build%20Minecraft%20Server.md#2-%E5%AE%89%E8%A3%85minecraft%E6%9C%8D%E5%8A%A1%E7%AB%AF)

例如我的java版本如下

```bash
openjdk version "17.0.1" 2021-10-19
OpenJDK Runtime Environment (build 17.0.1+12-39)
OpenJDK 64-Bit Server VM (build 17.0.1+12-39, mixed mode, sharing)
```

如果没有返回版本信息则有两种方法安装java环境

1 使用以下命令自动安装Java运行环境

```bash
sudo apt install default-jdk -y
```

2 手动安装jdk

- 下载 openjdk 最新release包

```bash
curl -O https://download.java.net/java/GA/jdk17.0.1/2a2082e5a09d4267845be086888add4f/12/GPL/openjdk-17.0.1_linux-x64_bin.tar.gz
```

- 安装openjdk

解压

```bash
tar xvf openjdk-17.0.1_linux-x64_bin.tar.gz
```

等待解压结束将生成的文件夹移动至 /opt 目录

```bash
sudo mv jdk-17.0.1 /opt/
```

配置Java环境变量

```bash
sudo tee /etc/profile.d/jdk17.0.1.sh <<EOF
export JAVA_HOME=/opt/jdk-17.0.1
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
```

更新配置文件

```bash
source /etc/profile.d/jdk17.0.1.sh
```

确认Java版本

```bash
# 输入
echo $JAVA_HOME
# 此时应返回Java Home目录
/opt/jdk-17.0.1


# 输入
java -version
# 此时应返回jvm版本信息
openjdk version "17.0.1" 2021-10-19
OpenJDK Runtime Environment (build 17.0.1+12-39)
OpenJDK 64-Bit Server VM (build 17.0.1+12-39, mixed mode, sharing)
# 输入
javac -version
# 此时应返回jdk版本信息
javac 17.0.1
```

至此java环境便安装完毕

## #2 安装Minecraft服务端 ##

- 创建目录

更新 Minecraft 服务端版本跳过此步骤

```bash
mkdir 1.17 && cd 1.17
```

- 下载服务端文件

个人喜好原因修改文件名方便管理,虽然之后要改一些东西

```
wget https://launcher.mojang.com/v1/objects/a16d67e5807f57fc4e550299cf20226194497dc2/server.jar -O server_1.17.1.jar
```

第一次运行服务端程序

```java
java -jar -Xmx4G -Xms2G server_1.17.1.jar nogui
```

此时会运行一下就退出,不要慌,是正常现象

查看目录中 `eula.txt` 文件同意服务条款

```bash
vim eula.txt
```

最后一行将 `false` 改为 `true`

- 第二次运行服务端程序
```java
java -jar -Xmx4G -Xms2G server_1.17.1.jar nogui
```

这时服务端会进行初始化,并创建游戏世界,等待加载完毕提示 `Done (x.xxxs)! For help, type "help"`

此时,输入 `stop` 关闭服务端,此时查看文件夹内多了很多文件

接下来需要对服务器进行详细的配置

```bash
vim server.properties
```

具体配置信息可参考下值修改

```text
[allow-flight] <布尔值> '默认值:false'
    {
    允许玩家在安装添加飞行功能的mod前提下在生存模式下飞行。
    允许飞行可能会使恶意破坏者更加常见，因为此设定会使他们更容易达成目的。在创造模式下无作用。
        false - 不允许飞行。悬空超过5秒的玩家会被踢出服务器。
        true - 允许飞行。玩家得以使用任何能飞行的mod飞行。 
    },
[allow-nether] <布尔值> '默认值:true'
    {
    允许玩家进入下界。
        false - 下界传送门不会生效。
        true - 玩家可以通过下界传送门前往下界。
    },
[broadcast-console-to-ops] <布尔值> '默认值:true'
    {
    向所有在线OP发送所执行命令的输出。
        false - 禁用。
        true - 启用。
    },
[broadcast-rcon-to-ops] <布尔值> '默认值:true'
    {
    向所有在线OP发送通过RCON执行的命令的输出。
        false - 禁用。
        true - 启用。
    },
[difficulty] <字符串> '默认值:easy'
    {
    定义服务器的游戏难度（例如生物对玩家造成的伤害，饥饿和中毒对玩家的影响方式等）。
    如果设置了旧的数字ID，则会自动转化为英文的难度名称。
        peaceful (0) - 和平
        easy (1) - 简单
        normal (2) - 普通
        hard (3) - 困难
    },
[enable-command-block] <布尔值> '默认值:false'
    {
    是否启用命令方块
        false 不启用
        true 启用
    },
[enable-query] <布尔值> '默认值:false'
    {
    允许使用GameSpy4协议的服务器监听器。用于获取服务器信息。
        false 不启用
        true 启用
    },
[enable-rcon] <布尔值> '默认值:false'
    {
    是否允许远程访问服务器控制台。
        false 不启用
        true 启用
    },
[force-gamemode] <布尔值> '默认值:false'
    {
    强制玩家加入时为默认游戏模式。
        false - 玩家将以退出前的游戏模式加入
        true - 玩家总是以默认游戏模式加入
    },
[function-permission-level] <整数 (1-4)> '默认值:2'
    {
    设定函数的默认权限等级。
    4个等级的详情见 #op-permission-level。
    },
[gamemode] <字符串> '默认值:survival'
    {
    定义默认游戏模式。
    如果值是旧用的数字，会静默转换为对应游戏模式的英文名称。
        survival (0) - 生存模式
        creative (1) - 创造模式
        adventure (2) - 冒险模式
        spectator (3) - 旁观模式
    },
[generate-structures] <布尔值> '默认值:true'
    {
    定义是否能生成结构（例如村庄）。
        false - 新生成的区块中将不包含结构。
        true - 新生成的区块中将包含结构。
    注：即使设为false，地牢仍然会生成。
    },
[generator-settings] <字符串> '默认值:空白'
    {
    本属性质用于自定义世界的生成。详见超平坦世界和自定义了解正确的设定及例子。
    },
[hardcore] <布尔值> '默认值:false'
    {
    如果设为 true，服务器难度的设置会被忽略并且设为 hard（困难），玩家在死后会自动切换至旁观模式。
    },
[level-name] <字符串> '默认值:world'
    {
    "level-name"的值将作为世界名称及其文件夹名。你也可以把你已生成的世界存档复制过来，然后让这个值与那个文件夹的名字保持一致，服务器就可以载入该存档。
    部分字符，例如 "'" （单引号）可能需要在前面加反斜杠号 \ 才能被正常应用。
    },
[level-seed] <字符串> '默认值:空白'
    {
    与单人游戏类似，为你的世界定义一个种子。
    },
[level-type] <字符串> '默认值:default'
    {
    确定地图所生成的类型
        default - 带有丘陵，河谷，海洋等的标准的世界。
        flat - 一个没有特性的平坦世界，可用generator-settings修改。
        largebiomes - 如同预设（default）世界，但所有生物群系都更大。
        amplified - 如同预设世界，但世界生成高度提高。
        buffet - 如同预设世界，但generator-settings设置后不同。    
    },
[max-build-height] <整数> '默认值:256'
    {
    玩家在游戏中能够建造的最大高度。可能会在该值较小时生成超过该值的地形。
    },
[max-players] <整数（0-2147483647）> '默认值:20'
    {
    服务器同时能容纳的最大玩家数量。请注意，在线玩家越多，对服务器造成的负担也就越大。
    同样注意，服务器的OP具有在人满的情况下强行进入服务器的能力：找到在服务器根目录下叫ops.json的文件并打开，将需要此能力的OP下的bypassesPlayerLimit选项设置为true即可（默认值为false），这意味着OP将不需要在服务器人满时等待有玩家离开后再加入。
    过大的数值会使客户端显示的玩家列表崩坏。 
    },
[max-tick-time] <整数（0–(2^63 - 1)）> '默认值:60000'
    {
    设置每个tick花费的最大毫秒数。超过该毫秒数时，服务器看门狗将停止服务器程序并附带上信息：服务器的一个tick花费了60.00秒（最长也应该只有0.05秒）；判定服务器已崩溃，它将被强制关闭。遇到这种情况的时候，它会调用 System.exit(1)。
    如果你监测服务程序的返回代码，此时返回代码会为1。（习惯上，程序正常退出应当返回0）
        -1 - 完全停用看门狗（这个停用选项在 14w32a 快照中添加）
    },
[max-world-size] <整数（1-29999984）> '默认值:29999984'
    {
    设置可让世界边界获得的最大半径值，单位为方块。通过成功执行的命令能把世界边界设置得更大，但不会超过这里设置的最大方块限制。
    如果设置的 max-world-size 超过默认值的大小，那将不会起任何效果。
    例如：
    设置 max-world-size为1000将会有2000x2000的地图边界。
    设置 max-world-size为4000将会有8000x8000的地图边界。  
    },
[motd] <字符串> '默认值:A Minecraft Server'
    {
    本属性值是玩家客户端的多人游戏服务器列表中显示的服务器信息，显示于名称下方。
    MOTD 支持样式代码。
    MOTD 支持特殊符号，比如"♥"。然而，这些符号需要转换为Unicode转义字符。
    如果MOTD超过59个字符，服务器列表很可能会返回“通讯错误”。
    },
[network-compression-threshold] <整数> '默认值:256'
    {
    默认会允许n-1字节的数据包正常发送, 如果数据包为n字节或更大时会进行压缩。
    所以，更低的数值会使得更多的数据包被压缩，但是如果被压缩的数据包字节太小将反而使压缩后字节更大。
        -1 - 完全禁用数据包压缩
        0 - 压缩全部数据包
    注：以太网规范要求把小于64字节的数据包填充为64字节。
    因此，设置一个低于64的值可能没有什么好处。也不推荐让设置的值超过MTU（通常为1500字节）。
    },
[online-mode] <布尔值> '默认值:true'
    {
    是否让服务器对比Minecraft账户数据库验证登录信息。
    只有在你的服务器并未与 Internet 连接时，才将这个值设为false。如果设为false，黑客就能够使用任意假账户连接服务器！
    如果minecraft.net服务器宕机或不可访问，那么该值设为true的服务器会因为无法验证玩家身份而拒绝所有玩家加入。
    通常，这个值设为true的服务器被称为“正版服务器”。
    故意设定该变量为false的服务器称为“破解服务器”，这类服务器允许拥有未授权的Minecraft副本的玩家加入。
        true - 启用。服务器会认为自己具有 Internet 连接，并检查每一位连入的玩家。
        false - 禁用。服务器不会尝试检查玩家。
    },
[op-permission-level] <整数（1-4）> '默认值:4'
    {
    设定使用/op命令时OP的权限等级。所有存档会从之前的存档继承能力和命令。
        1 - OP可以绕过重生点保护。
        2 - OP可以使用所有单人游戏作弊命令（除了/publish，因为不能在服务器上使用；/debug也是）并使用命令方块。
            命令方块和领域服服主/管理员有此等级权限。
        3 - OP可以使用大多数多人游戏中独有的命令，包括 /debug，以及管理玩家的命令（/ban，/op等等）。
        4 - OP可以使用所有命令，包括 /stop, /save-all, /save-on 和 /save-off。
    },
[player-idle-timeout] <整数> '默认值:0'
    {
    如果不为0，服务器将在玩家的空闲时间达到设置的时间（单位为分钟）时将玩家踢出服务器
    注：当服务器接受到下列数据包之一时将会重置空闲时间：
        点击窗口
        附魔物品
        更新告示牌
        玩家挖掘方块
        玩家放置方块
        更换拿着的物品
        动画(挥动手臂)
        实体动作
        客户端状态
        聊天信息
        使用实体
    },
[prevent-proxy-connections] <布尔值> '默认值:false'
    {
    如果服务器发送的ISP/AS和Mojang的验证服务器的不一样，玩家将会被踢出。
        true - 启用。服务器将会禁止玩家使用虚拟专用网络或代理。
        false - 禁用。服务器将不会禁止玩家使用虚拟专用网络或代理。
    },
[pvp] <布尔值> '默认值:true'
    {
    是否允许PvP。也只有在允许PvP时玩家自己的箭才会受到伤害。
        true - 玩家可以互相残杀。
        false - 玩家无法互相造成伤害（也称作玩家对战环境（PvE））。
    注：由玩家造成的间接伤害（例如熔岩，火，TNT等，某种程度上还有水，沙子和沙砾）还是会伤害其他玩家。
    },
[query.port] <整数（1-65534）> '默认值:25565'
    {
    设置监听服务器的端口号（参见 enable-query）。
    },
[rcon.password] <字符串> '默认值:空白'
    {
    设置RCON远程访问的密码（参见enable-rcon）。
    RCON：能允许其他应用程序通过互联网与Minecraft服务器连接并交互的远程控制台协议。
    },
[rcon.port] <整数（1-65534)> '默认值:25575'
    {
    设置RCON远程访问的端口号。
    },
[resource-pack] <字符串> '默认值:空白'
    {
    可选选项，可输入指向一个资源包的URI。玩家可选择是否使用该资源包。
    注意若该值含":"和"="字符，需要在其前加上反斜线(\)，例如 http\://somedomain.com/somepack.zip?someparam\=somevalue
    资源包大小理应不能超过50 MiB（≈ 50.4 MB）。注意，下载成功或失败由客户端记录，而非服务器。
    },
[resource-pack-sha1] <字符串> '默认值:空白'
    {
    资源包的SHA-1值，必须为小写十六进制，建议填写它。这还没有用于验证资源包的完整性，但是它提高了资源包缓存的有效性和可靠性。
    },
[server-ip] <字符串> '默认值:空白'
    {
    将服务器与一个特定IP绑定。强烈建议留空该属性值！
    留空，或是填入你想让服务器绑定（监听）的IP。
    },
[server-port] <整数（1-65534）> '默认值:25565'
    {
    改变服务器（监听的）端口号。如果服务器在使用NAT的网络中运行，该端口必须被转发（在你有家用路由器/防火墙的前提下）。
    },
[snooper-enabled] <布尔值> '默认值:true'
    {
    是否允许服务端定期发送统计数据到http://snoop.minecraft.net。
        false - 禁用数据采集
        true - 启用数据采集
    },
[spawn-animals] <布尔值> '默认值:true'
    {
    决定动物是否可以生成。
        true - 动物可以正常生成。
        false - 动物生成后会立即消失。
    提示：如果你有严重的卡顿，可以设为false。
    },
[spawn-monsters] <布尔值> '默认值:true'
    {
    决定攻击型生物（怪物）是否可以生成。
        true - 启用。怪物会生成于夜晚和黑暗处。
        false - 禁用。不会有任何怪物。
    如果difficulty=0（即难度设置为和平）的话，该属性值不会有任何影响。
    提示：如果你有严重的卡顿，可以设为false。
    },
[spawn-npcs] <布尔值> '默认值:true'
    {
    决定是否生成村民。
        true - 启用。生成村民。
        false - 禁用。不会有村民。
    },
[spawn-protection] <整数> '默认值:16'
    {
    通过将该值进行2x+1的运算来决定出生点的保护半径。
    设置为0将不会禁用出生点保护，但会保护位于出生点的那一个方块。
    设置为1会保护以出生点为中心的3x3方块的区域，2会保护5x5方块的区域，3会保护7x7方块的区域，以此类推。
    这个选项不在第一次服务器启动时生成，只会在第一个玩家加入服务器时出现。如果服务器没有设置OP，这个选项会自动禁用。
    },
[use-native-transport] <布尔值> '默认值:true'
    {
    是否使用针对Linux平台的数据包收发优化。此选项仅会在Linux平台上生成。
        true - 启用。启用Linux数据包收发优化。
        false - 禁用。禁用Linux数据包收发优化。
    },
[view-distance] <整数（3-32）> '默认值:10'
    {
    设置服务端发送给客户端的世界数据量，也就是设置玩家各个方向上的区块数量（是以玩家为中心的半径，不是直径）。
    它决定了服务端的可视距离。（另见渲染距离）
    默认/推荐设置为10，如果有严重卡顿的话，减少该数值。
    注:该值小于9时会对服务器上的生物生成有显著影响，详见bugMC-2536。
    },
[white-list] <布尔值> '默认值:false'
    {
    启用服务器的白名单。
    当启用时，只有白名单上的用户才能连接服务器。白名单主要用于私人服务器，例如提供给相识的朋友、通过应用流程谨慎选择的陌生人等。
        alse - 不使用白名单。
        true - 从whitelist.json文件加载白名单。
    注: OP会自动被视为在白名单上，所以无需再将OP加入白名单。
    },
[enforce-whitelist] <布尔值> '默认值:false'
    {
    在服务器上强制执行白名单。
    当启用后，不在白名单（前提是启用）中的用户将在服务器重新加载白名单文件后从服务器踢出。
        true - 不在白名单上的用户不会被踢出。
        false - 不在白名单上的在线用户会被踢出。
    }
```

修改完配置之后先安装screen

- 安装screen

```bash
sudo apt install screen -y
```

- 运行screen并在screen里运行 Minecraft 服务端

```bash
screen -S 1.17

cd 1.17

# 第三次运行服务端程序
java -jar -Xmx4G -Xms2G server_1.17.1.jar nogui
```

> 使用<kbd>Ctrl</kbd>+<kbd>A</kbd>+<kbd>D</kbd> 来将screen放至后台运行

- screen的一些基本命令
> 语法 # screen [-AmRvx -ls -wipe][-d <窗口名称>][-h <行数>][-r <窗口名称>][-s ][-S <窗口名称>]
- 参数说明
> -A 　将所有的视窗都调整为目前终端机的大小  
  -d <窗口名称> 　将指定的screen窗口离线  
  -h <行数> 　指定视窗的缓冲区行数  
  -m 　即使目前已在窗口中的screen窗口，仍强制建立新的screen窗口  
  -r <窗口名称>或<窗口id> 　恢复离线的screen窗口  
  -R 　先试图恢复离线的窗口。若找不到离线的窗口，即建立新的screen窗口  
  -s 　指定建立新视窗时，所要执行的shell  
  -S <窗口名称> 　指定screen窗口的名称  
  -v 　显示版本信息  
  -x 　恢复之前离线的screen窗口  
  -ls或--list 　显示目前所有的screen窗口  
> -wipe 　检查目前所有的screen窗口，并删除已经无法使用的screen窗口  

- 常用的screen命令
```bash
screen -S <Minecraft版本名> # 建立新窗口 例 screen -S 1.17 注意 'S' 是大写

screen -ls # 查看当前所有窗口信息 正常应返回如下

    There is a screen on:
            1276.1.17     (10/31/2021 12:32:30 PM)        (Detached)
    1 Socket in /run/screen/S-root.

# 其中名称 1.17 前面的 1276 即是窗口id

screen -r <窗口名称或窗口id> # 还原窗口 例 screen -r 1.17 或 screen -r 1276

screen -X -S <创建的窗口名称或窗口id> quit # 完全关闭窗口 例screen -X -S 1.17 quit 或screen -X -S 1276 quit
#这里有可能创建两个同样名字的窗口而需要使用窗口id来关闭这个窗口 注意 'X' 和 'S' 都是大写
```

至此一个原版不带任何mod的Minecraft 服务器就已经搭建好了

连接服务器请使用 `ip:port` 格式进行连接  
- ip应当使用 `server.properties` 中 `server-ip` 项配置的IP地址进行连接,如果留空则默认使用服务器公网ipv4地址  
- port应当使用 `server.properties` 中 `server-port` 项配置的端口进行连接,默认应为 `25565`   

## #3 给服务端安装Fabric

- 停止 Minecraft 服务器

输入 `stop` 结束服务端运行

- 下载Fabric安装器

访问[Fabric](https://fabricmc.net/use/)官网下载 Fabric 安装器至服务器Minecraft服务端同一目录内

```bash
wget https://maven.fabricmc.net/net/fabricmc/fabric-installer/0.10.2/fabric-installer-0.10.2.jar
```

使用以下命令给服务端安装Fabric加载器

```bash
java -jar fabric-installer-0.10.2.jar server 1.17.1
```

> 命令解释:  
  fabric-installer-0.10.2.jar -----你下载的安装器文件全名  
  server -----安装到服务端  
  -snapshot -----*可选 启用快照版本*  
> 1.17.1 ------服务端对应版本号

等待所有文件下载完毕返回 `Done, start server by running fabric-server-launch.jar` 即代表安装完成  
若返回任何非上述文字提示,请重复执行命令重新安装Fabric

- 使用Fabric加载器启动MInecraft服务端

```bash
java -jar -Xmx4G -Xms2G fabric-server-launch.jar nogui
```

第一次启动因为修改了服务端文件名所以会自动结束运行

修改一下自动生成的加载器配置文件 `fabric-server-launcher.properties` 

```bash
vim fabric-server-launcher.properties
```

将 `server=` 后面修改为服务端文件名 即 `server_1.17.1.jar`

再次使用Fabric加载器启动MInecraft服务端

```bash
java -jar -Xmx4G -Xms2G fabric-server-launch.jar nogui
```

至此基于fabric加载器的 Minecraft 服务端就搭建好了  
以后每次启动带Fabric的服务端都需要输入上面的命令  
调试时,启动纯净 Minecraft 服务端则使用以下命令

```java
java -jar -Xmx4G -Xms2G server_1.17.1.jar nogui
```

> 此后只需要将mod放入 `mods` 文件夹,重启 Minecraft 服务端就可以加载mod了

## #4 更新

更新 Minecraft 服务端版本前请务必做好备份工作

- 更新 Minecraft 服务端版本

首先输入 `stop` 停止服务器

复制服务端目录作为备份

```bash
cp -R /root/1.17 /root/1.17_bak
```

升级前删除旧版本文件,Fabric安装器没有更新的话不需要删除,版本更新后mod也需要升级到对应版本所以需要删除mod文件夹

```bash
rm -rf .fabric/ .mixin.out/ mods/ fabric-installer-0.10.2.jar  fabric-server-launch.jar 
```

重复 [#2](https://github.com/Mashiro-qwq/Minecraft/blob/main/How%20To%20Build%20Minecraft%20Server.md#2-%E5%AE%89%E8%A3%85minecraft%E6%9C%8D%E5%8A%A1%E7%AB%AF) 安装新版本Minecraft服务端

别忘了顺手修改 `fabric-server-launcher.properties` 文件 将 `server=` 后面修改为你更新的服务端文件名

重复 [#3](https://github.com/Mashiro-qwq/Minecraft/blob/main/How%20To%20Build%20Minecraft%20Server.md#3-%E7%BB%99%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%AE%89%E8%A3%85fabric) 安装新版本Fabric

至此服务端完全安装完毕 Enjoy!!!
