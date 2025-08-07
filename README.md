# LTP-RevyOS-Tests

LTP testing on the RevyOS platform, evaluating system stability, compatibility, and performance to ensure reliable operation.

---

## 测试环境

#### 系统信息

* 系统版本：RevyOS 20250729

* 参考安装文档：参考安装文档： [RevyOS安装文档](https://revyos.github.io/docs/)

#### 硬件信息

* Lichee Pi 4A (16GB RAM + 128GB eMMC)

#### 本次测试相关信息

* 测试时间：2025/08/07

* 测试内容：在 RevyOS 平台上进行 LTP 测试，评估系统稳定性、兼容性和性能，确保系统的可靠运行。主要是用Lichee Pi 4A（荔枝派 4A）进行测试。

* 参考文档：[Linux Test Project 1.0 documentation](https://linux-test-project.readthedocs.io/en/latest/users/quick_start.html)

---

## 在RevyOS上运行LTP测试

1. 确保你的设备上刷写了最新的RevyOS系统，我们开始测试。

2. 拿取release
   
   进入你创建好的相关目录，使用wget或者其他方式获得LTP的最新release。
   
   ```bash
   wget https://github.com/linux-test-project/ltp/releases/download/20250530/ltp-full-20250530.tar.bz2
   ```

3. 解压缩
   
   成功获取到压缩包之后，我们将其解压。
   
   ```bash
   tar -xjf ltp-full-20250530.tar.bz2
   ```
   
   其中：
   
   * -xjf
     
     -x : 解压
     
     -j : 解压.bz2格式
     
     -f : 指定文件名

4. 构建项目
   
   * 进入创建好的目录。
   
   * 执行命令：
     
     ```bash
     ./configure
     ```
     
     这个命令讲帮你自动构建Makefile。
     
     我们可以将项目完整的构建，执行：
     
     ```bash
     make -j4
     ```
     
     其中4是荔枝派的核心数，测试者可根据自己设备来进行调整。
     
     ```bash
     make install
     ```
     
     这条命令将会将构建好的项目转移到系统目录之中。
     
     然后跳转到系统目录中开始测试。
     
     ```bash
     cd /opt/ltp
     ```

5. 我们可以挑选测试集进行测试，测试目录见``` /opt/ltp/runtest ```
   
   我们这里以scycalls测试集为例。

6. 创建目录 /opt/ltp_results/syscalls
   
   ```bash
   sudo mkdir /opt/ltp_results/syscalls
   ```

7. 运行syscalls测试
   
   在```/opt/ltp```目录下执行
   
   ```bash
   sudo ./kirk -f syscalls-ipc \
    -o /opt/ltp_results/syscalls-ipc/report.json \
    -T 10800 \
    -v
   ```
   
   执行测试并生成日志。
* 相关命令的解释：
  
   ```sudo``` 在这个测试中，有些测试需要管理员权限才能运行
  
   ```-f``` 执行指定测试集 
  
   ```-T 10800``` 限制时间设置为三个小时。默认时间是一个小时，一个小时跑不完时就会强制关闭，但是实际上你的测试没有卡死。
  
   ```-v``` 使测试输出更加详细的日志。
  
   ```-o``` 输出json日志到指定目录。
  
  测试脚本```kirk```的相关```help```：
  
  ```bash
  ./kirk --help
  usage: kirk [-h] [--version] [--verbose] [--no-colors] [--tmp-dir TMP_DIR] [--restore RESTORE]
              [--json-report JSON_REPORT] [--monitor MONITOR] [--sut SUT] [--framework FRAMEWORK]
              [--env ENV] [--skip-tests SKIP_TESTS] [--skip-file SKIP_FILE]
              [--run-suite [RUN_SUITE ...]] [--run-pattern RUN_PATTERN] [--run-command RUN_COMMAND]
              [--suite-timeout SUITE_TIMEOUT] [--exec-timeout EXEC_TIMEOUT] [--randomize]
              [--runtime RUNTIME] [--suite-iterate SUITE_ITERATE] [--workers WORKERS]
              [--force-parallel]
  
  Kirk - All-in-one Linux Testing Framework
  
  options:
    -h, --help            show this help message and exit
  
  General options:
    --version, -V         show program‘s version number and exit
    --verbose, -v         Verbose mode
    --no-colors, -n       If defined, no colors are shown
    --tmp-dir TMP_DIR, -d TMP_DIR
                          Temporary directory
    --restore RESTORE, -r RESTORE
                          Restore a specific session
    --json-report JSON_REPORT, -o JSON_REPORT
                          JSON output report
    --monitor MONITOR, -m MONITOR
                          Location of the monitor file
  
  Configuration options:
    --sut SUT, -u SUT     System Under Test parameters. For help please use '--sut help'
    --framework FRAMEWORK, -U FRAMEWORK
                          Framework parameters. For help please use '--framework help'
    --env ENV, -e ENV     List of key=value environment values separated by ':'
    --skip-tests SKIP_TESTS, -s SKIP_TESTS
                          Skip specific tests
    --skip-file SKIP_FILE, -S SKIP_FILE
                          Skip specific tests using a skip file (newline separated item)
  
  Execution options:
    --run-suite [RUN_SUITE ...], -f [RUN_SUITE ...]
                          List of suites to run
    --run-pattern RUN_PATTERN, -p RUN_PATTERN
                          Run all tests matching the regex pattern
    --run-command RUN_COMMAND, -c RUN_COMMAND
                          Command to run
    --suite-timeout SUITE_TIMEOUT, -T SUITE_TIMEOUT
                          Timeout before stopping the suite (default: 1h)
    --exec-timeout EXEC_TIMEOUT, -t EXEC_TIMEOUT
                          Timeout before stopping a single execution (default: 1h)
    --randomize, -R       Force parallelization execution of all tests
    --runtime RUNTIME, -I RUNTIME
                          Set for how long we want to run the session in seconds
    --suite-iterate SUITE_ITERATE, -i SUITE_ITERATE
                          Number of times to repeat testing suites
    --workers WORKERS, -w WORKERS
                          Number of workers to execute tests in parallel
    --force-parallel, -F  Force parallelization execution of all tests
  ```

* 需要注意的事项：
  
  * 荔枝派一小时经常跑不完测试集，建议增加最大时间
  
  * 建议建立规范的文件结构存放不同测试集的日志

---

## 测试中出现的broken和fail

目前运行完的测试中：

* **使用kirk出现的错误**：
  
  * LTP-RevyOS-Tests/log/file_tests/fs_readonly/report.json中报告了**35个**failed
    
    * 应该是脚本问题，不合适或者没有提前预处理。
  
  * LTP-RevyOS-Tests/log/net_work_tests/net.features/report.json中报告了**2个**failed
    
    * 第一个问题报告了```TFAIL: performance result is -214% < threshold -200%``` 这说明STCP测试效果低于预期-200%，认为不属于系统问题。
      
      
    * 第二个问题报告`TFAIL: performance result is -239% < threshold -200%`  这说明在IPv6 环境中，STCP测试效果低于预期-200%，认为也不属于系统问题。
      
      --- 
  
  * LTP-RevyOS-Tests/log/system_call_tests/syscalls/report_with_skip.json中报告了**2个**failed
    
    * 第一个问题报告了```clock_nanosleep() slept for too long``` 这说明```clock_nanosleep()``` 睡眠时间过长。猜测是测试环境下定时器精度下降或者测试效果受限于开发板性能。
      
      
    
    * 第二个问题报告了`TFAIL: umount(MNTPOINT) failed: EBUSY (16)` 根据错误编码，分析错误原因是设备或文件系统正在被使用，无法卸载。但是具体测试细节尚未进行研究。
   
    > **值得一提的是，这是第一次使用kirk测试syscalls测试集，其中跳过了runltp测试中失败的测试点。报告了这两个错误，但是再次使用kirk完整测试syscalls测试集并没有再次出现任何测试点的失败**
    
      ---
  
  * LTP-RevyOS-Tests/log/system_core_tests/hugetlb/report.json中报告了**2个**failed
    
    - 第一个问题报告了```hugemmap15.c:195: TFAIL: icache unclean``` 并且提示了
      
      ```bash
      HINT: You _MAY_ be missing kernel fixes:
      
      https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=cbf52afdc0eb
      ```
      
      报告指出CPU指令缓存没被正确刷新，很可能是与**RISC-V 架构相关的问题** 。
      
      考虑到报告给出补丁地址，认为修复方向比较明确。
      
      
    
    - 第二个问题报告了```TFAIL: couldn't find 2 free neighbour slices: ENOMEM (12)``` 系统报告内存不足，报告表明是hugepage内存分配问题。**可能是系统缺陷** 。
      
      
* **使用kirk出现的broken**：
  
  * LTP-RevyOS-Tests/log/net_work_tests/net.ipv6/report.json

    * broken1：报告了```dnsmasq_tests 1 TBROK: dhclient failed``` 即 dhclient 启动失败，怀疑是兼容问题而并非系统问题。
    
  * LTP-RevyOS-Tests/log/system_call_tests/syscalls/report_not_skip.json
    
    * broken1：报告了```leapsec01.c:84: TBROK: adjtimex status 8208 not set``` 即调用 adjtimex() 后返回的状态值没有包含期望的“闰秒”标志。崩溃原因有待分析，可能仍与定时器相关。
      
    * broken2：测试未完成，超时被中止。
    
  * LTP-RevyOS-Tests/log/system_core_tests/hugetlb/report.json
    
    * broken1：报告显示```tst_taint.c:95: TBROK: Kernel is already tainted``` 也就是显示内核被污染，故测试不再继续进行。可能是与其他测试有关也可能存在别的原因。
      

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

```bash
tree
.
├── log
│   ├── file_tests
│   │   ├── fcntl-locktests
│   │   │   └── report_not_skip.json
│   │   ├── fs
│   │   │   └── report.json
│   │   ├── fs_bind
│   │   │   └── report.json
│   │   ├── fs_perms_simple
│   │   └── fs_readonly
│   │       └── report.json
│   ├── list.md
│   ├── net_work_tests
│   │   ├── net.features
│   │   │   └── report.json
│   │   ├── net.ipv6
│   │   │   └── report.json
│   │   ├── net.ipv6_lib
│   │   │   └── report.json
│   │   ├── net.multicast
│   │   │   └── report.json
│   │   ├── net.nfs
│   │   │   └── report.json
│   │   └── net.rpc_tests
│   │       └── report.json
│   ├── system_call_tests
│   │   ├── syscalls
│   │   │   ├── failed.txt
│   │   │   ├── report_not_skip.json
│   │   │   └── report_with_skip.json
│   │   └── syscalls-ipc
│   │       └── report.json
│   ├── system_core_tests
│   │   ├── cpuhotplug
│   │   │   └── report.json
│   │   ├── hugetlb
│   │   │   └── report.json
│   │   └── mm
│   │       └── report.json
│   └── template.md
└── README.md
```
