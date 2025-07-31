# Dify计算巢快速部署


>**免责声明：**本服务由第三方提供，我们尽力确保其安全性、准确性和可靠性，但无法保证其完全免于故障、中断、错误或攻击。因此，本公司在此声明：对于本服务的内容、准确性、完整性、可靠性、适用性以及及时性不作任何陈述、保证或承诺，不对您使用本服务所产生的任何直接或间接的损失或损害承担任何责任；对于您通过本服务访问的第三方网站、应用程序、产品和服务，不对其内容、准确性、完整性、可靠性、适用性以及及时性承担任何责任，您应自行承担使用后果产生的风险和责任；对于因您使用本服务而产生的任何损失、损害，包括但不限于直接损失、间接损失、利润损失、商誉损失、数据损失或其他经济损失，不承担任何责任，即使本公司事先已被告知可能存在此类损失或损害的可能性；我们保留不时修改本声明的权利，因此请您在使用本服务前定期检查本声明。如果您对本声明或本服务存在任何问题或疑问，请联系我们。

## 概述

Dify.AI 是一款 LLMOps 平台，帮助开发者更简单、更快速地构建 AI 应用。它的核心理念是通过可声明式的 YAML 文件定义 AI 应用的各个方面，包括 Prompt、上下文和插件等。Dify 提供了可视化的 Prompt 编排、运营、数据集管理等功能。这些功能使得开发者能够在数天内完成 AI 应用的开发，或将 LLM 快速集成到现有应用中，并进行持续运营和改进，创造一个真正有价值的 AI 应用。
本方案提供了部署Dify的最佳实践，基于阿里云ACK部署的Dify支持高可用、弹性伸缩等能力，且同时支持集成云数据库, 可以实现Dify稳定、高性能部署，推荐用于生产环境。

## 前提条件

部署Dify社区版服务实例，需要对部分阿里云资源进行访问和创建操作。因此您的账号需要包含如下资源的权限。
  **说明**：当您的账号是RAM账号时，才需要添加此权限。

| 权限策略名称                          | 备注                     |
|---------------------------------|------------------------|
| AliyunECSFullAccess             | 管理云服务器服务（ECS）的权限       |
| AliyunVPCFullAccess             | 管理专有网络（VPC）的权限         |
| AliyunROSFullAccess             | 管理资源编排服务（ROS）的权限       |
| AliyunCSFullAccess             | 管理容器服务（CS）的权限   |
| AliyunROSFullAccess             | 管理资源编排服务（ROS）的权限       |
| AliyunKvstoreFullAccess             | 管理云数据库Tair（兼容 Redis）的权限      |
| AliyunRDSFullAccess             | 管理云数据库服务（RDS）的权限      |
| AliyunOTSFullAccess             | 管理 TableStore 的权限      |
| AliyunComputeNestUserFullAccess | 管理计算巢服务（ComputeNest）的用户侧权限 |


## 计费说明

Dify社区版在计算巢部署的费用主要涉及：

- 所选vCPU与内存规格
- 系统盘类型及容量
- 公网带宽
- 所选的云数据库的规格

## 部署架构
<img src="22.jpg" width="1500" height="700" align="bottom"/>

  Dify应用的组件主要包括业务组件和基础组件两大类，业务组件包括：api/worker、web、sandbox。基础组件包括：db、verctor db、redis、nginx、ssrf_proxy。
此方案中基础组件既可以选择社区开源的redis、postgres以及weaviate，也可以选择阿里云的云数据库，如果您对这些基础组件有更高的性能、功能和SLA要求、或者对基础组件有运维和管理的压力，推荐使用云数据库。

## 部署流程
1. 访问计算巢Dify社区版[部署链接](https://computenest.console.aliyun.com/user/cn-hangzhou/serviceInstanceCreate?ServiceId=service-c8afb895dd314f70a020)按提示填写部署参数： 
 模板选择"高可用版"，配置Kubernetes, 如果想使用已有的ACK集群，需要选择ACK集群Id,为了提高部署成功率和稳定性，推荐使用新建容器集群，如果选择已有ACK,最后需要手动进行服务访问配置。
    ![image.png](9.png)
 若选择"新建容器集群"，需要配置节点规格、节点密码等。
    ![image.png](10.png)
配置Postgres数据库，需要进行相关配置
  ![image.png](11.png)
配置Reids数据库，需要进行相关配置
  ![image.png](12.png)
配置TablesStore，保持默认值即可
配置网络参数，可以选择新建专有网络, 也可以选择已有专有网络，选择两个可用区，提高服务可用性。
  ![image.png](14.png)
进行服务相关配置，如果您有可用的域名，可填写您自己的域名，否则默认使用计算巢提供的测试域名，测试域名需要绑定host后使用，否则无法访问。
  如果需要开启TLS配置，需要上传TLS证书
  建议开启高可用配置，开启后可以提高应用的容灾能力和对峰值负载的处理能力。
2. 参数填写完成后可以看到对应询价明细，确认参数后点击**下一步：确认订单**。 确认订单完成后同意服务协议并点击**立即创建**
   进入部署阶段。
    ![image.png](16.png)
3. 等待部署完成后就可以开始使用服务，进入服务实例详情可查看使用说明和Dify访问页面，若使用域名，需要按照使用说明的提示绑定host或配置解析后使用
    ![image.png](17.png)
4. 注册账号。
    ![image.png](6.png)
5. 登录就能创建自己的dify应用了
    ![image.png](7.png)

## 使用说明
  ### 配置访问入口
  如果选择已有ACK集群进行部署，需要手动配置访问入口, 推荐两种方式
  
  **方式一：** 通过负载均衡访问，按照如下步骤进行配置
  ![img.png](18.png)
  配置完成后，您会看到ack-dify服务的外部IP地址（External IP），将该外部IP地址输入浏览器地址栏即可访问Dify服务。
  ![img.png](19.png)

  **方式二：** 通过配置Ingress访问，按照如下步骤进行配置
  首先集群中安装Nginx Ingress Controller组件，再配置相关路由，按照如下步骤配置
  ![img.png](20.png)
  配置完成后，您会看到Ingress的端点，将此端点对域名绑定host或配置解析后，即可通过域名访问Dify。
  ![img.png](21.png)

 ### 配置Dify 服务的弹性伸缩
   如果希望实现弹性扩缩容，
   节点的弹性扩缩容可以参考此文档进行[配置](https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/auto-scaling-of-nodes?spm=a2c4g.11186623.help-menu-85222.d_2_12_1_0.9ae546c6P5Pf9i)。
   负载的弹性伸缩可以打开部署页面的“高可用”开关
## 常见问题
1. 服务实例删除失败：如果服务实例删除时，会出现报错：DELETE_FAILED: The instance is not empty. ErrorCode: OTSValidationFail，原因是向量数据库Tablestore中有数据，Tablestore中的数据不支持自动删除，需要手动到[Tablestore控制台](https://otsnext.console.aliyun.com/cn-guangzhou/totalList), 找到对应的实例，删除所有的数据表。
