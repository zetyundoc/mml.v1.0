# 安装基础组件

## 环境准备
使用MML安装包自动化部署集群，需要准备以下部署环境，请严格按照文档操作以保证能够部署成功。

### 开启SSH服务
在MML的安装过程中，需要通过SSH登录各主机，因此需要在集群中每个主机上启动SSH服务。
1. 启动SSH服务

```
# service sshd start
```
2. 将SSH设置为自启动服务。

```
# chkconfig sshd on
```

### 设置无密码SSH登录

在MML的安装过程中，需要通过BASE1基础节点将MML自动部署到集群中其他管理节点和计算节点上，因此需要在BASE1主机和集群中所有其他主机之间对root用户设置无密码的SSH连接。
设置无密码的SSH连接，在BASE1节点上按照以下步骤操作：

1. 在BASE1主机上生成公有和私有SSH密钥。

    ```
    # ssh-keygen -t rsa
    ```
2. 把BASE1主机的公钥复制到远程机器的authorized_keys文件中(包括BASE1主机)。

    ```
    # ssh-copy-id <remote_host>
    ```
    说明：
        * 其中<remote_hosts>是指包括BASE1主机在内的集群内所有主机的IP地址，而不是主机名。
        * 执行该命令后需要输入yes并回车，然后根据提示输入远程主机的SSH密码。
        * 执行完该命令后，BASE1的root用户可以无密码登录其它节点。

### 检查网络和主机名
* 查询主机IP地址

    使用ip address命令分别查询每个主机的IP地址，接下来在MML的安装过程中需要配置这些IP地址(每个IP地址与主机上指定的网卡相绑定)，而不是配置弹性IP、映射IP或者虚拟IP，否则安装过程将会出现异常。

    ```
    # ip addr
    ```
* 检查主机是否在同一局域网

    MML要求集群内所有主机必须在同一局域网内且可以正常通信，否则MML的安装和使用都会受到影响。

    ```
    # ping <remote_host>
    ```
    说明：其中<remote_hosts>是指集群内的所有主机的IP地址，并且这个IP地址一定是属于主机的某个网卡的，而不是弹性IP、映射IP或者虚拟IP。

* 设置主机名

    使用hostnamectl命令在集群中每个主机上设置主机名。例如:

    ```
    # hostnamectl set-hostname=<fully.qualified.domain.name> --static
    ```
    说明：其中<full.qualified.domain.name>是指完全合格域名/全称域名，由主机名+域名组成。故而设置主机名时，最好加上域名，如base1.zetyun.com，而不是直接使用base1作为主机名。





