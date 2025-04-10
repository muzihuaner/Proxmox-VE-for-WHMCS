# Proxmox VE for WHMCS (Module) Provision & Manage Proxmox VE for WHMCS（模块）配置和管理



**Salvation, a free and open-source solution for beloved PVE!** If you love it, REVIEW & SHARE IT! ❤️
**Salvation，一款免费开源的 PVE 解决方案！** 如果您喜欢它，欢迎评论并分享！❤️

```
TNC Dev are looking for a co-developer to assist with finishing the project overhaul.
If you have proven and public git-logged experience, or similar, please say g'day.

Please note: We are only looking for high-quality applicants with spare time.
As it stands, we won't have much spare dev time for PVEWHMCS in early 2025.
```



[![Logo for the Proxmox VE for WHMCS module](https://github.com/muzihuaner/Proxmox-VE-for-WHMCS/raw/master/zLOGO.png)](https://github.com/muzihuaner/Proxmox-VE-for-WHMCS/blob/master/zLOGO.png)

- Configure VM/CT plans with custom CPU/RAM/VLAN/On-boot/Bandwidth/etc
  使用自定义 CPU/RAM/VLAN/启动时/带宽/等配置 VM/CT 计划
- Automatically Provision VMs & CTs in **Proxmox VE** from **WHMCS** easily
  轻松从 **WHMCS** 自动在 **Proxmox VE** 中配置 VM 和 CT
- Allow clients to view/manage VMs using the WHMCS Client Area
  允许客户端使用 WHMCS 客户端区域查看/管理虚拟机
- Create/Suspend/Unsuspend/Terminate via WHMCS Admin Area
  通过 WHMCS 管理区域创建/暂停/取消暂停/终止
- Statistics/Graphing is available in the Client Area for services :)
  客户区可提供统计数据/图表服务：)
- Leverage the power of QEMU & LXC with PVE's convenience
  利用 QEMU 和 LXC 的强大功能以及 PVE 的便利性

Repo: https://github.com/The-Network-Crew/Proxmox-VE-for-WHMCS/
仓库： https://github.com/The-Network-Crew/Proxmox-VE-for-WHMCS/

## ❤️ RTFM: Read the Manual & Review the Module! ❤️ RTFM：阅读手册并查看模块！



**Please read the entire README.md file before getting started with Proxmox VE for WHMCS.** Thanks!
**在开始使用 Proxmox VE for WHMCS 之前，请阅读完整的 README.md 文件。** 谢谢！

We're pretty much done overhauling the Module to suit our needs at The Network Crew Pty Ltd & Merlot Digital.
我们基本上已经完成了对模块的全面改造，以满足 The Network Crew Pty Ltd 和 Merlot Digital 的需求。

> **Please review the module!** https://marketplace.whmcs.com/product/6935-proxmox-ve-for-whmcs#reviews
> **请查看模块！** https://marketplace.whmcs.com/product/6935-proxmox-ve-for-whmcs#reviews
>
> *If you want it to remain free and fabulous, it could use a moment of your time in reviewing it.* **Thanks!**
> *如果您希望它继续免费且精彩，请花点时间查看一下。* **谢谢！**

## 🎯 MODULE: System Requirements (PVE/WHMCS) 🎯 模块：系统要求（PVE/WHMCS）



### WHMCS must have >100 services! WHMCS 必须有 >100 服务！



New Biz: Fresh Installations/Businesses using WHMCS need to take note of the Service ID <100 case.
新业务：使用 WHMCS 的新安装/企业需要注意服务 ID <100 案例。

**SID >100:** The WHMCS Service ID requirement is CRITICAL, as **Proxmox reserves VMIDs <100 (system).**
**SID >100：** WHMCS 服务 ID 要求非常关键，因为 **Proxmox 保留了 VMID <100（系统）。**

*If you don't have enough services (of any status) in WHMCS (DB: tblhosting.id), create enough dummy/test entries to reach Service ID 101+.* **Else you're likely to see an error which explains this:**
*如果您在 WHMCS 中没有足够的服务（无论状态如何）（数据库：tblhosting.id），请创建足够的虚拟/测试条目，以达到服务 ID 101 及以上。* **否则，您可能会看到一个错误，解释此问题：**

```
HTTP/1.1 400 Parameter verification failed. (invalid format - value does not look like a valid VM ID)
```

To check, browse to your **latest** service in WHMCS, then check the URL - it will reveal the Service ID. If it is less than 100, subtract it from 100 to deduce how many "dummy services" you need to add in a dummy order.
要检查，请在 WHMCS 中浏览到您**最新的**服务，然后检查 URL——它会显示服务 ID。如果服务 ID 小于 100，请从 100 中减去该值，以推算出您需要在虚拟订单中添加多少个“虚拟服务”。

Once over 100, it fits the requirement & you're good!
一旦超过 100，就符合要求了，而且你很棒！

### General Requirements 一般要求



- (WHMCS) v8.x.x stable (HTTPS)
  （WHMCS）v8.xx 稳定版（HTTPS）
- (WHMCS) **Service ID above 100**
  （WHMCS） **服务 ID 大于 100**
- (PHP) v8.x.x (latest stable version)
  （PHP）v8.xx（最新稳定版本）
- (PHP) max_execution_time = 300
  （PHP）最大执行时间 = 300
- (Proxmox) VE v8.x (current)
  （Proxmox）VE v8.x（当前）
- (Proxmox) 2 users (API/VNC)
  （Proxmox）2 个用户（API/VNC）

## ✅ MODULE: Installation & Configuration ✅ 模块：安装和配置



**DON'T SKIP ANY PART OF THIS README.md - please don't raise pointless Issues - thank you!
不要跳过本 README.md 的任何部分 - 请不要提出毫无意义的问题 - 谢谢！**

Firstly, you need to upload, activate and make the WHMCS Module available to Administrators.
首先，您需要上传、激活并让管理员可以使用 WHMCS 模块。

Once you've done all of that, in order to get the module working properly, you need to:
完成所有这些操作后，为了使模块正常工作，您需要：

1. Proxmox VE > Create an additional VNC-only user, per instructions below
   Proxmox VE > 按照以下说明创建一个额外的仅 VNC 用户
2. WHMCS Admin > Config > Servers > Add your PVE host/s (user: root; IP: PVE's)
   WHMCS 管理员 > 配置 > 服务器 > 添加您的 PVE 主机（用户：root；IP：PVE）
3. WHMCS Admin > Addons > Proxmox VE for WHMCS > Module Config > VNC Secret (see below)
   WHMCS 管理员 > 附加组件 > WHMCS 的 Proxmox VE > 模块配置 > VNC 密钥（见下文）
4. WHMCS Admin > Addons > Proxmox VE for WHMCS > Add KVM/LXC Plan/s
   WHMCS 管理员 > 附加组件 > Proxmox VE for WHMCS > 添加 KVM/LXC 计划
5. WHMCS Admin > Addons > Proxmox VE for WHMCS > Add an IP Pool
   WHMCS 管理员 > 附加组件 > WHMCS 的 Proxmox VE > 添加 IP 池
6. WHMCS Admin > Config > Products/Services > New Service (create offering)
   WHMCS 管理员 > 配置 > 产品/服务 > 新服务（创建产品）
7. " " > Newly-added Service > Tab 3 > **SAVE** (links Module Plan to WHMCS Service type)
   “” > 新增服务 > 标签 3 > **保存** （将模块计划链接到 WHMCS 服务类型）

## 🥽 noVNC: Console Tunnel (Client Area) 🥽 noVNC：控制台隧道（客户端区域）



After forking the module, we considered how to improve security of Console Tunneling via WHMCS. We decided to implement a routing method which uses a secondary user in Proxmox VE with very restrictive permissions. This is due to be re-built again to further enhance security.
在分叉该模块后，我们考虑了如何通过 WHMCS 提升控制台隧道的安全性。我们决定实现一种路由方法，该方法使用 Proxmox VE 中权限非常严格的二级用户。为了进一步增强安全性，我们将再次构建该方法。

### To offer VNC via WHMCS Client Area 通过 WHMCS 客户端区域提供 VNC



1. Install & configure the module properly
   正确安装和配置模块
2. Follow the PVE User Requirement info below
   遵循以下 PVE 用户要求信息
3. Public IPv4 for PVE (or proxy to private)
   用于 PVE 的公共 IPv4（或代理到私有 IPv4）
4. PVE and WHMCS on the same Domain Name*
   同一域名上的 PVE 和 WHMCS*
5. Have valid PTR/rDNS for the PVE Address
   拥有有效的 PTR/rDNS 作为 PVE 地址

noVNC has been overhauled. It isn't guaranteed, nor the project at all. :-)
noVNC 已全面改版。目前尚无保证，整个项目也完全无法保证。:-)

- Note #1 = You must use different Subdomains on the same Domain Name, for the cookie (anti-CSRF).
  注意#1 = 您必须在同一个域名上使用不同的子域名来获取 cookie（反 CSRF）。
- Note #2 = If your Domain Name has a 2-part TLD (ie. co.uk) then you will need to fork & amend `novnc_router.php` - ideally we/someone will optimise this to better cater to all formats.
  注释#2 = 如果您的域名有两部分 TLD（即 co.uk），那么您将需要分叉并修改 `novnc_router.php` - 理想情况下，我们/某人将对其进行优化以更好地满足所有格式。

## 👥 PVE: User Requirements (API & VNC) 👥 PVE：用户需求（API 和 VNC）



**You must have a root account to use the Module at all.** Configured via WHMCS > Servers.
**您必须拥有 root 账户才能使用该模块。** 通过 WHMCS > 服务器进行配置。

Additionally, to improve security, for VNC you must also have a Restricted User. Configured in the *Module*.
此外，为了提高安全性，对于 VNC，您还必须拥有受限用户。在*模块*中配置。

### Creating the VNC user within PVE 在 PVE 中创建 VNC 用户



1. Create User Group "VNC" via PVE > ` Datacenter / Permissions / Group`
   通过 PVE > ` Datacenter / Permissions / Group` 创建用户组“VNC”
2. Create new User "vnc" > `Datacenter / Permissions / Users` - Group: "VNC", Realm: pve
   创建新用户“vnc”> `Datacenter / Permissions / Users` - 组：“VNC”，领域：pve
3. Create new Role -> `Datacenter / Permissions / Roles` - Name: "VNC", Privileges: VM.Console (only)
   创建新角色 -> `Datacenter / Permissions / Roles` - 名称：“VNC”，权限：VM.Console（仅限）
4. Permit VNC Access -> `Datacenter / Permissions / Add Group Permissions` - Group: "VNC", Role: "VNC"
   允许 VNC 访问 -> `Datacenter / Permissions / Add Group Permissions` - 组：“VNC”，角色：“VNC”
5. WHMCS > Modules > Proxmox VE for WHMCS > Module Config > VNC Secret = 'vnc' password (PVE) you set
   WHMCS > 模块 > WHMCS 的 Proxmox VE > 模块配置 > VNC 密钥 = 您设置的“vnc”密码（PVE）

> Do NOT set less restrictive permissions. The above is designed for hypervisor security.
> 请勿设置限制较少的权限。以上设置是为了虚拟机管理程序的安全性而设计的。
>
> However, if you wish for proper security, wait for VNC to be further improved.
> 但是，如果您希望获得适当的安全性，请等待 VNC 进一步改进。

## ⚙️ VM/CT PLANS: Setting everything up ⚙️ VM/CT 计划：设置一切



These steps explain the unique requirements per-option.
这些步骤解释了每个选项的独特要求。

Custom Fields: Values need to go in Name & Select Options.
自定义字段：值需要进入名称和选择选项。

> **Unsure?** Consult the zMANUAL-PVE4.pdf *legacy* manual file.
> **不确定？** 请参阅 zMANUAL-PVE4.pdf *旧版*手册文件。

### VM Option 1: KVM, using PVE Template VM VM 选项 1：KVM，使用 PVE 模板 VM



Firstly, create the Template in PVE. You need its unique PVE ID.
首先，在 PVE 中创建模板。您需要一个唯一的 PVE ID。

Use that ID in the Custom Field `KVMTemplate`, as in `ID|Name`.
在自定义字段 `KVMTemplate` 中使用该 ID，如 `ID|Name` 。

> Note: `Name` is what's displayed in the WHMCS Client Area.
> 注意： `Name` 是 WHMCS 客户端区域中显示的名称。

### VM Option 2: KVM, WHMCS Plan + PVE ISO VM 选项 2：KVM、WHMCS 计划 + PVE ISO



Firstly, create the Plan in WHMCS Module. Then, WHMCS Config > Services.
首先，在 WHMCS 模块中创建计划。然后，WHMCS 配置 > 服务。

> Under the Service, you need to add a Custom Field `ISO` with the full location.
> 在服务下，您需要添加具有完整位置的自定义字段 `ISO` 。

### CT Option: LXC, using PVE Template File CT 选项：LXC，使用 PVE 模板文件



Firstly, store the Template in PVE. You need its unique File Name.
首先，将模板存储在 PVE 中。您需要一个唯一的文件名。

> Use that full file name in the Custom Field `Template`, as in:
> 在自定义字段 `Template` 中使用该完整文件名，如下所示：
> `ubuntu-99.99-standard_amd64.tar.gz|Ubuntu 99`

Then make a 2nd Custom Field `Password` for the CT's root user.
然后为 CT 的根用户创建第二个自定义字段 `Password` 。

## 🌐 IPv4/v6: Networking (IP Pools) 🌐 IPv4/v6：网络（IP 池）



Please make sure you create an IP Pool with sufficient scope/size to be able to deploy addresses within it to your guest VMs and CTs. Else it won't be able to create a Service for you.
请确保您创建的 IP 池具有足够的范围/大小，以便能够将其中的地址部署到您的客户虚拟机和 CT。否则，它将无法为您创建服务。

**Private IPs for PVE Hosts:** Note that VNC may be problematic without work due to the strict requirements introduced in Proxmox v8.0 (strict same-site attribute).
**PVE 主机的私有 IP：** 请注意，由于 Proxmox v8.0 中引入的严格要求（严格的同站点属性），VNC 可能会出现问题而无法工作。

### IPv6: SLAAC default, 2nd vNIC IPv6：SLAAC 默认，第二个 vNIC



Per [The-Network-Crew#33](https://github.com/The-Network-Crew/Proxmox-VE-for-WHMCS/issues/33) there's SLAAC/DHCP/off available (2x vNICs) (May 2024).
每 [The-Network-Crew#33](https://github.com/The-Network-Crew/Proxmox-VE-for-WHMCS/issues/33) 都有可用的 SLAAC/DHCP/off (2x vNIC)（2024 年 5 月）。

You can of course add different config via PVE/`pvesh` manually, if you need to specify a prefix.
当然，如果您需要指定前缀，您可以通过 PVE/ `pvesh` 手动添加不同的配置。

## 💅 FEATURES: PVE v8.x bling 💅 特点：PVE v8.x bling



There are new features deployed into Proxmox VE upstream in the v8 branch which are exciting and should be added to this module.
在 v8 分支中，Proxmox VE 上游部署了一些令人兴奋的新功能，应该添加到此模块中。

### Proxmox v8.0



1. Create, manage and assign resource mappings for PCI and USB devices for use in virtual machines (VMs) via API and web UI.
   通过 API 和 Web UI 创建、管理和分配 PCI 和 USB 设备的资源映射以供虚拟机 (VM) 使用。
2. (DONE) Add virtual machine CPU models based on the x86-64 psABI Micro-Architecture Levels and use the widely supported x86-64-v2-AES as default for new VMs created via the web UI.
   （完成）基于 x86-64 psABI 微架构级别添加虚拟机 CPU 模型，并使用广泛支持的 x86-64-v2-AES 作为通过 Web UI 创建的新虚拟机的默认值。

### Proxmox v8.1



1. Secure Boot support. 安全启动支持。
2. Software Defined Networking (SDN).
   软件定义网络 (SDN)。
3. New flexible notification system (SMTP & Gotify).
   新的灵活通知系统（SMTP 和 Gotify）。
4. MAC Organizationally Unique Identifier (OUI) BC:24:11: prefix!
   MAC 组织唯一标识符 (OUI) BC:24:11: 前缀！

### Proxmox v8.2



1. Import Wizard for Guests.
   为客人导入向导。
2. Unattended PVE Install (answer file).
   无人值守 PVE 安装（应答文件）。
3. Backup Fleecing (local disk as data block buffer).
   备份 Fleecing（本地磁盘作为数据块缓冲区）。
4. Firewall Preview (based on nftables).
   防火墙预览（基于 nftables）。

### Proxmox v8.3



1. Software-defined Networking/Firewall.
   软件定义网络/防火墙。
2. Better guest importing from OVA/OVF.
   更好地从 OVA/OVF 导入客人。
3. Webhook target for system alerting.
   系统警报的 Webhook 目标。
4. Better change detection for PBS.
   更好地检测 PBS 的变化。

PVE Roadmap: https://pve.proxmox.com/wiki/Roadmap
PVE 路线图： https://pve.proxmox.com/wiki/Roadmap

## 🤬 ABUSE: Zero Tolerance (ZT) 🤬 虐待：零容忍（ZT）



This module has been overhauled and remains functionally-OK but not thoroughly tested nor reviewed.
该模块已经彻底检修，功能仍然正常，但尚未经过彻底测试或审查。

Your support and assistance is always welcomed per the spirit of FOSS (Free Open-source Software)!
本着 FOSS（免费开源软件）的精神，我们始终欢迎您的支持和帮助！

If you cannot accept this, do not download nor use the code. Complaints, nasty reviews, and similar behaviour is against the spirit of FOSS and will not be tolerated.
如果您无法接受这一点，请不要下载或使用该代码。投诉、恶意评论以及类似行为违背了自由/开源软件 (FOSS) 的精神，我们绝不容忍。

**Be grateful & considerate - thank you!
心存感激和体贴——谢谢！**

## 🆘 HELP: Best-effort Support 🆘 帮助：尽力支持



**Before raising a GitHub Issue, please check:
在提出 GitHub Issue 之前，请检查：**

1. The Wiki. 维基百科。
2. The README.md. README.md。
3. Open GitHub Issues on the repo.
   在 repo 上打开 GitHub Issues。
4. HTTP, PHP, WHMCS & debug logs (see below).
   HTTP、PHP、WHMCS 和调试日志（见下文）。
5. PVE logs; best practices; network; etc.
   PVE 日志；最佳实践；网络；等等。
6. Read the errors. Do they explain it?
   阅读错误。它们能解释吗？

> Help: Including logs, details, steps to reproduce, etc, please raise a **GitHub Issue**.
> 帮助：包括日志、详细信息、重现步骤等，请提出 **GitHub Issue** 。
>
> Logs: We work to ensure that Proxmox VE for WHMCS passes through error details to you.
> 日志：我们努力确保 Proxmox VE for WHMCS 将错误详细信息传递给您。

### Issues/etc raised must include: 提出的问题等必须包括：



#### Logging & Debug Logging 日志记录和调试日志记录



- (Logs: PHP) `error_log` contents
  （日志：PHP） `error_log` 内容
- (Logs: WHMCS) Module Debug Logging*
  （日志：WHMCS）模块调试日志记录*
- (Logs: Config) WHMCS Display/Log Errors = ON
  （日志：配置）WHMCS 显示/日志错误 = 开启
- (Logs: PVE) Logs from Proxmox Host (`pveproxy` etc)
  （日志：PVE）来自 Proxmox 主机（ `pveproxy` 等）的日志

#### Other Support Requirements 其他支持要求



- (Visibility) Screenshots of the issue
  （可见性）问题的屏幕截图
- (Configs) WHMCS/PHP/Module/Proxmox/etc
  （配置）WHMCS/PHP/Module/Proxmox/etc
- (Reproduction) `pvesh` etc variants of failing calls
  （复制） `pvesh` 等失败调用的变体
- (Network) Proof WHMCS Server can talk to PVE OK
  (网络) 证明 WHMCS 服务器可以与 PVE 通信 OK
- (PEBKAC) *PROOF THAT YOU'VE FOLLOWED THIS README!*
  （PEBKAC） *证明您已经阅读了此自述文件！*

The more info/context you provide up-front, the quicker & easier it will be!
您预先提供的信息/背景越多，它就会越快越容易！

\* Debug: Also enable Debug Logging in Proxmox VE for WHMCS > Settings, as needed.
\* 调试：还可以根据需要在 Proxmox VE 中为 WHMCS > 设置启用调试日志记录。

**Please note that this is FOSS and Support is not guaranteed at all.
请注意，这是 FOSS，并且根本不保证提供支持。**

**If you don't read, listen or actively try, no help is given.
如果您不阅读、不听或不积极尝试，就不会得到任何帮助。**

## 🔄 UPDATING: Patching the Module 🔄 更新：修补模块



WHMCS Admin > Addon Modules > Proxmox VE for WHMCS > Support/Health shows updates.
WHMCS 管理员 > 附加模块 > WHMCS 的 Proxmox VE > 支持/健康显示更新。

You can download the new version and upload it over the top, then run any needed SQL ops.
您可以下载新版本并将其上传到顶部，然后运行任何需要的 SQL 操作。

Please consult the **UPDATE-SQL.md** file, open your WHMCS DB & run the statements. Then you're done.
请查阅 **UPDATE-SQL.md** 文件，打开您的 WHMCS 数据库并运行其中的语句。然后就大功告成了。

## 🖥️ INC: Libraries & Dependencies 🖥️ INC：库和依赖项



- (MIT) PHP Client for PVE2 API (Dec 5th, 2022) https://github.com/CpuID/pve2-api-php-client
  (MIT) PVE2 API 的 PHP 客户端（2022 年 12 月 5 日） https://github.com/CpuID/pve2-api-php-client
- (GPLv2) TigerVNC VncViewer.jar (v1.14.0 in repo) https://sourceforge.net/projects/tigervnc/files/stable/
  (GPLv2) TigerVNC VncViewer.jar（存储库中的 v1.14.0） https://sourceforge.net/projects/tigervnc/files/stable/
- (MPLv2) noVNC HTML5 Viewer (v1.5.0 in repo) https://github.com/novnc/noVNC
  （MPLv2）noVNC HTML5 查看器（repo 中的 v1.5.0） https://github.com/novnc/noVNC
- (GPLv3) SPICE HTML5 Viewer (v0.3 in repo) https://gitlab.freedesktop.org/spice/spice-html5
  （GPLv3）SPICE HTML5 查看器（repo 中的 v0.3） https://gitlab.freedesktop.org/spice/spice-html5
- (MIT) IPv4/SN Validation (August 2012) https://github.com/tapmodo/php-ipv4/
  (麻省理工学院) IPv4/SN 验证（2012 年 8 月） https://github.com/tapmodo/php-ipv4/

## 📄 DIY: Documentation & Resources 📄 DIY：文档和资源



- Proxmox API: https://pve.proxmox.com/pve-docs/api-viewer/
  Proxmox API： https://pve.proxmox.com/pve-docs/api-viewer/
- TigerVNC: https://github.com/TigerVNC/tigervnc/wiki
  TigerVNC： https://github.com/TigerVNC/tigervnc/wiki
- noVNC: https://github.com/novnc/noVNC/wiki
  noVNC： https://github.com/novnc/noVNC/wiki
- WHMCS: https://developers.whmcs.com/
  WHMCS：https: [//developers.whmcs.com/](https://developers.whmcs.com/)
- x86-64-ABI: https://gitlab.com/x86-psABIs/x86-64-ABI/-/jobs/artifacts/master/raw/x86-64-ABI/abi.pdf?job=build
  x86-64-ABI： [https://gitlab.com/x86-psABIs/x86-64-ABI/-/jobs/artifacts/master/raw/x86-64-ABI/abi.pdf?](https://gitlab.com/x86-psABIs/x86-64-ABI/-/jobs/artifacts/master/raw/x86-64-ABI/abi.pdf?job=build) job=build

## 🎉 FOSS: Contributions & Open-source 🎉 FOSS：贡献与开源



If you'd like to contribute to the Module, please open a Pull on GitHub >> The-Network-Crew/Proxmox-VE-for-WHMCS
如果您想为该模块做出贡献，请在 GitHub 上打开 Pull >> The-Network-Crew/Proxmox-VE-for-WHMCS

The original module was written in 2 months by @cybercoder for sale online in 2016, though didn't sell any copies so they kindly open-sourced it and removed the licensing requirement.
原始模块由@cybercoder 在 2 个月内编写，于 2016 年在线销售，但没有出售任何副本，因此他们好心地将其开源并取消了许可要求。

We would like to thank @cybercoder and @WaldperlachFabi for their original contributions and troubleshooting assistance respectively.
我们要分别感谢@cybercoder 和@WaldperlachFabi 的原创贡献和故障排除帮助。

Thank you to psyborg® for the module's logo design! We love it.
感谢 psyborg® 为该模块设计了徽标！我们非常喜欢。

FOSS is only possible thanks to dedicated individuals!
只有依靠敬业的个人，FOSS 才有可​​能实现！

## Usage License (GPLv3) & Links to TNC & Co. 使用许可证（GPLv3）和 TNC & Co. 链接



***This module is licensed under the GNU General Public License (GPL) v3.0.
该模块根据 GNU 通用公共许可证 (GPL) v3.0 获得许可。***

GPLv3: https://www.gnu.org/licenses/gpl-3.0.txt (by the Free Software Foundation)
GPLv3： https://www.gnu.org/licenses/gpl-3.0.txt （由自由软件基金会提供）

### Corporate Sites: TNC & Merlot Digital 公司网站：TNC 和 Merlot Digital



**The Network Crew Pty Ltd** :: [https://tnc.works](https://tnc.works/)
**Network Crew Pty Ltd** :: [https://tnc.works](https://tnc.works/)

**Merlot Digital** :: [https://merlot.digital](https://merlot.digital/)
**梅洛数字** :: [https://merlot.digital](https://merlot.digital/)

### Support: Best-effort via GitHub Issues 支持：通过 GitHub Issues 尽力支持



Browse issues, raise a new one: **GitHub Issues**
浏览问题，提出新问题： **GitHub Issues**
