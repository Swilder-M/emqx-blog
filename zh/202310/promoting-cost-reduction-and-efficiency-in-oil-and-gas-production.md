## 摘要

随着油气储量减少、全球市场竞争的加剧，越来越多的油气生产企业寻求数字化转型，希望提升生产效率和优化资源。EMQ 为油气行业提供了云边协同物联网方案，针对异构化数据采集、系统烟囱式建设、繁重的运维工作等行业普遍面临的问题，提供一体化解决方案。方案能将不同的系统（油气生产、运营管理、设备监控与维护、安全与环境监控等）产生的数据汇聚到一个平台，方便油气生产者用更精准的管理来降低成本。

根据历史案例，EMQ 油气行业方案可帮助油气企业每年节约 25% 以上的采集系统运维费用，节约 40% 以上的数据点位接入费用。

## EMQ 云边协同方案构建智能油气物联网

通过整合工业数采、边缘计算、数据集成、远程运维管理等关键环节和技术，EMQ 形成完善的云边协同物联网应用架构，为油气行业实时监控系统、设备管理、HSE 管理、智慧生产、智能应用等数字化转型业务场景提供了专业的一体化解决方案。

![智能油气物联网架构](https://assets.emqx.com/images/5686b3933a09ef12dcbba4c0ba9f9eaf.png)

- **工业设备实时数采**

  利用 [NeuronEX](https://www.emqx.com/zh/products/neuronex) 工业协议网关软件，可支持 [Modbus](https://www.emqx.com/zh/blog/modbus-protocol-the-grandfather-of-iot-communication)、OPC-UA 等总线协议以及西门子、ABB、 三菱、施耐德等多种工业设备协议接入，能完成对生产场站现场油井传感器、油井生产系统各类 PLC、拉运系统设备、管线输送系统、站场控制系统等进行实时数据采集。Neuron 将不同工业协议转换成标准的 [MQTT 协议](https://www.emqx.com/zh/blog/the-easiest-guide-to-getting-started-with-mqtt)消息进行上传，数据点位具备统一格式标准，以及及毫秒级采集时间戳。同时，Neuron 具备离线缓存的能力，保障网络中断时历史数据的完整性。

  NeuronEX 同时具备规则引擎，在边缘端就能实现实时计算分析、规范报文、过滤清洗、智能告警、业务路由、数据持久化等功能，为油气生产者在边缘端提供数据预处理、事件逻辑处理等能力。

  NeuronEX具备 HTTP PULL 的能力，能感知生产现场安全、监控等IT智能设备配置状态、运行数据。

  NeuronEX也能与 AI 算法融合，通过对物联网数据、视频流的实时处理可实现视频监控、安全管理、设备维护、故障预测、巡检和安全监控等智能化能力。

- **企业物联网消息总线**

  将 [EMQX Enterprise](https://www.emqx.com/zh/products/emqx) 布置在厂级信息中心或集团云上，在专网或互联网上为油气生产数据提供统一的平台，实现高可用、高并发、低延时的数据传输、分析、处理、集成的能力。 EMQX Enterprise 平台能同时处理超过百万个物联网设备的连接，并且每秒能处理超过十万次以上的数据交互，是一个非常适合用于油气生产环境的、高性能、高并发、功能强大的生产级物联网消息中间件。

- **云边协同管理**

  [EMQX ECP](https://www.emqx.cn/products/emqx-ecp) 提供项目管理能力，可为 EMQX 集群实例提供基于项目的分类与管理监控；EMQX ECP 提供了统一云边模块的 WEB 管控界面，实现边缘软件 NeuronEX 的远程配置、实时监控及日志分析等功能，降低了现场运维的成本。

## 方案价值

- **油气生产全面感知**：帮助油气企业采集与管理油气生产场景中产生的多样化数据。方案覆盖生产设备，如油井传感器、油井生产系统各类 PLC、拉运系统设备、管线输送系统、站场控制系统等；同时，方案也可以覆盖园区安全与监控设备，如井场围栏设备、视频监控设备等。
- **统一的数据整合：**油气场景里的各类系统建设时期不同，采用的数据采集设备、采集方式和采集系统异构化严重。EMQ 的一体化能力、云边协同的方案架构，能帮助油气生产者集中管理原本独立且分散的各类系统中的数据。
- **降本增效**：油气生产过程中面临繁杂的运维工作，传统运维工作需要大量现场人工操作，对于油田运维人力有限的现状提出了较大的挑战。方案可以帮助油气生产者统一管理设备接入、设备巡检、设备配置、升级更新、告警处理等诸多方面运维环节。最终，方案能实现减少运维成本，同时大幅度提升智能化管理水平。
- **持续适应未来创新：**方案灵活易用，提供灵活、易扩展的接口，方便油气业务平台未来融入更多新的系统和技术。边缘端支持 MATLAB、C、C++、Python、GO 算法集成及 AI 算法的融合，促进边缘智能化的创新发展。
- **自主可控与快速部署**：技术自主可控，可支持主流的芯片、国产操作系统和部分国产数据库。标准化产品配置帮助用户快速构建和复制油气物联网平台，使业务应用开发更加高效和快捷。
- **基于云原生创新**：EMQ 整个方案可以基于容器化部署，支持云原生架构实现统一的容器编排和管理，促进智慧油气系统与云原生的融合与创新。

## 成功案例

**中石油华北油田：** 通过采用 EMQ 提供的云边协同物联网解决方案，中石油华北油气田成功实施了数据中心的远程配置和集中化存储，从而进行了生产数据的实时采集，并在云平台上探索和实践了对生产数据的实时采集。

案例链接：[https://www.emqx.com/zh/customers/huabei-oilfield-company](https://www.emqx.com/zh/customers/huabei-oilfield-company)

## 推荐产品

### NeuronEX

[NeuronEX](https://www.emqx.com/zh/products/neuronex) 是一款用于工业物联网连接和边缘计算的边缘服务，通过标准协议或专用协议与众多不同类型的工业设备进行通信，实现将多个设备连接至物流平台。其提供了以下特性：

- 多样的连接性：NeuronEX 提供多个可插拔模块，如 Modbus、OPC-UA、Ethernet/IP、BACnet、Siemens、Mitsubishi 等，有助于访问各类物流资产，确保数据连接。
- 灵活部署：NeuronEX 具有极低的内存占用，在x86、ARM、RISC-V 等低配置架构设备上运行表现出色。此外，它还支持类似 Docker 的容器化部署，能够与 K8s 环境中的其他容器共同运行。
- 优越集成：通过 MQTT 协议，NeuronEX 将工业物联网应用、大数据以及 AI/ML 分析软件无缝集成至各个平台。
- 流式处理：NeuronEX 内置流处理引擎，通过边缘侧的流式 SQL 语句，实现 AI/ML 分析、数据过滤、数据操作、设备控制以及数据持久化，将数据存储于时序数据库中。

### EMQX Enterprise

[EMQX Enterprise](https://www.emqx.com/zh/products/emqx) 是一个强大的企业级物联网消息平台，专为大规模部署和物联网应用中的高可靠性而设计。在油气物联网平台建设中，EMQX Enterprise 通过以下能力为用户带来收益：

- 高可靠性和可扩展性：EMQX Enterprise 采用分布式架构，具有高可用性和可扩展性，可以处理大规模并发消息传输。它支持水平扩展，以适应不断增长的物联网设备和数据流量，确保系统稳定性。
- 丰富的协议支持：除了 MQTT 协议外，EMQX Enterprise 还支持多种消息传输协议。它允许开发人员扩展以支持各种私有协议，以满足其应用需求。
- 数据集成：EMQX Enterprise 与各种数据存储服务、消息队列、云平台和应用无缝集成。它可以连接到云服务，实现远程数据传输和基于云的分析。
- 安全和认证：EMQX Enterprise 提供强大的安全功能，包括TLS/SSL加密传输、客户端认证和访问控制。它支持多种认证方法，如用户名/密码、X.509 证书和 OAuth，确保物联网通信的安全性。
- 规则引擎和数据处理：EMQX Enterprise 具有灵活的规则引擎，可以基于设备数据进行实时数据处理和转发。它支持数据过滤、转换、聚合和持久化等操作，帮助用户根据业务需求进行分析和决策。
- 可视化监控和管理：EMQX Enterprise 提供直观的可视化监控和管理界面，允许用户实时监控物联网设备和消息传输。用户可以查看连接状态、消息流量和其他指标，还可以进行设备管理、故障排除和系统配置操作。

### EMQX ECP

[EMQX ECP（EMQX Edge Cloud Platform）](https://www.emqx.cn/products/emqx-ecp)是专为边缘与云端协作而设计的先进 MQTT 平台。它为您提供对 EMQX 云和边缘服务的高效和有效控制，弥合了边缘设备与云基础设施之间的差距。EMQX ECP 为企业级用户提供了多租户管理、多项目管理、多集群管理、云边协作、身份验证安全和审计需求的解决方案，主要功能包括：

- 边缘与云端协作：EMQX ECP 边缘服务管理实现了对边缘软件 NeuronEX 等边缘服务部署、管理、配置下发、批量操作、监控和优化。通过提供统一的管理平台，实现双向数据传输与运管边能力；
- 多集群管理：在分租户分项目的基础上实现多集群管理，可以创建新集群或纳管已有集群，并对管理的集群进行修复、水平和垂直扩展、修改网络类 型、修改连接数、升降级、集群转让和删除等操作；同时集成 EMQX Dashboard，方便用户直接在 ECP 平台操作和配置 EMQX 集群；
- 多组织多项目管理：EMQX ECP 提供的组织管理功能是一个支持企业级多租户的管理系统，能够实现不同组织的资源隔离和管理。EMQX ECP 可以在同一平台中为多个企业或业务部门提供隔离的 EMQX 数据基础架构服务，每个组织都能够使用自己的数据和配置，互不干扰，保证数据安全。
- 企业级安全性：基于角色的访问控制（RBAC）、单点登录（SSO）、链路加密以及带审计的操作日志确保企业级安全性。
- 监控运维与告警：ECP 的监控平台提供了在云端 EMQX 集群和边缘端进行统一管理和监控的方案。 在 ECP 平台中，系统会收集和分析来自云端 EMQX 集群和边缘端的监控数据，以提供更全 面和精细化的管理和监控进行预测故障和风险。

 



<section class="promotion">
    <div>
        联系 EMQ 解决方案专家
    </div>
    <a href="https://www.emqx.com/zh/contact?product=solutions" class="button is-gradient px-5">联系我们 →</a>
</section>
