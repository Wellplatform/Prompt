```

目标：改造nuttx为libos；libos的定义是内核作为库的形式，库可以静态链接单个应用，单个地址空间；该库可以运行在用户态，也可以运行在内核态；可以支持kvm和xen虚拟环境；保留nuttx的全部的POSIX支持；每次结束后需要输出README文档

继续做，目标是1. 可以跑通sim环境下一个POSIX demo应用（用户态运行时）2. 可以跑通host-x86_64 linux环境下kvm / aarch64 qemu xen作为后端的POSIX demo应用（内核态运行时）；3. 再增加一个支持裸机作为后端，可以跑通aarch64 qemu环境下的一个POSIX demo应用（内核态运行时）

运行条件配置：用aarch64 qemu启动linux，由linux开启Kvm，基于kvm启动libos；用aarch64 qemu启动xen，由xen启动libos

支持sel4+microkit实现用户态和内核态运行时

kvm可以使用QEMU/Fire/KVMTOOLS作为VMM，XEN使用自带的DOM0 VMM，其他的平台可以使用我们自定义的VMM

sim目录下的libos相关的代码修改应该用#sym:CONFIG_LIBOS 控制，不影响原有的逻辑。改完这个后，验证下，继续跑

继续做，驱动和板级支持，暂时使用qemu作为板级支持，以及virtio作为驱动支持，包括网络，磁盘（TBD：真实的评估板）

继续做，支持host-x86-64 linux crun提供的容器环境来跑nuttx，并在容器环境使用nuttx跑通一个posix应用（用户态运行时）；后面可以支持基于crun的podman

继续做，支持协同内核模式（LINUX）类似Xenomai

继续做，host-x86-64 linux后续可以更新为host-aarch64 linux环境

继续做，支持mcu和mpu两个方向，mcu暂时仅支持裸机作为后端，使用arm qemu环境跑通一个POSIX demo应用

继续做，支持启动多个nuttx guest

继续做，为我提供一个全局的libos配置工具，包括构建选项（分多个目标），资源配置（包括以下环境：kvm/xen/sim/crun-linux-cgroup/裸机）可以用xml或yaml等方式配置内存，中断，CACHE，定时器，CPU，IO，网络，磁盘等，驱动配置（设备树），shell引导（initramfs:option）, 适合nuttx的rootfs(option)，nsh（作为调试选项的配置:option）等；这个配置工具也可以生成用于crun容器配置的config.json

继续做，posix需要支持libos nuttx ltp测试，保证posix接口的覆盖度

继续做，适配nuttx的posix apps，支持micro-ROS

继续做，将非posix应用转换为posix应用的代码翻译工具

支持生成一个描述资源的状态视图文件，包括预留的，初始化分配的，剩余的资源

interface的函数我建议要做成一种资源标准，命名可以参考posix标准

freertos支持m/r/a各一个样例，sel4支持a，fuchsia支持a

我的目标是改造microkit，改造点：

1. microkit使用静态配置XML定义资源分配，由initialiser和tool/microkit目录的rs代码来调用sel4接口分配资源，我想用C语言重写，类似genode的结构，保留capDL initialiser的逻辑（这个思路很好）可以支持静态和动态调用sel4接口分配资源，并后续兼容更多的微内核（甚至是Libos），因此在设计时需要设计不同OS的系统调用抽象层

2. 保留loader的逻辑（为启动sel4提供环境，后续可以兼容更多的微内核，并不是取代Uboot，而是补充微内核所必须的环境准备）

3. monitor继续做监控异常的逻辑，可以处理缺页异常以及其他可以恢复的异常处理操作；需要扩展成支持hypervisor+VMM的逻辑，VMM可以捕获sel4传递的异常，且针对异常做CPU虚拟化，内存虚拟化以及设备/IO虚拟化的逻辑（VMM依赖monitor，但是额外需要其他实现支撑）

4. libmicrokit是提供sel4应用的运行环境，这块我打算沿用这套逻辑，不过需要配合动态分配资源的逻辑的改造，libmicrokit库可以被monitor调用，当然也可以直接使用libos，快速获取posix接口支持

支持sel4/freertos/Fuchsia/fiasco/libos+lkvm作为backend

支持OpenModelica作为模型载体

claude-obsidian作为知识库载体

```


1. 空气动力学 => 神经网路 => GPU
2. 多传感器融合仿真 => 神经网络 => GPU
3. AI合成+AI爬虫模拟世界的数据集+数采+仿真飞行（游戏）=> 数据源
4. 模拟世界：输入（算法控制）/输出（环境反馈，可视化接口） => ROS2
5. 3D可视化引擎 => X
6. ProjectAirSim + Carla <= 生成的神经网络做成组件合入到carla生态+改动carla生态支持静态和动态飞行环境（支持多种数据集，支持数据集泛化）

第一步将carla转换为airla



