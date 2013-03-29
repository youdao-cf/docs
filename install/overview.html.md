---
title: installation overview
---


## 为什么要重新写安装文档？

cloudfoundry 有两种安装方式： vcap（使用chenf-solo安装，支持物理机和虚拟机） 和 bosh安装（仅支持虚拟机）

cloudfoury官方推荐的是bosh，但是这个需要你已经拥有一个类似openstack这样的虚拟机集群，并不是每个人都有这个条件的。 加上官方现在正在开发2.0 the next generation ，vcap 不再继续进行开发，如果你想使用2.0 那只能使用bosh。


其次，官方的cf是只支持ubuntu10.04的，事实上，cf的所有模块理论上都应该是可以单独安装，不局限于哪个linux发行版本的。

再者，如果来研究cf，分离安装的方式更容易理解。



主要模块：

* nats-server
* warden
* dea
* router
* cloud_controller
* health_manager
* uaa

note： nats-server是消息总线，需要首先安装，由于dea在2.0以来warden，所以warden需要在dea之前安装，剩下的模块没有安装顺序


---
