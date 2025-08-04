# LTP-RevyOS-Tests
LTP testing on the RevyOS platform, evaluating system stability, compatibility, and performance to ensure reliable operation.

测试使用系统：

测试使用开发版：

测试时间：

测试内容：


---
## 在RevyOS上运行LTP测试

1. 确保你的设备上刷写了最新的RevyOS系统，我们开始测试
2. 拿取release
3. 解压缩
4. 执行命令...
5. 创建目录...
6. 执行命令...

* 相关命令的解释：

* 需要注意的事项：

---
## 测试中出现的broken和fail

* 使用runltp出现的syscalls错误：
* 使用kirk出现的syscalls错误：
* 测试网络时候出现的broken：

--- 


## 相关测试表单

**系统调用相关**

```
syscalls
syscalls-ipc
```

**文件系统相关**

```
fcntl-locktests
fs
fs_bind
fs_perms_simple
fs_readonly
```

**网络相关**

```
net.features
net.ipv6
net.ipv6_lib
net.multicast
net.nfs
net.rpc_tests
net.sctp
net.tcp_cmds
net.tirpc_tests
net_stress.appl
net_stress.broken_ip
net_stress.interface
net_stress.ipsec_dccp
net_stress.ipsec_icmp
net_stress.ipsec_sctp
net_stress.ipsec_tcp
net_stress.ipsec_udp
net_stress.multicast
net_stress.route
```

**内存/调度/NUMA/CPU**

```
cpuhotplug
hugetlb
mm
numa
sched
```

**安全/权限/加密**

```
capability
crypto
ima
smack
```

**容器/虚拟化/控制器**

```
containers
controllers
kvm
```

**硬件/驱动/架构相关**

```
dma_thread_diotest
input
irq
s390x_tests
scsi_debug.part1
tpm_tools
```

**压力/冒烟/实验/特殊**

```
crashme
ltp-aio-stress
ltp-aiodio.part1
ltp-aiodio.part2
ltp-aiodio.part3
ltp-aiodio.part4
smoketest
staging
```

**电源管理**

```
power_management_tests
power_management_tests_exclusive
```

**其他/杂项/工具**

```
can
commands
cve
dio
hyperthreading
kernel_misc
math
nptl
pty
realtime
tracing
uevent
watchqueue
```


---

## 测试集文件结构
```
.
├── list.md
├── log
│   ├── cpuhotplug
│   │   └── report.json
│   ├── fcntl-locktests
│   │   └── report_not_skip.json
│   ├── fs
│   │   └── report.json
│   ├── fs_bind
│   │   └── report.json
│   ├── fs_perms_simple
│   ├── fs_readonly
│   │   └── report.json
│   ├── hugetlb
│   │   └── report.json
│   ├── mm
│   │   └── report.json
│   ├── net.features
│   │   └── report.json
│   ├── net.ipv6
│   │   └── report.json
│   ├── net.ipv6_lib
│   │   └── report.json
│   ├── net.multicast
│   │   └── report.json
│   ├── net.nfs
│   │   └── report.json
│   ├── net.rpc_tests
│   │   └── report.json
│   ├── syscalls
│   │   ├── failed.txt
│   │   ├── report_not_skip.json
│   │   └── report_with_skip.json
│   ├── syscalls-ipc
│   │   └── report.json
│   └── template.md
└── README.md
```
