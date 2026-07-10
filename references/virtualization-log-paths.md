# Reference: 常见虚拟化组件日志路径

- 类型：官方文档与厂商支持资料
- 作者 / 来源：libvirt、OpenStack、Open vSwitch、Ceph、Xen Project、Broadcom、Proxmox
- 访问日期：2026-07-11

## URL

- <https://www.libvirt.org/kbase/debuglogs.html>
- <https://docs.openstack.org/nova/rocky/admin/configuration/logs.html>
- <https://docs.openstack.org/neutron/ussuri/admin/archives/use.html>
- <https://docs.openvswitch.org/en/stable/howto/selinux/>
- <https://docs.ceph.com/en/latest/rados/troubleshooting/log-and-debug/>
- <https://lists.xenproject.org/archives/html/xen-devel/2024-05/pdfVd7LDki17y.pdf>
- <https://knowledge.broadcom.com/external/article/306962/location-of-esxi-log-files.html>
- <https://forum.proxmox.com/threads/proxmox-audit-logins.79462/>

## 一句话概括

各虚拟化平台常见文件日志目录的依据，用于 ThorTerminal 服务器日志收集预设。

## 关键摘录

- libvirt 的 QEMU 虚拟机日志位于 `/var/log/libvirt/qemu/<domain>.log`；守护进程在文件模式下使用 `/var/log/libvirt/libvirtd.log`。
- OpenStack Nova 和 Neutron 的文件日志通常分别位于 `/var/log/nova/` 和 `/var/log/neutron/`。
- Open vSwitch 默认文件日志目录是 `/var/log/openvswitch/`。
- Ceph 的传统文件日志目录是 `/var/log/ceph/`，但现代 cephadm 部署默认使用 journald，文件不一定存在。
- Xen 的 xl、QEMU 设备模型等文件日志位于 `/var/log/xen/`。
- 当前 ESXi 文档将组件日志目录列为 `/var/run/log/`，其中包括 `vmkernel.log`、`hostd.log` 和 `vpxa.log`。
- Proxmox 工作人员给出的 Web 代理访问日志路径是 `/var/log/pveproxy/access.log`。

## 我们的备注

ThorTerminal 当前只收集远程文件，不执行 `journalctl`；因此预设只覆盖文件日志。服务改用 journald、容器日志或自定义输出目录时，仍需手动添加实际路径。

## 被哪些文档引用

- [2026-07-11 为 ThorTerminal 增加服务器日志路径预设](../sessions/2026-07-11-add-thor-terminal-server-log-presets.md)
