近日，全球领先的开源物联网数据基础设施软件供应商 EMQ 映云科技宣布，旗下核心产品开源分布式物联网 [MQTT 消息服务器 EMQX 5.0](https://github.com/emqx/emqx) 版本正式发布！

> **EMQX GitHub：**
> [https://github.com/emqx/emqx](https://github.com/emqx/emqx)

5.0 是 EMQX 自发布以来最重要的版本之一，由欧洲、中国以及全球的社区和团队开发完成。它是有史以来支持 MQTT 并发连接规模最大、同时也是全球首个实现 MQTT over QUIC 的 MQTT Broker，在消息传输的可靠性、产品体验的易用性等方面也进行了大幅优化升级，这也标志着 EMQX 再次凭实力巩固了其作为物联网消息平台领导者的地位。

EMQ 创始人兼 CEO 李枫表示：“EMQX 5.0 是 MQTT 领域的一个里程碑式的成果。它不仅是全球首个单集群支持 1 亿连接的分布式 MQTT 消息服务器，也是首个将 QUIC 引入 MQTT 的开创性产品。EMQ 始终关注前沿技术趋势，通过不断加速的产品迭代开发保持世界领先的技术竞争力，与我们的全球客户与合作伙伴一同，应对与日俱增的物联网设备部署规模及日益重要的车联网等关键业务场景带来的巨大挑战。”


## EMQX 5.0：为亿级物联网连接的时代而生

随着 5G 和物联网技术在各行各业的深度融合，全球物联网应用和设备正面临爆发式增长，将真正迎来亿级万物互联的新时代。据 IoT Analytics 最新发布的《2022 年春季物联网状况》研究报告显示，到 2022 年，物联网市场预计将增长 18%，达到 144 亿活跃连接。在如此大规模的物联网需求下，海量的设备接入和设备管理对网络带宽、通信协议以及平台服务架构都带来了巨大的挑战。

EMQX 自 2013 年在 GitHub 发布开源版本以来，获得了来自 50 多个国家和地区的 20000 余家企业用户的广泛认可，累计连接物联网关键设备超过 1 亿台。

作为全球下载量超 2000 万的开源云原生分布式 MQTT 消息服务器，EMQX 多年来历经 200 多个版本迭代升级，凭借着支持亿级连接和千万级消息吞吐的超高性能，为超大规模的物联网项目及应用提供高效、可靠、安全的物联网数据基础设施解决方案；同时与开源[工业协议网关软件 Neuron](https://www.emqx.com/zh/products/neuronex)、[轻量级 MQTT 消息服务器 NanoMQ](https://www.emqx.com/zh/products/nanomq)、[流数据库 HStreamDB](https://hstream.io/zh) 等 EMQ 旗下产品组合探索物联网云边协同应用场景，打造云-边-端物联网海量数据的接入、存储、实时处理与分析的一站式管理。



## 全球最具扩展性的MQTT消息服务器——EMQX 5.0 重大革新功能一览

面对高速发展的物联网产业和不断增长的用户规模，EMQX 持续致力于为「面向未来的物联网应用」打造可靠、安全的数据基础设施。全新发布的 EMQX 5.0 将有以下重大的升级迭代：

**可扩展性及可靠性显著提升，首个实现单集群 1 亿 MQTT 连接** 

通过采用 Erlang 的 Mnesia 数据库的新 Mria 扩展，以及即将在后续 5.1 版本中提供的基于 RocksDB 的会话持久化，EMQX 5.0 的横向扩展能力及消息传输可靠性得到了指数级提升。最新的性能测试表明，EMQX 5.0 可以轻松支持单个集群的 [1 亿 MQTT 连接](https://www.emqx.com/zh/blog/reaching-100m-mqtt-connections-with-emqx-5-0) —— 比以前的版本增加了 10 倍，这使得 EMQX 5.0 成为目前全球最具扩展性的 MQTT Broker，能够轻松承载超大规模的物联网应用。

**全球首个实现 MQTT over QUIC 的消息服务器**

EMQX 5.0 也是首个引入 QUIC 支持的 MQTT Broker。QUIC 是下一代互联网协议 HTTP/3 的底层传输协议，与 TCP 协议相比，它在减少连接开销与消息延迟的同时，提升了整体吞吐量和移动连接的稳定性。基于 QUIC 这些极适用于物联网消息传输场景的优势，EMQX 5.0 设计了独特的消息传输机制和管理方式，以不断的技术革新持续为行业、社区和客户提供最先进、最具竞争力的 MQTT 消息服务器。

**安全保障持续增强**

EMQX 基于 MQTT over TLS/SSL 和 QUIC 确保数据安全，支持基于国密算法的传输加密认证集成方案。提供用户名/密码、LDAP、JWT、PSK 和 X.509 证书等多种身份认证功能。提供 ACL 机制以及动态 ACL 规则更新能力，能够灵活地实现物联网设备发布/订阅权限控制。在 EMQX 5.0 中，各项安全能力均得到显著增强，且用户在 Dashboard 中即可为整个集群启用客户端连接认证和权限控制，拥有更灵活、易用的操作体验。

**更强大的数据集成，实现“南北向”数据流统一处理**

EMQX 5.0 版本利用统一接口来管理南北向数据流，用户可以结合更强大的规则引擎和数据桥接功能，来实现数据处理与集成。相比之前版本，EMQX 5.0 拥有更丰富的数据处理能力，并能够通过规则处理云端到设备的南向消息。

同时，EMQX 为数据集成提供了可视化查看能力（Flows），通过 Dashboard 页面，用户可以清晰看到物联网数据如何通过规则处理，以及数据如何流向外部数据服务或设备。

后续版本中，我们将支持在 Dashboard 上以拖拽的方式编排规则和数据桥接，通过可视化界面将物联网硬件数据流轻松连接在一起。

 **更多功能更新**

- **全新极简配置，易用性大幅提升：** 全新的 Dashboard 菜单栏为不同角色的使用者进行了更加清晰的功能聚合，同时以简洁易读的 HOCON 格式配置文件、符合 OpenAPI 3.0 规范的 REST API 文档为开发者带来更好的使用体验；
- **全新网关架构，更方便扩展协议：** 在对 MQTT 3.1、3.1.1、5.0 协议的完整支持之外，也为 CoAP/LwM2M、STOMP、MQTT-SN 等其他主流物联网协议提供独立的管理接口和安全认证能力；
- **可操作性与可观测性大幅提升：** 在 Dashboard 上提供长达 7 天的详细监控指标，可以一键集成 Prometheus 与 Grafana、Datadog/StatsD 等指标监控告警系统。提供更多的诊断工具如慢订阅、在线追踪帮助用户快速排查生产环境中的问题，提供更友好的结构化日志以及 JSON 格式日志支持；
- **更灵活的拓展定制方式：** 引入全新的插件架构，用户可用独立插件包的形式编译、分发、安装自己的拓展插件对 EMQX 进行自定义扩展；

## 以 EMQX 为核心迎接面向未来的全连接时代

在全球物联网规模激增的今天，需要的是能够支撑更大规模单体物联网应用、产品伸缩能力与云底层能力深度适配的物联网数据基础设施。EMQX 5.0 在集群扩展性、产品稳定性等方面的技术突破，将为物联网关键业务提供更加高效可靠的海量设备连接、高性能的消息与事件流数据实时处理。新版本对产品可操作性、可观测性以及易用性的提升，则可以帮助企业解决物联网业务开展过程中面临的管理与培训成本、网络安全等问题。

随着物联网业务在更多行业中开展落地，愈发丰富的场景和多样的需求是很难靠单一的技术和产品满足实现的。EMQ 以 EMQX 为核心，结合自身从边缘到云端的完整产品矩阵，可以实现实时数据的统一连接、移动、处理与分析，将为全行业赋予物联网数据价值发掘与转化的能力，为未来世界构建坚实的创新数字基座。


<section class="promotion">
    <div>
        现在试用 EMQX 5.0
    </div>
    <a href="https://www.emqx.com/zh/try?product=broker" class="button is-gradient px-5">立即下载 →</a>
</section>
