# 半小时带你深入了解NFV
- 目录
- [1.NFV概述](#1)
  - [1.1NFV发展背景](#1.1)
  - [1.2什么是NFV](#1.2)
  - [1.3NFV优势](#1.3)
  - [1.4NFV面临的挑战](#1.4)
- [2.NFV体系架构](#2)
  - [2.1NFV系统结构](#2.1)
  - [2.2NFV参考架构](#2.2)
  - [2.3VNF结构](#2.3)
  - [2.4VNF完整实例架构](#2.4)
- [3.VNF开发](#3)
  - [3.1资源申请](#3.1)
  - [3.2C10接口开发](#3.2)
  - [3.3镜像制作](#3.3)
  - [3.4模板配置](#3.4)
- [4.完整流程](#4)
- [附录-名词解释](#5)
- [附录-参考文档](#6)

## <h2 id="1">1.NFV概述</h2>
### <h3 id="1.1">1.1NFV发展背景</h2>
![NFV发展背景](./img/NFV发展背景.jpg)
- a) 新型网络业务迅速发展导致流量急速增加（IoV、IoT、AI、VR等）
- b) 运营商部署大量专用网络设备满足需求（专用设备软硬件一体化、扩展受限且设备管理低效）
- c) 新增网络服务时，需开发新设备（周期漫长，且需提供机房空间、供电及维护等）
- d) 运营商业务量增幅与业务收入增幅出现“剪刀差”,同时CAPEX/OPEX不断上升
- e) 互联网OTT(Over The Top)业务的大规模发展，导致运营商收入大幅度减少。
- f) 为获取一种低成本、高收入的运营方式，网络运营商们提出了NFV
### <h3 id="1.2">1.2什么是NFV</h3>
- 概念
    > NFV，即网络功能虚拟化，Network Function Virtualization。
      NFV是利用IT虚拟化技术将现有的网络设备功能整合进标准的服务器、存储和交换机等设备，以软件的形式实现网络功能，以此取代目前网络中私有、专用和封闭的网元设备。  
      电信网络将借助NFV技术提高网络设备利用率，降低运营商OPEX/CAPEX支出，加快业务创新的步伐，为用户带来更佳的业务使用体验，以此构造一个有竞争力的、创新的开放生态系统。
- 图解
![什么是NFV](./img/什么是NFV.jpg)   
- 传统网络及未来愿景      
![传统网络节点设备](./img/传统网络节点设备.jpg)      

### <h3 id="1.3">1.3NFV优势</h3>
- 1.3.1工业标准硬件
    - 降低设备更新周期
    - 降低设备成本
    - 标准设备易于维护
    - 直接降低OPEX/CAPEX
- 1.3.2网络功能软件
    - 缩短新应用开发周期
    - 降低实验阶段成本价格
    - 提高新业务、新应用失败容忍度
    - 虚拟资源代替物理设备，节省能源、空间
    - 自动化管理应用，降低运维管理成本
    - 软硬件解耦，灵活部署，加快业务推向市场
    - 自动弹性扩缩容
- 总的来说，降低OPEX和CAPEX，同时加速创新步伐。
### <h3 id="1.4">1.4NFV面临的挑战</h3>
![NFV面临挑战](./img/NFV面临挑战.jpg)
- 1.4.1性能问题
    - 描述：电信级的网元设备要求高可靠性、高性能（5～6个9）。COTS(商用现成品或技术)通用服务器2～3个9
    - 解决方案：VNF设计上采用多线程、独立存储结构、独立的网络协议栈
- 1.4.2虚拟功能间的连接问题
    - 描述：传统直接连，NFV需要考虑是否在同一个物理服务器、是否在同一个局域网
    - 解决方案：SR-IOV单根虚拟化技术
- 1.4.3网络安全问题
    - 描述：虚拟环境下，安全攻击的可能性大大增加
    - 解决方案：虚拟环境域、计算域、基础设施域做相应的安全防护
- 1.4.4NFV标准问题
    - 描述：新架构产生新的逻辑功能单元(Orchestrator、VNFM、VIM),各单元之间的交互需要新的接口，哪些需要标准化并开放
- 1.4.5系统集成问题
    - 描述：各逻辑功能单元可能由不同的厂家提供，集成在一起部署业务系统是一个很复杂的问题
- 1.4.6运营商业务引入NFV问题 
     - 描述：需要考虑技术路线、网络架构、业务部署、运维模式等
## <h2 id="2">2.NFV体系架构</h2>
### <h3 id="2.1">2.1NFV系统结构</h3>
![NFV系统结构](./img/NFV系统结构.jpg)
### <h3 id="2.2">2.2NFV参考架构</h3>
![NFV系统结构](./img/NFV参考架构.jpg)
### <h3 id="2.3">2.3VNF结构</h3>
![VNF结构](./img/VNF结构.jpg)
### <h3 id="2.4">2.4VNF完整实例架构</h3>
![VNF完整实例架构](./img/一二级部署拓扑图.png)
## <h2 id="3">3.VNF开发</h2>
### <h3 id="3.1">3.1资源申请</h3>
- 计算资源（虚拟机CPU、内存、硬盘、个数等）
- 网络资源（根据各业务系统情况申请内网外网的IP段、子网掩码、网关等）
### <h3 id="3.2">3.2C10接口开发</h3>
- 鉴权（uri: /v1/vnf/authentication/token）
- 查询实例状态（uri: /v1/vnf/instances/event/）
- 配置信息下发（uri: /v1/vnf/instances/configuration）
- 服务中止通知（uri: /v1/vnf/instances/terminate）
- 准备扩缩容通知（uri: /v1/vnf/instances/scaling/notify）
- 扩缩容完成童子（uri: /v1/vnf/instances/scaling/complete）
- 配置信息修改后下发（uri: /v1/vnf/instances/configuration）
- 服务器性能指标查询（uri: /v1/vnf/instances/vdus/）
### <h3 id="3.3">3.3镜像制作</h3>
- 方法一：命令行执行
    >  先下载CentOS-7-x86_64-Minimal-1708.iso  
       qemu-img create -f qcow2 c10.qcow2 100G  
       virt-install -n centosimg -r 8196 --cpu host -c CentOS-7-x86_64-Minimal-1708.iso --check disk_size=off --disk path=/openstack-image/ydhy_nlkf_vnf.qcow2,device=disk,bus=virtio,size=100,format=qcow2 --vnc --vncport=5900 --vnclisten=0.0.0.0 -v  
       VNC-viewer完成虚拟机的创建  
       部署业务系统  
       关掉虚拟机后得到c10.qcow2镜像文件
- 方法二：linux服务器中用虚拟系统管理器（简单方便）
![linux虚拟系统管理器](./img/linux虚拟系统管理器.jpg)
### <h3 id="3.4">3.4模板配置</h3>
- VNF信息
- VDU信息
- 网络信息
- 弹性扩缩容信息
## <h2 id="4">四、完整流程</h2>
- 登录VNFM系统
- 上传模板
- 上传镜像
- 开始部署
- 访问业务
## <h2 id="5">附录-名词解释</h2>
- NFV
> NFV，即网络功能虚拟化，Network Function Virtualization。通过使用x86等通用性硬件以及虚拟化技术，来承载很多功能的软件处理。从而降低网络昂贵的设备成本。可以通过软硬件解耦及功能抽象，使网络设备功能不再依赖于专用硬件，资源可以充分灵活共享，实现新业务的快速开发和部署，并基于实际业务需求进行自动部署、弹性伸缩、故障隔离和自愈等。
  典型应用是一些CPU密集型功能，并且对网络吞吐量要求不高的情形。主要评估的功能虚拟化有：WAN加速器，信令会话控制器，消息路由器，IDS,DPI，防火墙，CG-NAT, SGSN/GGSN, PE, BNG, RAN等。
- SDN
> 软件定义网络（Software Defined Network, SDN ），是Emulex网络一种新型网络创新架构，是网络虚拟化的一种实现方式，其核心技术OpenFlow通过将网络设备控制面与数据面分离开来，从而实现了网络流量的灵活控制，使网络作为管道变得更加智能。
- OpenFlow
> 随着互联网的发展，今天的互联网业务对互联网提出了越来越高的传输质量要求，如何修改互联网以满足新业务的需求，出现了改良派和改革派两种不同的做法。改良派认为可以在原有的基础设施上添加新的协议来解决问题，改革派则认为必须推倒一切重来。改革派提出这样的两个问题：“就目前掌握的知识，如果我从一个全新的开始设计互联网，我会怎么做”和“15年后的互联网应该是什么样子”。为此，改革派们开始了一系列新的设计方案，OpenFlow就是改革派提出的一种新型网络交换模型，与此相应的，他们还成立了OpenFlow交换机论坛(The OpenFlow Switch Consortium，后文简称OpenFlow论坛)。  
- IMS
> IMS(IP Multimedia Subsystem)是IP多媒体系统,是一种全新的多媒体业务形式，它能够满足的终端客户更新颖、更多样化多媒体业务的需求。IMS被认为是下一代网络的核心技术，也是解决移动与固网融合，引入语音、数据、视频三重融合等差异化业务的重要方式。但是，全球IMS网络多数处于初级阶段，应用方式也处于业界探讨当中。
- CSCF
> CSCF呼叫会话控制功能（Call Session Control Function）是IP多媒体子系统（IMS：IP Multimedia Subsystem）内部的功能实体，是整个IMS网络的核心。主要负责处理多媒体呼叫会话过程中的信令控制。它管理IMS网络的用户鉴权、IMS承载面QoS、与其它网络实体配合进行SIP会话的控制，以及业务协商和资源分配等。
- VOLTE 
> VoLTE是基于IMS的语音业务。IMS由于支持多种接入和丰富的多媒体业务，成为全IP时代的核心网标准架构。经历了过去几年的发展成熟后，如今IMS已经跨越裂谷，成为固定话音领域VoBB、PSTN网改的主流选择，而且也被3GPP、GSMA确定为移动语音的标准架构。VoLTE即Voice over LTE，它是一种IP数据传输技术，无需2G/3G网，全部业务承载于4G网络上，可实现数据与语音业务在同一网络下的统一。换言之，4G网络下不仅仅提供高速率的数据业务，同时还提供高质量的音视频通话，后者便需要VoLTE技术来实现。
- CS
> 电路交换（CS:circuit switching）是通信网中最早出现的一种交换方式，也是应用最普遍的一种交换方式，主要应用于电话通信网中，完成电话交换，已有100多年的历史。
电话通信的过程是：首先摘机，听到拨号音后拨号，交换机找寻被叫，向被叫振铃同时向主叫送回铃音，此时表明在电话网的主被叫之间已经建立起双向的话音传送通路；当被叫摘机应答，即可进入通话阶段；在通话过程中，任何一方挂机，交换机毁拆除已建立的通话通路，并向另一方送忙音提示挂机，从而结束通话。
从电话通信过程的描述可以看出，电话通信分为三个阶段：呼叫建立、通话、呼叫拆除。电话通信的过程，即电路交换的过程，因此，相应的电路交换的基本过程可分为连接建立、信息传送和连接拆除三个阶段。
- EPC（4G核心网络）
> 4G作为第四代移动通信技术，有着无法比拟的优越性，它能够快速传输语音、文本、视频和图像信息，能够满足几乎所有用户对于无线服务的要求，国际电信联盟对于4G系统的标准为符合100 Mbit/s数据传输速度的系统，当之无愧的被称为机器之间的高速对话。LTE（Long Term Evolution）主要研究3GPP无线接入网的长期演进技术，升级版的LTE Advanced将最终满足国际电信联盟对4G系统的要求，SAE（System Architecture Evolution）则是研究核心网的长期演进，它定义了一个全IP的分组核心网EPC（Evolved Packet Core），该系统的特点为仅有分组域而无电路域、基于全IP结构、控制与承载分离且网络结构扁平化，其中主要包含MME、SGW、PGW、PCRF等网元。其中SGW和PGW常常合设并被称为SAE-GW。  
   EPC主要由MME、SGW、PGW、PCRF等网元构成。其中：  
   MME：Mobility Management Entity，原3G网络中SGSN网元的控制面功能；  
   SGW：Serving Gateway，原3G网络中SGSN网元的用户面功能，有时也写为S-GW；  
   PGW：PDN Gateway，原3G网络中GGSN网元的功能，有时也写为P-GW；  
   PCRF：Policy and Charging Rules Function，完成对用户数据报文的策略和计费控制。
- VAS 
> value added service 表示一种增值服务     
-  网元
> 网元由一个或多个机盘或机框组成， 能够独立完成一定的传输功能。网管系统中的网元其实和这个差不多，简单理解就是网络中的元素，网络中的设备。总之，网元是网络管理中可以监视和管理的最小单位，值得注意的是，网络元素和网元和被管设备是同义语，但被管设备容易被人理解成硬件。   
  如PDH设备、SDH-ADM、DACS、TEM、REG、PCM等等。
- GSM(全球移动通信系统)
> 全球移动通信系统Global System for Mobile Communication就是众所周知的GSM，是当前应用最为广泛的移动电话标准。全球超过200个国家和地区超过10亿人正在使用GSM电话。GSM标准的无处不在使得在移动电话运营商之间签署"漫游协定"后用户的国际漫游变得很平常。 GSM 较之它以前的标准最大的不同是它的信令和语音信道都是数字式的，因此GSM被看作是第二代 (2G)移动电话系统。 这说明数字通讯从很早就已经构建到系统中。GSM是一个当前由3GPP开发的开放标准。
   2015年，全球诸多GSM网络运营商，已经将2017年确定为关闭GSM网络的年份。
- 3GPP 
> 第三代合作伙伴计划。3GPP的目标是实现由2G网络到3G网络的平滑过渡，保证未来技术的后向兼容性，支持轻松建网及系统间的漫游和兼容性。 其职能： 3GPP主要是制订以GSM核心网为基础，UTRA(FDD为W-CDMA技术，TDD为TD-CDMA技术)为无线接口的第三代技术规范。   
- UMTS(通用移动通信系统)
> 一种第三代（3G）移动电话技术。通用移动通信系统，简称UMTS(Universal Mobile Telecommunications System)，UMTS作为一个完整的3G移动通信技术标准，UMTS并不仅限于定义空中接口。除WCDMA作为首选空中接口技术获得不断完善外，UMTS还相继引入了TD-SCDMA和HSDPA技术。
- LTE(长期演进技术)
> LTE（Long Term Evolution，长期演进)是由3GPP（The 3rd Generation Partnership Project，第三代合作伙伴计划）组织制定的UMTS（Universal Mobile Telecommunications System，通用移动通信系统）技术标准的长期演进，于2004年12月在3GPP多伦多会议上正式立项并启动。
- BRAS
> 宽带远程接入服务器（Broadband Remote Access Server,简称BRAS）是面向宽带网络应用的新型接入网关，它位于骨干网的边缘层，可以完成用户带宽的IP/ATM网的数据接入（目前接入手段主要基于xDSL/Cable Modem/高速以太网技术（LAN）/无线宽带数据接入(WLAN)等），实现商业楼宇及小区住户的宽带上网、基于IPSec（IP Security Protocol）的IP VPN服务、构建企业内部Intranet、支持ISP向用户批发业务等应用。
- OLT
> OLT: optical line terminal(光线路终端），用于连接光纤干线的终端设备。
- BBU
> RRU（射频拉远单元）和BBU（基带处理单元）之间需要用光纤连接。一个BBU可以支持多个RRU。采用BBU+RRU多通道方案，可以很好地解决大型场馆的室内覆盖。
- PGW
> PGW（PDN GateWay，PDN网关）是移动通信网络EPC中的重要网元。EPC网络实际上是原3G核心网PS域的演进版本，而PGW也相当于是一个演进了的GGSN网元，其功能和作用与原GGSN网元相当。
- CDN
> CDN的全称是Content Delivery Network，即内容分发网络。其基本思路是尽可能避开互联网上有可能影响数据传输速度和稳定性的瓶颈和环节，使内容传输的更快、更稳定。通过在网络各处放置节点服务器所构成的在现有的互联网基础之上的一层智能虚拟网络，CDN系统能够实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上。其目的是使用户可就近取得所需内容，解决 Internet网络拥挤的状况，提高用户访问网站的响应速度。
- HSS
> Home Subscriber Server，即HSS服务器缩写，是IMS（IP Multimedia Subsystem，IP多媒体子系统）中控制层的重要组成部分。
- MME
> MME（Mobility Management Entity）是3GPP协议LTE接入网络的关键控制节点，它负责空闲模式的UE(User Equipment)的定位，传呼过程，包括中继，简单的说MME是负责信令处理部分。
  它涉及到bearer激活/关闭过程，并且当一个UE初始化并且连接到时为这个UE选择一个SGW(Serving GateWay)。通过和HSS交互认证一个用户，为一个用户分配一个临时ID。MME同时支持在法律许可的范围内，进行拦截、监听。MME为2G/3G接入网络提供了控制函数接口，通过S3接口。为漫游UEs，面向HSS同样提供了S6a接口
- OSS （运营支撑系统）
> OSS是一个综合的业务运营和管理平台，同时也是真正融合了传统IP数据业务与移动增值业务的综合管理平台。OSS是电信运营商的一体化、信息资源共享的支持系统，它主要由网络管理、系统管理、计费、营业、账务和客户服务等部分组成，系统间通过统一的信息总线有机整合在一起。它不仅能在帮助运营商制订符合自身特点的运营支撑系统的同时帮助确定系统的发展方向，还能帮助用户制订系统的整合标准的，改善和提高用户的服务水平。  
- NB-IoT 
> 基于蜂窝的窄带物联网（Narrow Band Internet of Things, NB-IoT）成为万物互联网络的一个重要分支。NB-IoT构建于蜂窝网络，只消耗大约180KHz的带宽，可直接部署于GSM网络、UMTS网络或LTE网络，以降低部署成本、实现平滑升级。
- 蜂窝网络 
> 蜂窝网络或移动网络(Cellular network)是一种移动通信硬件架构，把移动电话的服务区分为一个个正六边形的小子区，每个小区设一个基站，形成了形状酷似“蜂窝”的结构，因而把这种移动通信方式称为蜂窝移动通信方式。
- MANO
> MANO系统是ETSI提出的全新的云管理系统，该系统主要包括云管理所需的网元的生命周期管理功能、网元所需的加载模板和安装包的管理、网络资源的管理和分配等功能。
- EMS （网元管理系统）
> EMS（Element Management System，网元管理系统）是管理特定类型的一个或多个电信NE（Network Element，网络单元）的系统。
- VIM
> Virtualised Infrastructure Manager虚拟化基础设施管理器
- ETSI
> 欧洲电信标准化协会（ETSI）(European Telecommunications Standards Institute)是由欧共体委员会1988年批准建立的一个非营利性的电信标准化组织，总部设在法国南部的尼斯。ETSI的标准化领域主要是电信业，并涉及与其他组织合作的信息及广播技术领域。ETSI作为一个被CEN（欧洲标准化协会）和CEPT（欧洲邮电主管部门会议）认可的电信标准协会，其制定的推荐性标准常被欧共体作为欧洲法规的技术基础而采用并被要求执行。
## <h2 id="6">附录-参考文档</h2>
- [linux添加开机自启动脚本示例详解](https://blog.csdn.net/linuxshine/article/details/50717272)
- [CentOS 7.0系统安装配置图解教程](https://www.osyunwei.com/archives/7829.html)
- [shell脚本修改文件内容时遇到的坑](https://www.cnblogs.com/ljhoracle/p/6277064.html)
- [Linux下安装Tomcat服务器和部署Web应用](https://www.cnblogs.com/xdp-gacl/p/4097608.html)
- [Linux配置浮动IP实现WEB高可用](https://www.cnblogs.com/victorwu/p/7061168.html)
- [负载均衡选型](https://blog.csdn.net/lovewebeye/article/details/52846624)
- [基于nginx + lua实现的反向代理动态更新](https://www.cnblogs.com/shihuc/p/8044753.html)
- [OpenStack Virtual Machine Image Guide](https://docs.openstack.org/image-guide/)
- [命令行利用KVM创建虚拟机(virt-install)](http://blog.51cto.com/tianhao936/1343767)
- [shell脚本获取配置文件中的内容](https://blog.csdn.net/wyl9527/article/details/72578473)
- [十张图看懂SDN与NFV的区别与联系](http://www.doit.com.cn/p/255330.html)