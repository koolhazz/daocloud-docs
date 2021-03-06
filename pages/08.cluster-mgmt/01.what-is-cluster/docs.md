---
title: 集群的概念与自有主机
taxonomy:
    category:
        - docs
process:
    twig: true
---

#### 集群

集群，是 DaoCloud 平台上资源的结合。集群为用户提供了统一管理计算资源的一种方式。

![](cluster.png?resize=800)

集群是一个逻辑概念，您可以创建自有集群，并向集群中添加属于自己的主机。集群用来区分不同目的的资源和应用交付目的地，比如供团队内部测试和交付的部门测试集群、位于公司私有云或公有云之上的大规模应用预发布平台。

使用容器化软件交付，在完成镜像构建后，我们可以非常方便的把一个或者一组镜像部署到不同的集群之上，用于不同的交付目的。

#### 自有主机

您可以通过自有主机功能将您的主机接入 DaoCloud 集群。

自有主机管理服务是国内首个支持包括公有云、私有云、虚拟化平台和物理服务器，并且可跨云跨网进行混合式管理的容器服务平台，该项服务能够与 DaoCloud 云端持续集成、镜像构建、应用发布流程无缝对接，实现容器化应用在公有云、私有云的灵活交付。

混合式容器主机管理服务是 DaoCloud 的一项独创技术，使用这项功能，DaoCloud 用户可以通过一致的界面和流程，管理在公有云、私有云甚至是企业防火墙之后的各类物理和虚拟主机资源，把这些资源汇聚成跨云跨网的分布式容器主机资源池，实现容器化应用的高速部署和灵活调度。DaoCloud 已经支持包括微软 Azure、亚马逊 AWS、阿里云、UCloud、青云等国内外一线公有云厂商，并且正在帮助企业客户实施内网私有云容器集群的混合式管理，为企业客户打造能够支持微服务和轻应用的新一代混合式容器云平台。

了解更多：
* [接入自有主机](http://docs.daocloud.io/cluster-mgmt/add-node-by-script)
* [部署应用到自有主机](http://docs.daocloud.io/deploy-app-to-own-machine/deploy-to-cluster)

#### 创建新集群

1. 在「我的集群」界面，点击右下角的绿色按钮，「添加新集群」
2. 在弹出的界面，输入集群名称，（名称输入后，可以在集群管理界面进行修改）
3. 点击创建集群，至此，您的集群已经创建完毕，可以向此集群添加或者导入主机

![](add%20cluster.png?resize=800)

我们可以向集群添加胶囊主机、来自公有云镜像市场的主机和通过脚本命令手动添加主机。
后续的文档会为您做详细的介绍。

![](newcluster.png?resize=800)