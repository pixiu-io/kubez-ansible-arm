# Kubez-ansible Overview

该项目为 [kubez-ansible](https://github.com/pixiu-io/kubez-ansible) 子项目，用于支持国产化系统和ARM架构的离线部署

![Build Status][build-url]
[![Release][release-image]][release-url]
[![License][license-image]][license-url]

## 支持系统列表
todo

## Supported Components
- 集群指南
  - [单节点集群](docs/install/all-in-one.md) 单节点集群的快速部署
  - [多节点集群](docs/install/multinode.md) 多节点集群部署 (1master + 多node)
  - [高可用集群](docs/install/availability.md) 高可用集群部署 (3master + 多node)
  - [离线部署](https://github.com/pixiu-io/kubez-ansible-offline)
  - [扩容](docs/install/expansion.md)
  - [销毁](docs/install/destroy.md)

- 网络插件
  - [flannel](https://github.com/flannel-io/flannel)
  - [calico](https://github.com/projectcalico/calico)

- 容器运行时
  - [docker](https://github.com/docker)
  - [containerd](https://github.com/containerd/containerd)

- 云原生应用
  - 基础应用
    - [Helm3](docs/apply/helm3-guide.md)
    - [Nginx Ingress](docs/apply/ingress.md)
    - [Dashboard](docs/apply/dashboard.md)
    - [Metrics Server](docs/apply/metrics.md)

## 沟通交流
- 搜索微信号 `yingjuncz`, 备注（github）, 验证通过会加入群聊
- [bilibili](https://space.bilibili.com/3493104248162809?spm_id_from=333.1007.0.0) 技术分享

Copyright 2019 caoyingjun (cao.yingjunz@gmail.com) Apache License 2.0

[build-url]: https://github.com/gopixiu-io/kubez-ansible/actions/workflows/ci.yml/badge.svg
[release-image]: https://img.shields.io/badge/release-download-orange.svg
[release-url]: https://www.apache.org/licenses/LICENSE-2.0.html
[license-image]: https://img.shields.io/badge/license-Apache%202-4EB1BA.svg
[license-url]: https://www.apache.org/licenses/LICENSE-2.0.html
