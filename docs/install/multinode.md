# 多节点集群
本文档只是部署 1 master + 多 node 的场景， master 高可用的参考[高可用集群部署](docs/install/availability.md)

### 依赖条件
- [依赖安装](prerequisites.md)

### 部署步骤
1. 检查虚拟机默认网卡配置:

   a. 默认网卡为 `eth0`, 如果环境实际网卡不是 `eth0`，则需要手动指定网卡名称:
   ``` bash
    编辑 /etc/kubez/globals.yml 文件，取消 network_interface: "eth0" 的注解，并修改为实际网卡名称
   ```

2. 确认集群环境连接地址:

   a. 内网连接: 无需更改

   b. 公网地址:
   ``` bash
   编辑 /etc/kubez/globals.yml 文件,取消 #kube_vip_address: "172.16.50.250" 的注解,并修改为实际公网地址(高可用场景时为 LB 地址), 云平台环境需要放通公网ip到后端 master 节点的6443端口
   ```

3. 配置工作目录下的 `multinode` 配置文件, 根据实际情况添加主机信息, 并完成如下配置

    - 配置部署节点的 `/etc/hosts`, 添加 kubernetes 节点的ip和主机名解析
    - multinode 配置格式，推荐:
      * 如果 cri 选择 docker，则仅需配置 [docker-master] 和 [docker-node]
      ```shell
      [docker-master]
      kube01

      [docker-node]
      kube02

      [storage]
      kube01
      ```
      * 如果 cri 选择 containerd，则仅需配置 [containerd-master] 和 [containerd-node]
      ```shell
      [containerd-master]
      kube01

      [containerd-node]
      kube02

      [storage]
      kube01
      ```

4. 打通`部署节点`(运行 `kubez-ansible` 的节点) 到其他 `node` 节点的免密登陆 [批量开启免密登陆](auth-key.md) 或者 [配置密码/密钥](passwd-key.md)

5. (可选)修改 kubernetes 镜像仓库
    ``` bash
    编辑 /etc/kubez/globals.yml 文件，修改 image_repository: "" 为期望镜像仓库，默认是阿里云 registry.cn-hangzhou.aliyuncs.com/google_containers
    ```

6. (可选)修改基础应用镜像仓库
    ```bash
    编辑 /etc/kubez/globals.yml 文件，修改 app_image_repository: "" 为期望镜像仓库，默认是 pixiu镜像仓库 harbor.cloud.pixiuio.com/pixiuio
    ```

7. 执行如下命令，进行 `kubernetes` 的依赖安装
    ``` bash
    kubez-ansible -i multinode bootstrap-servers
    ```

8. 根据实际需要，调整配置文件 `/etc/kubez/globals.yml`
    ```bash

    cluster_cidr: "172.30.0.0/16"  # pod network
    service_cidr: "10.254.0.0/16"  # service network

    # network cni, 现支持flannel 和 calico, 默认是 flannel
    enable_calico: "no"
    ```

9. 执行如下命令，进行 `kubernetes` 的集群安装
    ``` bash
    kubez-ansible -i multinode deploy
    ```

10. 验证环境
   ```bash
   [root@kube01 ~]# kubectl get node
   NAME     STATUS   ROLES                  AGE     VERSION
   kube01   Ready    control-plane,master   21h     v1.23.6
   kube02   Ready    <none>                 21h     v1.23.6
   kube03   Ready    <none>                 3h48m   v1.23.6
   ```

11. (可选)启用 kubectl 命令行补全
    ``` bash
    kubez-ansible -i multinode post-deploy
    ```
