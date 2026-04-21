```

目标：改造nuttx为libos；libos的定义是内核作为库的形式，库可以静态链接单个应用，单个地址空间；该库可以运行在用户态，也可以运行在内核态；可以支持kvm和xen虚拟环境；保留nuttx的全部的POSIX支持；每次结束后需要输出README文档

继续做，目标是1. 可以跑通sim环境下一个POSIX demo应用（用户态运行时）2. 可以跑通host-x86_64 linux环境下kvm / aarch64 qemu xen作为后端的POSIX demo应用（内核态运行时）；3. 再增加一个支持裸机作为后端，可以跑通aarch64 qemu环境下的一个POSIX demo应用（内核态运行时）

运行条件配置：用aarch64 qemu启动linux，由linux开启Kvm，基于kvm启动libos；用aarch64 qemu启动xen，由xen启动libos

sim目录下的libos相关的代码修改应该用#sym:CONFIG_LIBOS 控制，不影响原有的逻辑。改完这个后，验证下，继续跑

继续做，驱动和板级支持，暂时使用qemu作为板级支持，以及virtio作为驱动支持，包括网络，磁盘（TBD：真实的评估板）

继续做，支持host-x86-64 linux crun提供的容器环境来跑nuttx，并在容器环境使用nuttx跑通一个posix应用（用户态运行时）；后面可以支持基于crun的podman

继续做，支持协同内核模式（LINUX）类似Xenomai

继续做，host-x86-64 linux后续可以更新为host-aarch64 linux环境

继续做，支持mcu和mpu两个方向，mcu暂时仅支持裸机作为后端，使用arm qemu环境跑通一个POSIX demo应用

继续做，为我提供一个全局的libos配置工具，包括构建选项（分多个目标），资源配置（包括以下环境：kvm/xen/sim/crun-linux-cgroup/裸机）可以用xml或yaml等方式配置内存，中断，CACHE，定时器，CPU，IO，网络，磁盘等，驱动配置（设备树），shell引导（initramfs:option）, 适合nuttx的rootfs(option)，nsh（作为调试选项的配置:option）等；这个配置工具也可以生成用于crun容器配置的config.json

继续做，posix需要支持libos nuttx ltp测试，保证posix接口的覆盖度

继续做，适配nuttx的posix apps，支持micro-ROS

继续做，将非posix应用转换为posix应用的代码翻译工具

```
