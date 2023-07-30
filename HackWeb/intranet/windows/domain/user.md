用户
========================================

用户组与工作组
----------------------------------------

用户
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Windows系统存在一些为了特定用途而设置的用户，分别是：SYSTEM(系统)、Trustedinstaller(信任程序模块)、Everyone(所有人)、Creator Owner(创建者)等，这些特殊用户不属于任何用户组，是完全独立的账户。其中SYSTEM拥有整台计算机管理权限的账户，一般操作无法获取与它等价的权限。

用户组
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Windows系统内置了许多本地用户组，用于管理用户权限。只要用户账户加入到对应的用户组内，则用户账户也将具备对应用户组所拥有的权限。

默认情况下，系统为用户分了7个组，并给每个组赋予不同的操作权限。这些组为：管理员组(Administrators)、高权限用户组(Power Users)、普通用户组(Users)、备份操作组(Backup Operators)、文件复制组(Replicator)、来宾用户组(Guests)、身份验证用户组(Authenticated Users)。

工作组
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
工作组（Workgroup）是最常用最简单最普遍的资源管理模式，默认情况下计算机都在名为workgroup的工作组中。工作组模式比较松散，适合网络中计算机数量较少，不需要严格管理的情况。

域中用户
----------------------------------------

域用户
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
域环境中的用户和本地用户的帐户不同，域用户帐户保存在活动目录中。在域环境中，一个域用户可以在域中的任何一台计算机上登录。在域中用户可以使用SID (Security Identifier) 来表明身份，用NTLM哈希或者Kerberos来验证身份。

机器用户
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
机器用户也被称作机器账号或计算机账号，所有加入域的主机都会有一个机器用户，机器用户的用户名以 ``$`` 结尾。

组策略
----------------------------------------
组策略(Group Policy)用于控制用户帐户和计算机帐户的工作环境。组策略提供了操作系统、应用程序和活动目录中用户设置的集中化管理和配置。其中本地的组策略(LGPO或LocalGPO)，可以在独立且非域的计算机上管理组策略对象。在域环境中的组策略通常被称作GPO(Group Policy Object)。