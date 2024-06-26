> 作者：**李伟**，EMQX 社区用户。研究无人洗车，洗车一体机，共享棋牌室等物联网应用。

自助洗车最近日益受欢迎，我受邀帮助开发了一个自助洗车系统。自助洗车可解决人工洗车费用高，且不能随时洗车的问题。门店提供洗车设备，然后基于小程序通过车牌识别、扫码等方式开启卷帘门并控制水电，系统开始计费，结算通过倒车识别、结算按钮、扫码等方式关门、断电。

上述流程对于一个专业做物联网的人来说，可能较容易实现，但因为笔者一直做 Java 后端开发，软件计费、小程序开发这些都较容易，反而对硬件、物联网相关知识有所欠缺。所以项目主要难点是如何通过后端程序控制门的开关、电力设施等物联网设备。本文将主要介绍如何解决实现物联网设备的控制过程中所遇到的难点。

## 方案选型

首先，我先去调研了市场上现有的洗车项目的方案。发现大多数是通过门口的一台车牌识别器相机，加上店里的一台电脑实现的。于是去买了车牌识别相机，然后通过相机厂家拿到了开发文档，发现相机支持 HTTP 和本地 SDK 调用。

1. HTTP 是首先排除的，因为 HTTP 通过轮询的方式工作，相机识别到车牌后会推送通知给服务端程序，但是如果后端要实时地控制门的开启，则会有延迟，所以首先排除；
2. 本地 SDK：推测现有的洗车项目里，很多店里之所以还需要一台电脑，就是因为他们是通过 SDK 调用开发的，所以最开始也采用了这种方式对接。但是本地 SDK 开发后，通过局域网连接相机是可以实现控制了，如何和服务端通信呢？我想了一个办法就是自己再包装一下，写一个 WebSocket 的程序和服务端通过制定好的规则实时通讯，来发送指令给相机，然后做了开机自启，断线重连，这样也能完美实现，并非常稳定。但这个方案在后期有很大的弊端，就是如果后台代码升级更新通讯后，必须连带修改再部署到电脑上。

## 方案改进

运营稳定后，我一直在网上查询有没有更好的方案支持，偶然间看到了 [MQTT](https://www.emqx.com/zh/blog/the-easiest-guide-to-getting-started-with-mqtt) 和 EMQX。了解到 MQTT 是物联网最常用的通讯协议，但因为相机层面不支持 MQTT 就没有去实践。后来要开新店了，去买了一台新的支持 MQTT 的车牌识别相机，并通过查阅 EMQX 文档开始开发。第一个版本使用的是 EMQX 4.4.X，代码经过升级后，不仅开发运营方便了，店里也不需要购买电脑增加额外的成本。

### WebHook 解决方案

EMQX 方案上线的初期也遇到了一些问题。因为之前使用了 Websocket 方式，服务端在发送入场开门命令的时候如果 try catch 到错误，会默认为门店电路或网络出现问题，就不会生成订单。而改用 MQTT 后，开门指令是发送给 EMQX 服务端，只能检测到在发送这一层不出现问题，而如果客户端没有订阅服务端主题的情况下，并不能接收开门指令，可能会造成的现象就是：订单生成了，门却没开。最终使用 WebHook 解决了这个问题。

> WebHook 是由 [emqx_web_hook](https://github.com/emqx/emqx-web-hook) 插件提供的将 EMQX 中的钩子事件通知到某个 Web 服务的功能。WebHook 的内部实现是基于[钩子](https://docs.emqx.com/zh/emqx/v4.4/advanced/hooks.html)，但它更靠近顶层一些。它通过在钩子上的挂载回调函数，获取到 EMQX 中的各种事件，并转发至 emqx_web_hook 中配置的 Web 服务器。WebHook 对于事件的处理是单向的，它仅支持将 EMQX 中的事件推送给 Web 服务，并不关心 Web 服务的返回。 借助 Webhook 可以完成设备在线、上下线记录，订阅与消息存储、消息送达确认等诸多业务。

总结一下就是，当客户端上线后，给指定的 Web 地址发送一条信息。

### 如何使用 WebHook

1. Webhook 的配置文件位于 etc/plugins/emqx_web_hook.conf，详细说明可以查看[配置项](https://docs.emqx.com/zh/emqx/v4.4/configuration/configuration.html)。这个配置非常简单，打开指定文件后，官方已经写好这些了，只需要修改 web.hook.url，和在你需要的钩子事件前的 # 号去掉即可。

   ```
   web.hook.url = http://127.0.0.1:8080/webhook
   
   web.hook.rule.client.connected.1     = {"action": "on_client_connected"}
   web.hook.rule.client.disconnected.1  = {"action": "on_client_disconnected"}
   #web.hook.rule.client.connect.1       = {"action": "on_client_connect"}
   #web.hook.rule.client.connack.1       = {"action": "on_client_connack"}
   #web.hook.rule.client.subscribe.1     = {"action": "on_client_subscribe"}
   #web.hook.rule.client.unsubscribe.1   = {"action": "on_client_unsubscribe"}
   web.hook.rule.session.subscribed.1   = {"action": "on_session_subscribed"}
   web.hook.rule.session.unsubscribed.1 = {"action": "on_session_unsubscribed"}
   #web.hook.rule.session.terminated.1   = {"action": "on_session_terminated"}
   #web.hook.rule.message.publish.1      = {"action": "on_message_publish"}
   #web.hook.rule.message.delivered.1    = {"action": "on_message_delivered"}
   #web.hook.rule.message.acked.1        = {"action": "on_message_acked"}
   ```

1. 在 EMQX 控制台中启用 WebHook 插件。
2. 然后自行搭建一个 Web 服务，当客户端有指定事件后会主动推送。

### 方案改进

该方案的优点是只要 Web 服务端不崩溃，就一定会收到上下线事件，可靠性有保障。而且耦合性更低。但也存在一些可以改进的点：

1. 没有加密通讯，所以如果 Web 服务对外暴露，小黑可以模拟发送一些数据，导致业务错乱。解决方案是在 Web 端限制访问 IP 为 EMQX 的服务端访问；
2. 该方案可作为设备在线/不在线状态的辅助，更新到数据库。定时监测不在线状态事件大于指定时间的即告警通知人工处理，目前的方案是离线 15 分钟会发送信息到公众号；
3. 本来打算该方案作为最终方案的，后来发现还有一个可靠性更高的，所以该方案就作为辅助了。可靠性更高的就是 EMQX 提供的 HTTP API ，在每次发送命令前调用检查，就能确认机器是否在线，未来有机会可以再分享新的方案。

## 升级 v5

自从 EMQX 4.4 上线后，一直运营得很稳定，在一次部署项目时拉取镜像包，发现 EMQX 升级了 v5。查阅文档发现 v5 增强了很多，特别是在配置方面的优化。4.4 版本认证、上下线通知等都需要开启插件，再配置文件修改，操作比较麻烦。升级后增强了 dashboard 管理界面，好多插件都成为内置插件，无需开启，且很多操作无需进入配置文件，直接在 Web 端就可以配置。于是很快升级到了 v5 版本，接下来会介绍下升级后出现的小问题以及是如何解决的。

1. 第一个出现的就是因为每次部署时候喜欢采用新版本，有段时间 EMQX 升级比较频繁，拉取了一个不成熟的镜像使用，配置的时候导致保存失败。这是因为在 EMQX Dashboard 设置 WebHook 请求体内容的时候是复制/黏贴进去的，加入了未知的空格、中文标点符号或其他字符，导致解析出错。而且一旦设置出错将导致 EMQX 服务端整个崩溃，新增、修改、删除将都无法操作，需要重新安装服务端才能解决。由于 EMQX v5 升级了Dashboard，以前需要在配置文件修改的方式都升级到 Web 控制台即可操作，但核心还是写到了配置文件里。这个配置文件就是 cluster-override.conf，所以找到这个文件修改为正确的格式即可。
2. 第二个是因为业务需要知道设备状态，所以用了 WebHook，出于性能考虑，在配置 WebHook 的时候选择了异步发送，可是后台的设备在线状态完全乱了，明明在线的，却显示成了离线，并且在 5.0.22  到  5.0.25 版本中都能复现这个问题。我在 EMQ 论坛[发帖提问](https://askemq.com/t/topic/4762/6)，得到了技术人员的回复：在单节点的 EMQX下，EMQX 内部产生的 connected, disconnected 事件是有序的；但由于桥接在做事件转发时使用了异步+进程池的方式写入到对端服务；所以可能会导致事件到达 Webserver 时出现乱序。这个问题会在 EMQX 未来的版本中修复，我暂时用同步的方式解决了目前的问题。

## EMQX 带来的好处

使用 EMQX 之后，只需要关心业务，不需要考虑中间如何通讯，大大节省了开发精力。项目的初期比较简单，就是通过车牌识别控制门、水电，可是有些门店不需要放车牌识别器怎么办？然后发现网上买开发版，基本都支持 MQTT，所以项目又集成了另外的开门方式，通过策略模式，增加新的通讯方案，可以说一套程序已经不限于某一种方案，可以支持市场上 90% 以上的自助洗车方案。这样对于以后不管开发还是运营都更简单、方便。

## 结语

最后，真心地感谢 EMQX，为我开启了一扇新大门。通过 EMQX 我间接学习了很多新的知识，如何控制硬件，以及一些常用设备的控制原理，以至于现在看到道闸、门禁、充电桩、自动售货机等，内心都会有一种 “我知道它们怎么回事，我也可以做” 的想法。也希望社区的开发者通过 EMQX 做出更多有趣、有用的产品来回馈社区。祝福 EMQX 越来越好，共勉。
