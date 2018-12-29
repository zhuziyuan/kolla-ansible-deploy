安装注意事项和步骤
================
脚本暂时只支持ubuntu系统

主要是pre-install.yml前期准备、网络配置和harbor部署

pre-install.yml主要包括安装源、DNS和更新ntp、raid添加和ceph前准备、kolla-prepare、安装pip依赖和docker环境、修改网卡名称、sysctl.conf和limits.conf优化

网络配置分为bond和nobond，主要是配置网卡信息，配置管理网、业务网络、存储网络和外网，bond主要用于生产环境，nobond主要用于实施或者测试环境

第一步: 前期准备
-----------------

修改hosts里面的harbor主机变量主机变量
* 修改变量: 本地源、dns、ntp_server、harbor、old_interface等
* 修改raid的磁盘数量,查看现在的raid磁盘是否被使用，需要删除子磁盘的数量，autoraid.sh脚本和format disk修改磁盘数量
* 执行命令: ansible-playbook -i hosts 01.pre-install.yml


第二步: net配置
----------------
* inventory: 先修改bond_hosts/nobond_hosts主机变量
* 根据需求修改templates下面的网卡模板
* 执行命令: ansible-playbook -i bond_hosts/nobond_hosts bond_net.yml/nobond_net.yml

第三步: 部署harbor
----------------
* inventory: 修改hosts里面的harbor主机变量
* 执行命令: ansible-playbook -i hosts 02.harbor-install.yml
