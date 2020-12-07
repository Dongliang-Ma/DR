.. title:: Introduction to Nutanix AHV

.. toctree::
  :maxdepth: 2
  :caption: Nutanix技术概览
  :name: _technology_overview
  :hidden:

  #what_is_nutanix/what_is_nutanix
  #nutanix_terminology/nutanix_terminology

.. toctree::
  :maxdepth: 2
  :caption: 通用配置实验
  :name: _nutanix_configuration_labs
  :hidden:

  #lab_nutanix_tech_overview/lab_nutanix_tech_overview
  #lab_storage_configuration/lab_storage_configuration
  #lab_network_configuration/lab_network_configuration

.. toctree::
  :maxdepth: 2
  :caption: 容灾方案实验
  :name: _deploying_and_managing_workloads
  :hidden:

  backup_and_dr/backup_and_dr
  lab_deploy_workloads/lab_deploy_workloads
  lab_manage_workloads/lab_manage_workloads
  lab_data_protection/lab_data_protection
  ssp/ssp
.. toctree::
  :maxdepth: 2
  :caption: 监控和管理实验
  :name: _monitoring_and_managing_the_environment
  :hidden:

  #monitoring_and_managing_env/monitoring_and_managing_env
  #lab_monitoring_env/lab_monitoring_env
  

.. toctree::
  :maxdepth: 2
  :caption: 可选高级实验
  :name: _optional_labs
  :hidden:

  #authentication/authentication
  #calm/calm
  #flow/flow


.. toctree::
  :maxdepth: 2
  :caption: 附录
  :name: _appendix
  :hidden:

  #appendix/glossary
  #appendix/basics

.. _前言:

---------------
前言
---------------

欢迎来到Nutanix Hands on Lab技术训练营！本实验手册将与实验讲师的指导相配合，重点对Nutanix技术和许多常见的管理任务进行演示和介绍。每个章节都有对应的知识点介绍和动手练习，为您提供实践练习。实验讲师将负责解释并回答您可能在动手实验中遇到的任何其他问题。

在训练营结束时，与会者应该可以对Nutanix企业云堆栈的基本概念和技术有所了解，并且能够有能力进行现场或利用远程环境进行POC或日常进行系统管理的能力。

初始化设置
+++++++++++++

- 记下使用的密码 *Passwords* 
- 登录连接到实验环境

环境细节信息
+++++++++++++++++++

Nutanix Bootcamp旨在在Nutanix托管POC环境中运行。 将为您的群集提供完成练习所需的所有必要镜像，网络和虚拟机。

网络
..........

Hosted POC 集群遵循标准的命名原则:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

在整个Workshop中，有多个实例需要用子网的正确八位位组替换 *XYZ*，例如：

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

每个集群有 2 个VLANs 供VM使用:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

用户凭据
...........

.. note::

  *<Cluster Password>* 对每个集群是唯一的，由讲师提供.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

每个集群有一个专门的 domain controller VM, **DC**, 负责向 **NTNXLAB.local** 域提供 AD 服务。域中填充了以下用户和组：
.. list-table::
   :widths: 25 35 40
   :header-rows: 1
   
   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

接入环境指导
+++++++++++++++++++

The Nutanix Hosted POC 环境有很多方法可以接入：

Lab 接入用户凭据
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<有讲师提供>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<由讲师提供>*


**Non-Employees** - Use **Lab Access User** Credentials

Employee Pulse Secure VPN
..........................

下载客户端:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

部署客户端

在Pulse Secure 客户端, **Add** 一个连接:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

Nutanix Version Info
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.11.2.3
- **PC Version** - 5.11.2.1
