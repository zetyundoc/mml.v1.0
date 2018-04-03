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

    使用**ip address**命令分别查询每个主机的IP地址，接下来在MML的安装过程中需要配置这些IP地址(每个IP地址与主机上指定的网卡相绑定)，而不是配置弹性IP、映射IP或者虚拟IP，否则安装过程将会出现异常。

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

    使用**hostnamectl**命令在集群中每个主机上设置主机名。例如:

    ```
    # hostnamectl set-hostname=<fully.qualified.domain.name> --static
    ```
    说明：其中<full.qualified.domain.name>是指完全合格域名/全称域名，由主机名+域名组成。故而设置主机名时，最好加上域名，如base1.zetyun.com，而不是直接使用base1作为主机名。

### 检查磁盘
* 检查主机磁盘
    
    使用**fdisk**命令在集群中的每个主机上查看磁盘数量以及每个磁盘的容量大小，确定每个主机上数据磁盘或分区(不包括系统盘或分区)的数量、以及每个磁盘或分区的容量大小，方便后续进行磁盘的格式化和挂载。例如:

    ```
    # fdisk -l
    ```
* 挂载主机磁盘
    
    安装MML需要满足磁盘的配置要求，因此用户需要按照配置要求来挂载磁盘。假设使用fdisk -l命令查到每个主机上分别有2个大小为1TB的磁盘需要挂载，如/dev/sdb和/dev/sdc。挂载步骤如下：
    
    1. 格式化磁盘为ext4格式。
        ```
        # mkfs -t ext4 /dev/sdb
        ```
    2. 创建挂载点目录。
        ```
        # mkdir /mnt/disk01
        ```
        说明：请记录好挂载点目录与磁盘或分区文件名的对应顺序，以避免混乱。
    3. 把磁盘或分区挂载到刚才创建的本地挂载点目录。
         ```
        # mount /dev/sdb /mnt/disk01
        ```       
    4. 查看挂载结果:
        ```
        df -h
        ```
        可以看到/dev/sdb磁盘文件已经挂载到了/mnt/disk01目录上，请用同样的步骤挂载/dev/sdc磁盘到本地挂载点目录，并在集群内所有主机上挂载自己所有的数据磁盘或分区。

* 挂载信息写入fstab文件

    把挂载信息写入/etc/fstable是非常重要的一步，否则主机重启之后，之前挂载磁盘的操作会失效。使用vi命令打开/etc/fstable文件，在文件最后写入磁盘挂载信息。例如/dev/sdb和/dev/sdc磁盘的挂载信息如下:
    ```
    /dev/sdb	/mnt/disk01	ext4	defaults	0  0
    /dev/sdc	/mnt/disk02	ext4	defaults	0  0
    ```
    说明:不能复制/etc/fstab文件到集群中的其它主机。即使每个主机的磁盘挂载设置（例如磁盘名称和挂载点目录）都是相同的也不能复制，因为fstab文件上面的磁盘或分区文件是通过唯一识别码UUID挂载的，而每个主机的系统分区UUID是不一样的，因此配置完1个节点后不能将/etc/fstab文件复制到其它所有节点。

## 执行安装步骤
MML基础组价提供自动化安装脚本，在执行该脚本前需要先修改配置文件，用于指定安装节点的IP地址等信息。

1. 将安装包mml-installer.tar.gz上传到基础节点root用户家目录。
2. 解压安装包。

    ```
    # cd 
    # tar -zxvf mml-installer.tar.gz
    ```

3. 修改配置文件。
    ```
    # vi conf/hosts
    ```
    配置文件示例如下所示，一般只需要根据实际情况修改其中的IP地址即可。
    
    ```
    # All host IP addresses of the cluster
    [all-ips:children]
    master-nodes
    slave-nodes
    app-server

    # master-node is also considered as slave-node by default
    [master-nodes:children]
    master1
    master2
    master3

    [slave-nodes]

    [master1]
    192.168.80.163
    [master2]
    192.168.80.164
    [master3]
    192.168.80.165

    [all:vars]
    # Store packages, high-capacity disk, without trailing slash.
    data_path=/mnt/disk01

    # Hostname pattern
    # Hostname will be updated if it equals 'localhost' or 'localhost.localdomain'
    hostname_prefix=base
    hostname_domain=zetyun.com

    [nginx-server:children]
    app-server

    [app-server]
    192.168.80.134
    ```

4. 修改安装脚本权限。

    ```
    # chmod a+x bin/*.sh 
    ```
5. 执行安装脚本。

    ```
    # ./bin/mml.sh 
    ```
    
    请等待安装脚本执行完毕。






































































