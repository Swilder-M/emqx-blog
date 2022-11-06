在大数据时代，数据信息成为了各行各业发展规划的一项重要依据。在工业领域，生产数据可视化的应用亦是将庞大的数据通过可视化分析，再由大屏将数据信息清晰明了地呈现出来。

基于企业环境、设备、产线、车间、系统及能源等生产数据的大数据分析，管理者能通过数据可视化展示的图表方便迅速地了解到工厂发展各个阶段的情况，以加强对当下工厂现状的认识。通过工厂数据可视化所展示出的历史各个时期的数据变化，预判未来发展趋势，结合大数据分析和人工智能技术协助管理者做出决策、制定未来方针和战略，并通过数据的实时反馈迅速进行修正和优化。这些都可以使企业的发展和自身管理水平及市场变化相适应，助力企业持续发展。

## **生产数据可视化应用构建现状分析**

设备实时数据感知与持久化以及价值挖掘是生产数据可视化的关键，目前实际生产数据可视化项目建设主要面临以下问题：

- 海量异构化设备的数据感知

  基于当前物联网、大数据、云计算、人工智能等新一代信息技术的发展，生产数据不再局限于产线的数据采集，而是扩展到整个工厂或园区包括产线、人员、环控、能源、安全、储存、运输、厂房等各个环节的工业物联网设备，关系到效率、产能、质量、安全、耗能、管理等工厂健康生产的核心指标。

  各类工业或物联网设备种类繁多决定了通讯协议的多样性，涉及到的协议有 Modbus、OPC、MQTT、HTTP 等。一个工厂的设备通讯协议往往从几种到十几种甚至更多，且不同厂家的设备信息存在不同的数据结构，导致业务层处理数据困难。

- 流式数据的处理需求

  生产数据从感知到传输的过程中，数据流往往包含了大量无用和冗余的信息，对有限的网络、存储以及计算资源的消耗巨大，大大增加了经济成本。项目建设需要考虑在近设备的边缘端提供数据的清洗、数据预处理及实时逻辑处理等能力。

- 高效的数据持久化需求 

  无论是企业基于大数据、人工智能算法通过生产数据进行生产趋势判断，还是制定企业方针和战略决策，高精度、持久化的历史数据都是准确预测和推理的基础。企业需要在数据海量、结构复杂的前提下，按照业务需求进行高频数据预处理与持久化。

## **EMQ 生产数据可视化解决方案**

基于 EMQ 云边协同的高并发、高吞吐数据基础设施，对整个生产数据传输和处理架构进行优化，可以高效、实时地响应车间、厂级中心、集团云等各层级业务和应用对生产数据的需求。

![EMQ IIoT 云边协同框架](https://assets.emqx.com/images/464db7c12bf2a4ea4ef53459a236b1e3.png)

<center>EMQ IIoT 云边协同框架</center>

1. 边缘端工业协议网关软件 Neuron 实现各类工业设备的接入，可以基于轻量级 MQTT 协议传输，实现在工业生产弱网环境下的各类数据实时感知与稳定传输。为海量异构工业设备、数十种工业协议提供一站式的设备连接、数据接入、MQTT 协议转换，实现工业设备彼此之间及其与工业物联网系统之间的互联互通，从边缘到云端实现对工业设备的数据采集、远程控制、配置更新和设备资产管理。  

   ![多协议工业软网关](https://assets.emqx.com/images/1d70f77bd4c49bc9b290826960f5d9b3.png)

   <center>多协议工业软网关</center>

2. 边缘端轻量级消息总线 NanoMQ 实现数据的汇聚和缓存，可以打通处于不同网络中的设备、不同系统间的数据壁垒，去除信息孤岛。同时，NanoMQ 可以在边缘端实现数据断点续传，保障业务数据的完整性，提升生产数据可视化及其后端大数据算法的高效与精准。

3. 超轻量物联网边缘数据流式分析引擎 eKuiper 实现流式计算、规则引擎、数据清洗、AI 扩展，为生产数据可视化提供数据清洗、数据预处理、事件逻辑处理等具体的能力。工业生产感知的数据是海量的、连续的，如果全部采用批量处理不对其进行分析，难以发掘数据的价值。由于生产数据可视化需要实时反应生产运行状态，所以对数据量和延迟有很高要求，因此通过 eKuiper 采用流处理的方式是更加合适的。

4. 企业级 MQTT 物联网接入平台 EMQX Enterprise 布置在厂级信息中心或集团云，在专网或互联网上为生产数据提供高可用、高并发、低延时的数据传输、分析、对接能力。同时基于规则引擎强大的能力，向生产数据大数据分析的数据库提供高频、可靠、高价值的数据持久化能力。 通过简易、灵活的方式，即可构建起车间、厂级中心、集团云等各层级业务和应用数据消费架构。

   ![image.png](https://assets.emqx.com/images/cf92500d3054becc7350314ce9f8f5b8.png)

   <center>生产数据多级展示消费架构</center>

## **方案优势**

### 一体化的「车间—工厂—集团云」数据消费架构

在车间可视化、厂级可视化、集团级可视化建立统一的数据传输通道，整个数据的传输通过低延时的发布/订阅方式实现，能去除数据孤岛，轻松实现各级可视化应用之间的数据流转和处理流程。基于「车间-工厂-集团云」数据消费架构，新的设备或者其他应用可以非常灵活通过对接数据，对新系统的扩展和创新应用开发提供巨大的便利性。

### 云-边大数据处理基座优化整个系统性能

基于 EMQ 云边模块的灵活部署，能在车间、厂级、集团级独自或者相互协同实现对数据进行实时计算分析、规范报文、过滤清洗、智能告警、业务路由、数据持久化等功能，方便各级可视化应用的搭建。同时，近设备端的边缘运算让数据实现就地清洗、优化和逐层级的价值抽取，大大减少了无价值、冗余数据对网络和存储资源的消耗以及应用端的数据处理负荷，使整个系统数据处理性能提升 40%-80%。

### 云边协同管理提升企业 IT 水平

EMQ 通过云边协同架构将 Neuron、eKuiper 等众多边缘软件进行远程统一管理，无论云边之间网络是直连模式还是穿透模式，都可以方便地实现参数配置、日志查看、实时监控等操作，提升了企业的 IT 管理水平。

## **结语**

基于 EMQ 云边端的生产数据可视化方案，架构起「车间-工厂-集团云」数据高速通道，保障了海量生产数据传输和持久化的实时性、可靠性、安全性，为大数据分析、人工智能应用提供良好的数据基础，同时助力企业快速构建上层应用，加速企业数字化、网络化、智能化转型。





<section class="promotion">
    <div>
        联系 EMQ 工业领域解决方案专家
    </div>
    <a href="https://www.emqx.com/zh/contact?product=solutions" class="button is-gradient px-5">联系我们 →</a>
</section>