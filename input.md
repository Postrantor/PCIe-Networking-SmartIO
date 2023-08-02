---
tip: translate by openai@2023-08-02 15:22:24
url: https://dl.acm.org/doi/fullHtml/10.1145/3462545
...

# SmartIO: Zero-overhead Device Sharing through PCIe Networking SmartIO: Zero-overhead Device Sharing through PCIe Networking(SmartIO：通过 PCIe 网络实现零开销的设备共享)

JONAS MARKUSSEN **and** [](#aff1)LARS BJØRLYKKE KRISTIANSEN, [](#aff1)Dolphin Interconnect Solutions, Norway PÅL HALVORSEN, [](#aff2)SimulaMet, Norway HALVOR KIELLAND-GYRUD, [](#aff3)Dolphin Interconnect Solutions, Norway HÅKON KVALE STENSLAND, [](#aff4)Simula Research Laboratory, Norway CARSTEN GRIWODZ, [](#aff5)University of Oslo, Norway

> 乔纳斯·马库森和拉尔斯·布罗利克·克里斯蒂安森，海豚互连解决方案，挪威帕尔·哈尔沃森，SimulaMet，挪威哈尔沃·基兰德-吉鲁德，海豚互连解决方案，挪威哈康·瓦勒·斯坦斯兰德，Simula 研究实验室，挪威卡斯滕·格里沃兹，奥斯陆大学，挪威

ACM Trans. Comput. Syst., Vol. 38, No. 1-2, Article 2, Publication date: July 2021.

> ACM 计算机系统学报，第 38 卷，第 1-2 期，文章 2，发表日期：2021 年 7 月。
> DOI: [https://doi.org/10.1145/3462545](https://doi.org/10.1145/3462545)

The large variety of compute-heavy and data-driven applications accelerate the need for a distributed I/O solution that enables cost-effective scaling of resources between networked hosts. For example, in a cluster system, different machines may have various devices available at different times, but moving workloads to remote units over the network is often costly and introduces large overheads compared to accessing local resources. To facilitate I/O disaggregation and device sharing among hosts connected using Peripheral Component Interconnect Express (PCIe) non-transparent bridges, we present SmartIO. NVMes, GPUs, network adapters, or any other standard PCIe device may be borrowed and accessed directly, as if they were local to the remote machines. We provide capabilities beyond existing disaggregation solutions by combining traditional I/O with distributed shared-memory functionality, allowing devices to become part of the same global address space as cluster applications. Software is entirely removed from the data path, and simultaneous sharing of a device among application processes running on remote hosts is enabled. Our experimental results show that I/O devices can be shared with remote hosts, achieving native PCIe performance. Thus, compared to existing device distribution mechanisms, SmartIO provides more efficient, low-cost resource sharing, increasing the overall system performance.

> 随着计算密集型和数据驱动型应用程序的大量出现，加速了对分布式 I/O 解决方案的需求，以实现成本效益的网络主机资源扩展。例如，**在集群系统中，不同的机器可能在不同的时间段有不同的设备可用，但是将工作负载移动到网络上的远程单元往往成本高昂，与访问本地资源相比，引入了大量的开销**。为了促进使用外设组件互连(PCIe)非透明桥接器连接的主机之间的 I/O 分离和设备共享，我们提出了 SmartIO。NVMes，GPU，网络适配器或任何其他标准 PCIe 设备都可以借用并直接访问，**就像它们本地属于远程机器一样**。我们通过将传统 I/O 与分布式共享内存功能相结合，提供了超出现有分离解决方案的功能，允许设备成为与集群应用程序相同的全局地址空间的一部分。软件完全从数据路径中移除，并允许在远程主机上运行的应用程序进程之间同时共享设备。我们的实验结果表明，可以与远程主机共享 I/O 设备，实现原生 PCIe 性能。因此，与现有的设备分发机制相比，SmartIO 提供更有效，低成本的资源共享，提高了整体系统性能。

**CCS Concepts:** **• Computer systems organization →** **Distributed architectures**; _Cloud computing_; **• Hardware →** _Buses and high-speed links_; **• Software and its engineering →** _Distributed memory_; **• Information systems →** Distributed storage; _Distributed systems organizing principles_;

> **CCS 概念：** **• 计算机系统组织 →** **分布式架构**; _云计算_; **• 硬件 →** _总线和高速链路_; **• 软件及其工程 →** _分布式存储_; **• 信息系统 →** 分布式存储; _分布式系统组织原则_;

**Additional Key Words and Phrases:** Resource sharing, composable infrastructure, I/O disaggregation, PCIe, cluster architecture, Device Lending, NVMe, GPU, NTB, distributed I/O

> **附加关键字和短语：**资源共享，可组合基础架构，I / O 解耦，PCIe，集群架构，设备出借，NVMe，GPU，NTB，分布式 I / O

ACM Reference format:

Jonas Markussen, Lars Bjørlykke Kristiansen, Pål Halvorsen, Halvor Kielland-Gyrud, Håkon Kvale Stensland, and Carsten Griwodz. 2021. SmartIO: Zero-overhead Device Sharing through PCIe Networking. _ACM Trans. Comput. Syst._ 38, 1-2, Article 2 (July 2021), 78 pages, DOI: [https://doi.org/10.1145/3462545](https://doi.org/10.1145/3462545).

> Jonas Markussen、Lars Bjørlykke Kristiansen、Pål Halvorsen、Halvor Kielland-Gyrud、Håkon Kvale Stensland 和 Carsten Griwodz。 2021。 SmartIO：通过 PCIe 网络实现零开销设备共享。 _ACM Trans. Comput. Syst._ 38，1-2，文章 2(2021 年 7 月)，78 页，DOI：[https://doi.org/10.1145/3462545](https://doi.org/10.1145/3462545)。

## 1 INTRODUCTION

High-performance computing workloads often have high requirements for I/O resources. For example, many computing clusters rely on compute accelerators, such as **graphics processing units (GPUs)** and **field-programmable gate arrays (FPGAs)**, to increase the processing speed. Moving data efficiently between networked nodes and onto such compute accelerators has been a research challenge for decades. In recent years, we have also seen a convergence of high-performance computing, big data, and machine learning research fields. This has led to new demands to I/O performance where distributed, high-volume storage is becoming a requirement for high-performance computing, while low latency networking and facilitating access to compute accelerators have become cloud computing issues [[16](#Bib0016), [80](#Bib0080), [84](#Bib0084)]. If I/O resources (devices) are distributed scarcely among hosts, then cluster nodes with I/O resources may become bottlenecks when a workload requires heavy computation on GPUs or fast access to storage. Contrarily, over-provisioning nodes with resources may lead to devices becoming underutilized if the workload's demands are more sporadic. Heterogeneous workloads may even require widely different compositions of devices for individual nodes. Being able to share and dynamically partition devices between nodes in a cluster leads to more efficient utilization, as I/O resources can be scaled up or down based on current workload requirements.

> 高性能计算工作负载通常对 I/O 资源有很高的要求。例如，许多计算集群依赖于计算加速器，如图形处理器(GPU)和可编程门阵列(FPGA)，来提高处理速度。几十年来，在网络节点和这类计算加速器之间高效地移动数据一直是一个研究难题。近年来，我们还看到高性能计算、大数据和机器学习研究领域的融合。这导致了对 I/O 性能的新需求，其中分布式大容量存储成为高性能计算的必要条件，而低延迟网络和促进对计算加速器的访问已经成为云计算问题[[16](#Bib0016)，[80](#Bib0080)，[84](#Bib0084)]。如果 I/O 资源(设备)在主机之间分布得很稀疏，那么具有 I/O 资源的集群节点可能在工作负载需要在 GPU 上进行大量计算或快速访问存储时成为瓶颈。相反，过度配置节点资源可能导致设备未被充分利用，如果工作负载的需求更加不稳定。异构工作负载甚至可能需要个别节点具有不同组成的设备。**能够在集群中共享和动态划分设备可以实现更有效的利用**，因为可以根据当前工作负载的需求来扩展或缩减 I/O 资源。

In cloud computing environments, such dynamic scaling and resource partitioning is often handled through virtualization. **Virtual machine (VM)** hypervisors may dynamically add virtual I/O devices to VM instances on demand. It is even possible to temporarily suspend computation to migrate VMs to hosts with more hardware resources, should the VM's requirements exceed the available local resources. However, resource virtualization may not be viable when the raw, bare-metal I/O performance is required, for example in the case of GPU-intensive machine learning workloads. In this regard, it is possible to “pass through” physical I/O devices to a VM guest using an **I/O Memory Management Unit (IOMMU)**. The IOMMU facilitates direct access to hardware from the guest without compromising the virtualized environment. Although pass-through allows physical hardware to be used with minimal software overhead, this technique suffers from a lack of flexibility as the physical devices are tightly coupled with the hosts they are installed in. Distributing VMs across hosts in the network in a way that maximizes resource utilization and adapts dynamically to varying I/O requirements, without sacrificing the bare-metal performance that pass-through provides, remains a challenge.

> 在云计算环境中，这种动态缩放和资源分区通常通过虚拟化来处理。**虚拟机(VM)**虚拟机管理程序可以根据需要动态添加虚拟 I / O 设备到 VM 实例。甚至可以暂时挂起计算以将 VM 迁移到具有更多硬件资源的主机，如果 VM 的要求超出了本地可用资源。但是，当需要原始的裸机 I / O 性能时，资源虚拟化可能不可行，例如在 GPU 密集型机器学习工作负载的情况下。在这方面，可以使用**I / O 内存管理单元(IOMMU)**将物理 I / O 设备“传递”到 VM 客户端。 **IOMMU 可以直接从客户端访问硬件，而不会破坏虚拟化环境**。虽然传递允许使用最少的软件开销使用物理硬件，但是这种技术缺乏灵活性，因为物理设备与安装它们的主机紧密耦合。在网络中将 VM 分布到主机，以最大化资源利用率，并根据不断变化的 I / O 要求动态适应，而不牺牲传递提供的裸机性能，仍然是一个挑战。

Another challenge is the networking technology itself. Many network adapters support zero-copy of application memory from one system to another through **remote direct memory access (RDMA)** [[32](#Bib0032)]. RDMA is not only used in many distributed shared-memory cluster applications, but is also frequently used for implementing resource disaggregation. Low-latency storage devices, such as **non-volatile memory express devices (NVMes)**, can be shared at the block-level in the cluster. This is the case for **NVMe over Fabrics (NVMe-oF)** [[29](#Bib0029)], where RDMA is used to provide direct access and avoid going through the block-layer on the **operating system (OS)** on the server. Similarly, the result of a GPU computation may be copied out of GPU memory and onto the network directly using RDMA, without being copied to system memory first and going through the network stack [[91](#Bib0091)]. RDMA disaggregation is usually implemented as application-specific middleware. Although this often requires application software to use specific programming models and semantics, such as message-passing, the benefit is that resources may be shared by several hosts in the network. However, while RDMA allows data to be transferred efficiently over the network, translation between the network protocol and the local I/O bus is unavoidable. Compared to accessing a local device, this protocol translation incurs latency overheads that are not insignificant.

> 另一个挑战是**网络技术本身**。许多网络适配器支持从一个系统到另一个系统的应用程序内存的零复制，通过远程直接内存访问(RDMA)[[32](#Bib0032)]。 **RDMA 不仅用于许多分布式共享内存集群应用程序，而且经常用于实现资源分散**。低延迟存储设备，如非易失性存储器 Express 设备(NVMes)，可以在集群中以块级分享。这就是 NVMe over Fabrics(NVMe-oF)[[29](#Bib0029)]的情况，其中 RDMA 用于提供直接访问，并避免通过服务器上操作系统(OS)的块层。类似地，**GPU 计算的结果可以直接从 GPU 内存复制到网络中，而无需首先复制到系统内存并经过网络堆栈**[[91](#Bib0091)]。 RDMA 分散通常实现为特定于应用程序的中间件。尽管这通常需要应用程序软件使用特定的编程模型和语义，例如消息传递，但其好处是可以在网络中的多个主机之间共享资源。但是，**尽管 RDMA 允许数据在网络上有效地传输，但网络协议和本地 I / O 总线之间的转换是不可避免的**。与访问本地设备相比，此**协议转换会带来不可忽略的延迟开销**。

**Peripheral Component Interconnect Express (PCIe)** is the most widely used standard for connecting devices to a computer system. Although it was originally designed as a local I/O bus connecting devices to the **central processing unit (CPU)** on a motherboard, extending the PCIe bus out of a single computer and connecting several systems is made possible by using a special type of device called **non-transparent bridge (NTB)**. NTBs can be embedded as a CPU feature [[77](#Bib0077), [95](#Bib0095)], but are more commonly implemented in PCIe switch chips [[13](#Bib0013), [82](#Bib0082)], allowing independent computer systems to interconnect with plug-in host adapter cards and external cables [[44](#Bib0044), [50](#Bib0050), [67](#Bib0067), [69](#Bib0069)]. Unlike other interconnection technologies, solutions built with PCIe networking allow resources to be accessed with very little performance overhead as no protocol translation is required. However, while some disaggregation approaches using NTBs have been proposed in the past [[31](#Bib0031), [89](#Bib0089)], these implementations present solutions where devices are owned by a dedicated server. As distributing resources is generally only possible to hosts that are directly connected to the same switch as this server, these approaches forgo the flexibility of fully distributed cluster computing systems. Alternative PCIe-based solutions rely on additional virtualization functionality in the PCIe switch chip hardware to partition the PCIe fabric and create virtual device trees for each individual host [[15](#Bib0015), [51](#Bib0051)]. These solutions allow devices to be directly attached a switch rather than a server. However, these solutions are only able to disaggregate resources at the device level. Sharing the same device with multiple hosts either requires virtualization support in the device itself, i.e., **Single-Root I/O Virtualization (SR-IOV)**, or additional distribution methods, such as RDMA.

> PCIe(外围设备互连总线)是最常用的连接设备到计算机系统的标准。虽然它**最初是设计用来将设备连接到主板上的中央处理器(CPU)的本地 I/O 总线**，但通过使用一种称为**非透明桥(NTB)的特殊类型的设备，可以将 PCIe 总线从单个计算机延伸出来并连接多个系统**。 NTB 可以嵌入到 CPU 功能中[[77]，[95]]，但更常用的是在 PCIe 交换芯片[[13]，[82]]中实现，允许独立的计算机系统通过插入式主机适配器卡和外部电缆[[44]，[50]，[67]，[69]]进行互连。与其他互联技术不同，使用 PCIe 网络构建的解决方案可以访问资源，而不需要任何性能开销，因为不需要协议转换。但是，尽管过去提出了一些使用 NTB 的解耦方法[[31]，[89]]，但这些实现提供的解决方案中的设备都由专用服务器拥有。由于资源的分发通常只能针对直接连接到与此服务器相同的交换机的主机，因此这些方法会放弃完全分布式集群计算系统的灵活性。另一种**基于 PCIe 的解决方案依赖于 PCIe 交换芯片硬件中的附加虚拟化功能来划分 PCIe 结构，并为每个单独的主机创建虚拟设备树**[[15]，[51]]。这些解决方案允许设备直接连接到交换机，而不是服务器。但是，这些解决方案只能在设备级别解耦资源。要想在多个主机上共享相同的设备，要么需要设备本身提供虚拟化支持，即单根 I / O 虚拟化(SR-IOV)，要么需要额外的分发方法，如 RDMA。

To address these challenges, we present our _SmartIO_ system for sharing resources and distributing devices in a heterogeneous, PCIe-interconnected cluster. Unlike existing solutions, our system is able to provide sharing and disaggregation capabilities at multiple abstraction levels: distributing devices to physical hosts, distributing devices to VMs, and enabling disaggregation of devices and memory in software. In addition, our SmartIO system is fully distributed. We avoid relying on dedicated servers and instead allow all hosts to contribute their own local resources and access remote resources, even at the same time. This blurs the distinction between remote and local resources, and scaling out and increasing the overall I/O resource utilization in the system becomes easier.

> 为了解决这些挑战，我们**提出了 _SmartIO_ 系统，用于在异构 PCIe 互连集群中共享资源和分发设备**。与现有解决方案不同，我们的系统能够在多个抽象层次上提供共享和分解功能：将设备分发到物理主机，将设备分发到虚拟机，以及在软件中实现设备和内存的分解。此外，我们的 SmartIO 系统是**完全分布式**的。我们避免依赖专用服务器，而是允许所有主机提供自己的本地资源并同时访问远程资源。这**模糊了远程和本地资源之间的区别**，使系统中 I/O 资源利用率的扩展和提高变得更加容易。

SmartIO is implemented on top of the inherent memory mapping capabilities of NTBs, allowing cluster nodes to map parts of the address space in remote hosts. Our system effectively makes all hosts, including their internal resources (both devices and memory), part of a common PCIe domain. Remote resources can be accessed directly over native PCIe, without requiring any software in the data path or network protocol translation. Furthermore, by relying on PCIe shared-memory techniques, SmartIO is able to abstract away the physical location of devices and memory resources. Our implementation translates memory addresses between different address domains and resolves paths through the PCIe network in a manner that is transparent to both application software and device drivers. As all nodes may contribute their resources, and not only dedicated servers, our SmartIO is able to provide optimizations based on resource locality and minimizing data movement, without requiring the user to be aware of the underlying PCIe topology. This unlocks a new potential in PCIe-connected cluster systems, as application software no longer needs to be written with accessing remote resources in mind, but can be implemented as if resources are local.

> **SmartIO 利用 NTB 的固有内存映射功能实现，允许集群节点映射远程主机的地址空间的部分**。我们的系统有效地使所有主机(包括其内部资源(设备和内存))成为共同的 PCIe 域的一部分。可以直接通过本机 PCIe 访问远程资源，而无需在数据路径或网络协议转换中使用任何软件。此外，通过依赖 PCIe 共享内存技术，SmartIO 能够抽象出设备和内存资源的物理位置。我们的实现在不同的地址域之间转换内存地址，并以对应用软件和设备驱动程序透明的方式通过 PCIe 网络解析路径。由于所有节点都可以提供资源，而不仅仅是专用服务器，因此我们的 SmartIO 能够基于资源局部性和最小化数据移动提供优化，而无需用户了解底层 PCIe 拓扑结构。这为 PCIe 连接的集群系统开启了新的潜力，因为**应用软件不再需要以访问远程资源为目的来编写，而可以像访问本地资源一样实现**。

We have previously demonstrated how Device Lending allows devices to be dynamically assigned to different machines, making it possible for a system to access remote PCIe devices as if they were locally installed [[41](#Bib0041)]. We have also shown how our Device Lending method extends to VMs by implementing a **mediated device interface (MDEV)**, which facilitates pass-through of remote PCIe devices to VMs running on any host in the cluster [[48](#Bib0048), [49](#Bib0049)]. Our new complete SmartIO sharing solution does not only incorporate this earlier work, but greatly extends and supersedes it. We have generalized the core components of our original Device Lending implementation, i.e., the mechanism that enables direct access over PCIe in a manner that is transparent to both device and device driver, and have developed an entirely new **application programming interface (API)**. This new API provides device driver functionality to shared-memory cluster applications, such as mapping shared memory regions for **direct memory access (DMA)** from the device and memory-mapping device registers into application address space. By making device operation part of distributed cluster applications and allowing devices to access shared memory regions using native DMA, it becomes possible to disaggregate devices in software. As such, our new API enables _simultaneous sharing_ of devices between software processes running on different hosts in the cluster, in addition to device-level distribution capabilities provided by Device Lending and MDEV.

> 我们之前演示了设备借用如何允许设备动态分配到不同的机器，使得系统可以像本地安装一样访问远程 PCIe 设备[[41](#Bib0041)]。我们还展示了我们的设备借用方法如何扩展到虚拟机，通过实现一个**中介设备接口(MDEV)**，它可以将远程 PCIe 设备传递给运行在集群中任何主机上的虚拟机[[48](#Bib0048), [49](#Bib0049)]。我们的新完整的 SmartIO 共享解决方案不仅包含了这些早期的工作，而且大大扩展和取代了它。我们普遍化了我们原来设备借用实现的核心组件，即以对设备和设备驱动程序透明的方式实现直接访问 PCIe 的机制，并开发了一个全新的**应用程序编程接口(API)**。这个新的 API 为共享内存集群应用程序提供设备驱动程序功能，例如映射共享内存区域以实现设备的**直接内存访问(DMA)**，以及将设备寄存器映射到应用程序地址空间中。通过将设备操作作为分布式集群应用程序的一部分，并允许设备使用本机 DMA 访问共享内存区域，就可以在软件中分离设备。因此，我们的新 API 可以在集群中的不同主机上的软件进程之间*同时共享*设备，除了设备借用和 MDEV 提供的设备级分布功能之外。

In short, SmartIO is a flexible framework for device distribution and resource sharing that enables cost-effective scaling of resources between PCIe-networked hosts. The main contributions of our work are listed as follows:

> 简而言之，SmartIO 是一个灵活的设备分发和资源共享框架，可以在 PCIe 网络主机之间有效地扩展资源。我们工作的主要贡献如下：

- We have incorporated our previous Device Lending method into our complete SmartIO solution.NVMes, GPUs, network adapters, and any standard PCIe device can be distributed to remote systems and used without any performance difference compared to local access. Devices appear as if they are dynamically hot-added to the system, and can be used by existing application software and device drivers without requiring any modifications.

> 我们已将我们以前的设备借用方法整合到我们的完整 SmartIO 解决方案中。NVMes、GPUs、网络适配器和任何标准 PCIe 设备都可以分发到远程系统，与本地访问相比，性能没有任何差异。设备看起来就像是动态热插拔到系统中，可以由现有的应用软件和设备驱动程序使用，无需任何修改。

- SmartIO also includes our MDEV extension to Device Lending. This interface extends the Linux **Kernel-based Virtual Machine hypervisor (KVM)**. Our extension facilitates direct access to remote physical devices for VM guests, allowing VMs to run on any host in the network and use (remote) devices with bare-metal performance.

> SmartIO 还包括我们的 MDEV 扩展，用于设备借用。该接口扩展了基于 Linux 内核的虚拟机管理程序(KVM)。我们的扩展为虚拟机客户端提供了直接访问远程物理设备的功能，允许虚拟机在网络中的任何主机上运行，并使用(远程)设备具有裸机性能。

- We have created a new device-oriented API for writing device drivers as shared-memory applications. This makes it possible to disaggregate devices in software, similarly to RDMA disaggregation solutions. Unlike RDMA, however, resources are accessed over native PCIe, which allows resources to be shared without introducing a performance penalty. Through our API, device driver implementations may take full advantage of PCIe shared memory capabilities, such as remote memory access and multicasting, without requiring awareness of the PCIe topology and the different address domains of remote systems. This makes it easier for application software to optimize data flow through the PCIe network.

> 我们**创建了一个新的面向设备的 API**，用于编写设备驱动程序作为共享内存应用程序。这使得像 RDMA 分散解决方案一样，可以在软件中分散设备。不同于 RDMA，资源是通过原生 PCIe 访问的，这允许资源在不引入性能损失的情况下共享。通过我们的 API，设备驱动程序实现可以充分利用 PCIe 共享内存功能，如远程内存访问和多播，而无需了解 PCIe 拓扑和远程系统的不同地址域。这使应用程序软件更容易优化通过 PCIe 网络的数据流。

- We have developed a prototype NVMe device driver using our new device-oriented API. Although the Device Lending component of SmartIO makes it possible to use existing device drivers, most device drivers are written in a way that assumes exclusive control over the device. Using Device Lending alone, a device may only be used by a single host at the time. To demonstrate software-enabled disaggregation, we have implemented a _distributed_ NVMe driver. As a proof of concept, we show a single NVMe device can be shared and operated by 30 cluster nodes simultaneously, without requiring SR-IOV. This driver also demonstrates how multiple sharing aspects of our system may be combined, by disaggregating (remote) GPU memory and enabling memory access optimizations.

> 我们使用我们的新型设备导向 API 开发了一个 NVMe 设备驱动程序的原型。尽管 SmartIO 的设备借用组件使得可以使用现有的设备驱动程序，但大多数设备驱动程序的编写方式假定对设备具有独占控制权。仅使用设备借用，一个设备只能被一个主机同时使用。为了演示软件实现的分散，我们实现了一个分布式 NVMe 驱动程序。作为一个概念验证，我们**展示了一个单一的 NVMe 设备可以被 30 个集群节点同时共享和操作，而无需 SR-IOV**。该驱动程序还演示了如何结合我们系统的多个共享方面，通过分散(远程)GPU 内存并实现内存访问优化。

- To prove that our solution enables zero-overhead sharing, we provide a comprehensive performance evaluation covering all components of our SmartIO solution, including our earlier Device Lending and MDEV work. We have performed entirely new experiments, using both synthetic microbenchmarks and realistic large-scale workloads. Our experimental results confirm that I/O devices can be distributed to, and shared with, remote hosts, without any performance penalty beyond what is expected for longer PCIe paths. In fact, all our experiments prove that remote devices can be used _without any performance overhead_ compared to local access in terms of latency and throughput.

> 为了证明我们的解决方案可以实现零开销共享，我们提供了一个全面的性能评估，涵盖了我们 SmartIO 解决方案的所有组件，包括我们之前的设备借用和 MDEV 工作。我们完全进行了新的实验，使用合成微基准和真实的大规模工作负载。我们的实验结果证实，I / O 设备可以分发到远程主机并与其共享，而不会产生任何性能损失，除了预期的更长的 PCIe 路径。事实上，我们**所有的实验都证明，与本地访问相比，远程设备可以在延迟和吞吐量方面 _没有任何性能损失_**。

The rest of this article is structured as follows: Section [2](#sec-7) gives a high-level overview of our SmartIO system. Section [3](#sec-10) explains the basic building blocks of shared-memory networking with PCIe. In Section [4](#sec-14), we detail our Device Lending method, and in Section [5](#sec-19), we explain how the original Device Lending was enhanced with hypervisor support (MDEV). In Section [6](#sec-25), we describe our new software API and use a distributed NVMe driver implementation as an example implementation. We present our experimental results and extensive evaluation in Section [7](#sec-31), before we provide a discussion of other aspects and considerations of our SmartIO solution in Section [8](#sec-47). Finally, we put the work in the context of state of the art in Section [9](#sec-55), and conclude the article in Section [10](#sec-61).

> 本文的其余部分结构如下：
>
> - 第 2 节(#sec-7)简要介绍了我们的 SmartIO 系统；
> - 第 3 节(#sec-10)解释了 PCIe 共享内存网络的基本构建模块；
> - 第 4 节(#sec-14)详细介绍了我们的设备借用方法，
> - 第 5 节(#sec-19)解释了如何通过虚拟机支持(MDEV)来增强原有的设备借用方法；
> - 第 6 节(#sec-25)描述了我们的新软件 API，并以分布式 NVMe 驱动程序实现为例；
> - 第 7 节(#sec-31)展示了我们的实验结果和广泛的评估；
> - 第 8 节(#sec-47)讨论了我们的 SmartIO 解决方案的其他方面和考虑因素；最后，
> - 第 9 节(#sec-55)将本文的工作置于最新技术的背景之下，
> - 第 10 节(#sec-61)对本文做出结论。

## 2 SYSTEM OVERVIEW

Our SmartIO solution allows the local resources of a host, i.e., memory and devices, to be accessed directly by remote hosts, over standard PCIe. SmartIO works for _all_ standard PCIe devices. Individual device functions of multi-function devices may be distributed to different hosts in the network, or to the same host should it require multiple resources. It is even possible to disaggregate a single device (function) in software, and distribute it to multiple hosts.

> 我们的 **SmartIO 解决方案允许远程主机直接访问主机的本地资源，即内存和设备，通过标准 PCIe**。SmartIO 适用于 _所有_ 标准 PCIe 设备。多功能设备的各个设备功能可以分发到网络中的不同主机，或者如果需要多个资源，可以分发到同一个主机。甚至可以在软件中分解单个设备(功能)，并将其分发到多个主机。

As depicted in Figure [1](#fig1), we can imagine this as hosts contributing their internal resources to a pool of shared resources. Through a process of borrowing devices and releasing them when they are no longer needed, it is possible to support a dynamic and composable I/O infrastructure consisting of a combination of local and remote resources. Whether devices are actually local or remote becomes irrelevant to the user, as SmartIO eliminates this distinction, both function and performance wise. In other words, SmartIO is a solution for scaling out and using more hardware resources than there are available in a single host.

> 如图 1 所示，我们可以想象主机将其内部资源投入到一个共享资源池中。通过借用设备并在不再需要时释放它们的过程，可以支持由本地和远程资源组成的动态可组合的 I / O 基础架构。实际上，设备是本地的还是远程的对用户来说就不重要了，因为 SmartIO 在功能和性能方面消除了这种区别。换句话说，SmartIO 是一种扩展和使用比单个主机更多硬件资源的解决方案。

> ![](https://dl.acm.org/cms/attachment/066a4e2f-1f8c-412c-8337-bb9b17be1b14/tocs380102-02-f01.jpg)
> Fig. 1. SmartIO allows the internal devices of hosts in the network to be shared with other hosts connected to the same fabric. Nodes in a PCIe-networked cluster can contribute their internal devices to a shared device pool, and borrow resources from that pool when needed.

### 2.1 Motivation and Challenges

Due to its very low latency overhead and memory addressing properties, using PCIe as a high-speed interconnection technology is a compelling alternative to traditional networking technologies [[44](#Bib0044), [50](#Bib0050), [67](#Bib0067)]. However, because PCIe was originally designed as a local I/O bus, connecting devices to the CPU on a motherboard, individual computer systems operate with different PCIe address domains. Interconnecting systems using PCIe require translating memory transactions from one address domain to another. The most common method of translating addresses is to use NTBs [[69](#Bib0069), [82](#Bib0082), [87](#Bib0087)]. Figure [2](#fig2) illustrates how several computer systems may be interconnected in a cluster, by implementing adapter cards and cluster switches with NTBs. The inherent memory address translation capabilities of NTBs make it possible to map (parts of) the address space of remote systems. More interesting, however, is the fact that in such PCIe networks, both CPUs and internal PCIe devices are attached to the same, shared PCIe fabric.

> 由于**其非常低的延迟开销和内存地址映射属性，使用 PCIe 作为高速互连技术是传统网络技术的一个有力替代选择**[[44](#Bib0044)，[50](#Bib0050)，[67](#Bib0067)]。然而，由于 PCIe 最初是设计为本地 I / O 总线，将设备连接到主板上的 CPU，因此各个计算机系统具有不同的 PCIe 地址域。**使用 PCIe 连接系统需要将内存事务从一个地址域转换到另一个地址域。最常用的地址转换方法是使用 NTB** [[69](#Bib0069)，[82](#Bib0082)，[87](#Bib0087)]。图[2](#fig2)描述了如何通过实现具有 NTB 的适配器卡和集群交换机来将几个计算机系统连接到集群中。**NTB 的固有内存地址转换功能使得可以映射(远程系统的部分)地址空间**。更有趣的是，在这种 PCIe 网络中，CPU 和内部 PCIe 设备都连接到同一个共享的 PCIe 结构。

> ![](https://dl.acm.org/cms/attachment/d013889d-7324-41e5-9e60-db3970d3225d/tocs380102-02-f02.jpg) 
> Fig. 2. We can create a heterogeneous PCIe cluster by interconnecting nodes (hosts) with external PCIe links using adapter cards capable of non-transparent bridging (NTB). In such clusters, the CPUs as well as the internal devices of each node are all attached to the same PCIe network fabric.

Remote resources, such as memory and I/O devices, can be mapped into a local system and accessed through the NTB. Similarly, a remote device capable of DMA may also use the NTB to access local resources. This eliminates the need to use memory on the remote node as an intermediate step when transferring data. As illustrated in Figure [3](#fig3), software overhead can be avoided, since all memory address translations can be done in NTB hardware.

> **远程资源，如内存和 I / O 设备，可以映射到本地系统，并通过 NTB 访问**。类似地，具有 DMA 功能的远程设备也可以使用 NTB 访问本地资源。这消除了在传输数据时使用远程节点上的内存作为中间步骤的需要。如图 3 所示，可以**避免软件开销，因为所有的内存地址转换都可以在 NTB 硬件中完成**。


![](https://dl.acm.org/cms/attachment/33ef8be9-e1ef-4724-ab85-396600735560/tocs380102-02-f03.jpg) 
Fig. 3. Many disaggregation solutions have performance overheads, because they rely on middleware or other forms of software facilitation on the remote system. Using SmartIO, remote hardware can be accessed directly without any software in the critical path by setting up memory mappings over the NTB.

However, setting up such NTB mappings requires awareness of the address space on the remote system. When initiating DMA transfers, a device driver must use addresses that corresponds with the remote device's address space to enable a DMA-capable device to read or write across the NTB. This greatly increases the programming complexity of device drivers. Therefore, our SmartIO system provides a mechanism for using NTBs while remaining agnostic about the address space in remote systems. The physical location of a resource, as well as the address space layout in the host it is installed in, is entirely abstracted away.

> 然而，**设置这种 NTB 映射需要了解远程系统的地址空间**。在发起 DMA 传输时，设备驱动程序必须使用与远程设备地址空间对应的地址，以使 DMA 可能的设备可以跨 NTB 读写。这大大增加了设备驱动程序的编程复杂性。因此，我们的 SmartIO 系统提供了一种使用 NTB 的机制，同时保持对远程系统地址空间的不可知性。资源的物理位置以及它安装在主机中的地址空间布局完全被抽象化了。

Nevertheless, this abstraction gives rise to another challenge; a device driver that is unaware that a device is remote may assume that the entire local address space can be reached by the device. It is generally not possible to predict in advance which memory addresses a device driver may use, yet NTB mappings must be in place before the device driver initiates DMA transfers. Deferring mappings until the device driver initiates DMA would require synchronizing with the remote system in the critical path, thus increasing the overall latency. A naive workaround is mapping the entire memory for the device, but this solution does not scale for multiple hosts. SmartIO solves this, and is able to prepare necessary memory-mappings in advance, without introducing any communication overhead in the critical path.

> 尽管如此，这种抽象也带来了另一个挑战：一个不知道设备是远程的设备驱动程序可能会假定整个本地地址空间都可以被设备访问。通常无法预先预测设备驱动程序可能使用哪些内存地址，但是**在设备驱动程序启动 DMA 传输之前，必须先建立 NTB 映射**。将映射推迟到设备驱动程序启动 DMA 时，就需要在关键路径上与远程系统同步，从而增加整体延迟。一个简单的解决方案是为设备映射整个内存，但是这种解决方案不能用于多个主机。SmartIO 解决了这个问题，可以**提前准备必要的内存映射，而不会在关键路径上引入任何通信开销**。

### 2.2 Overall Design

Our system is composed of _“borrowers”_ and _“lenders.”_ A lender is a computer system that registers one or more of its internal PCIe devices with SmartIO, allowing the devices to be distributed to and used by remote hosts. A borrower is a system that is currently using such a device. While a device only has one lender, namely, the computer system where it is physically installed, there can be several borrowers using it simultaneously.[1](#fn1) SmartIO also makes it possible for a system to act as both lender and borrower at the same time, lending out its own local devices and simultaneously borrowing remote devices from other hosts.

> 我们的**系统由“借用者”和“出借者”组成**。出借者是一个计算机系统，它**将其一个或多个内部 PCIe 设备注册到 SmartIO，允许这些设备被分发到远程主机并被使用**。借用者是当前正在使用这样一个设备的系统。虽然一个设备只有一个出借者，即它物理安装的计算机系统，但可以有几个借用者同时使用它。[1](＃fn1)SmartIO 还使得一个系统同时充当出借者和借用者，出借自己的本地设备，同时从其他主机借用远程设备。

Building PCIe networking into our system is a crucial part of our design, as it enables access to remote resources with very low latency and extremely low computing overheads. The hard separation between local and remote is blurred, with regard to both functionality and performance. Furthermore, this design means that the implementation complexity of SmartIO lies in software. SmartIO can be implemented for existing computer systems that are connected with NTBs, using either on-board PCIe switch chips or plug-in adapter cards, in any network topology.

> 在我们的系统中建立 PCIe 网络是设计的关键部分，因为它可以**以非常低的延迟和极低的计算开销访问远程资源**。本地和远程之间的硬分离在功能和性能方面变得模糊。此外，这种设计意味着 SmartIO 的实现复杂度位于软件中。SmartIO 可以用 NTB 连接的现有计算机系统实现，使用板载 PCIe 交换芯片或插入式适配器卡，在任何网络拓扑中实现。

Figure [4](#fig4) illustrates the different components of our system and how they fit together:

> 图 4(#fig4)描绘了我们系统的不同组件以及它们如何结合在一起：

> ![](https://dl.acm.org/cms/attachment/7c350e06-fe90-4961-9b64-b39659402814/tocs380102-02-f04.jpg) 
> Fig. 4. SmartIO provides different interfaces that facilitate access to a remote resource. These interfaces present an abstraction layer to application software and device drivers, providing a logical decoupling of devices and which physical hosts they are installed in.

1.  **Low-level NTB driver:** Our SmartIO solution is built on top of NTB interconnection technology. The low-level NTB driver makes it possible to connect hosts over a PCIe network fabric and set up memory-mappings on demand. Moreover, the NTB driver also enables individual systems to contribute parts (or “segments”) of their local memory to a cluster-wide, distributed shared-memory space. Cluster applications may use the **Software Infrastructure Shared-Memory Cluster Interconnect API (SISCI)** [[22](#Bib0022)] to manage local and remote segments of memory and map them into the application's local address space.

> 我们的 **SmartIO 解决方案建立在 NTB 连接技术之上**。低级 NTB 驱动程序可以通过 PCIe 网络结构连接主机，并根据需要设置内存映射。此外，NTB 驱动程序还可以使各个系统将其本地内存的部分(或“段”)贡献给集群范围的分布式共享内存空间。集群应用程序可以使用**软件基础设施共享内存集群互连 API(SISCI)**[[22](＃Bib0022)]来管理本地和远程内存段，并将它们映射到应用程序的本地地址空间中。

2.  **Resource abstraction mechanism:** SmartIO provides functionality for transparently translating I/O addresses between different address domains, resolving paths in the cluster, and dynamically setting up necessary NTB mappings for the borrowing system and the device. This makes it possible to abstract away the location of the device, i.e., which host machine it is installed in, in a manner that is transparent to both the device and the software process using the device. With this abstraction, SmartIO can facilitate the use of remote resources (both memory and devices) without requiring software to be aware of the underlying, physical PCIe topology or the internal I/O address space layout of remote hosts. SmartIO also supports setting up mappings between multiple devices, even when they reside in different lenders, allowing PCIe transactions between them to be routed along the shortest path in the PCIe network (peer-to-peer).

> SmartIO 提供功能，可以在不同的地址域之间透明地转换 I/O 地址，解析集群中的路径，并为借用系统和设备动态设置必要的 NTB 映射。这使得可以以设备和使用设备的软件进程都透明的方式抽象出设备的位置，即它安装在哪台主机上。通过这种抽象，SmartIO 可以在不需要软件了解底层的物理 PCIe 拓扑或远程主机的内部 I/O 地址空间布局的情况下，促进远程资源(内存和设备)的使用。 SmartIO 还支持在多个设备之间设置映射，即使它们位于不同的出借人，也允许在 PCIe 网络上沿着最短路径(点对点)路由 PCIe 事务。

3.  **Device Lending:** SmartIO incorporates our Device Lending method [[41](#Bib0041)], which allows devices to be time-shared among hosts in the PCIe network. By borrowing a device and inserting it into the local device tree, the remote device appears to be hot-added to a local system. Devices can, therefore, be dynamically added to the system, without requiring the borrowing host to reboot. When the host performs configuration cycles and sets up memory mappings, SmartIO is able to intercept this and inject resolved remote addresses. This allows existing software to use our system without requiring any modifications or special adaptions; device drivers, application software and even the OS can use the device as if it was locally installed. While Device Lending only allows devices to be distributed to a single host at the time, it is nevertheless highly suitable in the case where a device has a complex or proprietary device driver, and using existing drivers is the only viable option for operating the device.

> 3. **设备借用：**SmartIO 采用我们的设备借用方法[[41](#Bib0041)]，允许设备在 PCIe 网络中进行时间共享。通过借用设备并将其插入本地设备树，远程设备似乎已经被热添加到本地系统中。因此，可以动态地将设备添加到系统中，而无需借用主机重新启动。当主机执行配置周期并设置内存映射时，SmartIO 能够拦截并注入解析的远程地址。这使得现有的软件可以使用我们的系统，而无需任何修改或特殊适配；设备驱动程序、应用程序甚至操作系统都可以像使用本地安装的设备一样使用该设备。虽然设备借用只允许将设备同时分发到一台主机，但在设备具有复杂或专有设备驱动程序的情况下，它仍然非常适用，并且使用现有驱动程序是操作设备的唯一可行选择。

4.  **MDEV:** Our MDEV extension to the KVM hypervisor [[48](#Bib0048), [49](#Bib0049)] facilitates pass-through of borrowed devices to VMs running on the host. VM guests can access these devices directly without breaking out of the memory isolation provided by the virtualization, even when the devices are remote. This allows VMs to be distributed on different hosts in the cluster while benefiting from the bare-metal performance of direct access to physical hardware.

> 我们对 KVM 虚拟化器的 MDEV 扩展[[48](#Bib0048), [49](#Bib0049)]可以实现借用设备的直通，虚拟机客户端可以直接访问这些设备，而不会破坏虚拟化提供的内存隔离，即使这些设备是远程的。这样，虚拟机就可以分布在集群的不同主机上，同时也能够从直接访问物理硬件的裸机性能中受益。

5.  **Device driver API:** As an alternative to Device Lending and MDEV, our SmartIO solution also provides a new device driver API extension for managing devices and developing distributed device drivers using cluster functionality. This new contribution extends the existing SISCI API with programming semantics for memory-mapping device registers and making shared memory segments available for a DMA-capable device. Device operation becomes part of the cluster application itself, allowing devices to access shared memory segments using native DMA. Furthermore, by relying on our SmartIO system to resolve memory addresses between the individual address domains, a driver implementation does not need to consider the system-local address space of the cluster node where the device is installed. This greatly reduces the complexity of implementing distributed applications, as it becomes possible for software to assume that resources are local, while taking full advantage of PCIe-based shared memory capabilities. Using this API extension, devices may be disaggregated at the software level and shared simultaneously between application processes running on different remote hosts.

> 5. **设备驱动程序 API：**作为 Device Lending 和 MDEV 的替代方案，我们的 SmartIO 解决方案还提供了一个新的设备驱动程序 API 扩展，用于管理设备并使用集群功能开发分布式设备驱动程序。这一新贡献扩展了现有的 SISCI API，为内存映射设备寄存器和使具有 DMA 能力的设备可用的共享内存段提供了编程语义。设备操作成为集群应用程序本身的一部分，允许设备使用本机 DMA 访问共享内存段。此外，通过依赖我们的 SmartIO 系统来解析各个地址域之间的内存地址，驱动程序实现不需要考虑安装设备的集群节点的系统本地地址空间。这大大降低了实现分布式应用程序的复杂性，因为软件可以假定资源是本地的，同时充分利用基于 PCIe 的共享内存功能。使用此 API 扩展，设备可以在软件层面被分离，并在运行在不同远程主机上的应用程序进程之间共享。

Finally, it should be noted that the design of our system enables sharing at multiple abstraction levels. It is possible to combine the different interfaces of SmartIO. For example, using our API extension, we can disaggregate the device memory of a remote GPU being borrowed with Device Lending, even if it is managed by a proprietary device driver that is unaware that the device is remote.

> 最后，应该指出，我们系统的设计可以在多个抽象层次上共享。可以结合 SmartIO 的不同接口。例如，使用我们的 API 扩展，即使远程 GPU 被借用的设备内存由不知道设备是远程的专有设备驱动程序管理，我们也可以对其进行解耦。

## 3 PCIE-INTERCONNECTED CLUSTERS

While there are several networking technologies that make it possible to build clusters of networked computers, such as Infiniband, 100/200 Gigabit Ethernet, and Fibre Channel, PCIe is interesting in that connecting multiple systems with PCIe will also connect their internal devices to the same interconnection fabric. The idea of a unified bus for the inner components of a computer to connect the devices with the other cluster machines, however, is not new. It was already imagined for both ATM [[72](#Bib0072)] and SCI [[6](#Bib0006)]. Nevertheless, these ideas never got implemented, because neither technology were picked up as an internal interconnection network for computers. In contrast, PCIe is today the most widely adopted standard for connecting devices in a system [[25](#Bib0025)].

> 虽然有几种网络技术可以构建网络计算机集群，例如 Infiniband、100/200 Gigabit Ethernet 和 Fibre Channel，但 PCIe 有趣的是，使用 PCIe 连接多个系统也会将它们的内部设备连接到同一互连网络。然而，将计算机内部组件连接到其他集群机器的统一总线的想法并不是新的。它早已被用于 ATM [[72](#Bib0072)]和 SCI [[6](#Bib0006)]。然而，这些想法从未实施，因为这两种技术都没有被选为计算机的内部互连网络。相比之下，PCIe 今天是连接系统中设备的最广泛采用的标准 [[25](#Bib0025)]。

The most common way of extending the PCIe bus out of a single system to connect several systems to the same PCIe fabric, is by using special devices called NTBs [[50](#Bib0050), [67](#Bib0067), [69](#Bib0069), [87](#Bib0087), [89](#Bib0089)]. By implementing NTBs as a peripheral device, independent computer systems can interconnect with plug-in adapter cards and external cables. Using such adapter cards and cluster switches with NTB-capable ports, we have created a heterogeneous PCIe cluster, supporting up to 60 PCIe-networked nodes.

> 最常见的方式来扩展 PCIe 总线从单一系统连接多个系统到同一 PCIe 结构，是通过使用特殊设备称为 NTB [[50](#Bib0050), [67](#Bib0067), [69](#Bib0069), [87](#Bib0087), [89](#Bib0089)]。通过实现 NTB 作为外围设备，独立的计算机系统可以通过插件适配器卡和外部电缆连接。使用这种适配器卡和具有 NTB 能力端口的集群交换机，我们创建了一个异构 PCIe 集群，支持多达 60 个 PCIe 网络节点。

### 3.1 PCIe Endpoints

PCIe is a high-speed serial computer expansion bus standard and uses point-to-point links, where a link consists of 1 to 16 lanes. Each lane is a full-duplex serial connection. Data is striped across multiple lanes, so broader links yield higher bandwidth. PCIe revision 3.1 (Gen3) [[61](#Bib0061)] allows a theoretical maximum bandwidth of 15.75 GB/s for an x16 link — approximately 13.8 GB/s of usable throughput.

> PCIe 是一种高速串行计算机扩展总线标准，使用点对点链路，其中一个链路由 1 到 16 条通道组成。每条通道都是全双工串行连接。数据在多个通道上分片，因此更宽的链路可以产生更高的带宽。PCIe 3.1(Gen3)[[61](#Bib0061)]允许 x16 链路的理论最大带宽为 15.75 GB/s，可用吞吐量约为 13.8 GB/s。

As illustrated in Figure [5](#fig5), a PCIe domain is structured as a tree. At the top of the tree, we have the “root ports,” acting as the connection between the PCIe fabric and the CPU. This forms what is known as a “root complex.” Devices are the leaf nodes in the PCIe domain, and are known as “endpoints” in PCIe terminology.

> 如图 5 所示，PCIe 域结构如树状结构。在树的顶部，我们有“根端口”，作为 PCIe 结构和 CPU 之间的连接。这形成了所谓的“根复合体”。设备是 PCIe 域中的叶节点，在 PCIe 术语中称为“端点”。

![](https://dl.acm.org/cms/attachment/a3d4a1e0-a78c-4ffa-8d13-1c3fe1c03540/tocs380102-02-f05.jpg) Fig. 5. Example of a PCIe topology using an external link to connect an expansion chassis to a computer system. The devices in the expansion chassis are part of the same PCIe tree as the internal devices, because all downstream links (including the external cable) are _transparent_.

Some PCIe devices may support multiple functions, which appear to the system as a group of distinct devices, each with a separate set of resources and device memory regions. The term “device” actually refers to an individual function. An example of a multi-function device is a multi-port Ethernet adapter, where individual ports can be implemented as separate functions, or a GPU with a sound device, where the video controller appears as one device and the sound card as another. It is also possible for a device to implement SR-IOV [[62](#Bib0062)]. SR-IOV-capable devices appear to the system to have multiple (virtual) functions. Note that our SmartIO system makes no distinction between physical and virtual functions.

> 一些 PCIe 设备可以支持多种功能，它们在系统中显示为一组不同的设备，每个设备都有一组单独的资源和设备内存区域。“设备”这个词实际上指的是单个功能。多功能设备的一个例子是多端口以太网适配器，其中每个端口可以实现为单独的功能，或者带有声卡的 GPU，其中视频控制器显示为一个设备，声卡显示为另一个设备。设备也可以实现 SR-IOV[[62]](#Bib0062)。SR-IOV 兼容设备在系统中显示为具有多个(虚拟)功能。请注意，我们的 SmartIO 系统不区分物理功能和虚拟功能。

### 3.2 Address-based Routing

The defining feature of PCIe is that devices are mapped into the same address space as the CPU and system memory, as depicted in Figure [6](#fig6). Because this mapping exists, a CPU can read and write to device memory the same way it would access system memory.[2](#fn2) Likewise, if a device is capable of **direct memory access (DMA)**, then it can read from and write to system memory. A device may even access other devices on the fabric, as they too are mapped into the same address space.

> PCIe 的定义特征是，设备映射到与 CPU 和系统内存相同的地址空间，如图 6 所示。由于存在这种映射，CPU 可以像访问系统内存一样读取和写入设备内存。同样，如果设备具有**直接内存访问(DMA)**功能，那么它可以读取和写入系统内存。设备甚至可以访问结构中的其他设备，因为它们也映射到相同的地址空间。

![](https://dl.acm.org/cms/attachment/c8e8286d-d1b2-4577-b6d3-42b843855c56/tocs380102-02-f06.jpg) Fig. 6. Device memory regions (BARs) are mapped into the same address space as system memory.

This mapping occurs when a system enumerates the PCIe tree and accesses the configuration space of each device attached to the fabric. The configuration space contains a description of the capabilities of the device, such as the device's memory regions. The system will reserve a memory address range for each of the device's memory regions. The start addresses are then written to the device's **Base Address Registers (BARs)** in its configuration space, making the device aware of the address space mapping. Therefore, the term “BAR” is synonymously used for device memory regions, and a device may have up to six BARs.

> 这种映射发生在系统枚举 PCIe 树并访问连接到结构的每个设备的配置空间时。配置空间包含设备的功能描述，例如设备的内存区域。系统将为每个设备的内存区域保留一个内存地址范围。然后将起始地址写入设备的配置空间中的**基地址寄存器(BAR)**，使设备知道地址空间映射。因此，术语“BAR”可以用来指代设备内存区域，一个设备最多可以有六个 BAR。

Like other networking technologies, PCIe also uses a layered protocol. The physical layer and data link layer are responsible for flow control, error correction and signal encoding. The uppermost layer is called the transaction layer, and its responsibility includes forwarding memory reads and writes as “transactions.” Such transactions are routed in the PCIe fabric based on their addresses. The transaction layer is also responsible for packet ordering, ensuring that memory operations in PCIe are strictly ordered.[3](#fn3)

> 其他网络技术一样，PCIe 也使用分层协议。物理层和数据链路层负责流量控制、错误校正和信号编码。最上层称为事务层，其责任包括转发内存读写作为“事务”。这些事务根据它们的地址在 PCIe 结构中路由。事务层还负责包的排序，确保 PCIe 中的内存操作严格有序。[3](#fn3)

In Figure [5](#fig5), we also illustrate how the PCIe tree may be extended through the use of an expansion chassis. Devices in an expansion chassis are connected to the same root complex (CPU) through a series of transparent switches. These switches form subtrees in the network. During the enumeration, switch ports are assigned the combined address range of their downstream devices (Figure [6](#fig6)). This allows memory transactions to be routed hierarchically in the PCIe tree where memory transactions are forwarded either upstream or downstream based on the address. An invariant of this hierarchical routing is that memory accesses do not need to pass through the root, but can be routed using the shortest path. This is referred to as “peer-to-peer” in PCIe terminology. In Figure [5](#fig5), the internal switch in the expansion chassis will have the combined downstream address range of all three GPUs, allowing memory accesses to be routed directly between them. Some PCIe switch chips also support multicasting, allowing memory writes to be replicated to multiple selected ports in a single operation [[61](#Bib0061)].

> 在图 5 中，我们还描述了如何通过使用扩展机箱来扩展 PCIe 树。扩展机箱中的设备通过一系列透明交换机连接到同一个根复杂(CPU)。这些交换机形成网络中的子树。在枚举期间，交换机端口被分配给其下游设备的组合地址范围(图 6)。这允许内存事务以层次结构在 PCIe 树中路由，其中内存事务根据地址向上或向下转发。这种层次路由的不变量是内存访问不需要经过根，而是可以使用最短路径进行路由。这在 PCIe 术语中称为“点对点”。在图 5 中，扩展机箱中的内部交换机将具有所有三个 GPU 的组合下游地址范围，允许直接在它们之间路由内存访问。一些 PCIe 交换芯片还支持组播，允许在单个操作中将内存写入复制到多个选定端口(参见参考文献 61)。

PCIe also uses **message-signaled interrupts (MSI)** instead of physical interrupt lines. MSI-capable devices post a memory write to the CPU, using a specific address and payload assigned by the system. The memory write is then interpreted by the CPU, which uses the payload and address to raise an interrupt. MSI-X is an extension to MSI, allowing up to 2048 different interrupt vectors. A benefit of this is that an MSI-X interrupt can target a specific CPU core on multi-core systems. Additionally, separate MSI-X vectors can be used to indicate different types of events.

> PCIe 也使用消息信号中断(MSI)而不是物理中断线。具备 MSI 功能的设备会向 CPU 发布内存写入，使用系统分配的特定地址和有效负载。然后 CPU 会解释内存写入，使用有效负载和地址引发中断。MSI-X 是 MSI 的一个扩展，允许多达 2048 个不同的中断向量。这样的好处是，MSI-X 中断可以在多核系统上针对特定的 CPU 核心。此外，可以使用不同的 MSI-X 向量来指示不同类型的事件。

### 3.3 Non-transparent Bridging

As PCIe tree enumeration and address reservation is typically done during system start up, the address space layout will vary from system to system. Different systems, or different root complexes, will have independent address space layouts. Because of this, a PCIe domain has exactly _one_ active root complex at any point in time. Two independent CPUs are not allowed to coexist in the same domain. However, by using an NTB implementation [[44](#Bib0044), [69](#Bib0069), [82](#Bib0082)], two root complexes, meaning independent hosts, can be connected together over PCIe. Although not formally standardized, NTBs are a widely adopted solution, and all NTB implementations have similar capabilities [[87](#Bib0087)]. NTBs can be embedded as a CPU feature, such as Intel Xeon [[77](#Bib0077)] and AMD Zeppelin [[95](#Bib0095)], but are more commonly implemented in PCIe switch chips [[13](#Bib0013), [82](#Bib0082)].

> 由于 PCIe 树形枚举和地址预留通常在系统启动时进行，因此地址空间布局因系统而异。不同的系统或不同的根复合体将具有独立的地址空间布局。因此，PCIe 域在任何时候都只有*一个*活动的根复合体。不允许两个独立的 CPU 在同一个域中共存。但是，通过使用 NTB 实现[[44](＃Bib0044)，[69](＃Bib0069)，[82](＃Bib0082)]，两个根复合体，即独立的主机，可以通过 PCIe 连接在一起。尽管没有正式标准化，NTB 是一种广泛采用的解决方案，所有 NTB 实现都具有类似的功能[[87](＃Bib0087)]。 NTB 可以作为 CPU 功能嵌入，例如 Intel Xeon [[77](＃Bib0077)]和 AMD Zeppelin [[95](＃Bib0095)]，但更常见的是在 PCIe 交换芯片中实现[[13](＃Bib0013)，[82](＃Bib0082)]。

Figure [7](#fig7) depicts two independent root complexes connected using NTB adapter cards with an external PCIe cable. Despite the name, an NTB actually appears as a PCIe _endpoint_. Just like regular endpoints, NTBs appear to have one or more memory regions, or BARs, that are reserved and mapped by the system during the enumeration. However, instead of being backed by memory or device registers, reads and writes to these memory regions will be forwarded from one side of the NTB to the other, translating the memory addresses in the process. As these memory regions appear to the system as any other memory-mapped device memory region, a local CPU can read from or write to them as if it was local device memory.

> 图 7 描绘了使用 NTB 适配卡和外部 PCIe 电缆连接的两个独立的根复合体。尽管名字如此，NTB 实际上显示为 PCIe 端点。与普通端点一样，NTB 似乎具有一个或多个由系统在枚举期间保留和映射的内存区域或 BAR。但是，除了由内存或设备寄存器支持外，对这些内存区域的读取和写入将从 NTB 的一端转发到另一端，并转换内存地址。由于这些内存区域对系统而言就像其他任何内存映射设备内存区域一样，本地 CPU 可以像访问本地设备内存一样读取或写入它们。

![](https://dl.acm.org/cms/attachment/61349602-e9b7-4e71-8a53-8fc9025feedf/tocs380102-02-f07.jpg) Fig. 7. Example of two independent PCIe root complexes connected together using an NTB. The link between the two hosts is _non-transparent_, and the NTB translates addresses between the two domains. Host A has mapped parts, or segments, of Host B's memory through its local NTB, providing Host A with “windows” into the remote system's address space.

Note that the address space associated with the NTB BAR may be too small to cover all system memory of the remote system. While it is possible to adjust the BAR sizes and provide larger ranges, many systems do not support support large device memory regions. However, NTB implementations also support dividing their range into “windows.” By using a different base offset per NTB window, it is possible to map arbitrary ranges of the remote system's address space. Such offset mappings makes it is possible to map different parts of a remote system's address space into local address space. The far-side address of a mapping is stored in a look-up table, making the address translation between the two domains very fast. However, the number of NTB windows is limited by the number of entries in the look-up table.

> 注意，与 NTB BAR 相关联的地址空间可能太小，无法覆盖远程系统的所有系统内存。虽然可以调整 BAR 大小并提供更大的范围，但许多系统不支持大型设备内存区域。但是，NTB 实现还支持将其范围划分为“窗口”。通过每个 NTB 窗口使用不同的基本偏移量，可以映射远程系统的任意地址范围。这种偏移映射使得可以将远程系统的不同地址空间映射到本地地址空间。映射的远端地址存储在查找表中，使两个域之间的地址转换非常快。但是，NTB 窗口的数量受查找表中条目的数量的限制。

The SISCI shared memory API [[22](#Bib0022)] provides functionality for allocating linear “segments” from a pool of contiguous memory pages that is reserved by the low-level NTB driver in advance. These linear segments can be “exported,” allowing remote hosts to map them through their NTBs and access it as if it was local device memory. By allowing segments of their own local memory to be mapped by remote hosts, individual nodes effectively contribute to a distributed shared-memory architecture comprised of such memory segments. Multiple nodes may even map the same memory segment. By using the SISCI API, these memory segments can be mapped into the virtual address space used by application processes running on different nodes. This allows distributed applications to read and write to shared memory segments as is if it was local memory.

> API 共享内存 SISCI [[22](#Bib0022)] 提供了从由底层 NTB 驱动程序预先保留的连续内存页面池中分配线性“段”的功能。这些线性段可以“导出”，允许远程主机通过其 NTB 映射它们，并将其作为本地设备内存进行访问。通过允许远程主机映射自己的本地内存段，各个节点有效地为由这些内存段组成的分布式共享内存架构做出贡献。多个节点甚至可以映射相同的内存段。通过使用 SISCI API，这些内存段可以映射到运行在不同节点上的应用程序进程使用的虚拟地址空间中。这允许分布式应用程序读取和写入共享内存段，就像它是本地内存一样。

## 4 DEVICE LENDING

By using an NTB, it is possible to map the device memory regions, or _BARs_, of a remote PCIe device (see Figure [8](#fig8)). A local CPU can perform memory operations on a remote device, such as reading from or writing to device registers. Conversely, it is also possible to map local resources for a remote device, allowing it to access memory across the NTB. By making such mappings over the NTB transparent to a device and its driver, it is possible to facilitate use of a device without the system being aware that the device is actually remote. These mappings can be set up dynamically while systems are running, making it possible to reassign devices to different systems without rebooting.

> 通过使用 NTB，可以映射远程 PCIe 设备的设备内存区域或*BARs*(参见图 8)。本地 CPU 可以对远程设备执行内存操作，例如读取或写入设备寄存器。相反，也可以为远程设备映射本地资源，使其能够通过 NTB 访问内存。通过使这些 NTB 上的映射对设备及其驱动程序透明，可以在系统不知道设备实际上是远程的情况下使用设备。这些映射可以在系统运行时动态设置，使得可以在不重新启动的情况下将设备重新分配给不同的系统。

![](https://dl.acm.org/cms/attachment/1c46b1ca-1682-47bd-8628-dc115d4f3a41/tocs380102-02-f08.jpg) Fig. 8. Device Lending: Using NTBs, it is possible to map the memory regions of a remote device so a local CPU can access device registers. The remote system can in turn reverse-map local resources for the device, making DMA and MSI possible. Device Lending injects a hot-added “shadow device” into the Linux kernel device tree using these mappings, making remote device access transparent to both CPU and device.

Using this method, we have implemented Device Lending for an unmodified Linux kernel [[41](#Bib0041)]. As illustrated in Figure [8](#fig8), the implementation is composed of two parts, namely, a “lender,” allowing a remote system to use its device, and the “borrower” using the device. In this section, we will describe how we have implemented our Device Lending mechanism.

> 使用此方法，我们为未修改的 Linux 内核[[41](#Bib0041)]实现了设备借用功能。如图[8](#fig8)所示，实现由两部分组成，即“出借者”允许远程系统使用其设备，以及使用该设备的“借用者”。在本节中，我们将描述我们如何实现设备借用机制。

### 4.1 Shadow Device

In the Linux kernel, PCIe devices are represented with generic descriptors, providing device drivers with a generic handle that corresponds to a device. This allows Linux to provide a unified interface for functionality that is common for all PCIe devices, such as accessing a device's configuration space, setting up interrupt vectors, memory-mapping device memory and mapping buffers for device DMA. When Linux boots, it enumerates the PCIe device tree as explained in Section [3.2](#sec-12), and generates a corresponding tree of device descriptors.

> 在 Linux 内核中，PCIe 设备用通用描述符表示，为设备驱动程序提供一个与设备对应的通用句柄。这使得 Linux 能够为所有 PCIe 设备提供一个统一的接口，用于访问设备的配置空间、设置中断向量、映射设备内存和映射用于设备 DMA 的缓冲区。当 Linux 启动时，它会按照[3.2 节](#sec-12)中的说明枚举 PCIe 设备树，并生成相应的设备描述符树。

However, it is possible to manipulate this descriptor tree in software, while the system is running. By implementing our borrower component as part of the NTB driver, we can inject a virtual device, or “shadow device,” that appears as an endpoint alongside the NTB for each borrowed device. To Linux, it appears that a (virtual) device has been hot-added [[67](#Bib0067)] to the local system, and it will load any appropriate device drivers using our shadow device as the device handle. In other words, the shadow device acts as a local handle to the remote, borrowed device. By mapping the remote device's memory regions through the local NTB and overriding the shadow device's device memory regions with these mappings, a local device driver may read and write directly to physical device registers without being aware that the device is actually remote.

> 然而，可以在系统运行时通过软件操作这个描述符树。通过将我们的借用组件作为 NTB 驱动程序的一部分实现，我们可以注入一个虚拟设备或“影子设备”，它会作为每个借用设备的 NTB 的端点出现。对于 Linux 来说，它似乎有一个(虚拟)设备已经热添加到本地系统中[[67](＃Bib0067)]，它将使用我们的影子设备作为设备句柄加载任何适当的设备驱动程序。换句话说，影子设备充当远程借用设备的本地句柄。通过通过本地 NTB 映射远程设备的内存区域，并使用这些映射覆盖影子设备的设备内存区域，本地设备驱动程序可以直接读取和写入物理设备寄存器，而不知道设备实际上是远程的。

### 4.2 Intercepting Configuration Cycles

In order for a device to become aware of the memory addresses used for MSI interrupts, as explained in Section [3.2](#sec-12), the kernel must write these addresses to the device's configuration space. By setting the configuration space accessor functions on our shadow device, we can forward configuration space operations on the shadow device to the remote device in a manner that is transparent to the device driver. However, such interrupts must be mapped over the NTB to trigger the correct interrupt routine on the borrower.

> 为了让设备了解用于 MSI 中断的内存地址，如[3.2 节](#sec-12)所述，内核必须将这些地址写入设备的配置空间。通过在我们的影子设备上设置配置空间访问器函数，我们可以以对设备驱动程序透明的方式将影子设备上的配置空间操作转发到远程设备。但是，这些中断必须通过 NTB 映射到借用者上以触发正确的中断例程。

As illustrated in Figure [8](#fig8), we can prepare a mapping on the device-side NTB to the local interrupt vector assigned by the kernel (“MSI window”). By using the configuration space accessor functions, we can intercept specific configuration cycles and look for writes to the MSI offset, injecting the device-side address of the MSI window mapping into the actual configuration space of the device. This allows interrupts raised by the device to be routed across the NTB and trigger the correct interrupt routines on the borrowing system, transparent to both device and its driver. Additionally, intercepting configuration cycles also makes it possible to mask certain features for the borrower. For example, we can mask legacy interrupts, which can not be mapped over the NTB, so that the device driver will not attempt to use them.

> 如图 8 所示，我们可以在设备端 NTB 上准备一个映射，映射到内核分配的本地中断向量(“MSI 窗口”)。通过使用配置空间访问器函数，我们可以拦截特定的配置周期，并查找对 MSI 偏移量的写入，将设备端 MSI 窗口映射的地址注入到设备的实际配置空间中。这样，设备引发的中断就可以跨 NTB 传输，并在借用系统上触发正确的中断例程，对设备及其驱动程序透明。此外，拦截配置周期还可以为借用者屏蔽某些功能。例如，我们可以屏蔽无法通过 NTB 映射的传统中断，以便设备驱动程序不会尝试使用它们。

### 4.3 DMA Window

In order for a device to access local resources using DMA, the lender must set up mappings through the _device-side_ NTB to local memory as illustrated in Figure [8](#fig8). However, it is generally not possible to know in advance which memory addresses a device driver might use for DMA transfers. The pages used for DMA memory buffers may be scattered in physical memory, or an application or device driver may initiate multiple transfers to different parts of memory. Dynamically setting up mappings is not a feasible solution as it would require communication with the lender host and introduce a communication overhead. Additionally, as the number of mappings through the NTB is a finite resource, mapping individual memory pages scales rather poorly.

> 为了让设备能够使用 DMA 访问本地资源，出借方必须通过设备端 NTB 设置映射到本地内存，如图 8 所示。但是，通常无法预先知道设备驱动程序可能使用哪些内存地址进行 DMA 传输。用于 DMA 内存缓冲区的页面可能分散在物理内存中，或者应用程序或设备驱动程序可能会向不同的内存部分发起多次传输。动态设置映射不是可行的解决方案，因为它需要与出借方主机进行通信，并引入通信开销。此外，由于 NTB 上的映射数量是有限的资源，因此将单个内存页面映射起来的效率相对较低。

A naive solution is to make the lender to map the entire physical memory of the borrowing system through the NTB. However, while this would make it possible to set up a single mapping to the remote borrower, the address range of the NTB is not necessarily large enough, as mentioned in Section [3.3](#sec-13); the window on the device-side NTB must be equal to (or larger) than the size of physical memory on a borrowing system to cover the borrower's entire RAM. Moreover, a lender with multiple connected borrowers must potentially map all physical memory of every one of them. In other words, the naive solution would severely limit the number of borrowers as device memory requirements of the NTB itself would become too large.

> 一种简单的解决方案是让贷方将借方系统的整个物理内存映射到 NTB 上。然而，就像在第 3.3 节中提到的，虽然这样可以设置一个单独的映射到远程借方，但 NTB 的地址范围可能不够大；设备端 NTB 的窗口必须等于(或大于)借方系统的物理内存大小，以覆盖借方的整个 RAM。此外，拥有多个连接借方的贷方可能需要映射每个借方的整个物理内存。换句话说，这种简单的解决方案将严重限制借方的数量，因为 NTB 本身的设备内存要求可能太大。

Modern processor architectures implement an IOMMU, such as Intel's VT-d [[3](#Bib0003)]. The defining feature of the IOMMU is the ability to remap DMA operations issued by a device [[38](#Bib0038)], effectively translating virtual I/O addresses to physical addresses. By using an IOMMU on the borrowing systems, it is possible to remap scattered memory pages to a continuous range. Figure [9](#fig9) shows how we use the IOMMU on the borrower, allowing the lender to set up a single mapping through the NTB in advance (“DMA window”). When the device driver calls the Linux DMA API to create or map DMA buffers using the shadow device, we inject the device-side address of the DMA window with the appropriate offset, and set up a local IOMMU mapping to the local memory specified by the driver. The device driver passes our injected address to the device, completely unaware that the address is actually a far-side I/O address. This allows the device to reach across the NTB, transparent to both device and device driver.

> 现代处理器架构实现了 IOMMU，例如英特尔的 VT-d [[3](#Bib0003)]。IOMMU 的定义特征是能够重新映射设备发出的 DMA 操作[[38](#Bib0038)]，有效地将虚拟 I / O 地址转换为物理地址。通过在借用系统上使用 IOMMU，可以将分散的内存页面重新映射到连续的范围。图 9 显示了我们在借用者上使用 IOMMU 的方式，允许出借方提前设置单个映射通过 NTB(“DMA 窗口”)。当设备驱动程序调用 Linux DMA API 使用影子设备创建或映射 DMA 缓冲区时，我们将 DMA 窗口的设备端地址注入适当的偏移量，并设置本地 IOMMU 映射到驱动程序指定的本地内存。设备驱动程序将我们注入的地址传递给设备，完全不知道地址实际上是远端 I / O 地址。这使得设备可以跨越 NTB，对设备和设备驱动程序透明。

![](https://dl.acm.org/cms/attachment/a57e9838-59f4-4b5b-be1d-f94524b1b3f7/tocs380102-02-f09.jpg) Fig. 9. DMA window: We use the local IOMMU in order create a single continuous memory range. This allows us to conserve NTB resources by setting up a single mapping through the device-side NTB in order for the remote device to reach local RAM. Adding and removing memory pages from the local IOMMU group is inexpensive compared to actively communicating with the lender to set up mappings dynamically.

While our solution adds additional software when a device driver sets up DMA buffers, dynamically adding and removing memory pages from a local IOMMU group has a relatively low overhead compared to communicating with a remote host. Moreover, since mapping across the NTB is done in advance, and all address translations between the different address domains are done in the NTB and IOMMU hardware, our implementation achieves native PCIe performance in the data path.

> 当设备驱动程序设置 DMA 缓冲区时，我们的解决方案会添加额外的软件，但与与远程主机通信相比，动态地添加和删除本地 IOMMU 组中的内存页面具有相对较低的开销。此外，由于 NTB 上的映射是提前完成的，并且所有不同地址域之间的地址转换都在 NTB 和 IOMMU 硬件中完成，因此我们的实现在数据路径中实现了本机 PCIe 性能。

Some PCIe devices, such as Nvidia GPUs, may have addressing limitations that make them unable to reach higher addresses of the 64-bit I/O address space. For such devices, it can be difficult to configure large enough DMA windows, since the combined memory requirements of the DMA windows must fit through the NTB BAR. Depending on the device memory requirements of downstream devices in the PCIe tree, configuring the NTB BAR size too large may force the system to place the NTB at a high address (see Section [3.1](#sec-11)). Because of this, our implementation also supports optionally using the IOMMU on the _lender_. By using the lender's IOMMU, we can remap NTB mappings from high to low addresses if it is necessary, similar to how the IOMMU can be used to avoid so-called “bounce buffering” [[52](#Bib0052)]. An additional benefit is that it also becomes possible to put borrowed devices in their own IOMMU address domains, isolated from other devices in the system. This protects the lender system from any accidental address misconfiguration.

> 一些 PCIe 设备，如 Nvidia GPU，可能具有地址限制，使它们无法访问 64 位 I/O 地址空间的更高地址。对于这种设备，要配置足够大的 DMA 窗口可能会很困难，因为 DMA 窗口的组合内存要求必须通过 NTB BAR。根据 PCIe 树中下游设备的设备内存要求，如果配置 NTB BAR 大小过大，则系统可能会将 NTB 放置在高地址(参见第[3.1]节)。因此，我们的实现还可以选择使用*lender*上的 IOMMU。通过使用出借人的 IOMMU，如果有必要，我们可以将 NTB 映射从高地址重新映射到低地址，类似于 IOMMU 可以用来避免所谓的“反弹缓冲”[[52](#Bib0052)]。另一个好处是，也可以将借来的设备放入自己的 IOMMU 地址域中，与系统中的其他设备隔离开来。这可以保护出借人系统免受任何意外的地址配置错误的影响。

### 4.4 Shortest Path Routing

Some processing tasks may require the use of multiple devices, such as machine learning workloads that need several GPUs. Such workloads often transfer data from one device to another using DMA, where a device reads from or writes to the memory regions (BARs) of other devices. As described in Section [3.2](#sec-12), shortest path routing between such devices using _peer-to-peer_ is possible based on address ranges.

> 一些处理任务可能需要使用多个设备，例如需要多个 GPU 的机器学习工作负载。这类工作负载通常使用 DMA 从一个设备传输数据到另一个设备，其中一个设备读取或写入其他设备的内存区域(BARs)。如[3.2]节所述，基于地址范围，这些设备之间可以使用*点对点*进行最短路径路由。

In the case of Device Lending, however, devices installed in different lender systems use different address space domains. The local I/O address used by one host, i.e., the local address a borrower uses to reach a remote device, is not the same address different host would use to reach the same device. Furthermore, a lender may even use an entirely different NTB to reach the other device than it would for reaching the borrower. In order for a borrowed device to reach another borrowed device, we need a mechanism for resolving I/O addresses between the different domains.

> 在设备借用的情况下，不同出借者系统中安装的设备使用不同的地址空间域。借用者使用来访问远程设备的本地 I/O 地址与其他主机访问同一设备的地址不同。此外，出借者甚至可能使用完全不同的 NTB 来访问另一个设备，而不是用于访问借用者。为了让借用的设备访问另一个借用的设备，我们需要一种机制来解析不同域之间的 I/O 地址。

With the 4.9 version of the Linux kernel, functionality for setting up mappings between devices to do peer-to-peer DMA between them was added to the device DMA API. By implementing these functions for our injected shadow device, we are notified when a device driver is mapping the device memory regions of another device, and we can inject our prepared mappings. We have implemented the following method of resolving address domains in Device Lending, in order for a borrowed device (the _source_) to reach another borrowed device (the _target_):

> 随着 Linux 内核的 4.9 版本，为设备 DMA API 添加了在设备之间设置映射以进行点对点 DMA 的功能。通过为我们注入的影子设备实现这些功能，我们可以在设备驱动程序映射另一个设备的设备内存区域时得到通知，并且我们可以注入我们准备好的映射。我们已经实现了以下设备借用中解决地址域的方法，以便借用的设备(源)可以访问另一个借用的设备(目标)：

随着 Linux 内核 4.9 版本的发布，设备 DMA API 增加了在设备之间设置映射以实现点对点 DMA 的功能。我们为注入的影子设备实现了这些功能，当设备驱动程序映射另一个设备的设备内存区域时，我们会收到通知，并且可以注入我们准备好的映射。为了让借用的设备(源)可以访问另一个借用的设备(目标)，我们实现了设备借用中解决地址域的以下方法：

1.  **Same lender:** If the _target_ is installed in the same host as the _source_, then setting up a mapping is trivial. If the device-side IOMMU is disabled, then the lender simply returns its local device-side I/O addresses of the BARs of the _target_. If the IOMMU is enabled, then the lender additionally needs to set up IOMMU mappings, and returns the I/O virtual addresses.

> 如果目标设备安装在与源设备相同的主机上，那么设置映射就很简单。如果设备端 IOMMU 被禁用，那么出借方只需返回其本地设备端 BAR 的 I/O 地址即可。如果 IOMMU 启用，出借方还需要设置 IOMMU 映射，并返回 I/O 虚拟地址。

2.  **Local device:** If the _target_ is a device local to the borrower, i.e., residing within the borrowing host, then the _source_’s lender set up DMA windows to the individual BARs of the _target_, similar to how it has already mapped a DMA window to the borrower's RAM. The lender then returns the local device-side I/O addresses the _source_ would use to reach through the NTB to reach the the _target_’s BARs. This works for any device in the borrower, even local devices that are not registered with our system. However, in this case, our only works for setting up mappings for a remote device to a local device. The other way around is not possible unless the local device is registered with our system, as we are unable to intercept calls by the device driver without our virtual device handle (shadow device).

> 如果目标是借用主机内的本地设备，即所属主机，那么源方将设置 DMA 窗口到目标的各个 BAR，就像它已经映射到借用者 RAM 的 DMA 窗口一样。然后，贷方返回源方将使用的本地设备端 I/O 地址，以通过 NTB 访问目标的 BAR。这适用于借用者中的任何设备，甚至未注册到我们系统中的本地设备。但是，在这种情况下，我们只能为远程设备设置本地设备的映射。另一方面，除非本地设备已经注册到我们的系统中，否则不可能实现，因为我们无法在没有虚拟设备句柄(影子设备)的情况下拦截设备驱动程序的调用。

3.  **Different lenders:** If the _target_ is a remote device, i.e., residing in a different lender host, then the _source_’s lender creates DMA windows through the appropriate NTB to the _target_’s lender. Note that this NTB may be different to the one used to reach the borrower. We then return the local device-side I/O addresses the _source_’s lender would use to reach through the NTB to the the _target_’ BARs.

> 如果目标是一个远程设备，即位于不同的贷款机构主机上，那么源贷款机构通过适当的 NTB 创建 DMA 窗口到目标贷款机构。请注意，这个 NTB 可能不同于用于访问借款人的 NTB。然后，我们返回源贷款机构将用于通过 NTB 访问 BAR 的本地设备端 I/O 地址。

The borrower, after resolving these lender-local I/O addresses, stores them along with its own physical addresses to the BARs of the _target_. When the device driver using the _source_ calls the DMA API functions to map the BARs of the _target_ for the _source_, the borrower is able to look up the corresponding lender-local I/O addresses and use these. When the driver in turn initiates DMA, it is completely unaware of the location of both the _source_ and the _target_, and the _source_ will be able to access the _target_ through the correct NTB. Figure [10](#fig10) shows that the the _source_ device can reach the _target_ device for all three scenarios. By resolving lender-local I/O addresses in advance, we have enabled devices to directly access each other using peer-to-peer. In other words, we have enabled device-to-device communication between remote devices with the lowest possible latency.

> 借款人在解析完这些贷方本地 I/O 地址后，将它们以及其自身的物理地址存储到目标的 BARs 中。当使用源设备的设备驱动程序调用 DMA API 函数映射目标的 BARs 时，借款人能够查找相应的贷方本地 I/O 地址，并使用它们。当驱动程序反过来启动 DMA 时，它完全不知道源和目标的位置，源将能够通过正确的 NTB 访问目标。图 10 显示源设备可以在三种情况下访问目标设备。通过提前解析贷方本地 I/O 地址，我们已经使设备能够通过点对点直接访问对方。换句话说，我们已经使远程设备之间的设备到设备通信具有最低的延迟。

![](https://dl.acm.org/cms/attachment/9cfb2997-a33b-4748-a100-d5d9485c1870/tocs380102-02-f10.jpg) Fig. 10. Shortest path routing: By resolving addresses of device memory regions and preparing mappings for them in advance, we can route device-to-device using the shortest path when a device driver initiates a DMA transfer. Our solution covers all three scenarios: (1) when both devices are in the same lender, (2) when the target device is in the borrower, and (3) when the target device resides in a different lender.

## 5 VM PASS-THROUGH USING MDEV

To provide I/O capabilities to a VM, a VM hypervisor may use emulated devices or paravirtualization. Software-emulated devices appear to the VM guest as an I/O device, but all functionality is handled in the VM implementation. Paravirtualized devices also offer device functionality in software, but relies on facilitation by the hypervisor to use host resources. In many cases, paravirtualized devices are backed by actual hardware. However, emulation and paravirtualization may not be viable options when bare-metal processing power is required.

> 为虚拟机提供 I/O 功能，虚拟机管理程序可以使用模拟设备或分时虚拟化。软件模拟设备对虚拟机客户机显示为 I/O 设备，但所有功能都在虚拟机实现中处理。分时虚拟化设备也提供软件设备功能，但需要管理程序协助使用主机资源。在许多情况下，分时虚拟化设备由实际硬件支持。但是，当需要裸机处理能力时，模拟和分时虚拟化可能不是可行的选择。

In this regard, it is possible to to remap DMA and interrupts using an IOMMU. Similarly to pages mapped by an MMU for individual processes, an IOMMU can group devices into IOMMU domains. As each domain has its own individual mappings, members of an IOMMU domain consequently have their own private virtual address space. Such a domain can be part of the virtualized address space of a VM, enabling direct access to physical memory by the physical device, while other devices and the rest of memory remain isolated. As such, the IOMMU provides a hardware virtualization layer between I/O devices and the rest of the system. This allows a VM hypervisor to facilitate direct access to the physical device from within the VM guest, without compromising the memory isolation provided by the virtualization. This facilitation is often referred to as “pass-through.”

> 在这方面，可以使用 IOMMU 重新映射 DMA 和中断。与 MMU 为每个进程映射的页面类似，IOMMU 可以将设备分组到 IOMMU 域中。由于每个域都有自己的单独映射，因此 IOMMU 域的成员具有自己的私有虚拟地址空间。这样的域可以成为虚拟机的虚拟地址空间的一部分，使物理设备能够直接访问物理内存，而其他设备和其余内存仍然被隔离。因此，IOMMU 在 I / O 设备和系统其余部分之间提供了硬件虚拟化层。这允许 VM 虚拟机管理器从 VM 客户端直接访问物理设备，而不会破坏虚拟化所提供的内存隔离。这种便利通常被称为“传递”。

In this section, we explain how we have implemented support for such pass-through of _remote_ devices in our SmartIO system [[48](#Bib0048), [49](#Bib0049)]. We explain how we generalized the core functionality in our Device Lending mechanism, providing us with the necessary software capabilities for implementing a kernel-space interface for the hypervisor. By implementing functionality for dynamically assigning remote devices to VMs, we have extended our device distribution mechanism to support OSes other than Linux, such as Microsoft Windows.

> 在本节中，我们将解释我们如何在 SmartIO 系统中实现对远程设备的透传支持[[48](#Bib0048), [49](#Bib0049)]。我们将解释我们如何将设备借用机制的核心功能概括化，为实现虚拟化器的内核空间接口提供必要的软件功能。通过实现动态将远程设备分配给虚拟机的功能，我们已经扩展了我们的设备分发机制，以支持除 Linux 外的操作系统，如 Microsoft Windows。

### 5.1 Mediated Devices

On Linux, pass-through of devices is supported in the KVM hypervisor by using **Virtual Function I/O (VFIO)** [[37](#Bib0037)]. By implementing a VFIO interface for a device, KVM is able to use the IOMMU and map I/O virtual addresses for the device to the same _guest-physical_ address layout used by a VM.

> 在 Linux 上，KVM 虚拟化器通过使用虚拟功能 I/O(VFIO)[[37](#Bib0037)]来支持设备的透传。通过为设备实现 VFIO 接口，KVM 能够使用 IOMMU 并为设备的 I/O 虚拟地址映射到与 VM 相同的*guest-physical*地址布局。

Intuitively, a solution for passing through remote devices to a VM would be for the host to borrow a device, injecting the device into its local device tree, and then use VFIO. However, this would not be feasible as VFIO requires that pass-through devices are placed in a separate IOMMU domain per VM guest. As described in Section [4.3](#sec-17), Device Lending places all borrowed devices in the same IOMMU domain to preserve mappings over the NTB. Additionally, pass-through requires the entire guest-physical memory of a VM to be mapped for the device. We need a mechanism for detecting, pinning and mapping the physical memory pages used by the VM instance, in order for the device to be able to DMA to it. VFIO does not provide this mechanism, thus detecting the presence of a VM and mapping its memory is not possible.

> 直观地说，通过远程设备传递到虚拟机的解决方案是主机借用设备，将设备注入其本地设备树，然后使用 VFIO。但是，由于 VFIO 要求将传递设备放置在每个 VM 客户机的单独 IOMMU 域中，因此这是不可行的。如第 4.3 节所述，设备借用将所有借用的设备放置在同一 IOMMU 域中，以保留 NTB 上的映射。此外，传递需要为设备映射虚拟机的整个客户端物理内存。我们需要一种机制来检测、固定和映射 VM 实例使用的物理内存页，以便设备能够对其进行 DMA。VFIO 不提供此机制，因此无法检测虚拟机的存在并映射其内存。

In the 4.10 version of the Linux kernel, an extension to VFIO called **mediated device drivers (MDEV)** was introduced [[33](#Bib0033)]. The MDEV extension introduces the concept of a physical parent device having virtual child devices, allowing a host device driver to emulate multiple virtual devices, while still allowing some direct access to hardware. In other words, MDEV facilitates a form of paravirtualization that enables “SR-IOV in software.” Some operations on the virtual device, such as configuration cycles and device resets, are trapped (handled) by the parent device driver running on the host, allowing some hardware resources to be emulated while other resources are accessed directly. In our case, using this MDEV interface provides us with a finer-grained control over what the hypervisor and VM guest is attempting to do with the device.

> 在 Linux 内核的 4.10 版本中，引入了一种名为**中介设备驱动程序(MDEV)**的 VFIO 扩展[[33](#Bib0033)]。 MDEV 扩展引入了物理父设备具有虚拟子设备的概念，允许主机设备驱动程序模拟多个虚拟设备，同时仍允许直接访问硬件。换句话说，MDEV 提供了一种形式的分布式虚拟化，可以实现“软件 SR-IOV”。在虚拟设备上的某些操作，例如配置周期和设备重置，被主机上运行的父设备驱动程序捕获(处理)，允许模拟某些硬件资源，同时直接访问其他资源。在我们的情况下，使用此 MDEV 接口为我们提供了更细粒度的控制，以便虚拟机管理程序和虚拟机客户端尝试对设备进行什么操作。

Our implementation registers itself as an MDEV parent device driver for devices under the control of SmartIO. With Device Lending, a device would be exclusively borrowed by the physical host for as long as it runs, regardless of whether any VM instances is using it or not. By implementing functionality for borrowing and releasing device references without injecting them into the local device tree, KVM is able to pass through the device to a VM without it being borrowed first. Only when the VM guest boots up and resets the device, do we actually borrow the device. Similarly, when the guest OS releases the device, either by shutting down or hot-removing the device, we return it. Not only does this limit the lifetime of a borrowed device to when a VM is running and using it, but it also makes it possible to hot-add a device to a live VM.

> 我们的实现将自己注册为 SmartIO 控制下的设备的 MDEV 父设备驱动程序。通过设备借用，设备将被物理主机独占借用，只要它运行，无论有没有虚拟机实例在使用它。通过实现借用和释放设备引用而不将它们注入本地设备树的功能，KVM 可以将设备传递给虚拟机，而无需先借用设备。只有当虚拟机客户端启动并重置设备时，我们才真正借用设备。同样，当客户端操作系统释放设备时，无论是关闭还是热移除设备，我们都会将其归还。这不仅将借用设备的生命周期限制为虚拟机运行时和使用它的时候，而且还使得可以将设备热添加到现有的虚拟机中。

### 5.2 Mapping VM Memory for Device

Using Device Lending, we can react to calls to the DMA API on a shadow device to dynamically add or remove pages from the local IOMMU domain. In contrast, we have no way of knowing which addresses a device driver running in the guest may use for DMA. Therefore, the only option is to map all of the guest-physical memory used by the VM for the device.

> 使用设备借用，我们可以对影子设备上的 DMA API 调用做出反应，从而动态地添加或删除本地 IOMMU 域中的页面。相比之下，我们无法知道虚拟机中的设备驱动程序可能使用哪些地址进行 DMA。因此，唯一的选择就是为设备映射虚拟机使用的所有虚拟内存。

By using an MDEV parent device driver instead of VFIO, we are aware of a VM instance using the device. However, while the MDEV interface provides us with a method of using KVM to resolve guest-physical addresses to host-physical and pinning the physical memory pages used by the VM instance, we know nothing about the memory layout of a VM instance or even when memory has been allocated. Other implementations using MDEV implement virtual child devices, each with their own set of _emulated_ resources. For example, when a guest driver initiates DMA transfers, the parent device driver is notified by trapping emulated device registers, and is able to resolve addresses and pin the appropriate pages in memory just before initiating the DMA engine on the physical device. Our implementation, however, is actually passing through the physical device itself. In our case, the VM instance maps all of the physical device registers and accesses the entire device directly. This means that without making assumptions about the type of device being used and implementing virtual registers for it, we are not able to replicate this specific behavior. This poses a challenge, as the memory used by the VM has not yet been allocated when the virtual device is first picked up by a VM instance.

> 使用 MDEV 父设备驱动程序而不是 VFIO，我们可以知道使用该设备的虚拟机实例。然而，尽管 MDEV 接口为我们提供了一种使用 KVM 解析客户端物理地址到主机物理地址并锁定由虚拟机实例使用的物理内存页面的方法，但我们对虚拟机实例的内存布局一无所知，甚至不知道何时分配了内存。其他使用 MDEV 实现的虚拟子设备，每个设备都有自己的*模拟*资源集。例如，当客户端驱动程序启动 DMA 传输时，父设备驱动程序会通过捕获模拟设备寄存器来通知，并能够在启动物理设备上的 DMA 引擎之前解析地址并锁定相应的页面。然而，我们的实现实际上是直接传递物理设备本身。在我们的情况下，虚拟机实例映射了所有物理设备寄存器，并直接访问整个设备。这意味着，如果不假设使用的设备类型并为其实现虚拟寄存器，我们就无法复制此特定行为。这就带来了一个挑战，因为当虚拟设备首次被虚拟机实例拾取时，虚拟机所使用的内存尚未分配。

However, before a PCIe device can use DMA, it must be enabled in a device's configuration space.[4](#fn4) This allows us to defer mapping of VM memory until our implementation detects a configuration cycle enabling DMA. By then, we can assume that the memory used for the VM is allocated. Even so, we still do not have any information about the address space layout. The naive solution is to map the entire range from start to end. As depicted in Figure [11](#fig11), this solution is wasteful as a VM's address space may be much larger than the guest-physical memory size, and not all of this address space should be reachable by the device.

> 然而，在 PCIe 设备使用 DMA 之前，必须在设备的配置空间中启用它[4] (#fn4)。这允许我们推迟映射 VM 内存，直到我们的实现检测到启用 DMA 的配置周期。到那时，我们可以假定用于 VM 的内存已分配。即使如此，我们仍然没有关于地址空间布局的任何信息。最简单的解决方案是映射从开始到结束的整个范围。如图 11 所示，这种解决方案是浪费的，因为 VM 的地址空间可能比客户端物理内存大得多，而且设备不应该可以访问这个地址空间的全部内容。

![](https://dl.acm.org/cms/attachment/2f8c6da1-4680-4299-b9ea-524a7ed8c617/tocs380102-02-f11.jpg) Fig. 11. Mapping VM memory for a device: The VM's address space may be much larger than the actual memory used by the guest. Only guest-physical memory needs to be mapped for a device.

Instead, we can rely on an assumption: as the x86 architecture uses well-defined starting addresses for low and high memory, we can start at these guest-physical addresses and use KVM to experimentally probe which address ranges resolves and which do not. This way, we are able to both dynamically discover the memory layout of the VM and only map those ranges that should be reachable by the device.

> 我们可以依赖一个假设：由于 x86 架构为低内存和高内存使用了明确的起始地址，我们可以从这些虚拟机物理地址开始，使用 KVM 进行实验性探测哪些地址范围可以解析，哪些不可以。这样，我们既可以动态发现虚拟机的内存布局，又可以只映射设备可达的范围。

Figure [12](#fig12) illustrates how a device is mapped into the address space of a VM. On the lender, we use the IOMMU to create a virtual I/O address space that maps over the NTB, mirroring the guest-physical memory layout. Because this mapping exists, a native device driver running in the VM guest can initiate DMA transfers on the physical device using guest-physical addresses. On the borrower, we use the IOMMU to provide continuous address ranges that are trivially mapped over the NTB. Note that we create a separate DMA window for the low and high memory ranges, allowing us to map the entire guest-physical memory, while being able to fit through the NTB window.

> 图 12(#fig12)说明了设备如何映射到虚拟机的地址空间中。在贷方，我们使用 IOMMU 创建一个虚拟 I/O 地址空间，该空间映射到 NTB，反映客户端物理内存布局。由于存在这种映射，运行在 VM 客户端中的本机设备驱动程序可以使用客户端物理地址启动物理设备上的 DMA 传输。在借方，我们使用 IOMMU 提供连续的地址范围，这些地址范围可以轻松映射到 NTB。请注意，我们为低内存和高内存范围创建了一个单独的 DMA 窗口，使我们能够映射整个客户端物理内存，同时能够适应 NTB 窗口。

![](https://dl.acm.org/cms/attachment/62a41cf1-7c29-4a46-9094-e9c0ff60af97/tocs380102-02-f12.jpg) Fig. 12. Pass-through of a remote device: By using IOMMUs on both sides of the NTB, it is possible to map a remote device into a local VM guest's address space. The borrower-side IOMMU provides continuous memory ranges that can be mapped over the NTB, while the lender-side IOMMU is used to map the virtual address space for the device, mirroring the guest-physical layout. We use two windows to map the VM's entire memory.

### 5.3 Peer-to-peer between Devices

Similarly to how guest-physical memory is mapped for a device, the guest-physical BARs of other devices passed through to the same VM can also be mapped for a device. When the guest OS enumerates its PCIe tree and write guest-physical addresses to a device's configuration space, our MDEV parent driver captures these addresses. For all _other_ devices, we are able to set up I/O virtual addresses that correspond to these guest-physical addresses using their lenders’ IOMMUs. Using the same method described in Section [4.4](#sec-18), we are able to resolve which NTB adapter to map over in order reach the device. This makes it possible to set up mappings between two or more devices using our MDEV implementation, even when they reside in different hosts.

> 类似于为设备映射客户端物理内存，将其他设备传递给同一个虚拟机的客户端物理 BAR 也可以为设备映射。当客户端操作系统枚举其 PCIe 树并将客户端物理地址写入设备的配置空间时，我们的 MDEV 父驱动程序捕获这些地址。对于所有*其他*设备，我们能够使用其出租人的 IOMMU 设置与这些客户端物理地址对应的 I/O 虚拟地址。使用[4.4](#sec-18)中描述的相同方法，我们能够解析出要映射哪个 NTB 适配器以访问设备。这使得使用我们的 MDEV 实现在不同主机之间设置两个或多个设备之间的映射成为可能。

However, while this enables device-to-device access between the physical devices, shortest path routing in the fabric is disrupted by the virtual address space. PCIe transactions must be routed to the IOMMU to resolve I/O virtual addresses to physical addresses (Figure [13](#fig13)). PCI-SIG has developed an extension to the transaction layer that allows devices that have an understanding of I/O virtual addresses to cache resolved addresses called **Address Translation Service (ATS)** [[60](#Bib0060)]. However, ATS is not widely available as it requires hardware support in devices.

> 然而，虽然这使物理设备之间的设备到设备访问成为可能，但织物中的最短路由被虚拟地址空间打断。PCIe 事务必须路由到 IOMMU 以将 I/O 虚拟地址解析为物理地址(图 13)。PCI-SIG 已经开发了一种扩展事务层，允许具有 I/O 虚拟地址理解的设备缓存已解析的地址，称为地址转换服务(ATS)(参见参考文献 60)。然而，ATS 并不广泛使用，因为它需要设备中的硬件支持。

![](https://dl.acm.org/cms/attachment/c8fbb264-1c3c-4a97-b2e1-19c23071ad62/tocs380102-02-f13.jpg) Fig. 13. Since IOMMUs introduce a virtual address space for devices, peer-to-peer transfers must be routed through the root in order for the IOMMU to resolve virtual addresses to physical addresses. As a consequence, shortest path routing is disrupted.

### 5.4 Relaying Interrupts

Similarly to VFIO pass-through, MDEV uses the _eventfd_ API [[36](#Bib0036)] to trigger interrupts in a VM instance. When our MDEV parent device driver gets notified to set up an interrupt for a VM, we register an interrupt request handler on the lender for the specified interrupt. Whenever the device raises an interrupt, this interrupt request handler is invoked, which in turn notifies our MDEV implementation. We can then use _eventfd_ to signal that an interrupt has been raised to the VM instance.

> 类似于 VFIO pass-through，MDEV 使用*eventfd* API [[36](#Bib0036)]来触发虚拟机实例中的中断。当我们的 MDEV 父设备驱动程序被通知为虚拟机设置中断时，我们会在指定的中断上为出借方注册一个中断请求处理程序。每当设备产生中断时，就会调用此中断请求处理程序，它反过来通知我们的 MDEV 实现。然后，我们可以使用*eventfd*向虚拟机实例发出已触发中断的信号。

This method is not ideal, as the latency between a device raising an interrupt and the interrupt routine being invoked within the VM increases. A latency reducing improvement would be to use the same approach as bare-metal Device Lending, and map MSI and MSI-X interrupts over the NTB. However, a benefit of the current implementation is that it allows us to enable legacy interrupts for devices borrowed by a VM, something that is not supported for bare-metal machines.

> 这种方法并不理想，因为设备触发中断和虚拟机中调用中断例程之间的延迟会增加。减少延迟的改进方法是使用与裸机设备借用相同的方法，将 MSI 和 MSI-X 中断映射到 NTB 上。然而，当前实现的好处是，它允许我们为虚拟机借用的设备启用传统中断，而裸机机器不支持这一功能。

### 5.5 VM Migration

As our SmartIO system abstracts away device location, our MDEV implementation supports so-called “cold migration.” It is possible to shutdown, migrate, and restart a VM on a different host, while keeping the same passed-through physical devices. If the VM emulator supports it, then it is also possible to hot-add and hot-remove devices to running VMs. Using such hot-swap functionality, live migration could theoretically be possible by first removing all devices, migrating, and then re-attaching them afterwards. However, since such a solution would temporarily disrupt device I/O and force guest drivers to reset all devices, its usefulness would be limited.

> 我们的 SmartIO 系统抽象出设备位置，我们的 MDEV 实现支持所谓的“冷迁移”。可以在不同的主机上关闭、迁移和重新启动虚拟机，同时保持相同的外接物理设备。如果虚拟机模拟器支持，那么也可以热插拔设备到运行中的虚拟机。使用这种热插拔功能，理论上可以通过首先移除所有设备，迁移，然后重新附加它们来实现实时迁移。但是，由于这样的解决方案会暂时中断设备 I / O 并迫使客户端驱动程序重置所有设备，因此其使用价值有限。

Supporting real hot-migration, remapping devices while they are in use without (or with minimal) disruption, is something we wish to implement in future work. Not only would such a solution require keeping memory consistent during the migration warm-up, but DMA transactions could potentially be in-flight during the migration. A mechanism for re-routing transactions, without violating the strict ordering required by PCIe, must be implemented, and will most likely require hardware support that does not exist today.

> 支持真正的热迁移，在使用过程中重新映射设备，而不会(或只有很小的)中断，是我们希望在未来的工作中实现的。这样的解决方案不仅需要在迁移预热期间保持内存一致，而且 DMA 事务可能在迁移期间正在进行。必须实施一种重新路由事务的机制，而不违反 PCIe 所要求的严格顺序，这很可能需要今天还不存在的硬件支持。

## 6 DISTRIBUTED NVME DRIVER

By borrowing a device and inserting it into the local device tree, using either Device Lending or passing the device through to a VM using our MDEV implementation, a device driver may use a device as if it was locally installed. No adaptations are required to use the device, allowing device drivers, OS, and application software to use the device without any modifications.

> 通过借用设备并将其插入本地设备树，使用设备借用或通过我们的 MDEV 实现将设备传递到 VM，设备驱动程序可以使用设备，就好像它是本地安装的一样。无需进行任何更改即可使用该设备，从而使设备驱动程序、操作系统和应用软件可以在不进行任何修改的情况下使用该设备。

However, most PCIe device drivers are written in a way that assumes exclusive control over the device. Consequently, a device may only be distributed to a single host at the time, preventing others from accessing it while it is used. This can lead to poor utilization of device resources, as it requires hosts to cooperatively time share a device, resetting it every time it is reassigned to a new host. Some devices implement SR-IOV [[62](#Bib0062)], making a single physical device to appear as multiple virtual devices, allowing each virtual device to be distributed by Device Lending. Regardless, as SR-IOV capability increases the complexity of hardware implementations, it is not widely available, especially for low- to medium-end devices.

> 然而，大多数 PCIe 设备驱动程序的编写方式假定对设备具有独占控制权。因此，设备一次只能分发给一台主机，以防止其他人在使用时访问它。这可能导致设备资源的低效利用，因为它要求主机协作共享设备，每次将其重新分配给新主机时都要重置它。一些设备实现了 SR-IOV [[62](#Bib0062)]，使单个物理设备看起来像多个虚拟设备，允许每个虚拟设备由设备借用分发。尽管如此，由于 SR-IOV 功能增加了硬件实现的复杂性，它并不普遍可用，特别是对于中低端设备。

During the development of our MDEV implementation (Section [5](#sec-19)), we isolated functionality shared with Device Lending and were able to expose this to userspace applications. Effectively, this makes it possible to write device drivers that enable simultaneous sharing and parallel operation of single-function devices by distributing it to multiple hosts at the same time.

> 在我们 MDEV 实施的开发过程中(第 5 节)，我们隔离了与设备借用共享的功能，并能够向用户空间应用程序暴露这些功能。实际上，这使得可以编写设备驱动程序，通过将其同时分发到多台主机，实现单功能设备的同时共享和并行操作。

In this section, we present our proof-of-concept NVMe driver allowing sharing to multiple hosts simultaneously. NVMe [[55](#Bib0055)] is an interface specification for non-volatile storage controllers that are attached to the PCIe bus, such as **solid state flash memory drives (SSDs)**. Compared to traditional spinning hard disks, where seek time and mechanical disk rotation cause significant delay, these storage drives have lower latency and support parallel operations. This is reflected in the design of NVMe, which supports this parallelism through the use of multiple I/O queues that operate independently and avoiding any form of locking in the I/O submission path. By distributing individual I/O queues, we demonstrate how a single NVMe storage drive may be shared among multiple hosts and operated in parallel.

> 在本节中，我们提出了一种允许同时共享多个主机的概念验证 NVMe 驱动程序。NVMe [[55](#Bib0055)]是一种非易失性存储控制器的接口规范，该控制器连接到 PCIe 总线，例如**固态闪存驱动器(SSD)**。与传统的旋转硬盘相比，其中寻道时间和机械磁盘旋转会导致显著延迟，这些存储驱动器具有更低的延迟并支持并行操作。这反映在 NVMe 的设计中，它通过使用独立运行的多个 I/O 队列来支持这种并行性，并避免 I/O 提交路径中的任何形式的锁定。通过分布单个 I/O 队列，我们演示了如何在多个主机之间共享单个 NVMe 存储驱动器，并并行操作。

### 6.1 Device Driver API

We have extended the SISCI API [[22](#Bib0022)] with device-oriented semantics, exposing core SmartIO capabilities through the same shared-memory API used to write cluster applications. In other words, by exposing this functionality through the SISCI API, it becomes possible to implement device drivers as part of the application. Integrating device operation into the application itself makes devices and drivers become part of the same shared global address space as distributed shared-memory applications.

> 我们通过 SISCI API [[22](#Bib0022)]扩展了设备导向的语义，通过相同的共享内存 API 来暴露核心 SmartIO 功能，用于编写集群应用程序。换句话说，通过 SISCI API 暴露这些功能，可以将设备驱动程序作为应用程序的一部分实现。将设备操作集成到应用程序本身中，使设备和驱动程序成为与分布式共享内存应用程序相同的共享全局地址空间的一部分。

As mentioned in Section [3.3](#sec-13), a userspace application may map “segments” of a remote system's memory into its own virtual address space using SISCI. Moreover, as we explained in Section [4.3](#sec-17), we can set up mappings to such shared memory segments for a _device_ as well (“DMA windows”). Devices may use DMA to access shared-memory segments directly, without requiring RDMA. Similarly, by exporting device BARs as shared memory segments, device memory regions may be mapped by several nodes, effectively disaggregating device memory. Memory segments (both system memory and device memory) are associated with _devices_, rather than with hosts. By providing functionality for translating device-side physical addresses, as well as resolving the path through the network between the device and shared memory segments, our API extension allows device driver implementations to be agnostic about address spaces in different cluster nodes. As such, these mechanisms alleviate some of the complexity of implementing distributed device drivers, as software can be written in a way that does not need to consider whether resources are local or remote. The same driver software can run on any node in the cluster, using any device in the cluster, without requiring that the application is actually aware of the specific PCIe topology.

> 根据[3.3 节](#sec-13)所述，用户空间应用程序可以使用 SISCI 将远程系统的内存的“段”映射到其自己的虚拟地址空间中。此外，正如我们在[4.3 节](#sec-17)中解释的那样，我们也可以为设备设置映射到这些共享内存段(“DMA 窗口”)。设备可以使用 DMA 直接访问共享内存段，而无需 RDMA。类似地，通过将设备 BAR 导出为共享内存段，可以由多个节点映射设备内存区域，从而有效地分散设备内存。内存段(系统内存和设备内存)与*设备*相关联，而不是与主机相关联。通过提供设备端物理地址的转换功能以及解析设备和共享内存段之间网络路径的功能，我们的 API 扩展允许设备驱动程序实现对不同集群节点中的地址空间无关。因此，这些机制缓解了实现分布式设备驱动程序的复杂性，因为软件可以以不需要考虑资源是本地的还是远程的方式编写。同样的驱动程序可以在集群中的任何节点上运行，使用集群中的任何设备，而无需应用程序实际上了解特定的 PCIe 拓扑结构。

Specifically, the following functionality was added to SISCI:

- API functions for letting application processes borrow and return devices. Borrowing a device can either be exclusive, allowing only one borrower at the time, or non-exclusive, allowing several borrowers simultaneously. It possible for a single application process to first take an exclusive reference, to reset, initiate and prepare the device, before allowing other processes in the cluster to borrow the device.

> API 功能允许应用程序进程借用和归还设备。借用设备可以是独占的，只允许一个借用者，或者是非独占的，允许多个借用者同时借用。单个应用程序可以先采取独占引用，重置、启动和准备设备，然后允许集群中的其他进程借用设备。

- Automatically exporting device memory regions (device BARs) as segments, allowing them to be memory-mapped into the application process’ virtual address space. Additionally, by exporting BARs as segments, it is possible to map them for other devices and set up shortest-path routing.

> 自动将设备内存区域(设备 BAR)导出为段，允许将其映射到应用程序进程的虚拟地址空间中。此外，通过将 BAR 导出为段，可以将其映射到其他设备，并设置最短路径路由。

- API functions for mapping SISCI segments on behalf of a device, effectively setting up DMA windows over the device-side NTB (lender's NTB). This allows the device to use native DMA to read and write to shared memory segments. Segments can be either local or remote to the device, and SmartIO will automatically resolve device-side physical addresses to (remote) memory segments under the hood, allowing the same software to run on any cluster node and remain agnostic about the specific address space layout in other hosts. Note that since BARs of any device registered with SmartIO are automatically exported as SISCI segments, we can map them for other devices as well.

> API 功能可以映射 SISCI 段，代表设备，有效地设置设备端 NTB(借方 NTB)上的 DMA 窗口。这允许设备使用本机 DMA 来读写共享内存段。段可以是本地的或远程的，SmartIO 将自动解析设备端物理地址(远程)内存段，允许相同的软件在任何集群节点上运行，并且对其他主机的特定地址空间布局保持不变。请注意，由于任何注册到 SmartIO 的设备的 BAR 都自动导出为 SISCI 段，我们也可以为其他设备映射它们。

- API functions for allocating SISCI segments using access pattern hinting. While the original SISCI implementation only allows hosts to allocate segments in local system memory, we have added functionality for letting SmartIO choose which host to allocate memory in based on expected access patterns. By relying on hinting rather than actively specifying which host to allocate memory in, we can consider memory locality without requiring awareness of the physical PCIe topology. Note that as these segments are associated with a device rather than cluster nodes, we retain the logical decoupling of machines and devices provided by SmartIO.

> 为了根据预期的访问模式来选择在哪个主机上分配内存，我们添加了功能，使得原本只允许主机在本地系统内存中分配段的 SISCI API 函数可以使用访问模式提示。通过依赖提示而不是主动指定在哪个主机上分配内存，我们可以考虑内存局部性，而不需要了解 PCIe 拓扑结构。请注意，由于这些段与设备而不是集群节点相关联，我们可以保留 SmartIO 提供的机器和设备的逻辑解耦。

Perhaps the most obvious trade-off from using our API extension is that it requires implementing a new device driver. Usually, implementing a driver from scratch entails a considerable engineering effort, and may not even be a viable option in most cases. After all, the main strength of both our Device Lending mechanism and MDEV extension is that they do not require any modifications of existing device drivers. However, as using this API extension allows a device driver to be implemented as part of cluster applications, it is potentially extremely useful for some application domains. By implementing a driver using our API extension, devices can be disaggregated at the software level, rather than at the PCIe device function level. Multiple application processes, running on different nodes, may share devices that do not support SR-IOV. Moreover, not only does our API extension provide an interface for distributed device drivers, but it also becomes possible to write device drivers that fully utilize PCIe shared-memory capabilities. For example, applications may use PCIe multicasting to stream data to several destinations in a single operation. It is even possible to exploit memory locality to optimize data flow through the network.

> 也许使用我们的 API 扩展最明显的权衡就是需要实现一个新的设备驱动程序。通常，从头开始实现一个驱动程序需要相当大的工程努力，在大多数情况下甚至可能不是一个可行的选择。毕竟，我们的设备借用机制和 MDEV 扩展的主要优势在于它们不需要对现有设备驱动程序进行任何修改。然而，由于使用此 API 扩展可以将设备驱动程序作为集群应用程序的一部分实现，因此对某些应用领域来说可能非常有用。通过使用我们的 API 扩展实现驱动程序，可以在软件层面对设备进行解耦，而不是在 PCIe 设备功能层面进行解耦。多个运行在不同节点上的应用程序可以共享不支持 SR-IOV 的设备。此外，我们的 API 扩展不仅提供了一个分布式设备驱动程序的接口，而且还可以编写完全利用 PCIe 共享内存功能的设备驱动程序。例如，应用程序可以使用 PCIe 多播将数据在一个操作中流向多个目标。甚至可以利用内存局部性来优化数据流经网络。

### 6.2 Driver Implementation

By avoiding contention in command submission and completion paths and supporting up to 65,535$\text{65,535}$ I/O queues per device, the NVMe standard [[55](#Bib0055)] enables highly parallel operation. Figure [14](#fig14)(a) illustrates how NVMe utilizes a submission and completion queue mechanism. One or more **submission queues (SQs)** are paired with a **completion queue (CQ)**, i.e., multiple SQs may be paired with the same CQ. Commands posted to an SQ will be completed by an entry in the associated CQ. Queues are implemented as ring-buffers, and are allocated in memory by the device driver software as depicted in Figure [14](#fig14)(b). Each queue has its own dedicated doorbell register, avoiding any contention. By allowing queues to operate in parallel, NVMe completely avoids locking and other forms of synchronization between queues.

> 通过避免命令提交和完成路径中的争议，并支持每个设备最多 65,535$\text{65,535}$个 I/O 队列，NVMe 标准[[55](#Bib0055)]可以实现高度并行操作。图 14(a)说明了 NVMe 如何使用提交和完成队列机制。一个或多个提交队列(SQ)与一个完成队列(CQ)配对，即多个 SQ 可以与同一个 CQ 配对。发布到 SQ 的命令将由关联 CQ 的条目完成。队列实现为环形缓冲区，并由设备驱动程序软件如图 14(b)所示在内存中分配。每个队列都有自己专用的门铃寄存器，避免任何争议。通过允许队列并行操作，NVMe 完全避免了队列之间的锁定和其他形式的同步。

![](https://dl.acm.org/cms/attachment/7ebc0bc7-b48f-41ec-a3a7-175fb92a593e/tocs380102-02-f14.jpg) Fig. 14. NVMe avoids contention in the command submission and completion path by using parallel queues that can be hosted anywhere in physical memory.

Figure [14](#fig14)(c) illustrates the basic operation of an NVMe device: The driver software places a command, e.g., “read N$N$ blocks,” into an SQ. It then “rings” the associated SQ doorbell (by writing the SQ tail pointer value). This notifies the NVMe device of how many new commands are ready to be processed. The device fetches commands from SQ memory using DMA. After executing the command, the drive writes a completion to the paired CQ, indicating the status of the operation. The driver must poll CQ memory for new completions,[5](#fn5) and, as commands may be executed out of order, the driver must keep track of command sequence numbers. Once completions are processed, the driver notifies the NVMe device by updating the CQ doorbell (writing the CQ head pointer value).

> 图 14(c)描述了 NVMe 设备的基本操作：驱动程序将命令(例如“读取 N$N$块”)放入 SQ 中。然后，它会通过写入 SQ 尾指针值来“环绕”相关的 SQ 门铃，以通知 NVMe 设备有多少新命令可以处理。该设备使用 DMA 从 SQ 内存中获取命令。执行命令后，驱动程序将完成写入配对的 CQ，以指示操作的状态。驱动程序必须轮询 CQ 内存以查找新的完成情况，由于命令可能按照不同的顺序执行，驱动程序必须跟踪命令序列号。一旦处理完成，驱动程序通过更新 CQ 门铃(写入 CQ 头指针值)来通知 NVMe 设备。

To configure I/O queues and manage the device, driver software must first “reset” the device. This is done by clearing a control register on the NVMe controller and writing the base address of the so-called “admin queues,” consisting of an admin SQ and an admin CQ. Whereas regular I/O queues use an I/O command set, i.e., reading and writing blocks, the admin queues use a different set of commands for managing the controller, e.g., creating and deleting I/O queues and retrieving controller status.

> 为了配置 I/O 队列并管理设备，驱动程序软件必须首先“重置”设备。这是通过清除 NVMe 控制器上的控制寄存器并写入所谓的“管理队列”的基地址来完成的，其由管理 SQ 和管理 CQ 组成。而普通的 I/O 队列使用 I/O 命令集，即读写块，而管理队列使用用于管理控制器的不同命令集，例如创建和删除 I/O 队列以及检索控制器状态。

Our driver implementation consists of a “manager” and one or more “clients,” running as userspace software applications. The manager is responsible for initializing the NVMe device, configuring admin queues and relaying admin commands on behalf of clients. A client is a userspace process using one or more I/O queue pairs to read or write data from the NVMe device directly; through using the SISCI API extension described in Section [6.1](#sec-26), the device can DMA directly to application memory with minimal latency. Note that the device manager and clients in this instance are _not_ synonymous with the lender and borrowers. Any node in the cluster may run a manager driver, and the same node may even run both a manager driver and client drivers.

> 我们的驱动程序实现由“管理器”和一个或多个“客户端”组成，作为用户空间软件应用程序运行。管理器负责初始化 NVMe 设备，配置管理队列并代表客户端中继管理命令。客户端是一个用户空间进程，使用一个或多个 I/O 队列对来直接从 NVMe 设备读取或写入数据；通过使用第[6.1]节中描述的 SISCI API 扩展，设备可以直接 DMA 到应用程序内存，具有极低的延迟。请注意，此实例中的设备管理器和客户端*不*等同于贷方和借方。集群中的任何节点都可以运行管理驱动程序，同一个节点甚至可以同时运行管理驱动程序和客户端驱动程序。

Figure [15](#fig15) illustrates how the driver implementation works. The manager, in this case running on Borrower B, takes control over the NVMe device by using our SISCI SmartIO API extension and borrowing the device. The device registers (NVMe BAR) are already exported as a memory segment, allowing the manager to memory-map them into application address space. Also using the API, the manager allocates a memory segment and maps it for the device (Segment B), retrieving the device-local I/O address (the address, as seen from the device). Finally, the manager resets the NVMe device and sets up admin queues using the device-local I/O address with the appropriate offsets. By having memory-mapped device registers, the manager may “ring” the queue doorbell registers, notifying the device that an admin queue event has occurred. Similarly, as the local memory segment is mapped for the device, the NVMe device is able to fetch commands and post completions over the NTB.

> 图 15(#fig15)说明了司机实施的工作原理。在这种情况下，运行在借用者 B 上的管理器控制 NVMe 设备，使用我们的 SISCI SmartIO API 扩展借用设备。设备注册(NVMe BAR)已经作为内存段导出，允许管理器将它们映射到应用程序地址空间中。同样使用 API，管理器分配一个内存段并为设备映射它(段 B)，检索设备本地 I / O 地址(从设备视图的地址)。最后，管理器重置 NVMe 设备并使用设备本地 I / O 地址以适当的偏移量设置管理队列。通过内存映射设备寄存器，管理器可以“环”队列门铃寄存器，通知设备发生了管理队列事件。同样，由于本地内存段被映射到设备，NVMe 设备能够通过 NTB 获取命令并提交完成。

![](https://dl.acm.org/cms/attachment/2af92cd7-e3c6-48ce-a338-6d661e0b85e4/tocs380102-02-f15.jpg) Fig. 15. Simultaneous sharing: The NVMe device can access queues residing in memory segments on different hosts by mapping the segments for the device (DMA windows). Likewise, the borrowers must in turn map the doorbell registers for their respective queues to notify the device about queue events. Each queue has a dedicated register, avoiding any contention between borrowers.

A client driver also borrows the device using the API and memory-maps device registers. Additionally, it can allocate a local segment and map it for the device, retrieving the device-local I/O address. By relaying admin commands using the manager, it can create SQs and CQs using the device-local I/O address. As seen in Figure [15](#fig15), Borrower A and Borrower C run client drivers and have successfully requested I/O queues for themselves. With these in place, the NVMe device may now be used for I/O, by multiple hosts in parallel. From the point of view of the device, the queues are accessed just like they would be in local memory. In other words, our distributed driver implementation facilitate queue-level sharing of a non-SR-IOV NVMe device, enabling distributed I/O with extremely low latency overhead.

> 客户端驱动程序也可以使用 API 和内存映射设备寄存器来借用设备。此外，它还可以分配本地段并将其映射到设备，以检索设备本地 I/O 地址。通过使用管理器中继管理命令，它可以使用设备本地 I/O 地址创建 SQs 和 CQs。如图 15 所示，借用者 A 和借用者 C 运行客户端驱动程序，并且已成功为自己请求 I/O 队列。有了这些，NVMe 设备现在可以用于 I/O，多个主机可以并行使用。从设备的角度来看，队列的访问就像在本地内存中一样。换句话说，我们的分布式驱动程序实现了非 SR-IOV NVMe 设备的队列级共享，实现了低延迟的分布式 I/O。

### 6.3 Multipath Failover

An added benefit of using our SmartIO API extension is that it becomes possible for systems with multiple NTB adapters to borrow the same device through different paths. In the case of our proof-of-concept NVMe driver explained in Section [6.2](#sec-27), it becomes possible to set up redundant I/O queues in advance, and set up mappings through different paths. If the primary path fails, then the driver software may switch over to a backup queue.

> 使用我们的 SmartIO API 扩展的另一个好处是，具有多个 NTB 适配器的系统可以通过不同的路径借用同一个设备。在我们在[6.2](#sec-27)节中解释的概念驱动程序的情况下，可以提前设置冗余的 I/O 队列，并通过不同的路径设置映射。如果主路径失败，驱动程序软件可以切换到备份队列。

Figure [16](#fig16) illustrates how this is possible: the borrower maps the NVMe device BAR through both its NTB adapters, providing it with two separate paths to the NVMe queue doorbell registers. It can then set up two separate queue pairs in local memory, and by specifying which of the local adapters it is using to reach the NVMe device, our SmartIO system will automatically resolve which of the lender-side NTB adapters to configure DMA windows through. Having established two separate paths, our NVMe driver then chooses one path as its primary path and the other for backup. In the case of a link failure, our NVMe driver is notified either by NVMe I/O command time-out events, or by the low-level NTB driver notifying the NVMe driver about a link event affecting its mapped segments. Reads and writes to mappings that are inactive are terminated by the local NTB adapter.[6](#fn6) Depending on the kind of failure, for example in the case of a cable being yanked out and plugged back in again, the link may come back up again with mappings still valid. In this case, our NVMe driver can resume using the old queue pair.

> 图 16(#fig16)说明了如何实现这一目标：借方将 NVMe 设备 BAR 映射到其两个 NTB 适配器上，为其提供两条独立的路径到 NVMe 队列门钟寄存器。然后，它可以在本地内存中设置两个独立的队列对，并通过指定使用哪个本地适配器来访问 NVMe 设备，我们的 SmartIO 系统将自动解析要通过哪个出借方 NTB 适配器配置 DMA 窗口。建立了两条单独的路径后，我们的 NVMe 驱动程序将其中一条作为主路径，另一条作为备份。在链路故障的情况下，我们的 NVMe 驱动程序可以通过 NVMe I/O 命令超时事件或低级 NTB 驱动程序通知 NVMe 驱动程序有关影响其映射段的链路事件来得到通知。对于不活动的映射，本地 NTB 适配器将终止读写操作。[6](#fn6)根据故障类型，例如拔出并插回电缆的情况，链路可能会重新恢复，映射仍然有效。在这种情况下，我们的 NVMe 驱动程序可以恢复使用旧的队列对。

![](https://dl.acm.org/cms/attachment/75900e08-10cc-4deb-9f63-f7abef7d7c97/tocs380102-02-f16.jpg) Fig. 16. Multipath failover: We can configure multiple NVMe queue pairs and mapping their memory for the device through different NTB adapters. Similarly, we can also map doorbell registers through separate adapters for the borrower. By having different paths for each I/O queue pair, we can continue operating the NVMe even if one of the paths fail.

The link may become active again with invalid mappings. In the case of I/O queue pairs, this is inconsequential as the NVMe standard supports deleting and creating I/O queues during operation; we can simply delete the old queues, set up new DMA windows and create new queues. However, special care must be taken with regards to the admin queues as they cannot be deleted and recreated without resetting the device and halting all operation. Because of this, we prefer running the manager driver (owning the admin queues) on the lender node. Even if a client's path to the manager is lost, it can have a backup communication path or can re-establish communication if the path comes back up again, without requiring a reset of the device.

> 链接可能会在无效映射的情况下再次激活。对于 I/O 队列对，这没有任何影响，因为 NVMe 标准支持在操作期间删除和创建 I/O 队列；我们可以简单地删除旧队列，设置新的 DMA 窗口并创建新的队列。但是，对于管理队列，必须特别小心，因为它们不能被删除和重新创建，而不需要重置设备和停止所有操作。因此，我们更喜欢在贷款节点上运行管理驱动程序(拥有管理队列)。即使客户端到管理器的路径丢失，它也可以有一个备用通信路径，或者如果路径再次恢复，可以重新建立通信，而不需要重置设备。

### 6.4 GPU Support

Many GPU-accelerated applications require fast access to storage. For example, the datasets in big data and machine learning tasks can be hundreds of terabytes. As datasets’ size for typical GPU workloads is only increasing, GPU applications become bounded by transfers between storage and GPU. To overcome this, many GPUs permit peer-to-peer DMA to avoid unnecessary copies via system memory [[11](#Bib0011)]. For Nvidia GPUs, such peer-to-peer DMA with third-party devices is supported using GPUDirect [[53](#Bib0053)]. Introduced in the 5.0 version of the CUDA API, memory allocated on the GPU can be exposed through the GPU's device memory regions. This allows third-party devices, such as NVMe devices and network cards, to access GPU memory directly [[70](#Bib0070), [91](#Bib0091)]. Figure [17](#fig17) illustrates the steps involved in reading from storage and loading data onto GPU memory before launching a CUDA kernel[7](#fn7) on the GPU. The unnecessary steps of first having to read from storage into system memory, and then copying the data to the GPU, as shown in Figure [17](#fig17)(a), can be avoided. Instead, we can map GPU memory for the NVMe (using GPUDirect) and allow the NVMe to access this memory directly using peer-to-peer DMA, as illustrated in Figure [17](#fig17)(b).

> 许多 GPU 加速的应用程序需要快速访问存储。例如，大数据和机器学习任务中的数据集可能有数百个 TB。由于典型 GPU 工作负载的数据集大小只会增加，GPU 应用程序受到存储和 GPU 之间传输的限制。为了克服这一点，许多 GPU 允许点对点 DMA，以避免通过系统内存进行不必要的复制[[11](#Bib0011)]。对于 Nvidia GPU，使用 GPUDirect 支持与第三方设备的点对点 DMA[[53](#Bib0053)]。在 CUDA API 5.0 版本中引入，可以通过 GPU 的设备内存区域公开在 GPU 上分配的内存[[70](#Bib0070)，[91](#Bib0091)]。图[17](#fig17)描述了从存储读取并在启动 GPU 上的 CUDA 内核[7](#fn7)之前将数据加载到 GPU 内存的步骤。如图[17](#fig17)(a)所示，不必要的步骤首先从存储中读取到系统内存，然后将数据复制到 GPU，可以避免。相反，我们可以使用 GPUDirect 为 NVMe 映射 GPU 内存，并允许 NVMe 直接使用点对点 DMA 访问此内存，如图[17](#fig17)(b)所示。

![](https://dl.acm.org/cms/attachment/66c64b03-a91d-4cd5-ac3c-11064f48a090/tocs380102-02-f17.jpg) Fig. 17. By exposing GPU memory through device memory regions (BARs), it is possible to read from storage directly onto the GPU. This reduces the number of steps required for loading data in to GPU memory.

However, while the CUDA driver does a decent job with regard to pipelining and scheduling, kernel launches are a costly operation from a computational point of view. A better approach would be to avoid interleaving storage I/O and launches altogether, by allowing a long-running kernel to initiate I/O instead. In version 8.0 of CUDA, additional support for registering device memory with the CUDA driver was added to GPUDirect [[90](#Bib0090)]. This feature makes it possible for CUDA applications to use the GPU's onboard DMA engine to access BARs of third-party devices. By memory-mapping the NVMe's BAR, and registering this memory with the CUDA driver, a CUDA kernel can directly access doorbell registers. Similarly, the NVMe is able to fetch commands and post completions to queues that are hosted in GPU memory by exporting GPU memory through GPUDirect. Figure [18](#fig18) depicts how both features of GPUDirect makes it possible to read from storage directly without involving any software running on the CPU. By operating queues directly, a long-running CUDA kernel can control the NVMe device itself. Loading and storing data can be initiated from the kernel running on the GPU, avoiding the CPU in the data path entirely. Not only does this reduce the latency of loading data onto the GPU, as the kernel may simply batch up read commands and initiate I/O on its own, but we also eliminate needing to schedule data copies from RAM between costly kernel launches.

> 然而，尽管 CUDA 驱动程序在管道和调度方面做得相当不错，但内核启动从计算角度来看是一项昂贵的操作。更好的方法是完全避免存储 I / O 和启动之间的交织，而是允许长时间运行的内核来发起 I / O。在 CUDA 8.0 版本中，增加了对将设备内存注册到 CUDA 驱动程序的支持，以支持 GPUDirect [[90](#Bib0090)]。此功能使 CUDA 应用程序能够使用 GPU 的本地 DMA 引擎访问第三方设备的 BAR。通过内存映射 NVMe 的 BAR，并将此内存注册到 CUDA 驱动程序，CUDA 内核可以直接访问门铃寄存器。同样，NVMe 能多通过 GPUDirect 将 GPU 内存导出，从而从 GPU 内存中获取命令并发布完成。图[18](#fig18)描绘了 GPUDirect 的两个功能如何使得可以直接读取存储而无需涉及 CPU 上运行的任何软件。通过直接操作队列，长时间运行的 CUDA 内核可以控制 NVMe 设备本身。加载和存储数据可以从 GPU 上运行的内核启动，完全避免了数据路径中的 CPU。这不仅降低了将数据加载到 GPU 的延迟，因为内核可以简单地批量读取命令并自行发起 I / O，而且还消除了在昂贵的内核启动之间从 RAM 调度数据副本的需要。

![](https://dl.acm.org/cms/attachment/35e9a7dc-318b-4091-b792-714278ac4a3f/tocs380102-02-f18.jpg) Fig. 18. Avoiding CPU synchronization: By hosting I/O queues in GPU memory and mapping doorbell registers for the GPU, a CUDA kernel running on the GPU can operate the NVMe without involving the CPU.

While controlling an NVMe device directly from a CUDA kernel is interesting in itself, it becomes particularly useful in the context of _remote_ devices. Using our SmartIO API extension, our NVMe driver implementation supports CUDA using GPUDirect, allowing queues and data to be accessed directly in GPU memory and “ringing” queue doorbell registers from software on the GPU. As our SmartIO system is aware of device memory regions and their BAR addresses, we can set up such peer-to-peer mappings between remote devices in a manner that is transparent for the CUDA driver. The NVMe may reside in the same host as the GPU, or a different host altogether. Furthermore, the GPU itself may be remote to the host currently running the CUDA driver, as depicted in Figure [19](#fig19). By using Device Lending and inserting the borrowed GPU into the local device tree, the CUDA application can launch kernels on a remote GPU. Since SmartIO resolves addresses between the different address spaces, the proprietary CUDA driver is completely unaware that both NVMe and GPU are remote devices. To the application, and the local CUDA driver, device memory is available through virtual address pointers that is mapped by our API extension, which are again passed to the GPU when the kernel is launched. This allows the kernel to operate the (remote) NVMe device entirely independent, without involving CPUs or system RAM in the data path at all.

> 控制 NVMe 设备直接从 CUDA 内核本身是有趣的，但在远程设备的背景下变得尤为有用。使用我们的 SmartIO API 扩展，我们的 NVMe 驱动程序实现支持使用 GPUDirect 的 CUDA，允许直接在 GPU 内存中访问队列和数据，并从 GPU 上的软件“响铃”队列门铃寄存器。由于 SmartIO 系统知道设备内存区域及其 BAR 地址，因此我们可以以 CUDA 驱动程序完全不知情的方式在远程设备之间设置这种点对点映射。NVMe 可以位于与 GPU 相同的主机上，也可以位于不同的主机上，如图 19 所示。通过使用设备借用并将借用的 GPU 插入本地设备树，CUDA 应用程序可以在远程 GPU 上启动内核。由于 SmartIO 解析不同地址空间之间的地址，因此专有的 CUDA 驱动程序完全不知道 NVMe 和 GPU 都是远程设备。对于应用程序和本地 CUDA 驱动程序来说，设备内存可以通过我们的 API 扩展映射的虚拟地址指针访问，并在启动内核时将其传递给 GPU。这允许内核完全独立地操作(远程)NVMe 设备，而不涉及数据路径中的 CPU 或系统 RAM。

![](https://dl.acm.org/cms/attachment/f1279a90-3bb6-434e-8367-26c8baf2cf0c/tocs380102-02-f19.jpg) Fig. 19. By combining the SmartIO API extension and Device Lending, our NVMe driver supports direct access to a remote NVMe from a borrowed GPU. To the CUDA driver running on the local system, both the NVMe device and GPU appear local. Our SmartIO system injects necessary peer-to-peer mappings transparently. Note that the GPU operates the NVMe independently; no CPU is needed to access storage.

### 6.5 Multicast

Some NTB-capable switch chips also support multicasting, as described in Section [3.2](#sec-12). Memory writes to a multicast address are routed out on several switch ports. By reserving a continuous address range and dividing it into equal sized “multicast groups,” the system can assign different groups to different switch ports. Subsequently, it is possible to use different destinations for different multicast groups.

> 一些支持 NTB 的交换芯片也支持多播，如第 3.2 节所述。对多播地址的内存写入会被路由到多个交换端口上。通过保留一个连续的地址范围并将其划分为相等大小的“多播组”，系统可以将不同的组分配给不同的交换端口。因此，可以为不同的多播组使用不同的目的地。

However, not all devices support multicast. To overcome this, switches may use a “multicast overlay BAR.” If a multicast write matches the overlay BAR on an outgoing switch port, then the top part of the address is replaced with an overlay address. As such, the overlay BAR provides a window into unicast address space for devices (endpoints) that do not support multicast natively. For example, a multicast address may be mapped onto the BAR of a downstream device.

> 然而，并非所有设备都支持多播。为了克服这一点，交换机可以使用“多播覆盖 BAR”。如果多播写入与输出交换机端口上的覆盖 BAR 匹配，则地址的顶部部分将被覆盖地址所取代。因此，覆盖 BAR 为不支持多播的设备(端点)提供了一个进入单播地址空间的窗口。例如，可以将多播地址映射到下游设备的 BAR 上。

Figure [20](#fig20) illustrates how we can use multicast to load data from storage to multiple destinations in a single operation. Our SmartIO API extension allows setting up NTB mappings to multicast addresses, allowing a single DMA write operation to be replicated by the switch chip hardware in the cluster switches. When the multicast write reaches the egress NTB adapter, we use an overlay BAR to map the address into anywhere in local address space as long as the destination memory is linear. This makes it possible to set up mappings to either system memory or the BAR of a device, for example GPU memory.

> 图 20(#fig20)描绘了我们如何使用多播从存储器一次操作中加载数据到多个目标。我们的 SmartIO API 扩展允许设置 NTB 映射到多播地址，允许单个 DMA 写操作由集群交换芯片硬件在集群交换机上复制。当多播写入到出口 NTB 适配器时，我们使用覆盖 BAR 将地址映射到本地地址空间的任何位置，只要目标内存是线性的。这使得可以设置映射到系统内存或设备的 BAR，例如 GPU 内存。

![](https://dl.acm.org/cms/attachment/87ddf3b0-b569-4bd5-a19e-a6354a1a874f/tocs380102-02-f20.jpg) Fig. 20. Multicast support makes it possible for a single DMA operation to be replicated to multiple destinations. It is possible to map multicast destination to system memory and device memory alike.

## 7 PERFORMANCE EVALUATION

The SmartIO system makes it possible to distribute PCIe devices in a PCIe-interconnected cluster. Our implementation relies on several software and hardware components that enable access to remote devices over the network. We have evaluated Device Lending and the MDEV extension in our previous work, explaining performance differences as being caused by increased latency from longer PCIe paths [[48](#Bib0048), [49](#Bib0049)]. However, by setting up the necessary memory-mappings in advance and injecting these prepared mappings during use of the device as explained in Section [4.3](#sec-17), there should not be _any_ impact on performance. After all, we only rely on native PCIe in the critical path. Although it may be extrapolated from our previous results that Device Lending and MDEV does not cause any performance degradation, it is not concrete evidence. The assertion that our SmartIO system has no performance overhead compared to local access warrants proper investigation, something our previous evaluations partly lacked.

> SmartIO 系统可以在 PCIe 互联集群中分发 PCIe 设备。我们的实现依赖于几个软件和硬件组件，可以通过网络访问远程设备。我们在以前的工作中评估了设备借用和 MDEV 扩展，解释性能差异是由于 PCIe 路径更长而增加的延迟[[48](#Bib0048)，[49](#Bib0049)]。但是，通过提前设置必要的内存映射，并在使用设备时注入这些准备好的映射，如[4.3](#sec-17)节所述，应该不会对性能产生任何影响。毕竟，我们只依赖关键路径上的本地 PCIe。虽然可以从我们以前的结果推断出设备借用和 MDEV 不会造成任何性能降低，但这不是具体的证据。我们断言 SmartIO 系统与本地访问相比没有性能开销，这需要适当的调查，这是我们以前的评估所缺乏的。

To remedy this, we present here an evaluation consisting of several, entirely new performance experiments. These new experiments are designed to verify that our sharing techniques themselves do not add any performance penalty compared to local access. By comparing the performance of using remote devices to using devices attached to a local PCIe bus, thus establishing a “local baseline” for comparison, any overhead caused by our implementation should be revealed. All parts comprising our SmartIO system is evaluated from multiple angles to verify that our solution is in fact “zero-overhead.” Not only do we here revalidate our previous findings [[48](#Bib0048), [49](#Bib0049)], but we also argue that this improved evaluation supersedes our previous work, as we present updated performance results using more recent hardware. In addition, we present evaluations on other parts of the system that have not been presented in earlier work, such as an isolated latency analysis of our memory-mapping routines, and an evaluation of the shared-memory capabilities of our new NVMe driver. In total, this gives a complete evaluation of the entire SmartIO system.

> 为了解决这个问题，我们在这里提出了一个由几个全新的性能实验组成的评估。这些新实验旨在验证我们的共享技术本身不会增加与本地访问相比的任何性能损失。通过比较使用远程设备与使用连接到本地 PCIe 总线的设备的性能，从而建立一个“本地基线”进行比较，应该可以揭示我们实现带来的任何开销。我们的 SmartIO 系统的所有组成部分都从多个角度进行了评估，以验证我们的解决方案实际上是“零开销”的。我们不仅在这里重新验证了我们以前的发现[[48](#Bib0048), [49](#Bib0049)]，而且还认为，这种改进的评估超越了我们以前的工作，因为我们使用更新的硬件提供了更新的性能结果。此外，我们还提出了以前的工作中没有提出的系统的其他部分的评估，例如我们的内存映射例程的隔离延迟分析，以及对我们新的 NVMe 驱动程序的共享内存功能的评估。总之，这给出了整个 SmartIO 系统的完整评估。

We have organized the evaluation of the different components of our SmartIO system as follows:

> 我们已经按照以下方式组织了对我们 SmartIO 系统的不同组件的评估：

- In Section [7.1](#sec-32), we perform a series of experiments comparing Device Lending to local configurations, showing that our implementation does not cause any performance degradation beyond what is expected for deeper PCIe device trees. Additionally, we prove the capability of running unmodified software and device drivers, by using standard benchmarking applications and native device drivers.

> 在第 7.1 节(#sec-32)中，我们进行了一系列比较设备借用和本地配置的实验，表明我们的实现除了预期的更深的 PCIe 设备树之外不会造成任何性能降低。此外，我们通过使用标准基准应用程序和本机设备驱动程序来证明运行未修改的软件和设备驱动程序的能力。

- In Section [7.2](#sec-39), we evaluate the usefulness of Device Lending for realistic workloads by presenting the performance of an image classification application implemented for Keras and Tensorflow [[1](#Bib0001), [2](#Bib0002)]. By training a convolutional neural network using several remote devices from different hosts, we prove the capability of Device Lending for scaling heavy workloads.

> 在第 7.2 节(#sec-39)中，我们通过展示使用 Keras 和 Tensorflow 实现的图像分类应用程序的性能，评估了设备借用在实际工作负载中的有用性[[1](#Bib0001), [2](#Bib0002)]。通过使用来自不同主机的几个远程设备来训练卷积神经网络，我们证明了设备借用用于扩展重型工作负载的能力。

- We evaluate our MDEV implementation in Section [7.3](#sec-40), where we pass-through physical GPUs to a VM guest and benchmark DMA performance. We are able to demonstrate that our implementation achieves the same performance as bare-metal configurations.

> 我们在[7.3](#sec-40)节中评估了我们的 MDEV 实现，在那里我们将物理 GPU 传递给 VM 客户机并对 DMA 性能进行基准测试。我们能够证明，我们的实现达到了与裸机配置相同的性能。

- Experiments using our distributed NVMe driver are presented in Section [7.4](#sec-43). We demonstrate the flexibility of shared-memory clustering and our distributed device driver API by demonstrating how memory locality can be fully exploited to reduce latency. Additionally, we prove the latency benefit of using PCIe networking by comparing our implementation to a state-of-the-art NVMe-oF implementation using InfiniBand RDMA.

> 实验使用我们的分布式 NVMe 驱动程序在第[7.4]节中进行了展示。我们通过展示如何完全利用内存局部性来降低延迟，来证明共享内存集群和我们的分布式设备驱动程序 API 的灵活性。此外，我们通过将我们的实现与使用 InfiniBand RDMA 的最先进的 NVMe-oF 实现进行比较，证明了使用 PCIe 网络的延迟优势。

Note that throughout our evaluation, we have used different software versions for the different experiments, such as different Linux distributions and CUDA installations. This is to fully demonstrate that our SmartIO system is not limited to a specific Linux version, but works for a wide variety of distributions and software versions, including older versions. We make a point of using standard and unmodified benchmarking software for our tests. Furthermore, while we relied mostly on GPUs in our previous evaluations, we present here results using GPUs, network adapters, and NVMe devices to show that we can share any standard PCIe device. This has the added benefit of demonstrating several sharing scenarios for a range of applications, which are made possible by our SmartIO solution. For each set of experiments, we explicitly state what kind of hardware and software is used in the configuration.

> 在我们的评估中，我们为不同的实验使用了不同的软件版本，例如不同的 Linux 发行版和 CUDA 安装。这是为了充分证明我们的 SmartIO 系统不仅仅限于特定的 Linux 版本，而且适用于各种各样的发行版和软件版本，包括较旧的版本。我们使用标准的、未经修改的基准测试软件进行测试。此外，尽管我们在以前的评估中主要依赖 GPU，但我们在这里提供了使用 GPU、网络适配器和 NVMe 设备的结果，以表明我们可以共享任何标准的 PCIe 设备。这还有一个额外的好处，即可以通过我们的 SmartIO 解决方案展示多种应用程序的多种共享场景。对于每组实验，我们都会明确指出所使用的硬件和软件配置。

### 7.1 Device Lending

Our previous Device Lending evaluations focused on investigating how the increased latency from longer PCIe paths affects performance with regard to increased DMA latency and decreased link utilization [[48](#Bib0048), [49](#Bib0049)]. In the past, we have argued that this difference in performance is very small when compared to other device distribution mechanisms, such as RDMA. While it may be extrapolated from our results that our implementation does not cause any performance degradation, it is not sufficient evidence by itself that the performance difference is _only_ caused by additional switch chips in the PCIe paths.

> 我们以前的设备借用评估专注于调查延迟增加的更长 PCIe 路径如何影响 DMA 延迟增加和链接利用率降低的性能[[48](#Bib0048), [49](#Bib0049)]。过去，我们认为，与其他设备分发机制(如 RDMA)相比，这种性能差异非常小。虽然可以从我们的结果中推断出，我们的实现不会造成性能降低，但这本身还不足以证明性能差异*仅*由 PCIe 路径中的额外交换芯片引起。

Device Lending makes it is possible for application software on a local system to use remote devices without requiring any modifications to device drivers, or even the OS. Comparing the performance of using remote devices to a local baseline can be done by creating local PCIe device trees that are as similar to to the Device Lending scenarios as possible, since all other conditions are the same. We have performed a series of new experiments comparing Device Lending scenarios to local performance using a BP-457-ATX PCIe expansion chassis, to create PCIe paths with the same number of switch chips (or “hops”) for both local and remote topologies.

> 设备借用使得本地系统上的应用软件可以使用远程设备，而无需对设备驱动程序或操作系统进行任何修改。通过创建尽可能与设备借用场景相似的本地 PCIe 设备树，可以将使用远程设备的性能与本地基线进行比较，因为所有其他条件都相同。我们已经进行了一系列新的实验，比较了使用 BP-457-ATX PCIe 扩展机箱创建具有相同数量交换芯片(或“跳”)的本地和远程拓扑的设备借用场景与本地性能之间的关系。

7.1.1 _Latency Tests._ To prepare a DMA transfer, memory must be mapped for a device. This involves locking pages in memory so they are not swapped out and resolving their I/O addresses. For reading from block device, i.e., a storage device, the Linux block-layer pin the pages used by a memory buffer and create a scatter/gather list containing the physical addresses of the buffer. This list is then passed to the device driver, which in turns iterates the list and resolves I/O addresses by using the Linux DMA API. If the IOMMU is enabled, then the same API is used to set up IOMMU mappings for the device. The driver can then use these I/O addresses and initiate DMA transfers.

> 7.1.1 测试延迟。为了准备 DMA 传输，必须为设备映射内存。这涉及锁定内存中的页面，以免将其交换出去，并解析其 I/O 地址。对于从块设备(即存储设备)读取，Linux 块层会锁定内存缓冲区使用的页面，并创建一个包含缓冲区的物理地址的散射/聚集列表。然后将此列表传递给设备驱动程序，该驱动程序迭代该列表，并通过使用 Linux DMA API 来解析 I/O 地址。如果启用了 IOMMU，则使用相同的 API 来为设备设置 IOMMU 映射。然后驱动程序可以使用这些 I/O 地址并启动 DMA 传输。

As explained in Section [4.3](#sec-17), by inserting a shadow device into the local PCIe tree, our Device Lending mechanism has a “hook” in the DMA API. When the device driver calls the DMA API using the shadow device, we can calculate offsets and inject corresponding I/O addresses that map over the device-side NTB. This allows us to prepare mappings over the NTB in advance (“DMA windows”), and no communication between the lender and borrower is required. However, the software routine that calculates offsets may still have an impact on performance, particularly in the case of device drivers that frequently maps and unmaps memory for a device.

> 根据第 4.3 节的解释，通过将一个影子设备插入本地 PCIe 树，我们的设备借用机制在 DMA API 中有一个“钩子”。当设备驱动程序使用影子设备调用 DMA API 时，我们可以计算偏移量并注入映射到设备端 NTB 的相应 I/O 地址。这样，我们就可以提前准备 NTB 上的映射(“DMA 窗口”)，无需借方和出借方之间进行通信。但是，计算偏移量的软件例程仍然可能会对性能产生影响，特别是在设备驱动程序经常为设备映射和取消映射内存的情况下。

To measure any performance impact of our mapping routine, we have used the _**Flexible I/O tester**_ **(FIO)** [[9](#Bib0009)]. FIO is a widely used userspace application for benchmarking the performance of storage devices, such as NVMe devices. By configuring FIO to perform reads and using the _sync_ engine, FIO opens a file descriptor to the block-device setting the O_DIRECT and O_SYNC options. This combination of options allows Linux to perform zero-copy reads from storage, bypassing the block-cache and forcing the block-layer and NVMe driver to map and unmap the userspace buffer for every single read operation. In other words, this FIO benchmark configuration forces our mapping routine to be invoked as part of the critical path.

> 为了衡量我们的映射程序的性能影响，我们使用了灵活的 I/O 测试器(FIO)[[9](#Bib0009)]。 FIO 是一个广泛使用的用户空间应用程序，用于基准测试存储设备(如 NVMe 设备)的性能。 通过配置 FIO 执行读取并使用同步引擎，FIO 将文件描述符打开到块设备，设置 O_DIRECT 和 O_SYNC 选项。 这种选项组合允许 Linux 从存储设备执行零复制读取，绕过块缓存并强制块层和 NVMe 驱动程序为每个单独的读取操作映射和取消映射用户空间缓冲区。 换句话说，这个 FIO 基准配置强制我们的映射程序作为关键路径的一部分被调用。

Figure [21](#fig21) shows the hardware topologies for our test scenarios:

![](https://dl.acm.org/cms/attachment/a2306a24-01f4-4f20-90b0-0edaf813b9b7/tocs380102-02-f21.jpg) Fig. 21. We benchmark our Device Lending driver software by using an NVMe benchmark that calls our mapping code in the critical path (FIO). By using an expansion chassis, the NVMe device is the same number of hops away from the CPU currently using it for both the Local Baseline comparison and Device Lending. The only difference is whether the switch chip is configured in transparent or NTB mode.

- **Local Baseline**, shown in Figure [21](#fig21)(a): An expansion chassis connected to a local host running CentOS 7, using the 3.10 version of the kernel and the built-in NVMe driver. We are running FIO version 3.7 as available from the CentOS 7 software repositories.

> 本地基线(见图 21(a))：一个连接到运行 CentOS 7 的本地主机的扩展机箱，使用 3.10 版本的内核和内置的 NVMe 驱动程序。我们正在使用 CentOS 7 软件存储库中可用的 FIO 3.7 版本运行。

    The expansion chassis is connected to the upstream host through One Stop Systems HIB68-x16 target adapter cards. These adapters use the same Broadcom PEX8733 switch chip used in the Dolphin PXH830 NTB adapters.[8](#fn8) By placing the NVMe device in an expansion chassis, we were able to create a similar PCIe path for both test scenarios. Additonally, the IOMMU was disabled, to make the configuration comparable to Device Lending described below.

- **Device Lending**, shown in Figure [21](#fig21)(b): Two are connected together in a back-to-back topology, using Dolphin PXH830 adapter cards and external PCIe cables Both hosts are running CentOS 7 with the 3.10 kernel, and the local system running the benchmark has borrowed the remote NVMe and inserted it into the local PCIe tree and using the in-kernel NVMe driver. The IOMMU on the borrower is disabled, and we have configured the the DMA window size large enough to map the entire memory of the borrowing system. By disabling the IOMMU on the borrower, we make sure that the only latency overhead is our own mapping routine. The same expansion chassis configuration as in the local baseline is used, and by disabling the IOMMU on the lender, PCIe transactions are routed peer-to-peer as illustrated. This ensures that the NVMe device is the same number of switch chips away from the CPU currently using it, making the configuration comparable to the local baseline configuration described above. The only difference is whether the switch chip in the adapter cards is configured in _transparent_ or _non-transparent_ mode (NTB).[9](#fn9)

> 设备借用，如图 21(b)所示：两台主机使用 Dolphin PXH830 适配器卡和外部 PCIe 线缆以背面对背的拓扑连接在一起，两台主机均运行 CentOS 7 操作系统，内核版本为 3.10。本地系统借用远程 NVMe，将其插入本地 PCIe 树中，并使用内核 NVMe 驱动程序。借用者的 IOMMU 已禁用，我们配置了足够大的 DMA 窗口以映射整个借用系统的内存。通过禁用借用者的 IOMMU，我们确保唯一的延迟开销是我们自己的映射程序。与本地基线配置相同的扩展机箱配置被使用，并且通过禁用放借者的 IOMMU，PCIe 事务按照上图路由点对点。这确保 NVMe 设备距离当前使用它的 CPU 的交换芯片数量与本地基线配置相同，只有适配器卡中的交换芯片配置为透明模式或非透明模式(NTB)[9]不同。

In both scenarios, FIO was configured to perform 8,192 reads per run, each read is a page-sized (4 kB) chunk at an offset generated by a pseudo-random generator. As FIO reuses the same buffer for every read call, we ran FIO several times and concatenated the results. In addition, we reloaded the NVMe driver between each fourth run to force the system to use different memory locations for the internal I/O command queues. Moreover, we also rebooted the system between each eighth run of FIO to ensure that the results were the same across multiple system reboots. In short, for both scenarios, we had 10 reboots, 2 reloads of the NVMe driver per reboot, 4 FIO runs per driver reload, and 8,192 read operations per run. As the purpose of this test is not to benchmark the performance of the NVMe device, but rather a potential overhead of our Device Lending mechanism, the NVMe drive we have used is a prototype RAM disk with an NVMe controller from PMC-Sierra. This is to avoid any effects caused by prefetching and caching that modern SSDs are capable of.

> 在两种情况下，FIO 都被配置为每次运行 8192 次读取，每次读取都是由伪随机生成器生成的偏移量的页面大小(4 kB)块。由于 FIO 重复使用相同的缓冲区，我们运行了几次 FIO 并连接了结果。此外，我们在每四次运行 FIO 之间重新加载 NVMe 驱动程序，以强制系统使用不同的内存位置来进行内部 I / O 命令队列。此外，我们还在每八次 FIO 运行之间重新启动系统，以确保结果在多次系统重新启动之间保持一致。简而言之，在两种情况下，我们都有 10 次重新启动，每次重新启动 2 次 NVMe 驱动程序重新加载，每次驱动程序重新加载 4 次 FIO 运行和每次运行 8192 次读取操作。由于此测试的目的不是对 NVMe 设备的性能进行基准测试，而是潜在的设备借用机制的开销，因此我们使用的 NVMe 驱动器是 PMC-Sierra 的原型 RAM 磁盘和 NVMe 控制器。这是为了避免现代 SSD 能够实现的预取和缓存带来的任何影响。

Figure [21](#fig21)(c) shows the latency distribution of read operations for both using a local NVMe device (Local Baseline) and when accessing a remote NVMe device using Device Lending. Although the purpose of the test is simply to compare Device Lending to local access, it is interesting to note that the distributions have distinctive “spikes” occurring at regular intervals. We suspect that these spikes may be caused by a combination of task scheduling in the kernel and interrupt aggregation by the NVMe device. We see that the two distributions overlap, and the medians differ with 23 ns. Considering the spread of the distribution, this is not statistically significant. We argue that this demonstrates that there is no significant difference in performance for local and remote.

> 图 21(c)显示了使用本地 NVMe 设备(本地基线)和使用设备借用访问远程 NVMe 设备的读操作的延迟分布。尽管测试的目的仅仅是比较设备借用和本地访问，但有趣的是，这些分布具有定期出现的“尖峰”。我们怀疑这些尖峰可能是内核任务调度和 NVMe 设备中断聚合的结果。我们看到两个分布重叠，中位数相差 23 ns。考虑到分布的范围，这没有统计学意义。我们认为这表明本地和远程之间没有显着的性能差异。

7.1.2 _Throughput Tests._ As mentioned in Section [4](#sec-14), it is not feasible for a lender to map the entire memory of multiple borrowers in a cluster. This would potentially require setting the NTB BAR size larger than what system limitations permits. Furthermore, not all devices support high I/O addresses, and such devices would be unable reach the higher address offsets of the NTB for large DMA windows. To overcome this, our implementation uses the IOMMU on the _borrower_ instead. By using the borrower-side IOMMU, we can create continuous address ranges using predetermined I/O addresses. These continuous ranges are trivially mapped by the device-side NTB (DMA windows) and can be done in advance. However, this requires dynamically adding memory pages to the IOMMU domain when the device driver is preparing DMA buffers. Our implementation must also make sure to not choose virtual I/O addresses that risk thrashing the IOMMU translation look-aside buffer [[7](#Bib0007)].

> 7.1.2 吞吐量测试。正如第 4 节所提及的，对于贷方来说，将多个借方的整个内存映射是不可行的。这可能需要将 NTB BAR 大小设置得比系统限制允许的更大。此外，并非所有设备都支持高 I/O 地址，这类设备将无法访问 NTB 的较高地址偏移以实现大型 DMA 窗口。为了克服这一点，我们的实现使用了借方的 IOMMU。通过使用借方 IOMMU，我们可以使用预定义的 I/O 地址创建连续的地址范围。这些连续范围可以由设备端 NTB(DMA 窗口)轻松映射，并且可以提前完成。但是，这需要在设备驱动程序准备 DMA 缓冲区时动态地将内存页添加到 IOMMU 域中。我们的实现还必须确保不要选择可能搅乱 IOMMU 转换查找缓冲区的虚拟 I/O 地址([7])。

By performing large DMA transfers, we saturate the PCIe links with DMA traffic and also stress system memory. This allows us to investigate if there is any performance difference between using a local device or a borrowed, remote device for high-throughput workloads. Any overhead caused by our IOMMU support would show as a noticeable performance difference in the achieved memory throughput. Figure [22](#fig22) shows the hardware topologies used in our tests:

> 通过执行大型 DMA 传输，我们可以使 PCIe 链路充满 DMA 流量，同时也可以压力系统内存。这样我们就可以调查使用本地设备或借用的远程设备进行高吞吐量工作时是否存在任何性能差异。我们 IOMMU 支持所带来的任何开销都会以显著的内存吞吐量性能差异的形式显示出来。图 22 显示了我们测试中使用的硬件拓扑结构：

![](https://dl.acm.org/cms/attachment/514e31da-2288-4bb2-9220-4edfadb4648e/tocs380102-02-f22.jpg) Fig. 22. By performing large DMA transfers, any overhead in the critical path would have been revealed as a difference in performance. As performance is the same for Device Lending and the Local Baseline, this is not the case, and the performance is indeed similar.

- **Local Baseline**, shown in Figure [22](#fig22)(a): A local system using a local Nvidia Quadro P4000 GPU in an expansion chassis. As with the NVMe tests, we use an expansion chassis to make the PCIe path similar to the Device Lending scenario. The IOMMU on the local CPU is enabled, and the Linux kernel decides IOMMU mappings. This makes the scenario comparable to the Device Lending scenario below.

> **本地基线**，如图 22(a)所示：使用本地 Nvidia Quadro P4000 GPU 的本地系统，在扩展机箱中。与 NVMe 测试一样，我们使用扩展机箱来使 PCIe 路径与设备借用场景相似。本地 CPU 上的 IOMMU 已启用，Linux 内核决定 IOMMU 映射。这使场景与下面的设备借用场景相当。

- **Device Lending**, shown in Figure [22](#fig22)(b): Two hosts connected back-to-back using Dolphin PXH830 NTB adapter cards, one host is borrowing the Quadro P4000 GPU. The IOMMU on the lender host is disabled, to enable DMA transfers to be routed shortest path over the NTB in the expansion chassis, making this scenario comparable to the Local Baseline. Since the GPU used in our tests is unable to reach high I/O addresses, mapping the entire memory of the borrower is not possible. Because of this, we configured the NTB BAR size to 1 GB. This is small enough for the system to place the NTB at low addresses during the PCIe bus enumeration described in Section [3.1](#sec-11). Since the IOMMU on the borrower is enabled, we can detect any overhead in how we use the IOMMU compared to the Local Baseline.

> 设备借用，如图 22(b)所示：使用 Dolphin PXH830 NTB 适配器卡连接两台主机，其中一台主机借用 Quadro P4000 GPU。出借主机的 IOMMU 被禁用，以便将 DMA 传输路由到扩展机箱中的 NTB 最短路径，使此场景与本地基线相当。由于我们测试使用的 GPU 无法达到高 I / O 地址，因此无法映射借用者的整个内存。因此，我们将 NTB BAR 大小设置为 1 GB。这个大小足够小，使系统在枚举 PCIe 总线期间将 NTB 放置在低地址中，如第 3.1 节所述。由于借用者的 IOMMU 已启用，因此我们可以检测与本地基线相比，我们如何使用 IOMMU 的任何开销。

We installed version 10.1 of CUDA (418.39 version of the Nvidia driver), and the systems are running Ubuntu 18.04.2 with the 4.15 version of the kernel. We used the _bandwidthTest_ program to create the workload. This CUDA program uses the GPU's on-board DMA engine to copy data between GPU memory and system memory, and is included in the CUDA Toolkit sample programs [[54](#Bib0054)]. For both scenarios, we configured bandwidthTest to initiate 1,000 DMA writes to system memory, and 1,000 DMA reads from system memory. We repeated this for sizes from 4 kB to 128 MB, to reveal any trends in increased transfer sizes.

> 我们安装了 CUDA 10.1(Nvidia 驱动程序 418.39 版本)，系统运行 Ubuntu 18.04.2，内核版本为 4.15。我们使用*bandwidthTest*程序创建工作负载。这个 CUDA 程序使用 GPU 的板载 DMA 引擎在 GPU 内存和系统内存之间复制数据，并包含在 CUDA 工具包样本程序中[[54](#Bib0054)]。对于两种情况，我们将 bandwidthTest 配置为启动 1000 次 DMA 写入系统内存，以及 1000 次 DMA 从系统内存读取。我们将大小从 4kB 到 128MB 重复此操作，以揭示传输大小增加的任何趋势。

Figure [22](#fig22)(c) depicts the results of our test, with DMA writes in the top row and DMA reads in the bottom row. The different transfer sizes are plotted along the X-axis. The left column depicts the median of 1,000 runs. To show that even the distribution of measurements are similar for local and remote, we depict the min–max distance of the reported throughput samples on the right column. Since the Nvidia driver actively trains down the PCIe link to conserve power consumption, we enabled persistence mode on the GPU. However, this was not enough to entirely avoid that the GPU's DMA engine takes some time to “warm up” caches on the GPU. Because of this, measurements below the 0.1th percentile are marked as outliers. The throughput for Local Baseline and Back-to-Back scenarios overlap almost perfectly, which should be interpreted as a strong indication that our Device Lending implementation does not introduce any overhead compared to local performance. Finally, we also observe a strange effect for DMA reads where the achieved throughput for Device Lending appears to overtake local performance. This “boost” is statistically significant, as can be seen in the min–max plot. We do not fully understand what causes this effect, but we suspect that it may be caused by different IOMMU mappings for the Local Baseline and Device Lending scenarios, since they are decided by the kernel and our implementation, respectively.

> 图 22(c)描绘了我们测试的结果，上行为 DMA 写入，下行为 DMA 读取。不同的传输大小沿 X 轴排列。左列描绘了 1000 次运行的中位数。为了表明本地和远程的测量分布相似，我们在右列描绘了报告吞吐量样本的最小-最大距离。由于 Nvidia 驱动程序积极训练 PCIe 链路以节省能耗，我们在 GPU 上启用了持久模式。但是，这还不足以完全避免 GPU 的 DMA 引擎需要花一些时间来“热身”GPU 上的缓存。因此，低于 0.1 百分位的测量结果被标记为异常值。本地基线和回回模式的吞吐量几乎完全重叠，这应该被解释为我们的设备借用实现与本地性能没有任何开销的强烈指示。最后，我们还观察到了一个奇怪的效果，即设备借用的吞吐量似乎超过了本地性能。这种“提升”在统计学上是显著的，如最小-最大图所示。我们不完全理解是什么引起了这种效果，但我们怀疑这可能是由于本地基线和设备借用场景的 IOMMU 映射由内核和我们的实现分别决定所致。

7.1.3 _Longer PCIe Paths._ PCIe transactions are either _posted_ or _non-posted_ operations, meaning that some transactions require a completion to be sent back. DMA reads are requests that require a completion with data. As such, reads are affected by the number of hops in the data path between requester and completer; the longer the path, the higher the request-completion latency becomes. In addition, the PCIe data link layer uses a credit-based flow control algorithm. The number of requests in flight is limited by how many uncompleted transactions a PCIe requester is able to keep open. Since it is not allowed to send more than the maximum payload size at the time,[10](#fn10) a requester may need to split requests into several transactions. Longer paths can therefore reduce DMA performance, as the link becomes underutilized when the distance between device and memory increases.

> 7.1.3 更长的 PCIe 路径。PCIe 事务可以是发布或非发布操作，这意味着某些事务需要发送回一个完成。DMA 读取是需要带有数据的完成的请求。因此，读取受到请求者和完成者之间数据路径中跳数的影响；路径越长，请求完成延迟就越高。此外，PCIe 数据链路层使用基于信用的流量控制算法。正在飞行中的请求数量受 PCIe 请求者能够保持打开的未完成事务的数量的限制。由于不允许一次发送超过最大有效载荷大小，[10](＃fn10)请求者可能需要将请求拆分为几个事务。因此，路径越长，DMA 性能就越低，因为当设备和内存之间的距离增加时，链接会被低利用。

We used the bandwidthTest program described in Section [7.1.2](#sec-34) and a borrowed, remote Nvidia Quadro P4000 GPU. Figure [23](#fig23) shows the topologies used to evaluate the performance impact of increased PCIe paths. By increasing the distance between device and memory with an additional hop, namely, the Microsemi PM8536 PFX switch used internally in the MSX824 cluster switch, we can compare the performance to the Back-to-Back scenario. The hosts are running Ubuntu 18.02.2 with the 4.15 version of the Linux kernel. As with our previous tests, we used CUDA version 10.1.

> 我们使用了第 7.1.2 节描述的带宽测试程序和借来的远程 Nvidia Quadro P4000 GPU。图 23 显示了用于评估 PCIe 路径增加对性能影响的拓扑结构。通过增加设备和存储器之间的距离，即 MSX824 集群交换机内部使用的 Microsemi PM8536 PFX 交换机，我们可以将性能与 Back-to-Back 场景进行比较。主机运行的是 Ubuntu 18.02.2，内核版本为 4.15。与我们以前的测试一样，我们使用的是 CUDA 版本 10.1。

![](https://dl.acm.org/cms/attachment/0bca0c50-ce85-49df-b3f1-70f0d908a99c/tocs380102-02-f23.jpg) Fig. 23. By increasing the distance with a single hop, we are able to determine the impact of longer PCIe paths on DMA performance. DMA reads are particularly affected by the decreased link utilization.

Figure [23](#fig23)(c) shows the results of our test. As expected, the additional hop in the Cluster Switch scenario affects DMA performance. We can see that smaller writes are affected by the increased latency through the switch, because even small differences in delay impact the time it takes for transactions with data to arrive. However, this additional latency becomes less significant for larger writes, as the number of transactions in flight increases. We see that the throughput converges towards the Back-to-Back performance for transfers larger than 512 kB.

> 图 23(c)显示了我们测试的结果。正如预期的那样，集群交换场景中的额外跳跃会影响 DMA 性能。我们可以看到，通过交换增加的延迟会影响较小的写入，因为即使很小的延迟也会影响带有数据的事务到达的时间。但是，随着正在飞行中的事务数量的增加，这种额外的延迟变得不那么重要了。我们看到，当传输大于 512 kB 时，吞吐量收敛到背靠背性能。

DMA reads suffer noticeably from the increased distance. Unlike writes, which are posted transactions, the number of read requests simultaneously being held open is limited. Moreover, PCIe allows a completer to respond with less data at the time than is actually requested. For example, a read requesting 512 bytes may terminate with 2 completions containing 256 bytes each, rather than a single completion with all 512 bytes. This depends on the maximum payload size and maximum read request size, configured by the system. Since the time before completions arrive increases because of the longer distance between the GPU and system memory, the link becomes underutilized as there are fewer transactions in flight. We observe this as a drop in the measured throughput, as seen on the right-hand plot in Figure [23](#fig23)(c).

> DMA 读取受到距离增加的明显影响。与写入(它们是发布的交易)不同，同时打开的读取请求的数量是有限的。此外，PCIe 允许完成者以比实际请求的更少的数据响应。例如，请求 512 字节的读取可能以两个包含 256 字节的完成终止，而不是单个完成包含所有 512 字节。这取决于系统配置的最大有效载荷大小和最大读取请求大小。由于 GPU 和系统内存之间距离较远，完成前的时间增加，因此由于飞行中的交易较少，链接变得未利用。我们在图[23](c)右侧的绘图中观察到这一点，表现为测量吞吐量的下降。

7.1.4 _Peer-to-peer: Local vs. Remote._ In addition to enabling access to individual remote devices, Device Lending also supports creating groups of arbitrary devices and enabling direct peer-to-peer access between them (shortest-path routing). To show that the address resolving method described in Section [4.4](#sec-18) enables shortest-path routing and to demonstrate that relying on the borrower-side IOMMU does not disrupt peer-to-peer transactions on the lender, we have performed DMA throughput and latency tests using two Nvidia Quadro P4000 GPUs. The borrower uses CUDA 10.1 with the 418.39 version of the Nvidia driver, and both borrower and lender run Ubuntu 18.04.2 with the 4.15 version of the Linux kernel. The configurations of the tests are shown in Figure [24](#fig24):

> 7.1.4 对等网络：本地与远程。除了使远程设备可以访问之外，设备借用还支持创建任意设备组并使它们之间可以直接进行对等网络连接(最短路径路由)。为了证明第 4.4 节中描述的地址解析方法可以实现最短路径路由，并证明借方 IOMMU 的依赖不会影响出借方的对等网络交易，我们使用两块 Nvidia Quadro P4000 GPU 进行了 DMA 吞吐量和延迟测试。借方使用 CUDA 10.1 和 Nvidia 驱动程序 418.39 版本，借方和出借方均运行 Ubuntu 18.04.2，内核版本为 4.15。测试配置如图 24 所示：

![](https://dl.acm.org/cms/attachment/bf08887e-3d96-4f46-b55b-e925f5e7a069/tocs380102-02-f24.jpg) Fig. 24. Peer-to-peer throughput: We demonstrate that our Device Lending implementation supports shortest path routing by comparing peer-to-peer DMA performance. The IOMMU on the borrowing system does not affect traffic between borrowed devices.

- **Local Baseline**, shown in Figure [24](#fig24)(a): A local system using two local GPUs in an expansion chassis. We have disabled the IOMMU on the local CPU, to enable shortest path routing within the expansion chassis.

> **\*本地基准**，如图 24(a)所示：使用扩展机箱中的两个本地 GPU 的本地系统。我们已禁用本地 CPU 上的 IOMMU，以启用扩展机箱内的最短路径路由。

- **Device Lending**, shown in Figure [24](#fig24)(b): Two hosts connected together using Dolphin PXH830 NTB adapter cards. Note that we also use a Dolphin MXS824 PCIe cluster switch in this test. Even though the switch increases the distance between CPU and two GPUs, it does not matter in this test; we only measure traffic between the two GPUs. The IOMMU on the lender is disabled to allow shortest path routing. Since the GPUs used in our tests are unable to reach high I/O addresses, we configured the DMA window size to 1 GB and enabled the IOMMU on the borrower.

> **\*设备出借**，如图 24(b)所示：使用 Dolphin PXH830 NTB 适配器卡连接两个主机。请注意，我们在此测试中还使用了 Dolphin MXS824 PCIe 集群交换机。尽管交换机增加了 CPU 和两个 GPU 之间的距离，但在此测试中并不重要；我们只测量两个 GPU 之间的流量。出借方的 IOMMU 被禁用以允许最短路径路由。由于我们测试中使用的 GPU 无法访问高 I / O 地址，我们将 DMA 窗口大小设置为 1 GB，并在借方启用 IOMMU。

Figure [24](#fig24)(c) shows the result of using the CUDA bandwidthTest program to copy memory from one GPU to the other using the first GPU's on-board DMA engine. For each transfer size, we configured bandwidthTest to do 1,000 transfers. On the left, we show the median throughput, and we show the distribution as a min–max distance on the right. Note that GPU memory latency varies significantly more than RAM (as seen in Figure [22](#fig22)).

> 图 24(c)显示了使用第一个 GPU 的本地 DMA 引擎从一个 GPU 复制到另一个 GPU 的内存时使用 CUDA 带宽测试程序的结果。对于每个传输大小，我们配置带宽测试程序进行 1,000 次传输。在左边，我们显示中值吞吐量，并且在右边显示分布作为最小-最大距离。请注意，GPU 内存延迟比 RAM 变化更大(如图 22 所示)。

Using the same topologies as depicted in Figure [24](#fig24), we have also measured the latency of DMA writes between the two GPUs. We developed a small CUDA program to measure peer-to-peer latency, as depicted in Figure [25](#fig25)(a). One GPU is tasked with increasing a counter, writing it to the other GPU's memory and waiting for an acknowledgement. The other GPU waits for the counter to increase by one, and acknowledges the received counter by writing it back to the first GPU. The whole round-trip is measured by recording the current GPU clock cycle and dividing it by the clock frequency. We call the elapsed time of one cycle of DMA transfers back and forth the _ping–pong_ latency. For getting the clock cycles, we use the clock64() function. We measured that calling this function takes around 32 ns on the P4000 GPUs. We also measured that reading from the local memory pointer takes around 15 ns. While this skews the results somewhat, we argue that the skew should be identical for both scenarios.

> 使用与图 24 所示的相同拓扑结构，我们还测量了两个 GPU 之间的 DMA 写入延迟。我们开发了一个小的 CUDA 程序来测量点对点延迟，如图 25(a)所示。一个 GPU 负责增加计数器，将其写入另一个 GPU 的内存，并等待确认。另一个 GPU 等待计数器增加一个，并通过将其写回第一个 GPU 来确认接收到的计数器。通过记录当前 GPU 时钟周期并将其除以时钟频率，我们测量了整个往返时间。我们称这一次 DMA 传输的耗时为*ping-pong*延迟。为了获取时钟周期，我们使用 clock64()函数。我们测量到在 P4000 GPU 上调用此函数大约需要 32 ns。我们还测量到从本地内存指针读取大约需要 15 ns。虽然这会使结果受到一定的偏差，但我们认为这种偏差应该在两种情况下是相同的。

![](https://dl.acm.org/cms/attachment/f233b8d3-a412-4954-be16-f34a117a2c78/tocs380102-02-f25.jpg) Fig. 25. Peer-to-peer latency: by implementing a ping-pong program in CUDA, we can measure the latency of DMA writes between two GPUs. One GPU writes a 4-byte message to the other GPU's memory, before waiting for an acknowledgement and recording the time before and after (ping). The other GPU waits for the message and sends an acknowledgement back (pong).

Figure [25](#fig25)(b) shows the latency distributions for the Local Baseline and Device Lending scenarios for 100,000 ping-pong iterations each. As the distribution has three distinct “steps,” with no measurements falling in between, we present it as a set of percentiles rather than a histogram. We see that the distributions of throughput and latency measurements are similar for both scenarios, proving that there is no difference between local and remote. From this, we can conclude that our implementation supports shortest-path routing between two devices, without adding any overhead in the critical path.

> 图 25(b)显示了本地基准和设备借用场景的延迟分布，每个 100,000 次 ping-pong 迭代。由于分布具有三个不同的“步骤”，没有测量值落在其中，因此我们将其呈现为百分位数而不是直方图。我们看到，吞吐量和延迟测量的分布在两种场景中相似，证明本地和远程之间没有差异。从这里，我们可以得出结论，我们的实现支持两个设备之间的最短路由，而不会在关键路径中增加任何开销。

7.1.5 _Peer-to-peer: Multiple Lenders._ As described in Section [4.4](#sec-18), our Device Lending implementation also supports shortest-path routing between devices even when they reside in _different_ lender systems. By composing a PCIe infrastructure consisting of devices spread out over multiple hosts in the cluster, the PCIe device tree unavoidably becomes deeper. While this can potentially increase resource utilization significantly, we need to evaluate the performance impact of moving resources further away as each additional hop in the data path will slightly increase the latency.

> 7.1.5 点对点：多个出借方。正如在第 4.4 节中描述的，我们的设备出借实现也支持即使设备位于不同出借系统中也能够实现最短路径路由。通过构建一个由集群中分布在多个主机上的 PCIe 设备组成的 PCIe 基础架构，PCIe 设备树不可避免地变得更深。虽然这可能会显著提高资源利用率，但我们需要评估数据路径中每个额外跳跃所带来的性能影响，因为这会略微增加延迟。

By using the same peer-to-peer benchmarks described in the previous section, we have evaluated the impact of moving one of the GPUs to a third host. Figure [26](#fig26) illustrates the topologies of our comparison tests:

> 通过使用前面一节中描述的相同的点对点基准测试，我们评估了将其中一个 GPU 移动到第三个主机的影响。图[26](#fig26)描绘了我们比较测试的拓扑结构：

![](https://dl.acm.org/cms/attachment/ec2955b5-19e4-4c64-a571-a5d7e6eb21b6/tocs380102-02-f26.jpg) Fig. 26. Peer-to-peer throughput: We evaluate the impact of increasing the distance between the devices.

- **Same Lender**, shown in Figure [26](#fig26)(a): Using two GPUs from the same lender. As we established in the previous section, this scenario is similar to a local system using local devices.

> - **同一提供商**，如图 26(a)所示：使用来自同一提供商的两个 GPU。正如我们在前面一节中所建立的，这种情况与使用本地设备的本地系统类似。

- **Different Lenders**, shown in Figure [26](#fig26)(b): Using two GPUs from different lenders. DMA transactions have to traverse four additional hops (NTB, cluster switch, NTB, internal switch) compared to the baseline. We expect the additional latency to manifest itself as an observable performance difference when compared to the Same Lender scenario:

> 不同的贷款机构，如图 26(b)所示：使用来自不同贷款机构的两个 GPU。与基线相比，DMA 事务必须经过四个额外的跳跃(NTB，集群交换，NTB，内部交换)。我们预计，额外的延迟将表现为与相同贷款机构场景相比可观察的性能差异：

    *   The PEX8733 switch chip used in the PXH830 NTB adapters specifies that up to 132 ns may be added to a transaction in worst-case [[13](#Bib0013)].
    *   The internal PEX8796 chip used internally in the expansion chassis can add up to 150 ns to transactions in worst case [[14](#Bib0014)].
    *   Experiments in our lab show that the PM8536 PFX chip used internally in the MXS824 cluster switch adds an average latency of around 170 ns.

All hosts are running Ubuntu 18.04.2 with the 4.15 version of the Linux kernel. As before, the borrower is using CUDA 10.1 with the corresponding 418.39 version of the Nvidia CUDA driver.

> 所有主机都运行 Ubuntu 18.04.2，并使用 Linux 内核 4.15 版本。与以前一样，借用者使用 CUDA 10.1，并使用相应的 Nvidia CUDA 驱动程序 418.39 版本。

Figure [26](#fig26)(c) shows the result of running the CUDA bandwidthTest program, copying memory from one GPU to the other using the on-board DMA engine with different transfer sizes. Figure [27](#fig27)(b) show the ping–pong latency using the CUDA program we described earlier. While we observe that the additional distance affects the measured throughput and back-and-forth latency, this difference is less than the worst case. This is a strong indicating that our implementation does not add any additional latency beyond what we expect from the hardware. We argue that the added latency from increasing the distance between GPUs is a reasonable trade-off with regards to increasing device utilization. It is also possible to optimize for data movement by borrowing devices that are physically close to each other in terms of number of hops, thus minimizing the distance between them. Finally, we can observe that when conditions are comparable, i.e., the PCIe path is similar, the performance is the same. We argue that this demonstrates that our Device Lending implementation does not add any overhead. After all, the speed of electrons through the silicone of the hardware is beyond the scope of our implementation.

> 图 26(c)显示了使用不同传输大小从一个 GPU 到另一个 GPU 使用本地 DMA 引擎运行 CUDA 带宽测试程序的结果。图 27(b)显示了使用我们之前描述的 CUDA 程序的乒乓延迟。虽然我们观察到增加的距离会影响测量的吞吐量和往返延迟，但这个差异小于最坏的情况。这表明我们的实现没有增加任何额外的延迟，超出我们期望的硬件。我们认为，增加 GPU 之间的距离带来的额外延迟是一种合理的折衷，以提高设备利用率。也可以通过借用物理上彼此距离较近的设备来优化数据传输，从而最大限度地减少它们之间的距离。最后，我们可以观察到，当条件相似(即 PCIe 路径相似)时，性能是相同的。我们认为，这表明我们的设备借用实现不会增加任何开销。毕竟，硬件硅片中电子的速度超出了我们实现的范围。

![](https://dl.acm.org/cms/attachment/15c391cc-c61c-4262-a065-b70d210bbec2/tocs380102-02-f27.jpg) Fig. 27. Peer-to-peer latency: using a ping–pong CUDA program, we measure the latency of DMA writes between two GPUs residing in different hosts. While the additional hops increase the ping–pong latency, this is expected for longer PCIe paths.

7.1.6 _Sharing SR-IOV Devices._ As mentioned in Section [3.1](#sec-11), the term “device” actually refers to individual PCIe endpoints, or rather device functions. Some devices may implement SR-IOV, allowing a single device to virtualize multiple device functions in hardware. Each virtual function appears to the system as a separate device function with its own resources. Since our SmartIO system does not make any distinction between physical and virtual functions, it is possible to disaggregate an SR-IOV device and assign a virtual function to a remote host (without any performance penalty) the same way SmartIO distributes physical functions. Therefore, we have conducted experiments using a Mellanox ConnectX-5 100 Gigabit Ethernet adapter, which supports up to 1024 virtual functions [[81](#Bib0081)]. Each virtual function implements a (virtual) Ethernet controller. By generating high network throughput and comparing the performance of a virtual function to the performance of the physical function, for both a local system and a remote system using Device Lending, we argue that this will reveal any hidden performance overheads caused by our implementation that could affect hardware virtualization.

> 7.1.6 共享 SR-IOV 设备。正如第 3.1 节所提及的，“设备”实际上指的是单独的 PCIe 端点，或者更准确地说是设备功能。有些设备可以实现 SR-IOV，允许单个设备在硬件中虚拟多个设备功能。每个虚拟功能在系统中看起来都是一个单独的设备功能，拥有自己的资源。由于我们的 SmartIO 系统不区分物理功能和虚拟功能，因此可以将 SR-IOV 设备分解，并将虚拟功能分配给远程主机(没有任何性能损失)，就像 SmartIO 分发物理功能一样。因此，我们进行了实验，使用 Mellanox ConnectX-5 100 Gigabit Ethernet 适配器，它支持多达 1024 个虚拟功能[[81]](#Bib0081)。每个虚拟功能实现一个(虚拟)以太网控制器。通过产生高网络吞吐量并比较虚拟功能与本地系统和使用设备借用的远程系统的物理功能的性能，我们认为这将揭示我们的实现可能影响硬件虚拟化的任何隐藏性能开销。

To create network workload and generate network traffic, we have used the _iperf2_ tool. This tool is widely used for measuring network performance, and is available on most Linux distributions. iperf2 supports creating TCP data streams between a _client_, running on a local host, and a _server_, running on a remote host. The client writes as much data to the TCP stream as it is able to, and the server reads from the stream.[11](#fn11) In this respect, TCP is designed to provide a reliable data stream over a lossy IP network where the kernel is involved in encapsulating raw data into TCP segments and IP packets, managing transmission and receive buffers, handling retransmissions and flow control, and network congestion avoidance—all of which require CPU time. Therefore, to fully saturate a 100 Gigabit link without becoming CPU-bound, iperf2 supports spawning dedicated threads for each individual TCP connection on both the server and the client. Each individual thread can run on its own CPU core.

> 为了创建网络工作负载并生成网络流量，我们使用了*iperf2*工具。这个工具被广泛用于测量网络性能，并且可以在大多数 Linux 发行版上使用。iperf2 支持在本地主机上运行的*客户端*和远程主机上运行的*服务器*之间创建 TCP 数据流。客户端尽可能地将数据写入 TCP 流，服务器从流中读取数据。[11](＃fn11)从这个意义上说，TCP 旨在在具有损耗的 IP 网络上提供可靠的数据流，其中内核参与将原始数据封装到 TCP 段和 IP 数据包中，管理传输和接收缓冲区，处理重传和流量控制以及网络拥塞避免-这些都需要 CPU 时间。因此，为了充分饱和 100 Gigabit 链路而不会变得 CPU 绑定，iperf2 支持为服务器和客户端上的每个单独的 TCP 连接生成专用线程。每个单独的线程都可以在自己的 CPU 内核上运行。

Figure [28](#fig28) depicts the configuration used in these tests, where the client connects to the server running on the receiver host:

> 图[28](#fig28)描绘了这些测试中使用的配置，其中客户端连接到在接收主机上运行的服务器：

![](https://dl.acm.org/cms/attachment/cf74e8da-936f-43c8-91dd-2b7b6a9c287d/tocs380102-02-f28.jpg) Fig. 28. TCP throughput comparison: We compare the achieved throughput for a client/server application.

- **Local Baseline**, shown in Figure [28](#fig28)(a): A local system using its local network adapter to connect to the dedicated Receiver Host, running the iperf2 server. The iperf2 client is running on the local CPU. We ran one test using the adapter's physical function and one test using one of the adapter's virtual functions, to rule out any performance overhead caused by the virtualization.

> **\*本地基线**，如图 28(a)所示：使用本地网络适配器连接到专用接收主机，运行 iperf2 服务器。iperf2 客户端运行在本地 CPU 上。我们运行了一个使用适配器的物理功能的测试，以及一个使用适配器的虚拟功能的测试，以排除由虚拟化引起的任何性能开销。

- **Device Lending**, shown in Figure [28](#fig28)(b): A borrower using a remote network adapter to connect to the dedicated Receiver Host. As with our Local Baseline tests, we borrowed first the physical function and then the virtual function, to rule out any performance difference.

> **\*设备借用**，如图 28(b)所示：借用者使用远程网络适配器连接到专用接收主机。与我们的本地基线测试一样，我们首先借用了物理功能，然后借用了虚拟功能，以排除任何性能差异。

All hosts run Ubuntu 18.04 with the 4.15 version of the Linux kernel, using the in-kernel Mellanox Ethernet driver. To compare apples to apples, we have disabled the IOMMU on both lender and borrower, as well as on the receiver host. In all cases, the iperf2 client runs for a duration of five minutes, writing to the TCP streams and reports the throughput every half-second. The client and the server were configured to use four parallel connections, and, consequently, using four threads each. We relied on the default kernel scheduler to schedule threads on different CPU cores. We also experimented with various network related settings in the kernel, such as increasing buffer sizes and using alternative TCP congestion control mechanisms. Additionally, we tried different offloading mechanisms supported by the adapter. However, besides setting the Ethernet maximum transfer unit to 9,000 bytes (“jumbo frames”), the default 4.15 kernel settings and disabling all forms of offloading provided highest throughput.

> 所有主机都运行 Ubuntu 18.04，使用 Linux 内核 4.15 版本，并使用内核中的 Mellanox 以太网驱动程序。为了比较同等的水果，我们在出借者和借用者以及接收主机上都禁用了 IOMMU。在所有情况下，iperf2 客户端运行五分钟，写入 TCP 流并每半秒报告吞吐量。客户端和服务器都配置为使用四个并行连接，因此每个使用四个线程。我们依靠内核调度程序将线程调度到不同的 CPU 内核上。我们还尝试了内核中的各种网络相关设置，例如增加缓冲区大小并使用替代的 TCP 拥塞控制机制。此外，我们还尝试了适配器支持的各种卸载机制。但是，除了将以太网最大传输单元设置为 9,000 字节(“大框架”)外，使用默认的 4.15 内核设置和禁用所有形式的卸载提供了最高的吞吐量。

Figure [28](#fig28)(c) shows the throughput measurements of our comparison, with performance for a physical function shown on the left (PF), and performance for a virtual function on the right (VF). Note that while it is common to describe network performance in terms of Giga*bits*, we have plotted performance in terms Giga*bytes* to be consistent throughout this article. By comparing the performance of these functions being used locally (Local Baseline) and remote (Device Lending), we prove that accessing a borrowed virtual function does not introduce any performance overhead. Additionally, we also observe that for the Mellanox adapter used in this experiment, there is no measurable difference when using a virtual function compared to using a physical function.

> 图 28(c)显示了我们的比较的吞吐量测量，左边(PF)显示物理功能的性能，右边(VF)显示虚拟功能的性能。请注意，尽管通常使用 Giga*bits*来描述网络性能，但我们已经绘制了以 Giga*bytes*为单位的性能，以便在本文中保持一致。通过比较本地(本地基线)和远程(设备借用)使用这些功能的性能，我们证明访问借用的虚拟功能不会引入任何性能开销。此外，我们还观察到，对于本实验中使用的 Mellanox 适配器，使用虚拟功能与使用物理功能相比没有可测量的差异。

Moreover, multiple hosts can share the same device by distributing individual virtual functions. Since most SR-IOV-capable devices support several virtual functions, this becomes highly useful with regard to our SmartIO system. To demonstrate this, we have performed an additional test where the lender and the borrower share the same sender-side network adapter simultaneously, to transmit data to the receiving server. Figure [29](#fig29)(a) shows the topology of this multi-host sharing test. We configured two virtual functions for the network adapter and assigned them to the two hosts: One function is used locally by the lender, and we run an iperf2 client on the lender with two parallel connections (threads) to the iperf2 server (Client on Lender). The other function is used simultaneously by the borrower, and we run an iperf2 client on the borrower as well, also using two threads (Client on Borrower).

> 此外，多个主机可以通过分配单独的虚拟功能来共享同一设备。由于大多数 SR-IOV 兼容设备支持多个虚拟功能，因此对于我们的 SmartIO 系统非常有用。为了证明这一点，我们进行了一个额外的测试，其中出借人和借用人同时共享同一发送端网络适配器来向接收服务器传输数据。图 29(a)显示了这个多主机共享测试的拓扑结构。我们为网络适配器配置了两个虚拟功能，并将它们分配给两个主机：一个功能由出借人本地使用，我们在出借人上运行一个 iperf2 客户端，使用两个并行连接(线程)连接到 iperf2 服务器(出借人客户端)。另一个功能同时由借用人使用，我们也在借用人上运行一个 iperf2 客户端，也使用两个线程(借用人客户端)。

![](https://dl.acm.org/cms/attachment/d8f86e34-4bb3-42c1-895c-d275dc008722/tocs380102-02-f29.jpg) Fig. 29. Two hosts using the same SR-IOV-capable network adapter simultaneously.

Figure [29](#fig29)(b) shows the results of our multi-host test, where we have plotted the reported throughput for both clients. The server's reported received data rate, which is the combined rate of the two clients, is also shown. While throughput for the two clients fluctuate a little, they approach the same throughput over time (as can also be seen by comparing the mean throughput). This is expected behavior for TCP streams, as they alternate between increasing transmission rate in an attempt to estimate the available network bandwidth, and backing off when they exceed their fair share of the total capacity.

> 图 29(b)显示了我们多主机测试的结果，我们绘制了两个客户端的报告吞吐量。服务器报告的接收数据速率，即两个客户端的组合速率，也显示出来。虽然两个客户端的吞吐量有一点波动，但它们随着时间的推移接近相同的吞吐量(也可以通过比较平均吞吐量来看出)。这是 TCP 流期望的行为，因为它们在试图估计可用网络带宽的同时，会在增加传输速率之间交替，并在超过总容量的公平份额时减速。

Finally, it should also be mentioned that sharing the Mellanox network adapter does not only provide connectivity to the receiver host for both lender and borrower, but it also becomes possible for the lender and borrower to establish IP connections to _each other_ as well. In a larger PCIe cluster, this could be useful for IP network applications that could communicate with each other, using only a single network adapter and without sending a single packet out on the Ethernet link.

> 最后，还应提及，共享 Mellanox 网络适配器不仅为出借者和借用者提供接收主机的连接，而且还可以使出借者和借用者之间建立 IP 连接。在更大的 PCIe 集群中，这对于只使用一个网络适配器并且不在以太网链路上发送任何数据包的 IP 网络应用程序来说是非常有用的，它们可以彼此通信。

### 7.2 Scaling Heavy Workloads

Another method of demonstrating that there is no hidden overhead in our Device Lending implementation, is investigating how it behaves under stress. It might be the case that there are small overheads caused by the implementation that only become visible when the system is under heavy load. Because of this, we have also designed an experiment using a realistic GPU-intensive machine learning workload, to prove that Device Lending is a solution for composable and disaggregated PCIe infrastructure suitable for real-world applications.

> 另一种证明我们的设备借用实施中没有隐藏开销的方法是调查它在压力下的行为。可能是实施中会产生小的开销，只有在系统处于重负载时才会变得可见。因此，我们还设计了一个使用真实的 GPU 密集型机器学习工作负载的实验，以证明设备借用是一种适用于真实应用程序的可组合和分散的 PCIe 基础设施解决方案。

Our workload is a typical convolutional neural network training using the Python machine learning framework Keras [[1](#Bib0001)]. Keras is a high level framework that wraps different lower level machine learning frameworks. In our case, Keras uses Tensorflow [[2](#Bib0002)] as its back-end. Keras also allows multiple GPUs to work together, by replicating the machine learning model being trained on each of the GPUs, and splitting the model's inputs into “sub-batches” and distributing them on the GPUs. When the GPUs are done, the sub-batches are concatenated on the CPU into one batch. This introduces quasi-linear speed-up. We used Python 3.6 and Keras 2.2.4, running on Ubuntu 16.04 (4.9 kernel) with CUDA 9.0 and cuDNN 7.1 in our tests.

> 我们的工作负载是使用 Python 机器学习框架 Keras [[1](#Bib0001)] 进行典型的卷积神经网络训练。Keras 是一个高级框架，封装了不同的低级机器学习框架。在我们的例子中，Keras 使用 Tensorflow [[2](#Bib0002)]作为其后端。Keras 还允许多个 GPU 一起工作，通过在每个 GPU 上复制正在训练的机器学习模型，并将模型的输入分割成“子批次”并分发到 GPU 上。当 GPU 完成时，子批次将在 CPU 上合并成一个批次。这引入了准线性加速。我们在测试中使用 Python 3.6 和 Keras 2.2.4，在 Ubuntu 16.04(4.9 内核)上运行 CUDA 9.0 和 cuDNN 7.1。

We wrote a program that trains available models in Keras on given datasets with given hyperparameters using transfer learning [[57](#Bib0057)]. In our case, we use a VGG19 [[76](#Bib0076)] model that is pre-trained on the ImageNet dataset [[20](#Bib0020)], and the model was re-trained using an 8-classes image dataset of the gastroaintestinal tract called Kvasir [[30](#Bib0030), [63](#Bib0063), [64](#Bib0064)] to perform disease classification [[65](#Bib0065)].

> 我们编写了一个程序，使用传输学习[[57](#Bib0057)]在 Keras 中使用给定的数据集和给定的超参数训练可用的模型。在我们的例子中，我们使用在 ImageNet 数据集[[20](#Bib0020)]上预先训练的 VGG19[[76](#Bib0076)]模型，并使用名为 Kvasir 的 8 类图像数据集[[30](#Bib0030), [63](#Bib0063), [64](#Bib0064)]重新训练模型以执行疾病分类[[65](#Bib0065)]。

We measure the runtime of 12 epochs of the model training on two Nvidia P4000 GPUs as well as loading images from storage and writing the results back using an Intel Optane P4800X NVMe device. While 12 epochs may not give the statistical significance needed for reliable machine learning results, we are only interested in system performance. Both GPUs and the NVMe were used in all scenarios. Figure [30](#fig30) shows the scenarios and results of our experiment:

> 我们在两块 Nvidia P4000 GPU 上测量了模型训练的 12 个时期的运行时间，以及使用 Intel Optane P4800X NVMe 设备从存储中加载图像并将结果写回。虽然 12 个时期可能不足以获得可靠机器学习结果所需的统计学意义，但我们只对系统性能感兴趣。所有场景都使用 GPU 和 NVMe。图 30 显示了我们实验的场景和结果：

![](https://dl.acm.org/cms/attachment/24b56275-e8d5-4154-8520-4df92cd30adc/tocs380102-02-f30.jpg) Fig. 30. Scaling heavy workloads: We demonstrate the usability of SmartIO for composable and disaggregated PCIe infrastructure, by comparing the performance of running a GPU-intensive machine learning workload on a local system using local devices to Device Lending using remote devices. As data is moved between the GPUs, the increased distance between them affects the total runtime. However, we can see that when the devices reside in the same host, our Device Lending implementation does not add any measurable overhead.

- **Local Devices**, shown in Figure [30](#fig30)(a): A local system using both GPUs and the NVMe device locally. This scenario serves as our baseline comparison. The IOMMU is disabled, to allow peer-to-peer transactions between the GPUs.

> - **本地设备**，如图 30(a)所示：本地系统使用 GPU 和 NVMe 设备。此场景作为我们的基准比较。IOMMU 被禁用，以允许 GPU 之间的对等交易。

- **Single Lender** (not depicted): A borrowing system connected back-to-back and accessing all three devices remotely. The number of hops in the path is similar to the Local Devices scenario. The IOMMU on the lender is disabled, while it is enabled on the borrower to shrink the DMA window size down to 1 GB. We can see from the results in Figure [30](#fig30)(c) that this scenario achieves approximately the same epoch runtimes as the local comparison scenario, demonstrating that there is no hidden overhead in our Device Lending implementation.

> **單一出借方**(未示出)：一個背對背連接的借貸系統，可遠程訪問所有三個設備。路徑中的跳數與本地設備情況相似。出借方的 IOMMU 被禁用，而在借方上啟用 IOMMU 以將 DMA 窗口大小縮小到 1 GB。從圖[30](c)中的結果可以看出，此情況實現的時代運行時間與本地比較情況相當，證明我們的設備出借實現中沒有隱藏的開銷。

- **Two Lenders**, shown in Figure [30](#fig30)(b): A borrowing system accessing devices from two separate lenders. The IOMMU on the borrower is enabled, while it is disabled on both lenders. As the GPUs reside in different hosts, the path between them increases. This appears to slightly affect the epoch runtimes, as seen in Figure [30](#fig30)(c).

> 两个出借者，如图 30(b)所示：一个借用系统从两个不同的出借者访问设备。借用者的 IOMMU 已启用，而两个出借者的 IOMMU 都已禁用。由于 GPU 位于不同的主机上，它们之间的路径增加了。如图 30(c)所示，这似乎略微影响了时代的运行时间。

Our machine learning workload proves it is possible to use Device Lending for realistic workloads in a PCIe cluster, dynamically creating configurations of both local and remote devices and accessing them without any performance penalty beyond what is expected for longer PCIe paths. We argue that this effectively demonstrates the capacity of our implementation for creating a disaggregated PCIe infrastructure that supports dynamic scaling of devices that are distributed in the cluster.

> 我们的机器学习工作负载证明，可以在 PCIe 集群中使用设备借用来进行现实工作负载，动态创建本地和远程设备的配置，并且在预期的更长的 PCIe 路径之外没有任何性能损失地访问它们。我们认为，这有效地证明了我们的实现能够创建一个分散的 PCIe 基础架构，支持分布在集群中的设备的动态扩展。

### 7.3 VM Pass-through with MDEV

While VFIO pass-through enables direct access to _local_ physical devices from a VM guest, our MDEV pass-through mechanism enables direct access to _remote_ devices. However, our MDEV extension to KVM requires the use of an IOMMU on the lender to map the device into the same guest-physical address space as the VM as explained in Section [5.2](#sec-21). This effectively disables shortest-path routing in the fabric, as transactions must be forwarded through the CPU on the lender in order for the IOMMU to resolve virtual addresses to physical addresses. Intuitively, we expect this to cause some performance degradation.

> 当 VFIO 直通使得虚拟机客户端能够直接访问本地物理设备时，我们的 MDEV 直通机制使得客户端能够直接访问远程设备。然而，我们的 MDEV 扩展需要在提供者上使用 IOMMU 来将设备映射到与虚拟机相同的客户端物理地址空间，如[5.2]节所述。这实际上会禁用结构中的最短路由，因为交易必须通过提供者上的 CPU 来转发，以便 IOMMU 解析虚拟地址到物理地址。直观地说，我们预计这会造成一些性能降低。

7.3.1 _IOMMU Performance Penalty._ Processor designs are complex and often not well-documented, making it difficult to determine what exactly happens with memory transactions in progress once they leave the PCIe root complex and enter the CPU. Memory transactions may be buffered while awaiting IOMMU translations, or the IOMMU may need to perform a multi-level table look up for resolving addresses.

> 7.3.1 IOMMU 性能惩罚。处理器设计复杂，通常没有得到很好的文档说明，这使得很难确定内存事务一旦离开 PCIe 根复合体进入 CPU 后究竟发生了什么。内存事务可能在等待 IOMMU 转换时被缓冲，或者 IOMMU 可能需要进行多级表查找来解析地址。

To distinguish between overhead caused by our software implementation and any overhead caused by the hardware address virtualization, we compare the performance of the MDEV implementation to bare-metal performance using Device Lending. As described in Section [4.3](#sec-17), Device Lending includes optional IOMMU support allowing us to isolate the performance penalty of enabling the IOMMU. As such, this establishes a baseline we can compare our MDEV implementation with. Note that our exhaustive evaluations of Device Lending presented in Section [7.1](#sec-32) demonstrate that the Device Lending mechanism does not add _any_ performance overhead compared to native access. Therefore, we argue that Device Lending a valid bare-metal comparison to our MDEV implementation to reveal any overhead caused by MDEV.

> 为了区分由我们的软件实现引起的开销和由硬件地址虚拟化引起的开销，我们比较了 MDEV 实现的性能与设备借用的裸机性能。如第 4.3 节所述，设备借用包括可选的 IOMMU 支持，允许我们隔离启用 IOMMU 时的性能损失。因此，这建立了我们可以将我们的 MDEV 实现与之进行比较的基线。请注意，我们在第 7.1 节中对设备借用机制的详尽评估表明，与原生访问相比，设备借用机制不会增加任何性能开销。因此，我们认为设备借用是一种有效的裸机比较，可以揭示 MDEV 引起的任何开销。

Two hosts are connected back-to-back with Dolphin PXH830 NTB adapters, and we use the same One Stop Systems expansion chassis as our previous tests. We installed an Nvidia Tesla K40c GPU alongside the NTB adapter in the chassis. The expansion chassis is connected upstream using Dolphin MXH832 host and MXH833 target transparent adapters. By turning the IOMMU on the lender on and off, we are able to compare the performance difference of address virtualization on peer-to-peer DMA transfers over the NTB. By using the expansion chassis, we are able to create a worst-case scenario for enabling the IOMMU, as the distance between the devices and the CPU increases. Figure [31](#fig31)(a) depicts the three scenarios compared in this evaluation:

> 两个主机使用 Dolphin PXH830 NTB 适配器连接在背面，我们使用与以前测试相同的 One Stop Systems 扩展机箱。我们在机箱中安装了一个 Nvidia Tesla K40c GPU 和 NTB 适配器。扩展机箱使用 Dolphin MXH832 主机和 MXH833 目标透明适配器上游连接。通过打开和关闭放债者的 IOMMU，我们可以比较 NTB 上的地址虚拟化对点对点 DMA 传输的性能差异。通过使用扩展机箱，我们可以为启用 IOMMU 创建最坏的情况，因为设备和 CPU 之间的距离增加。图[31](a)描述了本次评估中比较的三种情况：

![](https://dl.acm.org/cms/attachment/8c091f93-2d35-468b-b8a2-56af93af0739/tocs380102-02-f31.jpg) Fig. 31. IOMMU performance penalty: By using the IOMMU on the lender, shortest path routing is disrupted.

- **Bare-metal No-IOMMU**, where we use Device Lending to facilitate direct access to the remote GPU. The IOMMU on the lender is turned off to enable shortest-path routing within the expansion chassis. Since the GPU is unable to reach high I/O addresses, we enabled the borrower-side IOMMU and configured the DMA window size to 512 MB. We also made sure that the bandwidthTest program ran with the same CPU core affinity as the local NTB adapter.

> **无金属裸机无 IOMMU**，我们使用设备借用来实现对远程 GPU 的直接访问。借方的 IOMMU 被关闭以实现扩展机箱内的最短路径路由。由于 GPU 无法访问高 I/O 地址，我们启用了借方 IOMMU 并将 DMA 窗口大小设置为 512 MB。我们还确保带宽测试程序与本地 NTB 适配器具有相同的 CPU 核心亲和性。

- **Bare-metal IOMMU** is similar to the No-IOMMU scenario in every way, except that lender-side IOMMU is enabled. By using the lender's IOMMU, we are able to configure larger DMA windows while still setting up mappings over the NTB for the GPU using low addresses. Note that since we are using the expansion chassis, this becomes the aforementioned worst-case scenario for Device Lending; all transactions must be routed towards the lender's CPU so that the IOMMU can resolve virtual I/O addresses. As with the No-IOMMU scenario, we made sure to run the bandwidthTest program with the same CPU core affinity as the local adapter.

> **\*基础金属 IOMMU**与 No-IOMMU 场景除了启用出借方 IOMMU 外，在其他方面都是相似的。通过使用出借方的 IOMMU，我们可以配置更大的 DMA 窗口，同时使用低地址为 GPU 设置 NTB 映射。请注意，由于我们使用了扩展机箱，这就成为了设备出借的最坏情况；所有交易都必须路由到出借方的 CPU，以便 IOMMU 可以解析虚拟 I/O 地址。与 No-IOMMU 场景一样，我们确保使用与本地适配器相同的 CPU 核心亲和性运行带宽测试程序。

- **MDEV:** We also installed Qemu 2.10.1 on the local host and configured it to use the KVM hypervisor. Using our MDEV extension to KVM, we borrow and “pass through” the GPU to the VM guest, enabling direct hardware access to the guest driver. The VM was configured to have 4 GB memory, and we used 2 MB “huge pages” on the host. Our MDEV implementation probes the VM for low and high guest physical memory dynamically, and sets up respective DMA windows. Because of this, we need to configure the NTB BAR size to be larger than the VM memory. Finally, we also made sure that Qemu ran with the same CPU core affinity as the local NTB adapter.

> 我们还在本地主机上安装了 Qemu 2.10.1，并将其配置为使用 KVM 虚拟化程序。使用我们的 MDEV 扩展到 KVM，我们借用并“传递”GPU 到 VM 客户端，使客户端驱动程序可以直接访问硬件。VM 被配置为具有 4 GB 内存，我们在主机上使用 2 MB“巨大页面”。我们的 MDEV 实现动态探测 VM 的低端和高端物理内存，并设置相应的 DMA 窗口。因此，我们需要配置 NTB BAR 大小大于 VM 内存。最后，我们还确保 Qemu 以与本地 NTB 适配器相同的 CPU 核心亲和性运行。

We installed Ubuntu 16.04 with the 4.10 version of the Linux kernel on both machines, as well as the guest OS in the VM. Although Device Lending is currently only supported on Linux, any guest OS would have been possible, including Microsoft Windows. However, we chose to use same version of Linux as both host and guest OS, to run as similar software as possible in all scenarios. CUDA version 9 was installed on the local host and in the VM guest. We used the bandwidthTest program described in Section [7.1](#sec-32), to measure the throughput of DMA writes and DMA reads to system memory using the GPU's own on-board DMA engine. As with our previous evaluations, bandwidthTest was configured to do 1,000 iterations for each transfer size from 4 kB to 128 MB.

> 我们在两台机器上安装了 Ubuntu 16.04，并使用 Linux 内核 4.10 版本，以及虚拟机中的客户端操作系统。尽管设备借用目前仅支持 Linux，但任何客户端操作系统都是可能的，包括 Microsoft Windows。但是，我们选择使用相同版本的 Linux 作为主机和客户端操作系统，以便在所有场景中尽可能地运行相似的软件。在本地主机和虚拟机客户端上安装了 CUDA 版本 9.我们使用[7.1](#sec-32)节中描述的 bandwidthTest 程序，来测量使用 GPU 自带的 DMA 引擎对系统内存的 DMA 写入和 DMA 读取的吞吐量。与我们以前的评估一样，bandwidthTest 被配置为对每个传输大小从 4 kB 到 128 MB 进行 1,000 次迭代。

Figure [31](#fig31)(b) shows the median DMA read and write throughput for all three scenarios. We observe that the throughput drops significantly when the IOMMU is enabled, particularly for reads (drops from 10.2 GB/s to just a little over 1.5 GB/s. There are two primary reasons for this significant performance drop:

> 图 31(b)显示了所有三种情况下的 DMA 读写中值吞吐量。当启用 IOMMU 时，我们观察到吞吐量显著下降，特别是读取(从 10.2 GB/s 下降到 1.5 GB/s 以下)。这种显着性能下降有两个主要原因：

1.  Reads suffer particularly from the increased distance, as addresses are routed through the lender's CPU _twice_ per transaction; the first time in order for the IOMMU to translate the addresses of the read requests, and the second time for completions with the requested data.

> 读取受到距离增加的尤其严重的影响，因为每笔交易地址都要经过贷方的 CPU 两次；第一次是为了 IOMMU 能够翻译读取请求的地址，第二次是为了完成请求的数据。

2.  By using a PCIe tracer, similar in concept to that of network packet tracers, we were able to investigate what the actual transactions look like on the fabric. By first using the tracer in the GPU slot, and then in the lender-side NTB slot, we were able to observe that the transactions are modified by the Intel Xeon CPU used in our test; the GPU requests 256 bytes per request, but each request is emitted as 4 ×$\times$ 64 byte requests on the other side of the IOMMU. As the CPU is only able to keep a limited number of non-posted requests open at the same time, splitting up read requests into multiple smaller requests leads to very poor link utilization.

> 使用类似于网络数据包跟踪器的 PCIe 跟踪器，我们能够调查织物上实际交易的情况。首先在 GPU 插槽中使用跟踪器，然后在放贷方 NTB 插槽中使用，我们能够观察到，我们测试中使用的 Intel Xeon CPU 修改了交易；GPU 每次请求 256 字节，但每次请求在 IOMMU 的另一端被发出为 4×64 字节的请求。由于 CPU 只能同时保持有限数量的非发布请求，将读取请求分割成多个较小的请求会导致链路利用率非常低。

Regardless, by comparing the bare-metal scenario with the IOMMU enabled to MDEV, we observe that the performance of DMA transfers is almost identical for both scenarios. While the performance drops because of the increased paths and IOMMU address translation, our results indicate that our MDEV implementation does not add any overhead on top of the hardware virtualization.

> 无论如何，通过将裸机场景与启用 IOMMU 的 MDEV 进行比较，我们发现 DMA 传输的性能几乎相同。尽管由于增加的路径和 IOMMU 地址转换而导致性能下降，但我们的结果表明，我们的 MDEV 实现不会在硬件虚拟化之上增加任何开销。

7.3.2 _Pass-through Comparison._ We have also repeated the same peer-to-peer benchmarks described in Section [7.1](#sec-32) using VMs. By using the peer-to-peer benchmarks to measure throughput and latency between two GPUs, we are able to compare our MDEV extension using remote devices to “normal” VFIO pass-through on a local system.

> 我们还使用虚拟机重复了[7.1](#sec-32)中描述的同等对等基准测试。通过使用对等基准测试来测量两个 GPU 之间的吞吐量和延迟，我们能够将我们使用远程设备的 MDEV 扩展与“正常”的本地系统上的 VFIO 透传进行比较。

Figure [32](#fig32) shows the topologies used in our comparison evaluation:

> 图[32](#fig32)展示了我们比较评估中使用的拓扑结构：

![](https://dl.acm.org/cms/attachment/1c66b41c-98c3-4100-bea1-402e27310de2/tocs380102-02-f32.jpg) Fig. 32. Peer-to-peer topologies: We compare the measured throughput and latency between two GPUs passed through to a VM using local VFIO pass-through to using our MDEV pass-through of remote devices. Note that we have also included a configuration using bare-metal Device Lending.

- **Local VFIO**, shown in Figure [32](#fig32)(a): A Qemu 2.10.1 instance running on a local system using the KVM hypervisor. By using VFIO, we pass-through two local Nvidia Tesla K40c GPUs. The local IOMMU is enabled, in order for KVM to map the devices into the same guest-physical address space as the VM. The guest OS is Ubuntu 6.04 with the 4.10 version of the Linux kernel, and we are using CUDA version 9. The host OS is Fedora 29 using the 4.18 version of the kernel.

> **本地 VFIO**，如图 32(a)所示：使用 KVM 虚拟化器在本地系统上运行的 Qemu 2.10.1 实例。通过使用 VFIO，我们通过两个本地 Nvidia Tesla K40c GPU。启用本地 IOMMU，以便 KVM 将设备映射到与 VM 相同的客户端物理地址空间中。客户端操作系统是 Ubuntu 6.04，内核版本为 4.10，我们使用的是 CUDA 9.0 版本。主机操作系统是 Fedora 29，内核版本为 4.18。

- **MDEV**, shown in Figure [32](#fig32)(b): A Qemu 2.10.1 instance using the KVM hypervisor and our MDEV extension to borrow and pass-through two remote GPUs from the lender. We used the same OS image for the VM as the VFIO scenario, and Fedora 29 on the hosts. The lender's IOMMU is enabled, as is required by MDEV.

> - **MDEV**，如图 32(b)所示：使用 KVM 虚拟化器和我们的 MDEV 扩展的 Qemu 2.10.1 实例，从出借方借用并通过两个远程 GPU。我们为 VM 使用了与 VFIO 场景相同的操作系统映像，而主机上使用的是 Fedora 29。出借方的 IOMMU 已启用，这是 MDEV 所必需的。

- **Bare-metal**, shown in Figure [32](#fig32)(b): We also include a bare-metal baseline, running bandwidthTest natively on a bare-metal machine using Device Lending. Two remote GPUs are borrowed by a bare-metal machine. The bare-metal borrower machine boots the same OS image as we used for our VMs. On the lender, we ran Fedora 29. The lender's IOMMU is enabled, to make the data path comparable to MDEV.

> - **裸金属**，如图[32](#fig32)(b)所示：我们还包括一个裸金属基线，在裸金属机器上使用设备借用服务原生运行带宽测试。两个远程 GPU 由裸金属机器借用。裸金属借用机器启动的操作系统映像与我们用于虚拟机的相同。在出借方，我们运行 Fedora 29。出借方的 IOMMU 已启用，以使数据路径与 MDEV 相当。

Both VM instances were configured with 4 GB memory, and we enabled 2 MB huge pages on the host. We also set the CPU affinity to be the same as the local adapter in both the bare-metal and MDEV scenarios.

> 两个虚拟机实例均配置了 4 GB 内存，并且我们在主机上启用了 2 MB 的大页面。我们还将 CPU 亲和性设置为与裸机和 MDEV 场景中的本地适配器相同。

Like before, we configured bandwidthTest to copy memory from one GPU to another using transfer sizes from 4 kB to 128 MB. Figure [33](#fig33)(a) shows the median throughput (left) and throughput distribution as a min–max distance (right). Each transfer size is repeated 1,000 times, and we have marked measurements below the 0.2th percentile as outliers. We observe that the local VFIO pass-through scenario reports a slightly higher throughput than both our MDEV implementation and the bare-metal comparison(!) for smaller transfer sizes.

> 就像以前一样，我们配置了带宽测试，使用 4 KB 到 128 MB 的传输大小从一个 GPU 复制到另一个 GPU。图[33](a)显示了中位吞吐量(左)和作为最小-最大距离的吞吐量分布(右)。每个传输大小重复 1,000 次，我们将 0.2 百分位以下的测量结果标记为异常值。我们观察到，本地 VFIO 直通场景报告的吞吐量比我们的 MDEV 实现和裸机比较略高，对于较小的传输大小。

![](https://dl.acm.org/cms/attachment/9d1ec325-c54e-40e0-9571-6fb03dc8758f/tocs380102-02-f33.jpg) Fig. 33. Peer-to-peer evaluation: Using the same bandwidthTest and ping–pong CUDA programs as previous evaluations, we measure both throughput and latency of DMA writes between two GPUs. Our MDEV implementation does not add any overhead compared to bare-metal.

In order for the GPU to notify the host driver that the DMA transfer is complete, it relies on interrupts. The bandwidthTest program measures throughput by initiating a memory copy (DMA transfer) and recording the time elapsed until the transfer is complete. As KVM uses a different mechanism for notifying the VM guest about an interrupt for VFIO pass-through devices than our MDEV implementation, we speculate that interrupts raised by VFIO pass-through devices may cause KVM to briefly suspend the execution of Qemu to handle the interrupt and signal _eventfd_ events. This would in turn would affect timing measurements by software running in the VM. However, as the measured throughput converge for all three scenarios when the transfer size increases, this suspected measurement discrepancy seems to become less significant.

> 为了让 GPU 通知主机驱动程序 DMA 传输已完成，它依赖于中断。bandwidthTest 程序通过启动内存复制(DMA 传输)并记录传输完成所用的时间来测量吞吐量。由于 KVM 使用不同的机制来通知 VM 客户机有关 VFIO 透传设备的中断，而我们的 MDEV 实现则不同，我们推测 VFIO 透传设备引发的中断可能会导致 KVM 短暂暂停 Qemu 的执行以处理中断并触发*eventfd*事件。这反过来又会影响 VM 中软件的时间测量。然而，由于传输大小增加时所测得的吞吐量在所有三种情况下都会收敛，这种潜在的测量差异似乎变得不那么重要了。

Figure [33](#fig33)(c) shows the distribution ping–pong latency measurements using the CUDA program we described in Section [7.1.4](#sec-36), where two GPUs writes a counter back and forth to each other's memory. The maximum measurement for MDEV appears to be an outlier, so we have annotated the 99.99th percentiles instead. The distributions for MDEV and bare-metal are similar, indicating that our MDEV implementation does not add any additional overhead beyond hardware virtualization. Unlike the bandwidthTest program, which uses device interrupts for synchronizing timing measurements, the ping–pong measurements use elapsed clock cycle for recording time (as described in Section [7.1.4](#sec-36)). With this method, it appears that the strange effect where VFIO performs better than bare-metal is not present, which strengthens our suspicion that it is related to delivering interrupts to the VM.

> 图 33(c)展示了使用我们在第 7.1.4 节描述的 CUDA 程序进行的 Ping-Pong 延迟测量的分布情况，其中两个 GPU 互相写入对方的内存中的计数器。MDEV 的最大测量值似乎是一个异常值，因此我们注释了 99.99 百分位数。MDEV 和裸机的分布相似，表明我们的 MDEV 实现除了硬件虚拟化之外不会增加任何额外开销。与使用设备中断来同步定时测量的带宽测试程序不同，Ping-Pong 测量使用经过的时钟周期来记录时间(如第 7.1.4 节所述)。使用这种方法，似乎 VFIO 表现比裸机更好的奇怪效果不再存在，这加强了我们怀疑它与向虚拟机传递中断有关的怀疑。

### 7.4 Distributed NVMe Driver Evaluation

Our Device Lending and MDEV extension make it possible for a local device driver to operate a remote device in a manner that is fully transparent to both device and driver. This is possible as we prepare memory mappings in advance and inject addresses that map over the respective NTBs. However, as the physical memory allocated by a device driver or a VM instance is outside of our control, we are forced to map _all_ of local memory for a remote device. As we have seen in Sections [7.1.4](#sec-36) and [7.1.5](#sec-37), increasing the distance PCIe transactions has to travel has an impact on performance. Particularly non-posted transactions, such as reads, are affected by longer distance between requester and completer. In other words, increasing the distance between the borrower and the device will negatively impact performance, as the distance between the device and the memory it accesses also increases.

> 我们的设备借用和 MDEV 扩展使得本地设备驱动程序可以以对设备和驱动程序完全透明的方式操作远程设备。这是可能的，因为我们提前准备内存映射，并注入映射到相应 NTB 的地址。但是，由设备驱动程序或虚拟机实例分配的物理内存在我们的控制之外，我们被迫为远程设备映射*all*本地内存。正如我们在[7.1.4](＃sec-36)和[7.1.5](＃sec-37)中所看到的，增加 PCIe 事务必须旅行的距离会对性能产生影响。特别是非发布事务，如读取，受到请求者和完成者之间距离较长的影响。换句话说，借用者和设备之间距离的增加将对性能产生负面影响，因为设备与其访问的内存之间的距离也会增加。

However, a programmer can fully exploit shared memory capabilities in PCIe clusters by using the SISCI API [[22](#Bib0022)]. Local memory may be exported for other nodes, and remote memory can be mapped for the local application. It is even possible for a node to allocate memory buffers on local devices, such as GPUDirect-capable GPUs, and other nodes to map this memory through their own NTBs.

> 然而，程序员可以通过使用 SISCI API [[22](#Bib0022)]充分利用 PCIe 集群中的共享内存功能。本地内存可以导出给其他节点，远程内存可以映射到本地应用程序。甚至可以让一个节点在本地设备(如具有 GPUDirect 功能的 GPU)上分配内存缓冲区，其他节点可以通过自己的 NTB 映射这些内存。

Our SmartIO device driver extension to SISCI aims to combine the best of both worlds. Device drivers can remain agnostic about the local address space in the node where the device physically resides as our SmartIO system resolves local I/O addresses. Simultaneously, drivers may fully exploiting shared memory capabilities of the PCIe network by building on top of the existing SISCI functionality. The trade-off is that existing device drivers must be modified or rewritten to use this new API extension. In an attempt to make the case for why this trade-off might be worthwhile, we have evaluated the latency benefit our proof-of-concept distributed NVMe driver.

> 我们的 SmartIO 设备驱动程序扩展到 SISCI 旨在结合两者的优势。设备驱动程序可以不关心设备物理位于其中的节点的本地地址空间，因为我们的 SmartIO 系统解析本地 I/O 地址。同时，驱动程序可以充分利用 PCIe 网络的共享内存功能，建立在现有的 SISCI 功能之上。权衡的是，现有的设备驱动程序必须修改或重写以使用此新的 API 扩展。为了说明这种权衡可能是值得的，我们评估了我们的概念分布式 NVMe 驱动程序的延迟优势。

7.4.1 _Optimizing Data Access Patterns._ We outlined our userspace NVMe driver implementation using the SmartIO SISCI API extension in Section [6](#sec-25). Not only are we able to assign individual queues to different nodes, but we are also able use GPUDirect-capable GPUs to host queues in GPU memory as explained in Section [6.4](#sec-29). Since it is possible to combine the SmartIO API extension with borrowed GPUs (using Device Lending), we can design truly elastic workloads. _Any_ type of (linear) memory, such as RAM or device memory, may be exported and made available for a cluster application, whether it runs on a CPU, a GPU, or another PCIe computing accelerator—or even a combination of CPUs, GPUs, and accelerators.

> 7.4.1 优化数据访问模式。我们在第[6]节中概述了使用 SmartIO SISCI API 扩展的用户空间 NVMe 驱动程序的实现。我们不仅可以将单独的队列分配给不同的节点，而且还可以使用 GPUDirect 可用的 GPU 来托管队列，如第[6.4]节所述。由于可以将 SmartIO API 扩展与借用的 GPU(使用设备借用)结合起来，因此我们可以设计真正弹性的工作负载。任何类型的(线性)存储器，如 RAM 或设备存储器，都可以导出并供集群应用程序使用，无论它是在 CPU、GPU 还是其他 PCIe 计算加速器上运行，甚至是 CPU、GPU 和加速器的组合。

To avoid reading over long distances in the cluster, we can use this flexibility to facilitate moving data around in the cluster by using a “push” strategy instead. The NVMe standard does not have any restrictions regarding memory locations for paired queues; from the NVMe device's point of view, any address it can use DMA to is potentially a valid queue memory location. This means that we can allocate an SQ in memory close to the device, while allocating the associated CQ in memory close to the CPU that polls it. As explained in Section [6.1](#sec-26), our API extension supports specifying access pattern hints when allocating memory segments. By specifying that the CQ segment will be mostly read from by the CPU and only written to by the device, the CQ memory segment will be allocated in the borrower's local memory. Similarly, by specifying read access by the device (and only write access by the CPU) for the SQ memory segment, our SmartIO driver API will prefer memory close to the NVMe. As PCIe provides us with an ordering guarantee, the CPU or GPU may simply write the command to remote memory and immediately after ring the doorbell register.[12](#fn12) This means that when the NVMe device is notified by the doorbell write, we can be certain that the command has arrived in the queue, and the NVMe may read it using DMA.

> 为了避免在集群中进行过长距离的读取，我们可以利用这种灵活性来通过使用“推送”策略来在集群中移动数据。NVMe 标准不对配对队列的内存位置有任何限制；从 NVMe 设备的角度来看，它可以使用 DMA 的任何地址都可能是有效的队列内存位置。这意味着我们可以在设备附近分配一个 SQ，同时在探测它的 CPU 附近分配相关的 CQ。正如第 6.1 节中所解释的，我们的 API 扩展支持在分配内存段时指定访问模式提示。通过指定 CPU 主要读取 CQ 段，而设备只写入 CQ 段，CQ 内存段将分配到借用者的本地内存中。同样，通过指定设备读取(CPU 只写入)SQ 内存段，我们的 SmartIO 驱动程序 API 将优先选择靠近 NVMe 的内存。由于 PCIe 为我们提供了排序保证，CPU 或 GPU 可以简单地将命令写入远程内存，然后立即环绕门铃寄存器[12]。这意味着当 NVMe 设备被门铃写入通知时，我们可以确定命令已经到达队列中，NVMe 可以使用 DMA 读取它。

To evaluate the performance benefit of this strategy, we have designed the following experiment: a local CPU runs our proof-of-concept userspace NVMe driver (implemented as a CUDA application). It uses a local Nvidia Quadro P620 GPU and a remote Intel Optane P4800X DC NVMe device. The local GPU is managed by the native CUDA driver, while the remote NVMe device is operated by our application (proof-of-concept driver). The application reads data from the NVMe directly into GPU memory on the local GPU. Note that “local” and “remote” in this experiment refer to the CPU the application runs on. The NVMe CQ is allocated in the borrower's local RAM, while we have used three different memory locations for placing the SQ as shown in Figure [34](#fig34)(a):

> 为了评估这一策略的性能收益，我们设计了以下实验：本地 CPU 运行我们的概念验证用户空间 NVMe 驱动程序(实现为 CUDA 应用程序)。它使用本地 Nvidia Quadro P620 GPU 和远程 Intel Optane P4800X DC NVMe 设备。本地 GPU 由本机 CUDA 驱动程序管理，而远程 NVMe 设备由我们的应用程序(概念驱动程序)操作。该应用程序从 NVMe 直接读取数据到本地 GPU 上的 GPU 内存中。请注意，在本实验中，“本地”和“远程”指的是应用程序运行的 CPU。 NVMe CQ 分配在借用者的本地 RAM 中，而我们已经使用三个不同的内存位置来放置 SQ，如图[34](#fig34)(a)所示：

![](https://dl.acm.org/cms/attachment/95f057db-4f10-46c5-928c-e1908b85ac19/tocs380102-02-f34.jpg) Fig. 34. SQ placement: We evaluate the impact of moving the SQ closer to the NVMe device. By reducing the distance the NVMe device has to read to fetch I/O commands, we are able to reduce the command completion latency.

1.  **SQ hosted in Local RAM:** We allocated queue memory for the first SQ in local RAM, and mapped this for the NVMe device. When the application rings the doorbell register, the NVMe has to read across 4 hops along the path, including internal PEX8796 switch chip in the expansion chassis, the PM8536 PFX switch chip used internally in the MSX824 cluster switch, as well as the PEX8733 chips used in the PXH830 NTB adapter cards.

> 我们为第一个 SQ 在本地 RAM 中分配了队列内存，并将其映射到 NVMe 设备上。当应用程序敲门铃寄存器时，NVMe 必须沿着路径的 4 个跳跃读取，包括扩展机箱中的内部 PEX8796 交换芯片，MSX824 集群交换器内部使用的 PM8536 PFX 交换芯片，以及 PXH830 NTB 适配器卡中使用的 PEX8733 芯片。

2.  **SQ hosted in Remote RAM:** The memory for the second SQ was allocated in remote memory, i.e., RAM on the lender. As we use the same expansion chassis as previous evaluations with HIB68-16 transparent adapters, the NVMe has to read across 3 hops when the application rings the doorbell, including the internal switch chip in the expansion chassis and the PEX8733 chips used in the HIB68-16 transparent adapter cards.

> 第二个 SQ 托管在远程内存中，即贷方的 RAM 中。由于我们使用与先前评估相同的扩展机箱，并使用 HIB68-16 透明适配器，当应用程序敲门时，NVMe 必须跨越 3 个跳，包括扩展机箱中的内部交换芯片和 HIB68-16 透明适配器卡中使用的 PEX8733 芯片。

3.  **SQ hosted in Remote GPU memory:** Using Device Lending, we also borrowed an Nvidia Quadro P4000 GPU from the same lender and allocated memory for the third SQ as a memory buffer on this GPU. While the borrowed GPU is operated by the local CUDA driver, both Device Lending and the SmartIO API extension uses the same underlying SmartIO system, so mapping this memory for the NVMe device uses the same address resolving mechanism described in Section [4.4](#sec-18). As the GPU is installed next to the NVMe device in the same expansion chassis, the NVMe only has to read through the internal switch chip in the expansion chassis. Note that GPU memory has different memory characteristics than system RAM.

> 使用设备借用，我们还从同一个出租商借用了一个 Nvidia Quadro P4000 GPU，并为第三个 SQ 分配了在此 GPU 上的内存缓冲区。虽然借用的 GPU 由本地 CUDA 驱动程序操作，但 Device Lending 和 SmartIO API 扩展都使用相同的底层 SmartIO 系统，因此将此内存映射到 NVMe 设备使用[4.4](#sec-18)中描述的相同地址解析机制。由于 GPU 安装在与 NVMe 设备相同的扩展机箱中，因此 NVMe 只需要通过扩展机箱中的内部交换芯片读取。请注意，GPU 内存具有与系统 RAM 不同的内存特性。

Both hosts are running Ubuntu 18.04.4 with the 4.15 version of the Linux kernel, and the local host (borrower) is running CUDA 10.2 with the included GPU driver. The IOMMU is enabled on the local host, while it is disabled on the remote host (lender) to use shortest-path routing. While not strictly necessary for this experiment, we also enabled persistent mode on both GPUs.

> 两台主机都运行 Ubuntu 18.04.4，内核版本为 4.15，本地主机(借用者)运行 CUDA 10.2，其中包含 GPU 驱动程序。本地主机启用了 IOMMU，而远程主机(出借者)禁用了 IOMMU 以使用最短路径路由。虽然对于此实验不是必需的，但我们还启用了两个 GPU 的持久模式。

For each SQ location, one by one, our application executes 327,680 NVMe read commands of 4 kB chunks of data from storage each, starting at a pseudo-random offset for each chunk. The _command completion latency_ for each single command was recorded, and we used a queue depth of just one entry to avoid aggregated measurements. We define the command completion latency as the time elapsed between the driver writing a command to the SQ, followed by a write to the doorbell register, until the corresponding completion shows up in CQ memory (local memory). As we start the timer before writing the command, part of the latency measurement is the time it takes to write to (remote) memory. Note that our NVMe driver implementation uses polling instead of relying on interrupts, and that the data is written by the NVMe directly into memory onboard the local GPU using peer-to-peer DMA.

> 对于每个 SQ 位置，我们的应用程序从存储中每次执行 327,680 个 4 kB 数据块的 NVMe 读取命令，从每个块的伪随机偏移量开始。记录了每个单个命令的命令完成延迟，我们使用队列深度为 1 条以避免聚合测量。我们将命令完成延迟定义为驱动程序将命令写入 SQ 之后，写入门铃寄存器，直到相应完成出现在 CQ 内存(本地内存)中的时间段。由于我们在写入命令之前启动计时器，所以延迟测量的一部分是写入(远程)内存所需的时间。请注意，我们的 NVMe 驱动程序使用轮询而不是依赖中断，数据由 NVMe 直接使用点对点 DMA 写入本地 GPU 的内存中。

Figure [34](#fig34)(b) depicts the distributions of latency measurements for all three SQ placements. The same datasets are shown as both a histogram (left) and as a boxplot (right). Note that we have adjusted the Y-axis, so outliers are not shown. Our results demonstrate that moving the SQ memory closer to the NVMe device significantly reduces latency, as the distance that the NVMe device has to read across shrinks. We argue that this indicates that while there is a development cost of implementing device drivers using the SmartIO API extension, the reward is improved performance over Device Lending and native device drivers. There is also the added benefit of being able to fully utilize PCIe clustering capabilities to implement functionality such as streaming data directly into GPU memory.

> 图 34(b)描绘了所有三种 SQ 放置的延迟测量的分布情况。同样的数据集以直方图(左)和箱线图(右)的形式呈现。注意，我们已经调整了 Y 轴，因此不显示异常值。我们的结果表明，将 SQ 内存移动到 NVMe 设备附近可显著降低延迟，因为 NVMe 设备读取的距离缩短了。我们认为，这表明，虽然使用 SmartIO API 扩展实现设备驱动程序需要开发成本，但是与设备借用和本机设备驱动程序相比，性能有所提高。此外，还可以充分利用 PCIe 集群功能，实现将数据直接流入 GPU 内存等功能的额外好处。

Finally, it should be noted that the NVMe standard specifies optional support for one or more **controller memory buffers (CMBs)** [[55](#Bib0055)]. CMBs are BARs with generic device memory that an NVMe driver may read from and write to. The intention of CMBs is that becomes possible for a driver to host queue memory on the NVMe device itself, elminiating the need for the NVMe controller to use DMA to fetch commands entirely. While the Intel Optane P4800X DC NVMe device used in our experiments does not support CMB, implementing support for it to move queues _as close as possible_ to the NVMe would be trivial. Our SmartIO system automatically export device BARs as mappable memory segments, so supporting CMB would be a matter of mapping the BAR and setting up the necessary descriptors in CMB memory.

> 最后，应该指出的是，NVMe 标准规定可选择支持一个或多个控制器内存缓冲器(CMB)[[55](#Bib0055)]。 CMB 是具有通用设备内存的 BAR，NVMe 驱动程序可以从中读取和写入。 CMB 的目的是，NVMe 驱动程序可以在 NVMe 设备本身上托管队列内存，从而完全消除了 NVMe 控制器使用 DMA 获取命令的需要。虽然我们实验中使用的 Intel Optane P4800X DC NVMe 设备不支持 CMB，但实现对其的支持以尽可能将队列移动到 NVMe 将是微不足道的。我们的 SmartIO 系统将设备 BAR 自动导出为可映射的内存段，因此支持 CMB 只是将 BAR 映射并在 CMB 内存中设置必要的描述符的问题。

7.4.2 _Sharing a Single-function NVMe Device._ Due to the complexity of implementing SR-IOV in hardware, NVMe devices with SR-IOV support are not widely available. Most NVMe devices on the market are single-function devices. However, the inherent parallel design of the NVMe standard provides us with great flexibility. Each queue has its own dedicated doorbell register, which avoids contention. Pairs of SQs and CQs can operate completely in parallel, making it possible to distribute queue pairs to different nodes in the cluster using the SmartIO API extension, as explained in Section [6.2](#sec-27). As such, we can treat a non-SR-IOV device as a shared resource by using our NVMe driver implementation.

> 由于实现 SR-IOV 在硬件上的复杂性，市面上支持 SR-IOV 的 NVMe 设备并不普遍。大多数市面上的 NVMe 设备都是单功能设备。但是，NVMe 标准的固有并行设计为我们提供了很大的灵活性。每个队列都有自己专用的门铃寄存器，从而避免了争用。SQs 和 CQs 的对可以完全并行运行，这使得我们可以使用 SmartIO API 扩展将队列对分发到集群中的不同节点，如[6.2]节所述。因此，我们可以通过使用我们的 NVMe 驱动实现来将非 SR-IOV 设备作为共享资源来使用。

To demonstrate this, we designed an experiment in a larger cluster of nodes. The MSX824 cluster switch has 24 ×$\times$ 4 Gen3 ports that can be configured to ×$\times$8 and ×$\times$16 links by grouping two or four ports, respectively. This makes it possible to create a cluster of 60 nodes by connecting seven MSX824 switches in cascade (one top switch with six subswitches). Each individual node is connected to one of the subswitches through a x8 Gen3 link. One node was dedicated as lender, and was configured with an expansion chassis with the NTB adapter and an Intel Optane P4800X DC NVMe device as illustrated in Figure [35](#fig35). Using our 60 node cluster setup, we performed two experiments:

> 为了证明这一点，我们设计了一个更大的节点集群实验。MSX824 集群交换机具有 24×$\times$4 Gen3端口，可以通过将两个或四个端口分组配置为×$\times$8和×$\times$16 链路。这使得可以通过连接七个 MSX824 交换机(一个顶部交换机和六个子交换机)来创建 60 个节点的集群。每个单独的节点通过一个 x8 Gen3 链路连接到一个子交换机。一个节点被指定为贷款人，并配置有一个扩展机箱，其中包含 NTB 适配器和 Intel Optane P4800X DC NVMe 设备，如图 35 所示。使用我们的 60 节点集群设置，我们进行了两个实验：

![](https://dl.acm.org/cms/attachment/3179e6ec-7165-47c8-95c0-d4c5f56af7f8/tocs380102-02-f35.jpg) Fig. 35. By distributing an SQ and a CQ to 30 cluster nodes, we demonstrate that it is possible to concurrently share a single-function NVMe device in a larger cluster.

1.  **Simulatenous sharing:** The P4800X NVMe used in our experiment supports up to 32 queue pairs (one queue pair is reserved for admin queues). We configured the lender to be the NVMe manager, setting up the admin queues and resetting the device, and we configured 30 other nodes to act as NVMe clients as described in Section [6.2](#sec-27). Each of the 30 clients configured one SQ and one CQ, allowing them to operate the NVMe independently of the other nodes, as illustrated in Figure [35](#fig35). All 30 nodes each read chunks of 4 kB data in a loop, demonstrating that our queue-distribution mechanism works.

> 1. **同时共享：**我们实验中使用的 P4800X NVMe 支持最多 32 个队列对(其中一个队列对保留用于管理队列)。我们将贷方配置为 NVMe 管理器，设置管理队列并重置设备，并将其他 30 个节点配置为 NVMe 客户端，如[6.2]节所述。每个客户端配置了一个 SQ 和一个 CQ，使它们可以独立于其他节点操作 NVMe，如图[35]所示。所有 30 个节点都以 4kB 数据块的循环读取，表明我们的队列分配机制有效。

2.  **Multicast:** We configured all 59 nodes (all nodes excluding the lender) to subscribe to the same multicast group, allocating a buffer in their local memory and setting up multicast mappings. We then used one of the nodes to initiate an NVMe identify command using the address of the multicast segment. This replicated 4 kB of controller information to the memory of all 59 nodes in a _single_ operation.

> 我们配置了所有 59 个节点(所有不包括出借人的节点)订阅同一个多播组，在它们的本地内存中分配一个缓冲区，并设置多播映射。然后，我们使用其中一个节点使用多播段的地址启动 NVMe 标识命令。这将 4 kB 的控制器信息在*single*操作中复制到所有 59 个节点的内存中。

While number of switches in the path increases command completion latency (as is expected), hosting queues in the lender's RAM rather than in memory on the borrowers would provide a latency benefit similar to what we observed in Section [7.4.1](#sec-44). However, since the number of simultaneous borrowers is limited by the number of queues supported by the P4000X NVMe used in our experiments, our latency measurements are affected by the round-robin scheduling mechanism implemented in the NVMe controller hardware. Some borrowers suffer from starvation: they are unlucky with regard to timing, ending up having to wait significantly longer than other borrowers for their commands to be executed by the NVMe. Furthermore, the NVMe device itself is only PCIe Gen3 ×$\times$ 4, and the simultaneous read requests from several nodes far exceed the bandwidth capacity of the device. Thus, a performance analysis is not particularly interesting with respect to evaluating our queue-sharing concept, as we end up evaluating how well the NVMe device performs instead. Nonetheless, while the small amount of data and low throughput in our tests may not be particularly useful for an application, we have shown that it is possible for a larger number of nodes in a cluster to access the same storage device simultaneously. In practice, we have successfully demonstrated a form of “MR-IOV in software.”[13](#fn13) Newer NVMes with higher bandwidth and lower latency, as well as support for a higher number of queues, will benefit from this kind of sharing capability.

> 随着路径中开关的数量增加，命令完成延迟(如预期的那样)，将队列存储在贷方的 RAM 中而不是借方的内存中将提供与我们在第 7.4.1 节中观察到的相似的延迟优势。然而，由于我们实验中使用的 P4000X NVMe 支持的队列数量有限，因此延迟测量受到 NVMe 控制器硬件实现的轮询调度机制的影响。一些借方遭受饥饿：他们在时间上不幸，最终不得不等待比其他借方更长的时间，才能让 NVMe 执行命令。此外，NVMe 设备本身只有 PCIe Gen3 × $\times$ 4，来自几个节点的同时读取请求远远超出了设备的带宽容量。因此，性能分析在评估我们的队列共享概念方面并不特别有趣，因为我们最终会评估 NVMe 设备的性能。尽管如此，尽管我们的测试中的数据量少，吞吐量低，对于应用程序而言可能不是特别有用，但我们已经表明，集群中的更多节点可以同时访问同一存储设备。实际上，我们已经成功地演示了一种“软件中的 MR-IOV”。[13](#fn13)具有更高带宽和更低延迟以及支持更多队列的新型 NVMe 将从这种共享功能中受益。

7.4.3 _NVMe-oF RDMA Comparison._ NVMe-oF [[56](#Bib0056), [94](#Bib0094)] is a widely adopted standard for accessing remote NVMe devices over a network. NVMe-oF implementations are composed of two parts: a device-side “target” driver and a client-side “initiator” driver. The target driver is responsible for managing the NVMe device, setting up queue pairs and facilitating asynchronous access by allocating dedicated queue pairs for each individual initiator. I/O commands are forwarded by the initiator to the target driver, which enqueues them for the NVMe device. The NVMe-oF protocol is agnostic regarding the transport layer, allowing commands and completions to be transmitted over any kind of message-passing communication channel, and leaves the transportation of data entirely up to the network fabric.

> NVMe-oF[[56](#Bib0056), [94](#Bib0094)]是用于通过网络访问远程 NVMe 设备的广泛采用的标准。NVMe-oF 实现由两部分组成：设备端“目标”驱动程序和客户端“发起者”驱动程序。目标驱动程序负责管理 NVMe 设备，设置队列对并通过为每个单独的发起者分配专用队列对来实现异步访问。I / O 命令由发起者转发到目标驱动程序，该驱动程序将其入队以供 NVMe 设备使用。NVMe-oF 协议对传输层不可知，允许命令和完成通过任何类型的消息传递通信通道传输，并完全将数据传输留给网络结构。

For network fabrics that support InfiniBand RDMA, NVMe-oF can be supported with very high performance [[29](#Bib0029)]. The defining feature of InfiniBand RDMA is that **InfiniBand channel adapters (HCAs)** may access application memory directly, allowing data to be be transferred directly from the application on one host to the application on another host without going through a network stack. By avoiding kernel transmission buffers, InfiniBand RDMA applications have very high throughput and low latency. Additionally, as the CPU is not involved in transmission, RDMA is completely asynchronous, and avoids blocked send and receive calls.

> 对于支持 InfiniBand RDMA 的网络结构，可以支持具有非常高性能的 NVMe-oF [[29](#Bib0029)]。InfiniBand RDMA 的定义特征是**InfiniBand 通道适配器(HCAs)**可以直接访问应用程序内存，允许数据直接从一台主机上的应用程序传输到另一台主机上的应用程序，而无需经过网络堆栈。通过避免内核传输缓冲区，InfiniBand RDMA 应用程序具有非常高的吞吐量和低延迟。此外，由于 CPU 不参与传输，RDMA 完全是异步的，避免了阻塞的发送和接收调用。

In regard to NVMe-oF, the target driver can provide direct access to both data and queue memory via system memory on the target host.[14](#fn14) Application memory used for RDMA is registered with the InfiniBand driver in advance as so-called **memory regions (MRs)**. This allows the InfiniBand driver to pin the physical memory pages in memory, avoiding them being swapped out. Additionally, as it allows other hosts to resolve the local physical addresses of MRs, an NVMe-oF initiator driver can prepare I/O commands using _target-local_ addresses. In other words, the initiator is able to use the target's MR as intermediate memory for NVMe data.

> 关于 NVMe-oF，目标驱动程序可以通过目标主机上的系统内存直接访问数据和队列内存[14](#fn14)。用于 RDMA 的应用程序内存提前注册到 InfiniBand 驱动程序中，称为**内存区域(MR)**。这使得 InfiniBand 驱动程序可以将物理内存页面固定在内存中，避免它们被换出。此外，由于它允许其他主机解析 MR 的本地物理地址，因此 NVMe-oF 启动器驱动程序可以使用*target-local*地址准备 I / O 命令。换句话说，发起者可以使用目标的 MR 作为 NVMe 数据的中间存储。

Similar to the SQ and CQ queue pairing mechanism for NVMe devices described in Section [6.2](#sec-27), InfiniBand also uses queue pairs of **work queues (WQs)** and **completion queues (CQs)**. HCAs support hosting WQs on device memory (similar to NVMe CMBs described in Section [7.4.1](#sec-44)), and hosting CQs in system memory. This allows a userspace application to post work requests, such as send and receive operations, and poll for completions directly, bypassing the kernel entirely in the data path. An additional benefit is that this design maps very well onto the NVMe-oF architecture; the NVMe-oF target driver can “bind” the receive WQ to the NVMe SQ. This means that NVMe commands are already enqueued (in memory) when the target driver is notified about received commands, and the target driver may simply ring the SQ's doorbell register. Figure [36](#fig36) illustrates the steps involved in reading 4 kB of data from storage using RDMA:

> 类似于第 6.2 节描述的 NVMe 设备的 SQ 和 CQ 队列配对机制，InfiniBand 也使用工作队列(WQ)和完成队列(CQ)的队列对。 HCA 支持在设备内存中托管 WQ(类似于第 7.4.1 节描述的 NVMe CMB)，并在系统内存中托管 CQ。这允许用户空间应用程序发布工作请求，例如发送和接收操作，并直接轮询完成，完全跳过内核在数据路径中。另一个好处是，这种设计非常适合 NVMe-oF 架构; NVMe-oF 目标驱动程序可以将接收 WQ“绑定”到 NVMe SQ。这意味着当目标驱动程序收到接收到的命令时，NVMe 命令已经入队(在内存中)，目标驱动程序可以简单地环绕 SQ 的门铃寄存器。图 36 描述了使用 RDMA 读取 4kB 数据的步骤：

![](https://dl.acm.org/cms/attachment/535d5a66-0aa9-4eeb-8904-8682e9f70c20/tocs380102-02-f36.jpg) Fig. 36. Flow chart of an I/O read operation for NVMe-oF using InfiniBand RDMA. While the target-side CPU is required to initiate NVMe operations and start the RDMA write transfer, neither commands, completions, nor data is moved by the CPU. As InfiniBand queues and NVMe queues are bound to each other, commands and completions are written directly to the queues by the HCAs using DMA.

1.  The initiator prepares an I/O read command for the NVMe device with the desired block offset. Memory used for RDMA is already known to both NVMe-oF initiator, as it was registered by the target driver as a RDMA MR in advance. This allows the initiator to simply use target-side physical addresses of this MR in the read command. It then posts the command to the send WQ, sending the command across the network, directly to the target drivers memory.

> 发起者为 NVMe 设备准备一个 I/O 读取命令，其中包含所需的块偏移量。用于 RDMA 的内存已经被 NVMe-oF 发起者所知，因为它已经由目标驱动程序注册为 RDMA MR。这允许发起者简单地在读取命令中使用目标端的此 MR 的物理地址。然后，它将命令发布到发送 WQ，将命令直接通过网络发送到目标驱动程序的内存中。

2.  The target driver receives a receive completion indicating that it has received an NVMe command. As the HCA has already written the command to the appropriate location in target's memory, the target driver can immediately ring the doorbell register of the bound SQ, initiating the NVMe I/O operation. The initiator driver has already resolved target-side physical addresses in advance, so there is no processing required. After ringing the doorbell, it checks what type of NVMe command this is. Seeing that it is an read command, it starts preparing a WQ request for RDMA write from the local MR to a known MR on the initiator host.

> 目标驱动程序收到接收完成，表明它已经接收到了 NVMe 命令。由于 HCA 已经将命令写入目标内存中的适当位置，因此目标驱动程序可以立即对绑定的 SQ 的门铃寄存器进行敲铃，启动 NVMe I/O 操作。发起者驱动程序已经提前解析了目标端物理地址，因此不需要处理。敲铃后，它检查这是一个什么类型的 NVMe 命令。看到它是一个读取命令，它开始准备一个 WQ 请求，从本地 MR 到发起者主机上已知的 MR 进行 RDMA 写入。

3.  The target driver receives the NVMe command completion, indicating that the NVMe device has written data to memory. The target posts the prepared RDMA write request to the appropriate WQ. By using DMA to read from the MR, the HCA begins sending the data over the InfiniBand fabric. The initiator-side HCA will start writing received data into the initiators memory, also using DMA.

> 目标驱动程序收到 NVMe 命令完成，表明 NVMe 设备已将数据写入内存。目标将准备好的 RDMA 写请求发布到适当的 WQ。通过使用 DMA 从 MR 读取，HCA 开始通过 InfiniBand 网络发送数据。发起方 HCA 将开始使用 DMA 将接收到的数据写入发起方内存。

4.  Since requests in the same WQ are always ordered, the target driver immediately posts a send request for the NVMe completion, knowing that when the initiator driver receives the completion the data must have arrived before it. This optimization means that the target driver avoids needing to wait for the RDMA write completion, which is particularly useful for larger data transfers.

> 由于同一个 WQ 中的请求总是有序的，目标驱动程序立即发出一个 NVMe 完成的发送请求，知道当初始驱动程序收到完成时，数据必须在此之前到达。这种优化意味着，目标驱动程序可以避免等待 RDMA 写完成，这对于大数据传输特别有用。

5.  The initiator driver receives a receive completion for the NVMe command completion, and knows that the data must have arrived in its local memory before the completion due to WQ ordering. The data read from the remote NVMe device is now available for use.

> 发起者驱动程序收到 NVMe 命令完成的接收完成，并且由于 WQ 排序，知道数据必须在完成之前到达其本地内存。从远程 NVMe 设备读取的数据现在可以使用了。

We have designed an experiment to compare the **Storage Performance Development Kit (SPDK)** [[94](#Bib0094)] to our SmartIO NVMe driver implementation. SPDK is a storage application framework that implements support for a wide variety of storage devices, including NVMe devices. Similarly to our SmartIO NVMe driver, it is implemented in userspace, bypassing the kernel and primarily relying on polling. Furthermore, SPDK has a built-in NVMe-oF stack with support for InfiniBand RDMA. As such, SPDK is a suitable comparison for our SmartIO NVMe driver.

> 我们设计了一个实验来比较存储性能开发套件(SPDK)[[94](＃Bib0094)]和我们的 SmartIO NVMe 驱动程序实现。 SPDK 是一个存储应用框架，实现了对各种存储设备(包括 NVMe 设备)的支持。与我们的 SmartIO NVMe 驱动程序类似，它在用户空间中实现，绕过内核，主要依赖轮询。此外，SPDK 具有内置的 NVMe-oF 堆栈，支持 InfiniBand RDMA。因此，SPDK 是我们 SmartIO NVMe 驱动程序的合适比较。

However, as SPDK and our proof-of-concept NVMe driver are two different NVMe driver implementations, comparing them to each other would be comparing apples to oranges. As such, we have instead conducted two separate tests, one where we compare the standard SPDK NVMe driver to SPDK NVMe-oF, and the other where we compare our own NVMe driver using a local and a remote NVMe device. Figure [37](#fig37) depicts the four scenarios were used in our experiment:

> 然而，由于 SPDK 和我们的 NVMe 驱动程序概念验证是两种不同的 NVMe 驱动程序实现，因此将它们进行比较相当于比较苹果和橘子。因此，我们进行了两次独立的测试，一次是比较标准的 SPDK NVMe 驱动程序与 SPDK NVMe-oF，另一次是比较我们自己的 NVMe 驱动程序使用本地和远程 NVMe 设备。图[37](#fig37)描绘了我们实验中使用的四种情况：

![](https://dl.acm.org/cms/attachment/4e513844-069f-4c14-a1c4-bdad2b77dd82/tocs380102-02-f37.jpg) Fig. 37. The different scenarios in our NVMe-oF comparison experiment. Note that the local scenario is the same for both SPDK and SmartIO, the difference is only which NVMe driver software is running.

- **Local SPDK**, shown in Figure [37](#fig37)(a): The standard SPDK NVMe driver operating a local Intel Optane P4800X DC. The NVMe is installed in an expansion chassis, and connected upstream using the HIB68-16 transparent adapters. This scenario serves as our _local baseline_ comparison for SPDK.

> - **本地 SPDK**，如图 37(a)所示：标准的 SPDK NVMe 驱动程序操作本地 Intel Optane P4800X DC。NVMe 安装在扩展机箱中，并使用 HIB68-16 透明适配器上游连接。这种情况作为我们的 SPDK 本地基准比较。

- **SPDK NVMe-oF**, shown in Figure [37](#fig37)(b): The SPDK NVMe-oF driver stack (initiator and target) operating a remote P4800X using RDMA for transport. The two hosts are connected back-to-back with two Mellanox InfiniBand ConnectX-5 EDR channel adapters. The target driver has the same CPU core affinity as its InfiniBand HCA and the NVMe device. The InfiniBand maximum transfer unit was configured to 64 kB, leaving more than enough space within a packet for the data payload. This scenario is compared to the Local SPDK scenario. Note that while we are measuring latency, the EDR speed of 100 Gb/s is equivalent to 12.5 GB/s regardless. This is similar to an x16 Gen3 PCIe link.

> SPDK NVMe-oF，如图 37(b)所示：使用 RDMA 进行传输的远程 P4800X 的 SPDK NVMe-oF 驱动程序堆栈(初始器和目标)。两个主机使用两个 Mellanox InfiniBand ConnectX-5 EDR 通道适配器连接在一起。目标驱动程序具有与其 InfiniBand HCA 和 NVMe 设备相同的 CPU 核心亲和力。InfiniBand 最大传输单元被配置为 64 kB，在数据有效负载的数据包中留有足够的空间。该场景与本地 SPDK 场景进行比较。请注意，尽管我们正在测量延迟，但 EDR 速度为 100 Gb/s 相当于 12.5 GB/s。这类似于 x16 Gen3 PCIe 链接。

- **Local SmartIO**, shown in Figure [37](#fig37)(a): Our proof-of-concept NVMe driver implemented with the SmartIO API extension (as explained in Section [6.2](#sec-27)), operating a local P4800X. The topology is identical as the Local SPDK scenario, but we run our NVMe driver implementation instead of SPDK. As before, the HIB68-16 transparent adapters connecting the expansion chassis use the same PEX8733 switch chips used in the PXH830 NTB adapters. This scenario therefore serves as the _local baseline_ comparison for SmartIO.

> - **本地 SmartIO**，如图 37(a)所示：我们使用 SmartIO API 扩展实现的概念验证 NVMe 驱动程序(如第 6.2 节所述)，操作本地 P4800X。拓扑与本地 SPDK 场景相同，但我们运行我们的 NVMe 驱动程序而不是 SPDK。与之前一样，连接扩展机箱的 HIB68-16 透明适配器使用与 PXH830 NTB 适配器中相同的 PEX8733 交换芯片。因此，此场景作为 SmartIO 的*本地基准*比较。

- **Remote SmartIO**, shown in Figure [37](#fig37)(c): Our driver operating a remote P4800X NVMe. The two systems are connected back-to-back using PXH830 NTB adapters. Note that because we use the expansion chassis in our configuration, there is the same number of switch chips in the path as the Local SmartIO scenario.

> - **远程 SmartIO**，如图 37(c)所示：我们的驱动程序正在操作远程 P4800X NVMe。两个系统使用 PXH830 NTB 适配器连接在一起。请注意，由于我们在配置中使用了扩展机箱，因此在路径中的交换芯片数量与本地 SmartIO 场景相同。

On both hosts, we installed Ubuntu 18.04.2 with the 4.15 version of the Linux kernel, and we used version 19.1.1 of SPDK. We also disabled the IOMMU on both hosts in all four of the evaluated scenarios.

> 在两台主机上，我们安装了带有 Linux 内核 4.15 版本的 Ubuntu 18.04.2，并使用了 SPDK 的 19.1.1 版本。我们还在所有四种评估场景中在两台主机上禁用了 IOMMU。

To measure read latency, we used FIO version 3.13 [[9](#Bib0009)] to perform 327,680 reads, each read page-sized chunk (4 kB) with an offset generated by a pseudo-random number generator. Figure [38](#fig38) shows the latency distributions for SPDK (left) and our NVMe driver (right). We observe that compared to local access, where the NVMe device is able to access host memory directly, NVMe-oF introduces a significant performance overhead, even when using RDMA. There are two primary reasons for this performance difference. First, the CPU on the target host is involved in the critical path, as software is needed to ring the NVMe doorbell registers as well as starting RDMA writes back to the initiator. Second, to use RDMA, data must first be written to target's memory by the NVMe, in order for the InfiniBand HCA to access it and transfer it over the network fabric. In comparison, our SmartIO NVMe driver is able to initiate DMA regardless of whether the NVMe device is local or remote. Not only does this avoid the lender's CPU in the critical path entirely, but we also do not need to bounce data via memory on the lender in the same way RDMA does. In the SmartIO scenarios, because the device and the driver are the same number of switch chips apart, there is _no_ difference in performance for local and remote access. While SPDK and our own proof-of-concept driver are two widely different NVMe driver implementations, it is interesting to note that our driver appears to be slightly faster than local SPDK (around 600 ns on average), even for remote access.

> 为了测量读取延迟，我们使用 FIO 版本 3.13 [[9]](#Bib0009)来执行 327,680 次读取，每次读取页面大小的块(4 kB)，偏移量由伪随机数生成器生成。图 38(#fig38)显示了 SPDK(左)和我们的 NVMe 驱动程序(右)的延迟分布。我们观察到，与 NVMe 设备能够直接访问主机内存的本地访问相比，NVMe-oF 引入了显着的性能开销，即使使用 RDMA 也是如此。这种性能差异有两个主要原因。首先，目标主机上的 CPU 参与关键路径，因为需要软件来环绕 NVMe 门铃寄存器以及启动 RDMA 回写到发起者。其次，为了使用 RDMA，必须首先由 NVMe 将数据写入目标的内存，以便 InfiniBand HCA 能够访问它并将其传输到网络结构中。相比之下，我们的 SmartIO NVMe 驱动程序无论 NVMe 设备是本地还是远程，都能够启动 DMA。这不仅完全避免了借方的 CPU 在关键路径中，而且我们也不需要像 RDMA 那样通过借方的内存来反弹数据。在 SmartIO 场景中，由于设备和驱动程序距离同一个交换芯片的距离相同，因此本地和远程访问的性能没有任何差异。虽然 SPDK 和我们自己的概念驱动程序是两种完全不同的 NVMe 驱动程序实现，但有趣的是，我们的驱动程序似乎比本地 SPDK 略快(平均约 600 ns)，即使是远程访问也是如此。

![](https://dl.acm.org/cms/attachment/e76bb5ec-7dda-44be-abb7-0aa3fcdfc6cc/tocs380102-02-f38.jpg) Fig. 38. Distribution of I/O command completion latencies for Local SPDK and SPDK NVMe-oF (left) and using our proof-of-concept SmartIO NVMe driver (right). By avoiding the device-side CPU in the critical path, as well as being able to use DMA directly, our NVMe driver achieves the same performance for both local and remote. Meanwhile, SPDK NVMe-oF introduces a visible latency overhead compared to local SPDK.

Finally, it should be mentioned that Mellanox has implemented support for NVMe-oF target offloading in their InfiniBand adapters. Target offloading is a mechanism for avoiding target-side CPU in the critical path, by moving some of the target driver logic into hardware on the target-side HCA instead. For example, rather than relying on the target driver running on the CPU, the HCA itself can ring the NVMe doorbell by using peer-to-peer DMA when it receives an NVMe-oF command. However, we argue that a performance overhead compared to local access is unavoidable, since the RDMA mechanism inevitably requires the NVMe device to write data to memory before it can be accessed by the HCA and sent over the network.

> 最后，应该提到 Mellanox 已经在他们的 InfiniBand 适配器中实现了 NVMe-oF 目标卸载的支持。目标卸载是一种避免关键路径中的目标端 CPU 的机制，通过将一些目标驱动程序逻辑移动到目标端 HCA 上的硬件上。例如，HCA 本身可以通过使用点对点 DMA 当它收到 NVMe-oF 命令时环形 NVMe 门铃，而不是依赖于 CPU 上运行的目标驱动程序。但是，我们认为与本地访问相比，性能开销是不可避免的，因为 RDMA 机制不可避免地要求 NVMe 设备在 HCA 访问之前将数据写入内存并通过网络发送。

## 8 DISCUSSION

Our SmartIO solution offers several benefits over traditional approaches to distributed I/O. In the previous section, we presented experiments demonstrating the usefulness and the performance benefits of SmartIO. Particularly, we have performed experiments demonstrating that it is possible to facilitate remote access to devices with native PCIe performance. In this section, we provide a short discussion on some topics and considerations that have not yet been covered by our evaluation.

> 我们的 SmartIO 解决方案比传统的分布式 I/O 方法提供了多种好处。在上一节中，我们展示了实验，证明了 SmartIO 的有用性和性能优势。特别是，我们已经进行了实验，证明可以以本机 PCIe 性能提供远程访问设备。在本节中，我们将就我们的评估尚未涵盖的一些主题和考虑进行简短讨论。

### 8.1 Security

The challenge with security for distributed I/O and so-called “one-sided communication,” where only the initiator-side (sender) software is involved in initiating I/O but not the target (receiver), is an understudied research topic [[85](#Bib0085)]. In the case of accessing remote devices using our SmartIO system, particularly DMA is a security concern. By lending away a local device, the lender effectively yields control over it to software running on a remote system. A flawed device driver on the borrower may cause a device to read from or write to rogue memory addresses on the lender. For Device Lending, it is possible to protect against unintentional memory accesses by using the lender-side IOMMU. Our SmartIO system is able to isolate devices on the lender, protecting against accidental memory reads and writes. However, the current implementation is not able to sufficiently protect against a _malicious_ device driver, as any software running in kernel space on the borrower system in practice has full access to the local NTB adapter. Regardless, we argue that the in the case of a malicious kernel space driver, the entire _local_ system is compromised as well. In other words, we consider this scenario to be beyond the scope of our SmartIO implementation.

> 涉及分布式 I/O 和所谓的“单向通信”的安全性挑战，其中只有发起者端(发送者)软件参与发起 I/O，而目标(接收者)则不参与，是一个尚未研究的研究课题[[85]](#Bib0085)。在使用我们的 SmartIO 系统访问远程设备的情况下，特别是 DMA 是一个安全问题。通过借出本地设备，出借人实际上将控制权交给运行在远程系统上的软件。借方的设备驱动程序出现缺陷可能导致设备从出借人的非法内存地址读取或写入。对于设备借用，可以通过使用出借方 IOMMU 来防止意外的内存访问。我们的 SmartIO 系统能够隔离出借方的设备，从而防止意外的内存读写。但是，目前的实现无法充分保护针对*恶意*设备驱动程序，因为借方系统上运行的任何软件实际上都可以完全访问本地 NTB 适配器。尽管如此，我们认为，在恶意内核空间驱动程序的情况下，整个*本地*系统也会受到损害。换句话说，我们认为这种情况超出了我们的 SmartIO 实现的范围。

In case of the SmartIO extension to the SISCI API, where we expose device driver capabilities to userspace software, a malicious program on the borrower is also a valid concern. An attacker might intentionally use a DMA-capable device to overwrite memory on the lender, causing it to crash, or use the DMA engine to snoop data from memory. In cases where the userspace software cannot be trusted, we can also use the lender-side IOMMU to protect against undesired memory accesses. By placing devices in separate IOMMU domains, SmartIO creates a virtual I/O address space per device.[15](#fn15) This guarantees that the device is only able to access specific DMA windows mapped for it, thus protecting system memory and other devices on the lender. Unlike a device driver, a userspace application cannot exploit kernel space privileges to manipulate the local NTB, and is only able to set up mappings to remote memory by using the SISCI API. We argue that this provides sufficient protection against both defect and malicious userspace programs, as SISCI prevents setting up mappings to arbitrary memory by only allowing registered memory segments.

> 在 SmartIO 扩展到 SISCI API 的情况下，我们将设备驱动程序功能暴露给用户空间软件，恶意程序也是一个有效的关注点。攻击者可能会有意使用 DMA 可用的设备来覆盖借方的内存，导致其崩溃，或者使用 DMA 引擎从内存中窥探数据。在不能信任用户空间软件的情况下，我们也可以使用借方 IOMMU 来防止不需要的内存访问。通过将设备放置在不同的 IOMMU 域中，SmartIO 为每个设备创建一个虚拟 I/O 地址空间[15]。这保证了设备只能访问为其映射的特定 DMA 窗口，从而保护系统内存和借方的其他设备。与设备驱动程序不同，用户空间应用程序无法利用内核空间特权来操纵本地 NTB，并且只能通过使用 SISCI API 来设置映射到远程内存。我们认为，这提供了足够的保护，防止缺陷和恶意用户空间程序，因为 SISCI 通过仅允许注册的内存段来防止设置映射到任意内存。

Finally, for VM pass-through with KVM, our MDEV implementation requires using the lender's IOMMU, as explained in Section [5.2](#sec-21). By mapping a device to the guest-physical memory layout, we limit the passed through device to only accessing DMA windows to the VM it is assigned to. In other words, it is not possible for guest software to misuse our SmartIO system to break out of the virtualized environment, since SmartIO provides the same level of memory isolation as standard pass-through.

> 最后，对于使用 KVM 的 VM 透传，我们的 MDEV 实现需要使用出借方的 IOMMU，如[5.2](#sec-21)节所述。通过将设备映射到客户端物理内存布局，我们将通过的设备限制为仅访问分配给 VM 的 DMA 窗口。换句话说，客户端软件无法滥用我们的 SmartIO 系统来跳出虚拟化环境，因为 SmartIO 提供与标准透传相同的内存隔离级别。

It should be noted that relying on the lender-side IOMMU in combination with long PCIe paths may severely impair DMA performance, as we saw in our IOMMU evaluation in Section [7.3.1](#sec-41). As a general advice, we recommend trying to minimize the distance between a device its lender's IOMMU. Devices that support PCIe ATS [[60](#Bib0060)] are able to cache resolved I/O virtual addresses, thus avoiding routing transactions via the CPU. However, it has been demonstrated that some devices, such as FPGAs and programmable network adapters, can be exploited by an attacker to abuse ATS to break out of IOMMU isolation [[47](#Bib0047)].

> 应该指出，依靠贷方 IOMMU 结合长 PCIe 路径可能会严重影响 DMA 性能，正如我们在第 7.3.1 节中的 IOMMU 评估中所看到的那样。作为一般建议，我们建议尽量减少设备与其贷方 IOMMU 之间的距离。支持 PCIe ATS [[60](#Bib0060)]的设备能够缓存解析的 I/O 虚拟地址，从而避免通过 CPU 路由交易。然而，已经证明，一些设备，如 FPGA 和可编程网络适配器，可以被攻击者利用 ATS 来突破 IOMMU 隔离[[47](#Bib0047)]。

### 8.2 Supported OSes

As explained in Section [4.1](#sec-15), PCIe devices are represented in the Linux kernel using generic device handles. This handle provides device drivers with a unified interface for accessing a device's configuration space as well as mapping DMA buffers. Through hot-adding a virtual “shadow” device handle into the Linux device tree, the borrower component of our Device Lending mechanism is able to to intercept configuration cycles and calls to the Linux DMA API. As such, we are able to inject I/O addresses that map over the device-side NTB in a manner that is transparent to the device driver.

> 根据第 4.1 节所述，PCIe 设备在 Linux 内核中使用通用设备句柄表示。此句柄为设备驱动程序提供了一个统一的接口，用于访问设备的配置空间以及映射 DMA 缓冲区。通过热添加一个虚拟的“影子”设备句柄到 Linux 设备树中，我们的设备借用机制的借用方组件能够拦截配置周期和调用 Linux DMA API。因此，我们能够以对设备驱动程序透明的方式注入映射到设备端 NTB 的 I/O 地址。

Other OSes may represent devices differently in their system. Microsoft Windows, for example, does not provide such a unified device handle interface, and uses separate driver frameworks for different classes of devices instead. The lack of a generic PCIe device interface that we can easily manipulate makes porting the Device Lending mechanism to Windows non-trivial, and a large engineering effort is required to support similar capabilities.

> 其他操作系统可能会以不同的方式表示设备。例如，Microsoft Windows 不提供统一的设备句柄接口，而是为不同类别的设备使用单独的驱动程序框架。我们无法轻松操纵的通用 PCIe 设备接口的缺失使得将设备借用机制移植到 Windows 变得非常复杂，需要大量的工程工作才能支持类似的功能。

However, supporting the lender component of our SmartIO system is more straight forward. The lender's responsibility is essentially to facilitate remote access by setting up mappings over the NTB when it is requested by a borrower. The low-level NTB driver and SISCI API are supported on a wide variety of OSes, including Windows, meaning that a Windows machine lending out its devices is possible. Additionally, as the SISCI shared-memory API is also supported on Windows, so is our SmartIO API extension. This means that while Device Lending may not be possible on Windows, implementing userspace drivers is. We have proved this by running our proof-of-concept NVMe driver on a Windows 10 installation.

> 然而，支持我们 SmartIO 系统的贷方组件更加直接。贷方的责任基本上是在借款人要求时设置 NTB 上的映射来实现远程访问。低级 NTB 驱动程序和 SISCI API 在包括 Windows 在内的多种操作系统上都受支持，这意味着可以使用 Windows 机器借出其设备。另外，由于 SISCI 共享内存 API 也在 Windows 上受支持，因此我们的 SmartIO API 扩展也受支持。这意味着，虽然 Windows 上可能无法进行设备借用，但可以实现用户空间驱动程序。我们已经通过在 Windows 10 安装上运行我们的 NVMe 驱动程序的概念验证来证明这一点。

Finally, it should be noted that by using our MDEV extension to KVM, devices may be passed through to a VM running _any_ guest OS. By passing through an Intel Optane 900P NVMe and an Nvidia GTX 1080 Ti GPU to a VM instance using Qemu, booting the Windows 10 image from the NVMe device itself and using the GPU for video output, we have confirmed that it works. Investigating the possibility for extending our SmartIO solution by implementing support for other hypervisors, such as Xen or Hyper-V, is, however, a candidate for future work.

> 最后，应该指出，通过使用我们对 KVM 的 MDEV 扩展，可以将设备传递给运行*任何*客户端操作系统的虚拟机。通过将 Intel Optane 900P NVMe 和 Nvidia GTX 1080 Ti GPU 传递给使用 Qemu 的虚拟机实例，从 NVMe 设备本身启动 Windows 10 映像，并使用 GPU 进行视频输出，我们已经确认它可以正常工作。然而，研究通过实施对其他虚拟化管理器(如 Xen 或 Hyper-V)的支持来扩展我们的 SmartIO 解决方案的可能性是未来工作的候选项。

### 8.3 Supported CPU Architectures

While we primarily used Intel Xeon CPUs in our performance evaluation presented in Section [7](#sec-31), our implementation is not bound to any specific CPU architecture. For example, we have confirmed that our proof-of-concept NVMe driver works on an Nvidia Jetson TX2, running on its ARM Cortex-A57 processor, and accessing a remote NVMe device. Even so, our SmartIO implementation does require some considerations in regard to CPU architecture:

> 在我们在第 7 节(#sec-31)中提出的性能评估中，我们主要使用了 Intel Xeon CPU，但我们的实现并不局限于任何特定的 CPU 架构。例如，我们已经确认我们的概念验证 NVMe 驱动程序可以在运行 ARM Cortex-A57 处理器的 Nvidia Jetson TX2 上运行，并访问远程 NVMe 设备。即便如此，我们的 SmartIO 实现仍然需要考虑 CPU 架构的一些因素：

- Lenders must be able to support PCIe peer-to-peer to route transactions between the NTB and the device. In our experience, most CPU architectures are capable of this, but some consumer-level CPUs are not. However, this CPU limitation can be alleviated by using peer-to-peer capable switches in the PCIe tree, for example by using an expansion chassis.

> 贷款人必须能够支持 PCIe 点对点路由交易，以在 NTB 和设备之间进行路由。根据我们的经验，大多数 CPU 架构都能够实现这一点，但是一些消费级 CPU 不具备此功能。但是，可以通过在 PCIe 树中使用点对点可用的交换机(例如使用扩展机箱)来缓解这种 CPU 限制。

- Our implementation of Device Lending only includes support for Intel and AMD IOMMUs. While the borrower's IOMMU is not strictly required for Device Lending, without it, a lender needs to map the entire memory of the borrower for the device. This limits the number of devices that can be lent out to different borrowers at the same time, as explained in Section [4.3](#sec-17). However, userspace drivers using our SmartIO API extension do not need the IOMMU for anything else than protecting memory. It should be mentioned that we are currently working on implementing support for IOMMU on ARM (known as the System Memory Management Unit).

> 我们的设备借用实现仅支持 Intel 和 AMD IOMMUs。虽然借用者的 IOMMU 不是严格要求的设备借用，但没有它，出借人需要为设备映射整个借用者的内存。正如[4.3](#sec-17)节所解释的，这限制了同时借给不同借用者的设备的数量。但是，使用我们的 SmartIO API 扩展的用户空间驱动程序不需要 IOMMU 做任何其他事情，只需要保护内存。应该提一下，我们目前正在实现对 ARM(称为系统内存管理单元)的 IOMMU 的支持。

- Some systems do not support assigning 64-bit I/O addresses to BARs, limiting how large the NTB BAR size can be as the combined device memory requirements must fit below 4 GB. This may limit how many devices the system is able to borrow, or how many devices the system can lend out, depending on whether the system is used as a lender or a borrower. In our experience, most modern systems support 64-bit I/O address space by enabling it in the system's BIOS.

> 一些系统不支持将 64 位 I/O 地址分配给 BAR，限制 NTB BAR 大小，因为设备内存要求必须低于 4 GB。这可能会限制系统可以借用多少设备，或者系统可以借出多少设备，这取决于系统是作为出借人还是借款人使用。根据我们的经验，大多数现代系统通过在系统 BIOS 中启用来支持 64 位 I/O 地址空间。

### 8.4 Supported Devices

The main benefit of building our system on top of standard PCIe, is that our sharing idea will work for any standard PCIe device. As PCIe is the industry standard for connecting I/O devices to a computer system, our SmartIO system can support a wide range of devices with different form factors and connectors. Even though we primarily presented performance measurements using an Ethernet adapter, NVMe devices and Nvidia GPUs in our evaluation (Section [7](#sec-31)), we have during the development of SmartIO experimented with FPGAs, AMD GPUs, InfiniBand HCAs, sound cards, and PCIe-attached cameras. We are even able to lend out individual functions of multi-function and SR-IOV devices to different borrowers, as shown in Section [7.1.6](#sec-38).

> 主要好处是，将我们的系统建立在标准 PCIe 之上，我们的共享想法将适用于任何标准 PCIe 设备。由于 PCIe 是将 I / O 设备连接到计算机系统的行业标准，我们的 SmartIO 系统可以支持具有不同外形和连接器的各种设备。尽管我们主要使用以太网适配器，NVMe 设备和 Nvidia GPU 在我们的评估中展示性能测量(第[7]节)，但在开发 SmartIO 期间，我们还尝试了 FPGA，AMD GPU，InfiniBand HCAs，声卡和 PCIe 摄像头。我们甚至可以将多功能和 SR-IOV 设备的单个功能借给不同的借用者，如[7.1.6]节所示。

Legacy device interrupts is currently only supported by our MDEV extension to KVM. By setting up an interrupt handler on the lender for legacy interrupts, similar to how we forward interrupts in our MDEV implementation, it would be possible to use software for forwarding legacy interrupts while mapping MSI/MSI-X interrupts directly over the NTB as our current Device Lending implementation does. The same solution could be used to map MSI/MSI-X interrupts directly over the NTB for our MDEV implementation. However, we do not consider this a priority as the PCIe standard require devices to implement either MSI or MSI-X (or both) [[61](#Bib0061)].

> 当前只有我们的 MDEV 扩展对 KVM 支持传统设备中断。通过在贷款方设置中断处理程序，类似于我们在 MDEV 实现中转发中断的方式，可以使用软件转发传统中断，同时将 MSI/MSI-X 中断直接映射到 NTB，就像我们当前的设备借用实现一样。同样的解决方案可以用于将 MSI/MSI-X 中断直接映射到 NTB，用于我们的 MDEV 实现。但是，我们不认为这是优先考虑的，因为 PCIe 标准要求设备实现 MSI 或 MSI-X(或两者)[[61]](#Bib0061)。

### 8.5 Alternative NTB Implementations

Our low-level NTB driver is not limited to the specific Dolphin NTB adapter cards and cluster switches used in our experiments, but supports multiple families of NTB-capable switch chips from both Broadcom and Microsemi. Any hardware implementation integrating one of these switch chips can be trivially supported by our driver, requiring only minor software adjustments. Additionally, the SmartIO concepts themselves are general and could be implemented for _any_ NTB solution that supports similar capabilities. However, special attention may be required when using an integrated NTB as an embedded CPU rather than as a peripheral device. For example, it is possible that the lender IOMMU must always be enabled, to properly route DMA transactions over the NTB. We have not tested this, and we will investigate how embedded NTBs can be supported in future work.

> 我们的低级 NTB 驱动程序不仅限于我们实验中使用的 Dolphin NTB 适配器卡和集群交换机，而且还支持来自 Broadcom 和 Microsemi 的多个 NTB 兼容交换芯片家族。任何集成这些交换芯片的硬件实现都可以通过我们的驱动程序轻松支持，只需要做一些小的软件调整。此外，SmartIO 概念本身是通用的，可以用于支持类似功能的任何 NTB 解决方案。但是，使用集成 NTB 作为嵌入式 CPU 而不是作为外围设备时，可能需要特别注意。例如，可能总是需要启用借方 IOMMU，以正确路由 NTB 上的 DMA 事务。我们尚未进行测试，我们将在今后的工作中研究如何支持嵌入式 NTB。

Although the SmartIO implementation is incorporated into Dolphin's software stack due to its high-level shared memory support, it should be mentioned that the Linux kernel also has an NTB driver framework [[35](#Bib0035)]. A handful of NTB implementations are already supported in the kernel through this framework, such as Microsemi switches, Intel Xeon's NTB, and the AMD Zeppelin NTB. While this framework has only rudimentary support for low-level NTB functionality, i.e., setting up memory mappings and configuring interrupts, we hope that NTBs’ potential for PCIe-based interconnection and shared-memory clustering is something that eventually may be picked up by the community.

> 尽管 SmartIO 实现已经被整合到 Dolphin 的软件栈中，由于它的高级共享内存支持，但应该提及的是 Linux 内核也有一个 NTB 驱动框架[[35](#Bib0035)]。通过这个框架，已经有一些 NTB 实现被支持，例如 Microsemi 交换机，Intel Xeon 的 NTB 和 AMD Zeppelin NTB。虽然这个框架只有基本的低级 NTB 功能支持，即设置内存映射和配置中断，但我们希望 NTB 基于 PCIe 的互连和共享内存集群的潜力最终能够被社区接受。

### 8.6 Scalability

The Dolphin PXH830 NTB adapters and MXS824 cluster switches used in our experiments support external copper cables of lengths from 0.5 m up to 5 m. It is also possible to use fiber-optic cables that can be up to 100 m long [[21](#Bib0021)]. The MXS824 cluster switch has 24 ×$\times$ 4 Gen3 ports, which can be configured to different combinations of ×$\times$4, ×$\times$8, and ×$\times$16 links. By connecting 7 switches (1 top switch and 6 subswitches) in cascade, and connecting each node to a subswitch through a x8 link, we demonstrated in Section [7.4.2](#sec-45) that our SmartIO solution works in a 60 node cluster sharing an NVMe device. However, while up to 60 nodes can be supported in the cluster, there are some limitations with regard to the number of _devices_ that can be supported.

> 我们实验中使用的 Dolphin PXH830 NTB 适配器和 MXS824 集群交换机支持从 0.5 米到 5 米的外部铜缆。也可以使用可以达到 100 米长的光纤电缆[[21](#Bib0021)]。MXS824 集群交换机有 24×$\times$4 Gen3端口，可以配置为不同的×$\times$4、×$\times$8和×$\times$16 链接组合。通过在级联中连接 7 个交换机(1 个顶层交换机和 6 个子交换机)，并通过 x8 链接将每个节点连接到子交换机，我们在[7.4.2](#sec-45)节中证明了我们的 SmartIO 解决方案可以在共享 NVMe 设备的 60 节点集群中工作。但是，尽管可以支持最多 60 个节点的集群，但是关于可以支持的*设备*数量存在一些限制。

One such limitation is the number of available look-up table entries in the NTB implementation. As we briefly discussed in Section [3.3](#sec-13), the number of mappings over the NTB is limited by the number of entries in the NTB's internal look-up table. Reading from remote memory is a non-posted request that require a completion, as we described in Sections [7.1.3](#sec-35) and [7.3.1](#sec-41). While the request is routed based on its _address_, the completion (with data) is routed back again based on the _requester_. This means that to support read operations to remote memory, the NTB must support mapping requesters as well as addresses, to make sure that completions are routed back to requesters through the NTB. In other words, NTBs have two kinds of look-up tables, one used for translating a local I/O address into an arbitrary remote address, and another used for returning completions to the appropriate requester (a CPU or a device). The number of devices that can be borrowed or lent out is limited by the size of this requester look-up table.

> 一个这样的限制是 NTB 实现中可用的查找表条目的数量。正如我们在第 3.3 节中简要讨论的，NTB 上的映射数量受 NTB 内部查找表的条目数量的限制。从远程内存读取是一个非发布的请求，需要完成，正如我们在第 7.1.3 节和第 7.3.1 节中所描述的。尽管请求是根据其*地址*路由的，但完成(带有数据)是根据*请求者*再次路由回来的。这意味着，为了支持对远程内存的读操作，NTB 必须支持映射请求者以及地址，以确保完成被路由回请求者通过 NTB。换句话说，NTB 有两种查找表，一种用于将本地 I / O 地址转换为任意远程地址，另一种用于将完成返回到适当的请求者(CPU 或设备)。可以借用或借出的设备数量受此请求者查找表大小的限制。

The PXH830 NTB adapters used in our evaluation support 32 such requester mappings per adapter card. With two nodes connected back to back with PXH830 adapters, each of the two nodes may borrow up to 30 devices from the other node and (simultaneously) lend out up to 30 local devices. In this context, we are referring to devices, rather than individual _device functions_, and any of these 30 devices may have several device functions (such as an SR-IOV-capable device). Two mappings are reserved for each of the CPUs, which must also be able to reach across the NTB due to our implementation of the underlying shared-memory communication. While any single node may only lend out 30 local devices and/or borrow up to 30 remote devices, it is possible to add switches to the topology and connect more nodes, thus increasing the total number of available devices in the cluster.

> PXH830 NTB 适配器在我们的评估中支持每个适配器卡 32 个请求者映射。使用两个节点连接到 PXH830 适配器的背面，每个节点可以从另一个节点借用多达 30 个设备，并(同时)借出多达 30 个本地设备。在此上下文中，我们指的是设备，而不是单个设备功能，其中任何 30 个设备都可能具有几个设备功能(例如具有 SR-IOV 功能的设备)。每个 CPU 保留两个映射，由于我们对基础共享内存通信的实现，它们也必须能够跨 NTB 访问。虽然任何单个节点只能借出 30 个本地设备和/或借用多达 30 个远程设备，但可以添加交换机到拓扑中并连接更多节点，从而增加集群中可用设备的总数。

However, the cluster switch itself also has a finite number of available requester mappings per NTB-capable switch port. Setting up an outgoing requester mapping on one switch port consumes ingoing requester mappings on all the other ports. Therefore, adding switches and nodes to the topology will consume requester mappings cluster-wide, as CPUs will require two requester mappings each to reach all the other nodes in the cluster. Although the number of these mappings is very high, it does not scale indefinitely. The exact threshold for when adding more nodes starts decreasing the possible number of devices that can be shared varies, and depends on the configuration of the cluster. However, this limitation can be avoided by designing the cluster topology with device sharing in mind, rather than maximizing the number of nodes.

> 然而，集群交换机本身也有每个 NTB 可切换端口的有限请求者映射数。在一个交换端口上设置一个出站请求者映射会消耗所有其他端口的入站请求者映射。因此，将交换机和节点添加到拓扑中会消耗集群范围内的请求者映射，因为 CPU 每次需要两个请求者映射才能到达集群中的所有其他节点。尽管这些映射的数量很高，但它不会无限地扩展。添加更多节点开始降低可共享设备的可能数量的确切阈值因集群的配置而异。但是，可以通过以设备共享为目的设计集群拓扑来避免这种限制，而不是最大化节点数量。

Another limitation on the number of devices a borrower is able to borrow is the NTB BAR size, or, the size of the “NTB window.” As the borrower must map device BARs through its local NTB, borrowing devices with large BARs would use up more of the NTB window than devices with smaller BARs. For example, it would most likely be possible to borrow more NVMes than GPUs, as NVMes usually have smaller device memory requirements than GPUs. Moreover, the NTB window size can also affect how many devices a lender may lend out at any given time. Devices that require large DMA windows would use more of the NTB window than devices that do not require large DMA transfers. Because of this, it is desirable to set the NTB window size as large as possible.

> 另一个限制借用者能够借用的设备数量是 NTB BAR 大小，或者说“NTB 窗口”的大小。由于借用者必须通过其本地 NTB 映射设备 BAR，因此借用具有较大 BAR 的设备将比具有较小 BAR 的设备占用更多的 NTB 窗口。例如，借用 NVMes 可能比借用 GPU 更多，因为 NVMes 通常比 GPU 具有更小的设备内存要求。此外，NTB 窗口大小也可能影响出借方在任何给定时间可以出借的设备数量。需要大 DMA 窗口的设备将比不需要大 DMA 传输的设备占用更多的 NTB 窗口。因此，最好将 NTB 窗口大小设置得尽可能大。

However, some devices may have addressing limitations making them incapable of reaching high memory addresses. This can become an issue in the case where a lender has many devices or where the workflow requires very large DMA windows, and we need to configure a very large NTB BAR size. As we explained in Section [4.3](#sec-17), increasing device memory requirements may force the system to place the NTB at a high address. The sum of all device memory requirements, i.e., the combined size of the combined downstream BARs (including the NTB), may be so large that the system is forced to assign device memory at high addresses. In the case of the NTBs in our evaluation, devices with addressing limitations would be incapable of reaching DMA windows. The lender-side IOMMU can be used to remap DMA windows from high to low addresses for devices with addressing limitations, but this may come with a performance cost as we observed in Section [7.3.1](#sec-41). Without the lender's IOMMU, the number of devices within a lender is therefore limited by the devices’ memory requirements and addressing capabilities. However, in cases where device memory limitations is a concern, it is possible to simply add more dedicated lender nodes to the topology. This way, we can spread out devices over several lenders, ensuring that that any one lender's combined device memory requirements does not exceed the system's low/high memory assignment threshold.

> 然而，某些设备可能具有地址限制，使其无法访问高内存地址。如果贷款人拥有许多设备或者工作流程需要非常大的 DMA 窗口，我们需要配置非常大的 NTB BAR 大小，这可能成为一个问题。正如我们在第 4.3 节中所解释的，增加设备内存要求可能会迫使系统将 NTB 放置在高地址处。所有设备内存要求的总和，即下游 BAR(包括 NTB)的组合大小可能如此之大，以至于系统被迫将设备内存分配到高地址。在我们评估的 NTB 中，具有地址限制的设备将无法访问 DMA 窗口。贷款人侧 IOMMU 可用于将具有地址限制的设备的 DMA 窗口从高地址重新映射到低地址，但这可能会带来性能成本，如我们在第 7.3.1 节中观察到的。因此，没有贷款人的 IOMMU，贷款人内的设备数量受设备内存要求和地址能力的限制。但是，在设备内存限制成为问题的情况下，可以简单地向拓扑中添加更多专用的贷款人节点。这样，我们可以将设备分散到几个贷款人，确保任何一个贷款人的组合设备内存要求不超过系统的低/高内存分配阈值。

Thus, the limitation on the number of devices and nodes depends on several factors, such as cluster topology, addressing capabilities of the devices, memory requirements of the devices, and the NTBs’ look-up table sizes. As such, there is no straight forward answer to the question of how many devices and nodes can be supported (beyond the topologies already described in this article). However, it should be noted that these limitations stem from limitations in the hardware implementations of different devices and NTBs, and not from our SmartIO sharing methods.

> 因此，设备和节点的数量限制取决于几个因素，例如集群拓扑、设备的地址能力、设备的内存要求以及 NTB 的查找表大小。因此，对于本文中已经描述的拓扑之外的“有多少设备和节点可以支持”的问题，没有直接的答案。但是，应该注意，这些限制来自于不同设备和 NTB 的硬件实现的限制，而不是我们的 SmartIO 共享方法的限制。

### 8.7 Disaggregated and Composable Infrastructure

Using SmartIO, it is possible to design custom configurations of remote and local devices on the fly, while all systems are running. Multiple hosts in the PCIe-networked cluster may contribute their devices, effectively turning the entire cluster into a shared, disaggregated PCIe infrastructure. Individual nodes can dynamically allocate device resources according to immediate application requirements, and release them when they are no longer required. This can potentially greatly increase the utilization of devices in the cluster, as devices are no longer tightly coupled with the hosts they are installed in.

> 使用 SmartIO，可以即时设计远程和本地设备的自定义配置，而所有系统都在运行。PCIe 网络集群中的多个主机可以提供自己的设备，有效地将整个集群变成共享的、分散的 PCIe 基础架构。各个节点可以根据即时应用程序的要求动态分配设备资源，并在不再需要时释放它们。这可能会大大提高集群中设备的利用率，因为设备不再与安装它们的主机紧密耦合。

Moreover, as it is possible to use all three sharing aspects of our SmartIO framework, i.e., Device Lending, MDEV, and the new SmartIO API extension, in different combinations, we are able to support a wide variety of applications at different abstraction levels. Our system effectively eliminates the distinction between local and remote, as well as device and system memory, providing great flexibility with regard to heterogeneous cluster computing. This makes it easier for an application developer to scale out in a cluster and design advanced cluster workflows, e.g.:

> 此外，由于可以以不同组合使用我们 SmartIO 框架的三种共享方式，即设备借用、MDEV 和新的 SmartIO API 扩展，我们能够在不同抽象层次支持各种应用。我们的系统有效地消除了本地和远程以及设备和系统内存之间的区别，为异构集群计算提供了极大的灵活性。这使应用程序开发人员更容易在集群中扩展，并设计高级集群工作流，例如：

- Using Device Lending, remote devices appear to a system as if they are locally installed, facilitating remote access in a manner that is completely transparent to device driver, application, and even the OS. Large-scale CUDA programming tasks can make use of multiple GPUs that appear to be local, instead of writing a distributed applications or relying on middleware such as rCUDA [[23](#Bib0023), [68](#Bib0068)]. In Section [7.2](#sec-39), we for example demonstrated that it is possible to scale-out a GPU-intensive convolutional neural network training task. Pogorelov et al. [[66](#Bib0066)] have previously shown how a multimedia workload can be offloaded to remote GPUs using Device Lending to meet real-time requirements.

> 使用设备借用，远程设备就像本地安装一样出现在系统中，以完全透明的方式为设备驱动程序、应用程序甚至操作系统提供远程访问。大规模 CUDA 编程任务可以利用多个似乎是本地的 GPU，而不是编写分布式应用程序或依赖中间件，如 rCUDA [[23](#Bib0023), [68](#Bib0068)]。例如，在[7.2](#sec-39)节中，我们证明了可以扩展 GPU 密集型卷积神经网络训练任务。Pogorelov 等人[[66](#Bib0066)]之前已经展示了如何使用设备借用将多媒体工作负载移交到远程 GPU 以满足实时要求。

- Access latency in NVMe-oF can be avoided by borrowing the remote NVMe device instead, and accessing it directly, either by using Device Lending as demonstrated in Section [7.1.1](#sec-33) or extending our proof-of-concept NVMe driver as demonstrated in Section [7.4.3](#sec-46). Distributed database applications may reduce query times by using a combination of local and remote NVMe devices for caching and data consistency. By distributing I/O queues for NVMe devices to multiple nodes as demonstrated in Section [7.4.2](#sec-45), it becomes possible for each node to control data locality and thereby reduce the latency for data consistency across nodes.

> NVMe-oF 的访问延迟可以通过借用远程 NVMe 设备来避免，并直接访问它，无论是通过在[7.1.1](#sec-33)节中演示的设备借用，还是通过在[7.4.3](#sec-46)节中演示的扩展我们的概念 NVMe 驱动程序。分布式数据库应用程序可以通过本地和远程 NVMe 设备的组合来缓存和数据一致性，从而减少查询时间。通过在[7.4.2](#sec-45)节中演示的将 NVMe 设备的 I/O 队列分布到多个节点，每个节点都可以控制数据本地性，从而降低跨节点数据一致性的延迟。

- Using Device Lending, high-speed network interfaces, such as InfiniBand HCAs and 100 Gigabit Ethernet adapters, can be assigned to a node while it needs high throughput, and released when no longer needed. Furthermore, many network interfaces are also capable of SR-IOV, allowing a single network card to be distributed to multiple cluster nodes simultaneously, without requiring any software adaptions as demonstrated in Section [7.1.6](#sec-38).

> 使用设备借用，可以在节点需要高吞吐量时分配高速网络接口，如 InfiniBand HCAs 和 100 Gigabit Ethernet 适配器，并在不再需要时释放。此外，许多网络接口也具有 SR-IOV 功能，可以将单个网卡同时分发到多个集群节点，无需任何软件适配，如[7.1.6](#sec-38)节所示。

- In addition to enabling access to individual remote devices, SmartIO also supports creating groups of arbitrary devices and enabling direct peer-to-peer access between them. This makes it possible to create workflows that are optimized for both resource utilization and data locality. By combining Device Lending and the SmartIO API extension, we demonstrated in Section [7.4.1](#sec-44) how it is possible to stream data directly into GPUDirect-capable GPUs across the PCIe network. In Section [6.4](#sec-29), we also explained how a long-running GPU kernel may load and store data by itself, eliminating CPUs and system RAM in the data path entirely.

> 除了使远程设备获得访问权限外，SmartIO 还支持创建任意设备组并允许它们之间直接进行点对点访问。这使得可以创建既能够优化资源利用率又能够本地化数据的工作流程。通过结合设备借用和 SmartIO API 扩展，我们在[7.4.1](#sec-44)节中展示了如何可以通过 PCIe 网络直接将数据流入具有 GPUDirect 功能的 GPU 中。在[6.4](#sec-29)节中，我们还解释了一个长时间运行的 GPU 内核如何自行加载和存储数据，完全消除了数据路径中的 CPU 和系统 RAM。

Throughout this article, we have demonstrated how we can facilitate remote access to devices, without any performance overhead compared to local access. As such, we argue that we have demonstrated that our SmartIO sharing system turns a PCIe shared-memory cluster into a distributed, composable infrastructure.

> 在本文中，我们展示了如何在没有任何性能开销的情况下实现对设备的远程访问。因此，我们认为，我们已经证明了我们的 SmartIO 共享系统可以将 PCIe 共享内存集群转变为分布式、可组合的基础设施。

## 9 RELATED WORK

As a complete system with several components, each component of SmartIO could potentially be discussed at great length to place them in proper context. In fact, several aspects of related work has already been presented throughout the article, such as PCIe shared-memory networking in Section [3](#sec-10) and an implementation of NVMe-oF using RDMA in Section [7.4.3](#sec-46). Our SmartIO solution is at its core a system for sharing I/O devices and facilitating remote access. We have therefore condensed this section to compare our solution to disaggregation solutions we consider the most relevant. In particular, we summarize disaggregation techniques based on PCIe fabric partitioning, followed by a comparison to I/O distribution solutions implemented with NTBs. We also provide a short discussion on using RDMA for distributed I/O. This is followed by some background for the ideas behind our proof-of-concept NVMe driver, before we finally present memory disaggregation ideas that are related to our shared-memory techniques.

> 作为一个由多个组件构成的完整系统，SmartIO 的每个组件都可以进行详细的讨论，以将其放入适当的上下文中。实际上，本文中已经介绍了相关工作的几个方面，例如第 3 节(#sec-10)中的 PCIe 共享内存网络和第 7.4.3 节(#sec-46)中的基于 RDMA 的 NVMe-oF 实现。我们的 SmartIO 解决方案本质上是一个用于共享 I / O 设备并实现远程访问的系统。因此，我们将此部分简化为与我们认为最相关的解耦解决方案进行比较。特别是，我们总结了基于 PCIe Fabric 分区的解耦技术，然后与使用 NTB 实现的 I / O 分发解决方案进行比较。我们还提供了有关使用 RDMA 进行分布式 I / O 的简短讨论，然后是我们的概念验证 NVMe 驱动程序背后的思想，最后是与我们的共享内存技术相关的内存解耦思想。

### 9.1 PCIe Fabric Partitioning

The idea of using the PCIe bus as a shared interconnection fabric between independent host systems is not new. An early approach is **Multi-Root I/O Virtualization (MR-IOV)** [[59](#Bib0059)]. MR-IOV specifies how a single PCIe fabric may be logically partitioned into separate virtual PCIe trees, where each host sees its own PCIe tree without knowing about the others. This partitioning becomes possible using special multi-root aware switches in the fabric. Additionally, in the same way SR-IOV requires virtualization support implemented in hardware, MR-IOV too require devices to be multi-root aware and support multi-host access. Devices without multi-root capabilities can not be shared on the function level. Due to the complexity of implementing MR-IOV, particularly its requirement that both switches and devices are multi-root ware, it did not see any widespread adoption. At the time of writing, we are not aware of any commercially available devices capable of MR-IOV. Wong [[92](#Bib0092)] have demonstrated live partitioning using PLX/Broadcom switches without requiring multi-root aware switches and devices, but their solution does not allow splitting individual device functions or simultaneous access from multiple CPUs either.

> 使用 PCIe 总线作为独立主机系统之间的共享互连网络的想法并不是新鲜事物。早期的方法是**多根 I/O 虚拟化(MR-IOV)**[[59](#Bib0059)]。MR-IOV 指定如何将单个 PCIe 结构逻辑分割成单独的虚拟 PCIe 树，其中每个主机只能看到自己的 PCIe 树，而不知道其他树。通过网络中的特殊多根感知开关，可以实现这种分区。此外，与 SR-IOV 要求硬件实现虚拟化一样，MR-IOV 也需要设备具有多根感知能力并支持多主机访问。没有多根功能的设备无法在功能级别上共享。由于实现 MR-IOV 的复杂性，特别是其要求交换机和设备都具有多根感知能力，因此没有得到广泛采用。在撰写本文时，我们不知道有哪些商业可用的设备能够实现 MR-IOV。Wong [[92](#Bib0092)]已经使用 PLX / Broadcom 交换机演示了实时分区，而无需多根感知开关和设备，但他们的解决方案既不允许分割单个设备功能，也不允许多个 CPU 同时访问。

Rack-scale computers [[17](#Bib0017), [18](#Bib0018)] are computer systems where multiple (independent) CPUs and free-standing I/O devices are attached to the same PCIe fabric, usually connected by a so-called “top-of-rack” PCIe switch. These solutions support disaggregation by dynamically partitioning the shared fabric into different “subfabrics.” The partitioning is made possible by prefixing standard PCIe transactions with a custom header extension called fabric IDs. Devices are transparently attached to their respective partitioned fabric, and are only seen by a single CPU at the time. Unlike MR-IOV, these partitioning solutions does not require support in devices, but they do require dedicated switch chips that support the proprietary fabric ID header extension to configure routes between devices and CPUs through the fabric. Chung et al. [[15](#Bib0015)] present a proprietary solution using Broadcom PEX9797 chips to partition the fabric and distribute individual SR-IOV functions. Similar solutions also exist for Microsemi switch chips [[51](#Bib0051)].

> 机架规模计算机[[17](#Bib0017), [18](#Bib0018)]是一种计算机系统，其中多个(独立的)CPU 和独立的 I / O 设备连接到同一 PCIe 结构，通常由所谓的“机架顶部”PCIe 交换机连接。这些解决方案通过动态划分共享结构为不同的“子结构”来支持分解。划分是通过将标准 PCIe 事务与称为结构 ID 的自定义标头扩展一起前缀实现的。设备透明地连接到其各自分区的结构，并且一次只能被单个 CPU 看到。与 MR-IOV 不同，这些分区解决方案不需要设备支持，但它们确实需要专用交换芯片支持专有的结构 ID 标头扩展来配置设备和 CPU 之间通过结构的路由。Chung 等[[15](#Bib0015)]提出了一种使用 Broadcom PEX9797 芯片分割结构并分发单个 SR-IOV 功能的专有解决方案。类似的解决方案也适用于 Microsemi 交换芯片[[51](#Bib0051)]。

Solutions based on partitioning allow devices to be disaggregated at the (virtual) function level, thus they can be said to enable a composable infrastructure. However, they do not specify any memory-to-memory communication between hosts. In other words, partitioning solutions do not offer any shared-memory capabilities as part of the system, making a solution like our device driver API extension impossible. Consequently, fabric partitioning does not provide the same level of sharing capabilities compared to our shared memory-based system, and simultaneously sharing a device between multiple CPUs requires additional distribution methods, such as RDMA. In contrast, our SmartIO implementation is not only able to share distribute individual device functions (both physical functions and SR-IOV virtual functions), but makes it possible to implement “MR-IOV in software” even for non-SR-IOV single function devices. Our proof-of-concept NVMe driver described in Section [6.2](#sec-27) demonstrates this in practice.

> 解决方案基于分区，允许设备在(虚拟)功能级别上被分离，因此可以说它们能够实现可组合的基础架构。但是，它们不指定主机之间的任何内存到内存的通信。换句话说，分区解决方案不提供系统中的任何共享内存功能，使得像我们的设备驱动程序 API 扩展这样的解决方案不可能实现。因此，与我们的基于共享内存的系统相比，结构分区不能提供相同级别的共享功能，同时在多个 CPU 之间共享设备需要额外的分发方法，例如 RDMA。相比之下，我们的 SmartIO 实现不仅能够共享分发单个设备功能(物理功能和 SR-IOV 虚拟功能)，而且还可以为非 SR-IOV 单功能设备实现“MR-IOV 软件”。我们在[6.2](#sec-27)节中描述的概念验证 NVMe 驱动程序就是实践中的体现。

It should also be mentioned that most solutions based on fabric partitioning are only modular to the extent of a typical blade server configuration, and scaling beyond this requires I/O distribution over traditional network. As many of them rely on proprietary technology, adding new I/O devices or CPUs to the configuration requires additional modules, often only available from the same vendor. In comparison, SmartIO is fully distributed, and enables a heterogeneous computing system, where different CPU architectures may be connected in a cluster and sharing their devices. Any standard PCIe device may be distributed and shared.

> 应该提到，基于织物分割的大多数解决方案只具有典型的刀片服务器配置的模块性，超出此范围的扩展需要通过传统网络进行 I/O 分布。由于其中许多依赖于专有技术，因此要向配置中添加新的 I/O 设备或 CPU 需要额外的模块，通常只能从同一供应商处获得。相比之下，SmartIO 是完全分布式的，可以实现异构计算系统，其中不同的 CPU 架构可以连接在集群中并共享其设备。任何标准的 PCIe 设备都可以分布和共享。

### 9.2 NTB-based Solutions

Using the same Broadcom PEX8733 switch chips used in Dolphin's PXH830 NTB adapters, Lim et al. [[44](#Bib0044), [75](#Bib0075)] have developed NTB host adapters and connected three hosts in a cluster. By extending a shared-memory API with NTB support, their focus seem to be shared-memory functionality for high-performance computing applications, and distributing devices appears not to have been considered. It should be noted that the memory-mapping capabilities they have developed for their API support are similar to functionality already existing in the SISCI API [[22](#Bib0022)].

> 使用与 Dolphin 的 PXH830 NTB 适配器中相同的 Broadcom PEX8733 交换芯片，Lim 等人[[44](#Bib0044)，[75](#Bib0075)]开发了 NTB 主机适配器，并将三台主机连接在一个集群中。通过扩展具有 NTB 支持的共享内存 API，他们的重点似乎是高性能计算应用程序的共享内存功能，而分布式设备似乎没有被考虑。应该指出，他们为其 API 支持开发的内存映射功能与 SISCI API [[22](#Bib0022)]中已经存在的功能类似。

Bielski et al. [[12](#Bib0012)] summarize various disaggregation solutions of I/O devices in the context of high-performance computing. Interestingly, they point out that NTB-based device distribution solutions appear to have relied almost exclusively on network adapters in their performance evaluations, as they were only able to find one example that used GPUs in their evaluation. Additionally, they also point out that most solutions for disaggregating SR-IOV devices seem to be limited to distributing virtual functions to (remote) VMs. Our SmartIO system, however, works for any standard PCIe device. We have used network adapters, NVMe devices and GPUs in our experiments presented in Section [7](#sec-31). Moreover, while we also support pass through to VMs using our MDEV extension (Section [5](#sec-19)), we have demonstrated how we can share individual virtual SR-IOV functions to bare-metal machines in Section [7.1.6](#sec-38). Additionally, it becomes possible to disaggregate single-function devices in software by using our SISCI API extension, as demonstrated by our NVMe driver implementation explained in Section [6](#sec-25).

> Bielski 等人[[12](#Bib0012)]总结了高性能计算中 I/O 设备的分解解决方案。有趣的是，他们指出，基于 NTB 的设备分布解决方案似乎几乎完全依赖于网络适配器进行性能评估，因为他们只能找到一个使用 GPU 进行评估的例子。此外，他们还指出，大多数 SR-IOV 设备分解解决方案似乎仅限于将虚拟功能分发到(远程)虚拟机。然而，我们的 SmartIO 系统适用于任何标准 PCIe 设备。我们在[7](#sec-31)中提出的实验中使用了网络适配器，NVMe 设备和 GPU。此外，虽然我们也支持使用我们的 MDEV 扩展将其传递到虚拟机([5](#sec-19))，但我们已经证明了我们如何在[7.1.6](#sec-38)中将单个虚拟 SR-IOV 功能共享到裸机机器。此外，通过使用我们的 SISCI API 扩展，可以通过软件对单功能设备进行分解，如[6](#sec-25)中解释的 NVMe 驱动程序所示。

Suzuki et al. [[79](#Bib0079)] have implemented NTB-like capabilities in an FPGA in order distribute SR-IOV functions to different hosts. Although their solution is specific to tunnelling PCIe over Ethernet (ExpEther), their initial performance evaluation showed promising throughput measurements for a Gen2 x8 PCIe device. The authors have since shown that the additional network latency has a negative performance impact for DMA reads [[78](#Bib0078)].

> 鈴木等[[79](#Bib0079)]已經在 FPGA 中實現了類似 NTB 的功能，以便將 SR-IOV 功能分發給不同的主機。雖然他們的解決方案專門用於對以太網進行 PCIe 隧道(ExpEther)，但他們的初步性能評估顯示 Gen2 x8 PCIe 設備的吞吐量很有希望。該作者後來表明，額外的網絡延遲對 DMA 讀取有負面的性能影響[[78](#Bib0078)]。

Guleria et al. [[27](#Bib0027)] propose to connect an expansion chassis to one or more hosts using an NTB. By using an ARM CPU add-in card that enumerates the devices in the expansion chassis, they propose an interesting solution that allows programmable devices, such as a GPU, to continue to operate independent of host assignment. However, the implementation of the actual distribution method seems to be lacking, and the authors do not suggest any solutions for allowing the device to be seen by multiple CPUs, or providing any mechanism for dynamically setting up any memory-mappings necessary for DMA. Instead, they propose various traditional distribution methods, such as RDMA or adapting GPU-specific middleware.

> Guleria 等人[[27](#Bib0027)]提出将扩展机箱连接到一个或多个主机，使用 NTB。通过使用枚举扩展机箱中设备的 ARM CPU 插件卡，他们提出了一个有趣的解决方案，可以让可编程设备(如 GPU)继续独立于主机分配。但是，实际分发方法的实施似乎缺乏，作者没有提出任何解决方案，以允许设备被多个 CPU 看到，或提供任何用于动态设置 DMA 所需的内存映射的机制。相反，他们提出了各种传统的分发方法，如 RDMA 或适应 GPU 特定的中间件。

Hou et al. [[31](#Bib0031)] present a solution where hosts are connected to NTB-capable ports on a Broadcom PEX 8648 switch chip. Devices are installed in a dedicated server host, which enumerates the devices and assists the other hosts in setting up NTB mappings. However, instead of leveraging the NTB to map device BARs for the local host and mapping memory for the device, their solution appears to be based on transferring data from memory to memory, and then involving the local device driver on the dedicated server. This solution incurs a performance reduction compared to local device access, as reflected in their performance evaluation.

> 霍等[[31](#Bib0031)]提出了一种解决方案，其中主机连接到 Broadcom PEX 8648 交换芯片上的 NTB 兼容端口。设备安装在专用服务器主机上，该主机枚举设备并协助其他主机设置 NTB 映射。但是，他们的解决方案似乎不是利用 NTB 来映射本地主机的设备 BAR 以及映射设备的内存，而是基于从内存到内存的数据传输，然后涉及专用服务器上的本地设备驱动程序。与本地设备访问相比，这种解决方案会导致性能降低，正如他们的性能评估所反映的那样。

The Ladon system [[88](#Bib0088)] provides functionality that is very similar to our own MDEV implementation, and could potentially be extended to support something similar to both Device Lending and our device driver API. By using a top-of-rack switch with NTB-capabilities, Ladon facilitates access to the same SR-IOV device from multiple VM guests. The device and a dedicated “management host” are connected to the switch transparently, and the management host enumerates the PCIe fabric and takes ownership of the device. In that regard, the management host is conceptually similar to our lender. Multiple “compute hosts” are connected to the same switch through non-transparent switch ports, i.e., NTBs. The management host maps the entire memory of each compute host for the device, and assists the compute hosts in setting up mappings to individual (virtual) device functions, to pass them through to VM guests running on the compute hosts. Ladon's static setup, where all hosts are connected directly to the same top-of-rack switch as the device and the entire memory of each compute host is mapped, allows transactions to be routed directly to each compute host without relying on the IOMMU on the management host. Additionally, by extending the compute hosts’ hypervisor with a specialized host driver, Ladon can support mapping MSI-X interrupts directly into VMs [[86](#Bib0086), [89](#Bib0089)]. However, by requiring device-specific drivers, this interrupt mapping does not appear to be a generic solution.

> 系统 Ladon [[88](#Bib0088)]提供的功能与我们自己的 MDEV 实现非常相似，并且可以扩展到支持类似于设备借用和我们的设备驱动程序 API 的功能。通过使用具有 NTB 功能的机架顶部交换机，Ladon 可以方便地从多个 VM 客户端访问相同的 SR-IOV 设备。设备和专用的“管理主机”透明地连接到交换机，并且管理主机枚举 PCIe 架构并拥有该设备。在这方面，管理主机在概念上类似于我们的贷方。多个“计算主机”通过非透明交换机端口(即 NTB)连接到同一交换机。管理主机为设备映射每个计算主机的整个内存，并协助计算主机设置映射到单个(虚拟)设备功能，以将其传递给在计算主机上运行的 VM 客户端。Ladon 的静态设置，其中所有主机都直接连接到与设备和整个内存的同一机架顶部交换机，允许直接将事务路由到每个计算主机，而无需依赖管理主机上的 IOMMU。此外，通过扩展计算主机的虚拟化管理程序，Ladon 可以支持将 MSI-X 中断直接映射到 VM [[86](#Bib0086), [89](#Bib0089)]中。但是，由于需要特定于设备的驱动程序，这种中断映射似乎不是一种通用解决方案。

The main difference between Ladon and our SmartIO solution is that while a single host owns the device in Ladon, our SmartIO system is truly distributed by supporting multiple hosts acting as lenders. Hosts may even act as both lender and borrower at the same time. Moreover, in Ladon, the management host becomes a single point of failure. Ladon has since been extended with fail-over support, allowing a back-up management host to copy the PCIe fabric enumeration of the first host, and seamlessly take over ownership of the device in case the first management host fails [[87](#Bib0087)]. However, we argue that this still does not make Ladon distributed in the same sense our SmartIO system. It is not possible for a compute host to use devices from _different_ management hosts. In other words, the Ladon system appears to be limited to devices attached directly to a single rack switch managed by a single host (with fail-over). In contrast, SmartIO solution supports scaling out and using devices from several hosts across an entire cluster. Because the Ladon implementation maps the entire memory space of each physical compute host (rather than just memory used by the VMs), the number of compute hosts in Ladon setup will be limited to a handful hosts due to the combined device memory requirements of the NTBs. Our MDEV implementation, however, scales better by probing the guest-physical memory layout and only mapping VM memory, as explained in Section [5.2](#sec-21). Not only does this allow a lender to support more borrowers as we are able to fit more DMA windows through the NTB BAR, but we can simply add more lender systems should device memory requirements become an issue. Finally, the Ladon system appears to work only for VMs unless device-specific host drivers are implemented. Our SmartIO system, however, supports both physical machines and VMs alike by combining Device Lending and the MDEV extension. With Device Lending, devices can be used by the bare-metal host without requiring any modifications to driver software.

> 主要区别在于，Ladon 的设备由单个主机拥有，而我们的 SmartIO 系统是真正分布式的，支持多个主机充当出借方。主机甚至可以同时充当出借方和借用方。此外，在 Ladon 中，管理主机成为单点故障。Ladon 已经通过添加故障转移支持来扩展，允许备份管理主机复制 PCIe 结构枚举，并在第一个管理主机故障时无缝地接管设备[[87]](#Bib0087)。然而，我们认为这仍然不能使 Ladon 以与我们的 SmartIO 系统相同的方式变得分布式。计算主机无法使用来自不同管理主机的设备。换句话说，Ladon 系统似乎仅限于由单个主机管理的单个机架交换机直接连接的设备(具有故障转移)。相比之下，SmartIO 解决方案支持扩展和使用整个集群中的多个主机的设备。由于 Ladon 实施映射每个物理计算主机的整个内存空间(而不仅仅是虚拟机使用的内存)，因此由于 NTB 的组合设备内存要求，Ladon 设置中的计算主机数量将被限制为少数主机。然而，我们的 MDEV 实现可以更好地扩展，通过探测客户端物理内存布局，并且只映射虚拟机内存，如[5.2](#sec-21)节所述。这不仅允许出借方支持更多的借用方，因为我们可以通过 NTB BAR 适应更多的 DMA 窗口，而且如果设备内存要求成为问题，我们可以简单地添加更多的出借系统。最后，Ladon 系统似乎仅适用于虚拟机，除非实现设备特定的主机驱动程序。我们的 SmartIO 系统通过结合设备出借和 MDEV 扩展，既支持物理机器，也支持虚拟机。通过设备出借，设备可以在裸机主机上使用，而无需对驱动程序软件进行任何修改。

### 9.3 Distributed I/O Using RDMA

There are several widely adopted high-speed interconnection technologies used in high-performance computing clusters today, such as InfiniBand and 100/200 Gigabit Ethernet. To make use of their high throughput and low latency, many cluster applications make use of RDMA [[84](#Bib0084)]. RDMA variants are summarized by Huang et al. [[32](#Bib0032)], and include RDMA over InfiniBand, **RDMA over Converged Ethernet (RoCE)** and **Internet Wide Area RDMA Protocol (iWARP)**. By using one-sided communication and providing direct access to application memory, RDMA solutions have been shown to greatly improve performance for a variety of distributed applications [[32](#Bib0032), [34](#Bib0034), [45](#Bib0045)].

> 今天，在高性能计算集群中使用了几种广泛采用的高速互连技术，例如 InfiniBand 和 100/200 Gigabit Ethernet。为了利用它们的高吞吐量和低延迟，许多集群应用程序利用 RDMA [[84](#Bib0084)]。Huang 等人总结了 RDMA 变体[[32](#Bib0032)]，包括 InfiniBand 上的 RDMA、**RDMA over Converged Ethernet(RoCE)**和**Internet Wide Area RDMA Protocol(iWARP)**。通过使用单向通信并提供直接访问应用程序内存，已经证明 RDMA 解决方案可以大大提高各种分布式应用程序的性能[[32](#Bib0032)、[34](#Bib0034)、[45](#Bib0045)]。

One of the most successful GPU disaggregation frameworks on the market today is rCUDA [[23](#Bib0023), [68](#Bib0068)]. Similarly to how the shadow device in our Device Lending mechanism makes it possible to intercept calls to the kernel's DMA API, rCUDA uses virtualization techniques to intercept CUDA API calls and enable access to remote GPUs while the programming model remains simple. By supporting GPUDirect, rCUDA and other RDMA-based GPU disaggregation solutions are able to copy data directly in and out of GPU memory using RDMA with very high performance [[70](#Bib0070), [91](#Bib0091)]. However, these solutions are not as closely integrated to the PCIe fabric as our NTB-based solution; using Device Lending or our MDEV extension, we are able to support CUDA Unified Memory [[73](#Bib0073)], allowing GPUs to access host memory and memory of other GPUs directly. We are not aware of any RDMA-based GPU disaggregation solutions that are able to support this.

> 一个市场上最成功的 GPU 分散框架是 rCUDA [[23](#Bib0023), [68](#Bib0068)]。与我们的设备借用机制中的影子设备可以拦截对内核 DMA API 的调用一样，rCUDA 使用虚拟化技术拦截 CUDA API 调用，并使远程 GPU 可以访问，而编程模型仍然很简单。通过支持 GPUDirect，rCUDA 和其他基于 RDMA 的 GPU 分散解决方案可以使用 RDMA 直接从 GPU 内存中复制数据，性能非常高[[70](#Bib0070), [91](#Bib0091)]。然而，这些解决方案与 PCIe 结构的集成不如我们的 NTB 基于解决方案；使用设备借用或我们的 MDEV 扩展，我们能够支持 CUDA 统一内存[[73](#Bib0073)]，允许 GPU 直接访问主机内存和其他 GPU 的内存。我们不知道有哪些基于 RDMA 的 GPU 分散解决方案能够支持这一点。

Many different frameworks for distributed I/O using RDMA exist, such as NVMe-oF [[28](#Bib0028), [29](#Bib0029), [56](#Bib0056)] and rCUDA discussed above. However, RDMA solutions are tightly coupled with the device (or type of devices) they are implemented for. As illustrated in Figure [39](#fig39), even though RDMA facilitates memory-to-memory transmission, a specialized device driver is still required on the device-side system to initiate the actual I/O operation.

> 许多不同的分布式 I/O 框架使用 RDMA，例如 NVMe-oF [[28](#Bib0028)，[29](#Bib0029)，[56](#Bib0056)]和上面讨论的 rCUDA。但是，RDMA 解决方案与其实现的设备(或类型的设备)紧密耦合。正如图[39](#fig39)所示，尽管 RDMA 促进内存到内存的传输，但仍需要在设备端系统上安装一个专用的设备驱动程序来启动实际的 I/O 操作。

![](https://dl.acm.org/cms/attachment/2a7cf3e4-25b4-4e9f-8299-ab9b7c1b5a83/tocs380102-02-f39.jpg) Fig. 39. Even though RDMA allows efficient memory-to-memory transfer, a device-side driver is still needed to initiate the I/O operation. Using our SmartIO solution, the local device driver can initiate I/O operations directly and avoids software in the critical path.

Additional software complexity in the form of target-side drivers inevitably leads to a performance overhead compared to accessing a local device, as we observed in our NVMe-oF comparison in Section [7.4.3](#sec-46). Some of this target driver functionality can be implemented in network adapter hardware, for example in the case of NVMe-oF target offloading. Another approach is implementing network interface capabilities directly into device controllers, as proposed by Daglis et al. [[19](#Bib0019)]. While such solutions may improve I/O performance to the point were it becomes comparable to local access, we argue that these solutions become even more coupled with the specific devices they are implemented for by requiring implementation in hardware. In contrast, our SmartIO system is general in terms of device support, as we can distribute any PCIe device without requiring specific support in devices or device drivers.

> 除了本地设备访问之外，以目标端驱动程序形式的附加软件复杂性必然会导致性能开销，正如我们在第 7.4.3 节中观察到的 NVMe-oF 比较中所示。其中一些目标驱动程序功能可以在网络适配器硬件中实现，例如在 NVMe-oF 目标卸载的情况下。另一种方法是直接将网络接口功能实现到设备控制器中，正如 Daglis 等人提出的[[19]](#Bib0019)。虽然这些解决方案可以提高 I/O 性能，以至于它与本地访问相媲美，但我们认为，这些解决方案会变得更加依赖于它们实现的特定设备，因为它们需要在硬件中实现。相比之下，我们的 SmartIO 系统在设备支持方面是通用的，因为我们可以在不需要特定设备或设备驱动程序支持的情况下分发任何 PCIe 设备。

### 9.4 NVMe Queue Distribution

Using the SmartIO extension to the SISCI API, we have implemented a working proof-of-concept userspace NVMe driver, as described in Section [6.2](#sec-27). To the best of our knowledge, our driver is unique in that it is able distribute individual I/O queues of a single-function NVMe device to _remote_ systems in an cluster, without using RDMA. As such, we disaggregate an NVMe device at the software-level. However, similar ideas for sharing an NVMe device at the queue-level for userspace applications running on the same _local_ system can be found in several implementations, including SPDK [[94](#Bib0094)].

> 使用 SmartIO 扩展到 SISCI API，我们已经实现了一个可用的概念验证用户空间 NVMe 驱动程序，如[6.2]节所述。据我们所知，我们的驱动程序在于它可以在集群中将单功能 NVMe 设备的单个 I / O 队列分发到*远程*系统，而无需使用 RDMA。因此，我们在软件层面解耦 NVMe 设备。但是，可以在几种实现中找到类似的在*本地*系统上为运行在同一*本地*系统上的用户空间应用程序共享 NVMe 设备的想法，包括 SPDK [[94]](#Bib0094)。

Peng et al. [[58](#Bib0058)] implement a paravirtualized NVMe driver using the same mediated device driver interface we have used for our MDEV implementation. Instead of passing through the device itself, their solution is based on using passing through I/O queues instead. They accomplish this by assigning individual I/O queues to emulated NVMe child devices. An interesting observation is that the authors report that relying on polling instead of interrupts significantly increases performance, which could suggest that their performance measurements are affected by same interrupt notification delays we observed in our own MDEV evaluation (Section [7.3.2](#sec-42)). Furthermore, Kim et al. [[39](#Bib0039)] extend the Linux NVMe driver with a dedicated queue management kernel module that is responsible for creating and deleting SQs and CQs, as well as mapping DMA buffers and doorbell registers for a userspace application. This way, a userspace application is given control over queue memory and can submit I/O commands and poll for completions directly, without going through the kernel block layer. By mapping queue memory directly for the application, this solution is conceptually very similar to how our own driver is implemented, but by using our SmartIO system we can assign queues to applications running on _remote_ hosts as well.

> Peng 等人[[58](#Bib0058)]使用我们用于 MDEV 实现的相同的介导设备驱动程序接口实现了一个虚拟化的 NVMe 驱动程序。他们的解决方案不是通过设备本身传递，而是基于使用 I/O 队列传递。他们通过将单个 I/O 队列分配给仿真 NVMe 子设备来实现这一点。有趣的是，作者报告说，依靠轮询而不是中断可以显着提高性能，这可能表明他们的性能测量受到我们自己的 MDEV 评估(第[7.3.2](#sec-42)节)中观察到的相同的中断通知延迟的影响。此外，Kim 等人[[39](#Bib0039)]通过一个专用的队列管理内核模块来扩展 Linux NVMe 驱动程序，该模块负责创建和删除 SQs 和 CQs，以及为用户空间应用程序映射 DMA 缓冲区和门铃寄存器。这样，用户空间应用程序就可以控制队列内存，可以直接提交 I/O 命令并轮询完成，而无需经过内核块层。通过为应用程序直接映射队列内存，这种解决方案在概念上与我们自己的驱动程序的实现非常相似，但是通过我们的 SmartIO 系统，我们可以将队列分配给*远程*主机上运行的应用程序。

Our NVMe driver implementations also supports GPUDirect. Although several solutions using GPUDirect to facilitate peer-to-peer access between an NVMe device and a GPU already exist [[10](#Bib0010), [11](#Bib0011), [40](#Bib0040), [83](#Bib0083)], we believe our proof-of-concept device driver's ability use (remote) GPU memory to host I/O queues closer to an NVMe device to be a new idea. This becomes possible by combining our driver with using Device Lending to access remote GPUs. We have demonstrated the latency benefit of this in Section [7.4.1](#sec-44).

> 我们的 NVMe 驱动程序也支持 GPUDirect。尽管已经存在几种使用 GPUDirect 来促进 NVMe 设备和 GPU 之间的点对点访问的解决方案[[10](#Bib0010), [11](#Bib0011), [40](#Bib0040), [83](#Bib0083)]，但我们相信我们的概念验证设备驱动程序可以使用(远程)GPU 内存来更接近 NVMe 设备地主机 I / O 队列的能力是一个新的想法。通过结合我们的驱动程序和使用设备借用来访问远程 GPU，这成为可能。我们在[7.4.1](#sec-44)节中展示了这种延迟优势。

Additionally, our implementation supports offloading NVMe operation onto a GPU entirely and eliminating the CPU in the data path altogether, as we explained in Section [6.4](#sec-29). While this would arguably prove to be highly useful in the case of a local system, the utility of this increases significantly for applications that can now freely make use of accelerators, storage devices and memory anywhere in a cluster and optimize the data flow through the PCIe network. Supporting this kind of flexibility while allowing applications and application programmers to remain agnostic about address space layout on remote systems is, to the best of our knowledge, a novel contribution.

> 此外，我们的实现还支持将 NVMe 操作完全转移到 GPU 上，并完全消除数据路径中的 CPU，如我们在[6.4](#sec-29)节中所解释的。虽然在本地系统的情况下，这可能会极其有用，但是，当应用程序现在可以自由地在集群中的任何地方使用加速器、存储设备和内存时，这种效用会显著增加，并通过 PCIe 网络优化数据流。支持这种灵活性，同时允许应用程序和应用程序程序员对远程系统上的地址空间布局保持不变，据我们所知，是一项新颖的贡献。

### 9.5 Memory Disaggregation

In our work, we enable efficient distribution of devices across a cluster system, alleviating both load balancing problems and lack of or limited numbers of local devices. While we are primarily concerned with I/O device sharing to make active resources available to cluster nodes, our SmartIO implementation is made possible through distributed shared memory. After all, we are effectively mapping and enabling access to remote memory regions. As such, our work has an inherent relationship with memory disaggregation techniques.

> 在我们的工作中，我们可以有效地在集群系统中分发设备，缓解负载平衡问题和本地设备数量不足或有限的问题。虽然我们主要关注 I / O 设备共享以使集群节点可用，但我们的 SmartIO 实现是通过分布式共享内存实现的。毕竟，我们实际上是映射和使远程内存区域可访问。因此，我们的工作与内存分散技术有着内在的关系。

Memory disaggregation concepts originally sprung out of related ideas from early work on distributed memory and distributed OSes. Since CPUs have only operated on local memory, scarce memory would be augmented by swap space. Remote memory has frequently been proposed as a faster alternative to disk-backed swap spaces [[24](#Bib0024), [26](#Bib0026), [42](#Bib0042), [46](#Bib0046)], to overcome the limited throughput and high latency of hard disks. Although this premise has been questioned due to the software overhead [[8](#Bib0008)], performance gains have been measured with both centralized [[24](#Bib0024)] and decentralized [[42](#Bib0042), [46](#Bib0046)] approaches. By relying on the same PCIe-based distributed shared memory capabilities that our own SmartIO is built on, an implementation for (partial) memory disaggregation solution can be imagined. If combined with our extended SISCI API explained in Section [6.1](#sec-26), then it could support a combination of local and remote RAM, as well as remote _device memory_. In fact, we have already demonstrated something similar to this in Section [7.4.1](#sec-44). Even though the main purpose of this experiment was to prove reduced memory access latency for a remote NVMe, it also showed that we are able to map both remote RAM and onboard device memory (of the remote GPU) for the local CPU.

> 概念上的内存分散最初源自早期分布式内存和分布式操作系统的相关思想。由于 CPU 只能操作本地内存，稀缺的内存将通过交换空间来增加。远程内存经常被提出作为替代硬盘支持的交换空间的更快选择[[24](#Bib0024), [26](#Bib0026), [42](#Bib0042), [46](#Bib0046)]，以克服硬盘的有限吞吐量和高延迟。尽管这一前提已经受到软件开销[[8](#Bib0008)]的质疑，但是集中[[24](#Bib0024)]和分散[[42](#Bib0042), [46](#Bib0046)]的方法都已经测量到性能的提升。通过依赖我们自己的 SmartIO 所建立的基于 PCIe 的分布式共享内存功能，可以想象一个实现(部分)内存分散解决方案的实现。如果结合我们在[6.1](#sec-26)节中解释的扩展 SISCI API，那么它可以支持本地和远程 RAM 以及远程设备内存的组合。实际上，我们已经在[7.4.1](#sec-44)节中展示了类似的东西。尽管这个实验的主要目的是证明远程 NVMe 的访问延迟减少，但它也表明我们能够为本地 CPU 映射远程 RAM 和本地设备内存(远程 GPU)。

More recent memory disaggregation solutions rely on RDMA for efficient access to remote memory. Gu et al. [[26](#Bib0026)] show how software overhead of swapping to remote memory can nearly entirely be avoided by using RDMA. Similarly, Aguilera et al. [[5](#Bib0005)] propose a solution where clients use remote memory more explicitly, through a file system-like API that acts as an abstraction over RDMA. The most interesting aspect of this idea is that as their file system interface supports POSIX semantics, it becomes possible to support the mmap operation. A local process may memory-map a file descriptor, and, by relying on virtual memory trapping (page faults), RDMA transfers are initiated under the hood. By using the SISCI API to map remote memory directly into a process’ virtual address space, we avoid any latency from handling traps in software. Instead, the local CPU can access memory across the NTB directly, thus avoiding software in the path altogether.

> 最近的内存分解解决方案依赖于 RDMA 来有效访问远程内存。Gu 等人[[26](#Bib0026)]展示了如何通过使用 RDMA 几乎完全避免交换到远程内存的软件开销。同样，Aguilera 等人[[5](#Bib0005)]提出了一种解决方案，其中客户端更明确地使用远程内存，通过类似文件系统的 API 作为 RDMA 的抽象。这个想法最有趣的一点是，由于它的文件系统接口支持 POSIX 语义，它可以支持 mmap 操作。本地进程可以对文件描述符进行内存映射，并且通过依赖虚拟内存捕获(页面故障)，在背景中启动 RDMA 传输。通过使用 SISCI API 将远程内存直接映射到进程的虚拟地址空间，我们避免了处理软件中的任何延迟。相反，本地 CPU 可以直接跨 NTB 访问内存，从而完全避免了软件的路径。

So-called “byte-adressable” NVMe devices are becoming increasingly common. These NVMes implement memory controllers and expose non-volatile flash memory through device BARs, similar in concept to the GPUDirect-capable GPUs used in our experiments. As such, they are frequently used for persistent memory solutions [[71](#Bib0071), [93](#Bib0093)]. Abulila et al. [[4](#Bib0004)] argue that because non-volatile flash memory is approaching dynamic RAM speeds, traditional swapping semantics incur significant system performance overhead. They propose an extension to the Linux kernel virtual memory manager that short-cuts the Linux block-layer, to support efficient swapping to byte-addressable NVMe devices. With our SmartIO API extension, this solution could be extended to _remote_ NVMe devices as well, by mapping the remote BAR for the local CPU. However, additional adaptions would be required, to limit or, preferably, avoid reading over the NTB.

> 所谓的“字节可寻址”NVMe 设备越来越常见。这些 NVMes 实现内存控制器，通过设备 BAR 将非易失性闪存暴露出来，与我们实验中使用的 GPUDirect 可用 GPU 的概念类似。因此，它们经常用于持久性存储解决方案[[71](#Bib0071), [93](#Bib0093)]。Abulila 等[[4](#Bib0004)]认为，由于非易失性闪存正在接近动态 RAM 速度，传统的交换语义会带来显著的系统性能开销。他们提出了一个 Linux 内核虚拟内存管理器的扩展，以支持高效的字节可寻址 NVMe 设备交换。通过我们的 SmartIO API 扩展，这个解决方案也可以扩展到\_远程\_NVMe 设备，通过映射本地 CPU 的远程 BAR。然而，还需要进行额外的适应，以限制或最好避免通过 NTB 读取。

Although the use of dedicated blade servers may stretch the term disaggregation, Lim et al. [[43](#Bib0043)] nevertheless propose an interesting solution for swapping to remote memory blades over PCIe. They suggest a hardware modification to the memory controller by which the CPU could prefetch cache lines directly over the PCIe bus and “fault in” remote memory pages, by initiating DMA transfers on the remote server. While their proposed solution would avoid reading over the PCIe bus, their evaluation appears not to take into account any latency that would be added by this hardware DMA mechanism.

> 尽管使用专用刀片服务器可能会拉长分解的术语，但 Lim 等人[[43](#Bib0043)]仍然提出了一种有趣的解决方案，可以通过 PCIe 将内存刀片远程交换。他们建议对内存控制器进行硬件修改，使 CPU 可以直接通过 PCIe 总线预取缓存行，并通过远程服务器发起 DMA 传输来“故障”远程内存页面。虽然他们提出的解决方案可以避免通过 PCIe 总线读取，但他们的评估似乎没有考虑这种硬件 DMA 机制带来的延迟。

The disaggregation concept is perhaps taken to its most extreme by LegoOS [[74](#Bib0074)]. Here, processing, memory, and storage resources are all encapsulated into components that can be combined arbitrarily to serve cluster applications. Other hardware devices can be encapsulated in the same manner. Although the authors note that it is not possible to fully separate CPUs and memory management in practice, their idea of building disaggregation concepts into the OS itself instead of using middleware is intriguing. LegoOS only stops short of being a fully distributed OS by presenting users with virtualized nodes that appear as individual VMs. While it is possible to run existing Linux applications on these virtual nodes, device drivers must be adapted to fit this new OS design. In contrast, our own Device Lending mechanism is able to facilitate this kind of disaggregation at a level “underneath” the OS. In effect, it is possible to use remote devices without requiring any modifications to existing device drivers. Finally, LegoOS claims to have performance comparable to a standard Linux server, but their NVMe benchmark shows a significant reduction in number of I/O operations per second compared to Linux when the amount of data is more than a few kilobytes. This performance gap is explained with network overhead. In comparison, because our own SmartIO solution is PCIe-based, we have _zero_ overhead compared to local access, as shown in Section [7](#sec-31).

> 概念解散可能被 LegoOS [[74](#Bib0074)] 推向极限。在这里，处理、存储和存储资源都被封装成可以任意组合以服务集群应用的组件。其他硬件设备也可以以相同的方式封装。尽管作者指出实际上不可能完全分离 CPU 和内存管理，但他们将解散概念构建到操作系统本身而不是使用中间件的想法令人着迷。LegoOS 只是通过向用户提供显示为单个虚拟机的虚拟节点而停止成为完全分布式操作系统。虽然可以在这些虚拟节点上运行现有的 Linux 应用程序，但设备驱动程序必须适应这种新的操作系统设计。相比之下，我们自己的设备借用机制能够在操作系统“之下”实现这种解散。实际上，可以使用远程设备而无需对现有设备驱动程序进行任何修改。最后，LegoOS 声称其性能可与标准 Linux 服务器相媲美，但当数据量超过几 KB 时，他们的 NVMe 基准测试显示与 Linux 相比 I/O 操作每秒数量有显著减少。这种性能差距是由网络开销解释的。相比之下，由于我们自己的 SmartIO 解决方案是基于 PCIe 的，因此与本地访问相比，我们有*零*开销，如[7](#sec-31)节所示。

## 10 CONCLUSION

In this article, we have presented our SmartIO system for efficient, zero-overhead sharing of I/O devices in a heterogeneous PCIe cluster. By using memory-mapping capabilities inherent in NTBs, we combine traditional I/O with distributed shared-memory functionality over native PCIe. Our system consists of the following five components:

> 在本文中，我们提出了 SmartIO 系统，用于在异构 PCIe 集群中高效、零开销地共享 I / O 设备。通过利用 NTB 中固有的内存映射功能，我们将传统的 I / O 与本地 PCIe 上的分布式共享内存功能结合起来。我们的系统由以下五个组件组成：

1.  Our low-level NTB driver, facilitating shared-memory abilities and providing mechanisms for mapping remote memory. As such, we use this to create a global address space comprised of all hosts in the cluster, including their internal devices and memory.

> 我们的低级 NTB 驱动程序，可以支持共享内存功能，并提供映射远程内存的机制。因此，我们使用它来创建一个由集群中的所有主机(包括它们的内部设备和内存)组成的全局地址空间。

2.  A common abstraction mechanism, providing the necessary functionality for translating remote I/O addresses and resolving paths in the network. This allows both software and devices to be agnostic about address space layouts in remote hosts.

> 通用抽象机制，提供翻译远程 I/O 地址和解析网络中路径所需的功能。这使得软件和设备对远程主机中的地址空间布局保持不变。

3.  Our Device Lending method, making remote devices appear to a system as if they are locally installed. Existing device drivers, application software, and even the OS itself may use the remote devices without requiring _any_ adaptions.

> 我们的设备借用方法，使远程设备看起来像是本地安装的一样。现有的设备驱动程序、应用软件，甚至操作系统本身都可以使用远程设备，而不需要任何改动。

4.  Our MDEV extension to KVM hypervisor, allowing pass-through of remote devices to a VM, and enabling direct access to hardware over native PCIe without breaking out of memory isolation.

> 我们对 KVM 虚拟化管理器的 MDEV 扩展，允许远程设备的透传到虚拟机，并允许直接通过本地 PCIe 访问硬件，而不会破坏内存隔离。

5.  Our new SmartIO device driver API extension to the SISCI shared-memory API, enabling cluster applications to take full advantage of shared-memory capabilities and write device drivers optimized for shared-memory cluster workloads.

> 我们新的 SmartIO 设备驱动程序 API 扩展到 SISCI 共享内存 API，使集群应用程序可以充分利用共享内存功能，并编写专为共享内存集群工作负载优化的设备驱动程序。

Additionally, we have also presented our proof-of-concept NVMe driver implementation, using our SmartIO API extension. This driver demonstrates several aspects of I/O with shared-memory capabilities, such as simultaneously sharing a non-SR-IOV device among multiple hosts (“MR-IOV in software”) and enabling peer-to-peer memory access to (remote) GPUDirect-capable GPUs.

> 此外，我们还使用我们的 SmartIO API 扩展提出了我们的概念验证 NVMe 驱动程序实现。该驱动程序演示了具有共享内存功能的 I/O 的几个方面，例如在多个主机之间同时共享非 SR-IOV 设备(“软件中的 MR-IOV”)，以及启用对(远程)GPUDirect 可能 GPU 的点对点内存访问。

Using our SmartIO system, devices can be distributed in a way that meets current processing requirements, while at the same time the overall resource utilization in the cluster is improved as resources are no longer locked to individual hosts. We prove the flexibility of our solution through a broad range of performance evaluations for different scenarios and topologies for all three distribution aspects of our SmartIO system, i.e., Device Lending, MDEV, and the API extension (in the form of experiments with our proof-of-concept NVMe driver). By comparing the performance of using remote devices to local access, our results show that we do not add _any_ performance overhead beyond what is expected for longer PCIe paths.

> 使用我们的 SmartIO 系统，可以以满足当前处理要求的方式分布设备，同时由于资源不再锁定到单个主机，集群中的整体资源利用率得到改善。我们通过针对不同场景和拓扑的各种性能评估来证明我们解决方案的灵活性，即设备借用，MDEV 和 API 扩展(以我们的概念验证 NVMe 驱动程序的形式)。通过比较使用远程设备与本地访问的性能，我们的结果表明，除了预期的更长的 PCIe 路径之外，我们不会增加任何性能开销。

While our current system shows great potentials for resource sharing, there are still several areas that still may be investigated. For example, currently only cold migration of VMs with passed-through devices is possible. Adding support for a dynamic migration during runtime (live migration) is desirable. Such a solution would most likely require the development of new hardware as it must support either pausing or re-routing transactions without violating strict ordering required by PCIe. Additionally, our experimental results also show that a major performance bottleneck occurs when DMA read requests traverse the lender's CPU in order for addresses to be resolved by the IOMMU. The Intel Xeon CPUs used in our performance experiments alter the requests in a way that leads to poor link utilization. As our MDEV implementation requires the lender's IOMMU to map guest-physical memory for the device, this warrants further evaluations of alternative CPU architectures. Furthermore, while our proof-of-concept NVMe driver provides block-level access for userspace applications, implementing a file system or coordinating access is currently the responsibility of the application. Another candidate for improvement is therefore to implement the sharing idea in a kernel space driver, making it possible to implement a shared-disk file system for general use. As the device is shared on the queue-level, this solution could easily co-exist with the existing userspace implementation, and we could assign queues to both application instances and kernel drivers alike. Finally, as the Intel P4800X NVMe used in our queue-sharing experiments did not perform adequately, it would prove useful to perform a large-scale evaluation of our queue-sharing concept using a newer PCIe Gen4 NVMe with greater bandwidth capacity and support for a higher number of queues.

> 当前的系统已经显示出资源共享的巨大潜力，但仍有几个领域需要进一步研究。例如，目前只能进行带有传递设备的虚拟机的冷迁移。增加运行时(实时迁移)动态迁移的支持是可取的。这样的解决方案很可能需要开发新的硬件，因为它必须支持暂停或重新路由事务，而不违反 PCIe 的严格排序要求。此外，我们的实验结果还表明，当 DMA 读取请求穿过出借人的 CPU 以便 IOMMU 解析地址时，会出现主要性能瓶颈。我们的 MDEV 实现需要出借人的 IOMMU 来映射设备的虚拟物理内存，因此有必要进一步评估替代的 CPU 架构。此外，虽然我们的 NVMe 驱动程序的概念验证提供了用户空间应用程序的块级访问，但实现共享思想的文件系统或协调访问目前仍然是应用程序的责任。因此，另一个改进的候选是在内核空间驱动程序中实现共享思想，使得实现共享磁盘文件系统变得可能。由于设备是在队列级别共享的，因此这种解决方案可以很容易地与现有的用户空间实现共存，我们可以将队列分配给应用程序实例和内核驱动程序。最后，由于我们在队列共享实验中使用的 Intel P4800X NVMe 性能不佳，因此使用更新的 PCIe Gen4 NVMe(具有更高的带宽容量和支持更多队列的能力)进行大规模评估我们的队列共享概念将是有用的。

## Availability

The source code of our proof-of-concept distributed NVMe driver is licensed using the BSD software license, and is available at the following URL: [https://github.com/enfiskutensykkel/ssd-gpu-dma/](https://github.com/enfiskutensykkel/ssd-gpu-dma/). The source code of the ping-pong CUDA program used in our latency evaluation can be found at the following URL: [https://gist.github.com/enfiskutensykkel/2b0f7afcb35d12477165746f062c7453](https://gist.github.com/enfiskutensykkel/2b0f7afcb35d12477165746f062c7453). The datasets and benchmarking results in this article are available from the corresponding author upon request.

> 源代码我们的概念验证分布式 NVMe 驱动程序使用 BSD 软件许可证授权，可以在以下 URL 获取：[https://github.com/enfiskutensykkel/ssd-gpu-dma/](https://github.com/enfiskutensykkel/ssd-gpu-dma/)。我们延迟评估中使用的 ping-pong CUDA 程序的源代码可以在以下 URL 找到：[https://gist.github.com/enfiskutensykkel/2b0f7afcb35d12477165746f062c7453](https://gist.github.com/enfiskutensykkel/2b0f7afcb35d12477165746f062c7453)。本文中的数据集和基准测试结果可以向相关作者要求获取。

## ACKNOWLEDGMENTS

The authors thank the Dolphin Interconnect Solutions developers team, particularly Eivind Bergem and Eivind Eriksen for their input on data visualization. We also thank Friedrich Seifert, Preben N. Olsen, and Calin Iaru for their feedback on the manuscript. The authors also thank Hugo Kohmann and Roy Nordstrøm. Finally, we give a big thank you to all the anonymous reviewers. They have had a tedious task reviewing this long manuscript but still have provided a list of valuable insights and suggestions that greatly improved this article.

> 作者们要感谢 Dolphin Interconnect Solutions 开发团队，特别是 Eivind Bergem 和 Eivind Eriksen，他们在数据可视化方面提供了输入。我们也要感谢 Friedrich Seifert，Preben N. Olsen 和 Calin Iaru，他们对手稿提供了反馈。作者们还要感谢 Hugo Kohmann 和 Roy Nordstrøm。最后，我们要对所有匿名审稿人表示由衷的感谢。他们审阅了这篇冗长的手稿，但仍然提供了一系列有价值的见解和建议，大大改善了本文。

## REFERENCES

- Keras. [n.d.]. Retrieved from [https://keras.io](https://keras.io). Navigate tocitation 1citation 2

> Keras。[n.d.]。从[https://keras.io](https://keras.io)检索。导航到引文 1 引文 2。

- TensorFlow. [n.d.]. Large-Scale Machine Learning on Heterogeneous Systems. Retrieved from [https://www.tensorflow.org/](https://www.tensorflow.org/). Navigate tocitation 1citation 2

> TensorFlow。[n.d.]。在异构系统上进行大规模机器学习。从[https://www.tensorflow.org/](https://www.tensorflow.org/)检索。导航 tocitation 1citation 2

- Darren Abramson, Jeff Jackson, Sridhar Muthrasanallur, Gil Neiger, Greg Regnier, Rajes Sankaran, Ioannis Schoinas, Rich Uhlig, Balaji Vembu, and John Weigert. 2006. Intel virtualization technology for directed I/O. _Intel Technol. J._ 10, 03 (2006). DOI: [https://doi.org/10.1535/itj.1003.02](https://doi.org/10.1535/itj.1003.02) Navigate tocitation 1

> Darren Abramson、Jeff Jackson、Sridhar Muthrasanallur、Gil Neiger、Greg Regnier、Rajes Sankaran、Ioannis Schoinas、Rich Uhlig、Balaji Vembu 和 John Weigert。2006。Intel 虚拟化技术用于定向 I/O。_Intel Technol. J._ 10, 03(2006)。DOI：[https://doi.org/10.1535/itj.1003.02](https://doi.org/10.1535/itj.1003.02) 转到引用 1

- Ahmed Abulila, Vikram Sharma Mailthody, Zaid Qureshi, Jian Huang, Nam Sung Kim, Jinjun Xiong, and Wen-mei Hwu. 2019. FlatFlash: Exploiting the byte-accessibility of SSDs within a unified memory-storage hierarchy. In _Proceedings of the International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS)_. 971–985. DOI: [https://doi.org/10.1145/3297858.3304061](https://doi.org/10.1145/3297858.3304061) Navigate tocitation 1

> Ahmed Abulila、Vikram Sharma Mailthody、Zaid Qureshi、Jian Huang、Nam Sung Kim、Jinjun Xiong 和 Wen-mei Hwu。2019。FlatFlash：在统一的内存存储层次中利用 SSD 的字节可访问性。在编程语言和操作系统的架构支持国际会议(ASPLOS)上。971-985。DOI：[https://doi.org/10.1145/3297858.3304061](https://doi.org/10.1145/3297858.3304061) 转到引文 1

- Marcos K. Aguilera, Nadav Amit, Irina Calciu, Xavier Deguillard, Jayneel Gandhi, Stanko Novaković, Arun Ramanathan, Pratap Subrahmanyam, Lalith Suresh, Kiran Tati, Rajesh Venkatasubramanian, and Michael Wei. 2018. Remote regions: A simple abstraction for remote memory. In _Proceedings of the USENIX Annual Technical Conference (ATC’18)_. 775–787. Navigate tocitation 1

> 马科斯·K·阿吉莱拉、纳达夫·阿米特、伊琳娜·卡尔西乌、泽维尔·德古伊亚、詹尼尔·甘地、斯坦科·诺瓦科维奇、阿伦·拉马纳森、普拉塔普·苏布拉曼尼姆、拉利斯·苏雷什、基兰·塔蒂、拉杰什·文卡塔苏布拉曼尼安和迈克尔·魏。2018 年。远程区域：远程存储的简单抽象。在美国电子信息交换协会年度技术会议(ATC'18)上。775-787。转到引文 1。

- Knut Alnæs, Ernst H. Kristiansen, David B. Gustavson, and David V. James. 1990. Scalable coherent interface. In _Proceedings of the International Conference on Computer Systems and Software Engineering (CompEuro’90)_. 446–453. DOI: [https://doi.org/10.1109/CMPEUR.1990.113656](https://doi.org/10.1109/CMPEUR.1990.113656) Navigate tocitation 1

> Knut Alnæs、Ernst H. Kristiansen、David B. Gustavson 和 David V. James。1990 年。可扩展的相干接口。在《计算机系统与软件工程国际会议(CompEuro'90)》的论文集中。446-453。DOI：[https://doi.org/10.1109/CMPEUR.1990.113656](https://doi.org/10.1109/CMPEUR.1990.113656) 转到引文 1。

- Nadav Amit, Muli Ben-Yehuda, and Ben-Ami Yassour. 2010. IOMMU: Strategies for mitigating the IOTLB bottleneck. In _Proceedings of the International Symposium on Computer Architecture_( ISCA ’10). Springer, 256–274. DOI: [https://doi.org/10.1007/978-3-642-24322-6_22](https://doi.org/10.1007/978-3-642-24322-6_22) Navigate tocitation 1

> Nadav Amit、Muli Ben-Yehuda 和 Ben-Ami Yassour。2010 年。IOMMU：缓解 IOTLB 瓶颈的策略。在*计算机体系结构国际研讨会(ISCA'10)*上。斯普林格，256-274。DOI：[https://doi.org/10.1007/978-3-642-24322-6_22](https://doi.org/10.1007/978-3-642-24322-6_22) 转到引文 1

- Eric A. Anderson and Jeanna M. Neefe. 1994. _An Exploration of Network RAM_. Technical Report . EECS Department, University of California. Retrieved from [https://www2.eecs.berkeley.edu/Pubs/TechRpts/1998/CSD-98-1000.pdf](https://www2.eecs.berkeley.edu/Pubs/TechRpts/1998/CSD-98-1000.pdf). Navigate tocitation 1

> Eric A. Anderson 和 Jeanna M. Neefe. 1994. 《网络 RAM 探索》。技术报告。加州大学 EECS 系。从[https://www2.eecs.berkeley.edu/Pubs/TechRpts/1998/CSD-98-1000.pdf](https://www2.eecs.berkeley.edu/Pubs/TechRpts/1998/CSD-98-1000.pdf)获取。转到引文 1。

- Jens Axboe. [n.d.]. Flexible I/O Tester. Retrieved from [https://github.com/axboe/fio](https://github.com/axboe/fio). Navigate tocitation 1citation 2

> Jens Axboe。[n.d.]。灵活的 I/O 测试器。从[https://github.com/axboe/fio](https://github.com/axboe/fio)检索。导航到引文 1 引文 2。

- Stephen Bates. 2015. Project Donard. Retrieved from [https://github.com/sbates130272/donard](https://github.com/sbates130272/donard). Navigate tocitation 1

> 史蒂芬·贝茨(Stephen Bates)。2015 年。多纳德项目(Project Donard)。从[https://github.com/sbates130272/donard](https://github.com/sbates130272/donard)获取。转到引文 1。

- Shai Bergman, Tanya Brokhman, Tzachi Cohen, and Mark Silberstein. 2017. SPIN: Seamless operating system integration of peer-to-peer DMA between SSDs and GPUs. In _Proceedings of the USENIX Annual Technical Conference (ATC’17)_. 665–676. Navigate tocitation 1citation 2

> Shai Bergman、Tanya Brokhman、Tzachi Cohen 和 Mark Silberstein。2017 年。SPIN：SSD 和 GPU 之间的点对点 DMA 无缝操作系统集成。在*USENIX 年度技术会议(ATC'17)*上。665-676。导航到引用 1 引用 2

- Maciej Bielski, Christian Pinto, Daniel Raho, and Renaud Pacalet. 2016. Survey on memory and devices disaggregation solutions for HPC systems. In _Proceedings of the International Conference on Computational Science and Engineering and International Conference on Embedded and Ubiquitous Computing and International Symposium on Distributed Computing and Applications for Business Engineering (CSE-EUC-DCABES’16)_. 197–204. DOI: [https://doi.org/10.1109/CSE-EUC-DCABES.2016.185](https://doi.org/10.1109/CSE-EUC-DCABES.2016.185) Navigate tocitation 1

> Maciej Bielski、Christian Pinto、Daniel Raho 和 Renaud Pacalet。2016 年。关于高性能计算系统的内存和设备分解解决方案的调查。在*计算科学与工程国际会议和嵌入式和无处不在的计算国际会议和分布式计算和业务工程应用研讨会(CSE-EUC-DCABES'16)*上。197-204。DOI：[https://doi.org/10.1109/CSE-EUC-DCABES.2016.185](https://doi.org/10.1109/CSE-EUC-DCABES.2016.185)转到引文 1

- Broadcom. 2011. PEX8733, PCI Express Gen 3 Switch, 32 Lanes, 18 Ports. Retrieved from [https://docs.broadcom.com/docs/12351852](https://docs.broadcom.com/docs/12351852). Navigate tocitation 1citation 2citation 3

> Broadcom，2011 年。PEX8733，PCI Express Gen 3 交换机，32 轨道，18 端口。从[https://docs.broadcom.com/docs/12351852](https://docs.broadcom.com/docs/12351852)获取。导航到引文 1 引文 2 引文 3。

- Broadcom. 2012. PEX8796, PCI Express Gen 3 Switch, 96 Lanes, 24 Ports. Retrieved from [https://docs.broadcom.com/docs/12351860](https://docs.broadcom.com/docs/12351860). Navigate tocitation 1

> Broadcom，2012。PEX8796，PCI Express Gen 3 交换机，96 轨道，24 端口。从[https://docs.broadcom.com/docs/12351860](https://docs.broadcom.com/docs/12351860)获取。导航到引文 1。

- I.-Hsin Chung, Bulent Abali, and Paul Crumley. 2018. Towards a composable computer system. In _Proceedings of the International Conference on High Performance Computing in Asia-Pacific Region (HPCAsia’18)_. 137–147. DOI: [https://doi.org/10.1145/3149457.3149466](https://doi.org/10.1145/3149457.3149466) Navigate tocitation 1citation 2

> I.-Hsin Chung、Bulent Abali 和 Paul Crumley. 2018. 朝向可组合计算机系统. 在《亚太地区高性能计算国际会议(HPCAsia'18)论文集》中，137-147 页。DOI: [https://doi.org/10.1145/3149457.3149466](https://doi.org/10.1145/3149457.3149466) 导航参考文献 1 参考文献 2

- Adam Coates, Brody Huval, Tao Wang, David J. Wu, Andrew Y. Ng, and Bryan Catanzaro. 2013. Deep learning with COTS HPC systems. In _Proceedings of the International Conference on Machine Learning (ICML’13)_. 1337–1345. Navigate tocitation 1

> 阿当·科茨、布罗迪·胡瓦尔、陶·王、大卫·J·吴、安德鲁·Y·Ng 和布莱恩·卡坦扎罗。2013 年。使用 COTS HPC 系统进行深度学习。在*机器学习国际会议(ICML'13)*上。1337-1345。转到引文 1。

- Intel Corporation. 2015. Intel Rack Scale Design. Retrieved from [https://www.intel.com/content/www/us/en/architecture-and-technology/rack-scale-design-overview.html](https://www.intel.com/content/www/us/en/architecture-and-technology/rack-scale-design-overview.html). Navigate tocitation 1

> Intel Corporation. 2015. Intel 机架规模设计。从[https://www.intel.com/content/www/us/en/architecture-and-technology/rack-scale-design-overview.html](https://www.intel.com/content/www/us/en/architecture-and-technology/rack-scale-design-overview.html)检索。转到引文 1。

- Liqid Corporation. [n.d.]. Liqid Composable Infrastructure. Retrieved from [https://www.liqid.com/](https://www.liqid.com/). Navigate tocitation 1

> - Liqid Corporation(无日期)。Liqid 可组合基础架构。从[https://www.liqid.com/](https://www.liqid.com/)获取。转到引用 1

- Alexandros Daglis, Stanko Novaković, Edouard Bugnion, Babak Falsafi, and Boris Grot. 2015. Manycore network interfaces for in-memory rack-scale computing. _ACM SIGARCH Comput. Architect. News_ 43, 3 (2015), 567–579. DOI: [https://doi.org/10.1145/2872887.2750415](https://doi.org/10.1145/2872887.2750415) Navigate tocitation 1

> Alexandros Daglis、Stanko Novaković、Edouard Bugnion、Babak Falsafi 和 Boris Grot。 2015。 内存机架级计算的多核网络接口。 _ACM SIGARCH Comput. Architect. News_ 43, 3(2015)，567–579。 DOI：[https://doi.org/10.1145/2872887.2750415](https://doi.org/10.1145/2872887.2750415) 导航到引文 1

- Jia Deng, Wei Dong, Richard Socher, Li-Jia Li, Kai Li, and Li Fei-Fei. 2009. ImageNet: A large-scale hierarchical image database. In _Proceedings of the Conference on Computer Vision and Pattern Recognition (CVPR’09)_. 248–255. DOI: [https://doi.org/10.1109/CVPR.2009.5206848](https://doi.org/10.1109/CVPR.2009.5206848) Navigate tocitation 1

> 贾登、魏东、理查德·索舍尔、李佳莉、李凯、李飞飞。2009 年。ImageNet：一个大型分层图像数据库。在计算机视觉和模式识别会议(CVPR'09)上。248-255。DOI：[https://doi.org/10.1109/CVPR.2009.5206848](https://doi.org/10.1109/CVPR.2009.5206848)转到引文 1

- Dolphin Interconnect Solutions. [n.d.]. SFF-8644 MiniSAS-HD PCIe Gen3 cables. Retrieved from [https://www.dolphinics.com/products/PCI_Express_SFF-8644_cables.html](https://www.dolphinics.com/products/PCI_Express_SFF-8644_cables.html). Navigate tocitation 1

> 蓝鲸互联解决方案。[n.d.]。SFF-8644 MiniSAS-HD PCIe Gen3 电缆。检索自[https://www.dolphinics.com/products/PCI_Express_SFF-8644_cables.html](https://www.dolphinics.com/products/PCI_Express_SFF-8644_cables.html)。转到引文 1。

- Dolphin Interconnect Solutions [n.d.]. _SISCI API Documentation_. Dolphin Interconnect Solutions. Retrieved from [http://ww.dolphinics.no/download/SISCI_DOC_V2/](http://ww.dolphinics.no/download/SISCI_DOC_V2/). Navigate tocitation 1citation 2citation 3citation 4citation 5

> 解决方案海豚互联[n.d.]. _SISCI API 文档_. 海豚互联解决方案。从[http://ww.dolphinics.no/download/SISCI_DOC_V2/](http://ww.dolphinics.no/download/SISCI_DOC_V2/)检索。 导航引文 1 引文 2 引文 3 引文 4 引文 5

- José Duato, Antonio J. Pena, Frederico Silla, Rafael Mayo, and Enrique S. Quintana-Ortí. 2010. rCUDA: Reducing the number of GPU-based accelerators in high performance clusters. In _Proceedings of the International Conference on High Performance Computing and Simulation (HPCS’10)_. 224–231. DOI: [https://doi.org/10.1109/HPCS.2010.5547126](https://doi.org/10.1109/HPCS.2010.5547126) Navigate tocitation 1citation 2

> - José Duato、Antonio J. Pena、Frederico Silla、Rafael Mayo 和 Enrique S. Quintana-Ortí。2010。rCUDA：在高性能集群中减少 GPU 加速器的数量。在*高性能计算和模拟国际会议(HPCS'10)*上。224-231。DOI：[https://doi.org/10.1109/HPCS.2010.5547126](https://doi.org/10.1109/HPCS.2010.5547126) 转到引文 1 引文 2

- Michael J. Feeley, William E. Morgan, Frederic H. Pighin, Anna R. Karlin, and Henry M. Levy. 1995. Implementing global memory management in a workstation cluster. In _Proceedings of the ACM Symposium on Operating Systems Principles (SOSP’95)_. 201–212. DOI: [https://doi.org/10.1145/224056.224072](https://doi.org/10.1145/224056.224072) Navigate tocitation 1citation 2

> 麦克尔·J·费利、威廉·E·摩根、弗雷德里克·H·皮金、安娜·R·卡林和亨利·M·利维。1995 年。在工作站集群中实现全局内存管理。在 ACM 操作系统原理研讨会(SOSP'95)上。201-212。DOI：[https://doi.org/10.1145/224056.224072](https://doi.org/10.1145/224056.224072)转到引文 1 引文 2

- Trevor Fountain, Alexandra McCarthy, and Fangfang Peng. 2005. PCI express: An overview of PCI express, cabled PCI express and PXI express. In _Proceedings of the International Conference on Accelerator & Large Experimental Physics Control Systems_( ICALEPCS ’05). Navigate tocitation 1

> 特雷弗·芳特，亚历山德拉·麦卡锡和彭芳芳。2005 年。PCI Express：PCI Express，有线 PCI Express 和 PXI Express 概述。在国际加速器和大型实验物理控制系统会议(ICALEPCS'05)上。导航到引用 1。

- Juncheng Gu, Youngmoon Lee, Yiwen Zhang, Mosharaf Chowdury, and Kang G. Shin. 2017. Efficient memory disaggregation with INFINISWAP. In _Proceedings of the Symposium on Networked Systems Design and Implementation (NSDI’17)_. 649–667. Navigate tocitation 1citation 2

> Juncheng Gu、Youngmoon Lee、Yiwen Zhang、Mosharaf Chowdury 和 Kang G. Shin。2017 年。使用 INFINISWAP 进行高效的内存分解。在《网络系统设计与实施研讨会(NSDI'17)》的论文集中。649-667。转到引用 1 引用 2。

- Anubhav Guleria, J. Lakshmi, and Chakri Padala. 2019. EMF: Disaggregated GPUs in datacenters for efficiency, modularity and flexibility. In _Proceedings of the International Conference on Cloud Computing in Emerging Markets (CCEM’19)_. 1–8. DOI: [https://doi.org/10.1109/CCEM48484.2019.000-5](https://doi.org/10.1109/CCEM48484.2019.000-5) Navigate tocitation 1

> Anubhav Guleria、J. Lakshmi 和 Chakri Padala。2019 年。EMF：数据中心中的分解 GPU 以提高效率、模块化和灵活性。在“新兴市场云计算国际会议(CCEM'19)”上发表。1-8。DOI：[https://doi.org/10.1109/CCEM48484.2019.000-5](https://doi.org/10.1109/CCEM48484.2019.000-5) 转到引文 1

- Zvika Guz, Harry Li, Anahita Shayesteh, and Vijay Balakrishnan. 2017. NVMe-over-fabrics performance characterization and the path to low-overhead flash disaggregation. In _Proceedings of the International Systems and Storage Conference (SYSTOR’17)_. 1–9. DOI: [https://doi.org/10.1145/3078468.3078483](https://doi.org/10.1145/3078468.3078483) Navigate tocitation 1

> Zvika Guz、Harry Li、Anahita Shayesteh 和 Vijay Balakrishnan。 2017。 NVMe-over-fabrics 性能表征及低开销闪存分散的路径。 在国际系统和存储会议(SYSTOR'17)上。 1-9。 DOI：[https://doi.org/10.1145/3078468.3078483](https://doi.org/10.1145/3078468.3078483) 转到引文 1

- Zvika Guz, Harry Li, Anahita Shayesteh, and Vijay Balkrishnan. 2018. Performance characterization of NVMe-over-fabrics storage disaggregation. _ACM Trans. Stor._ 14, 4 ( Dec. 2018), 1–18. DOI: [https://doi.org/10.1145/3239563](https://doi.org/10.1145/3239563) Navigate tocitation 1citation 2citation 3

> Zvika Guz、Harry Li、Anahita Shayesteh 和 Vijay Balkrishnan。2018。NVMe-over-Fabrics 存储分离的性能特性。_ACM Trans. Stor._ 14，4(2018 年 12 月)，1-18。DOI：[https://doi.org/10.1145/3239563](https://doi.org/10.1145/3239563)

- Steven Alexander Hicks, Michael Riegler, Konstantin Pogorelov, Kim V. Ånonsen, Thomas de Lange, Dag Johansen, Mattis Jeppsson, Kristin Ranheim Randel, Sigrun Eskeland, and Pål Halvorsen. 2018. Dissecting deep neural networks for better medical image classification and classification understanding. In _Proceedings of the International Symposium on Computer-Based Medical Systems (CBMS’18)_. 363–368. DOI: [https://doi.org/10.1109/CBMS.2018.00070](https://doi.org/10.1109/CBMS.2018.00070) Navigate tocitation 1

> Steven Alexander Hicks、Michael Riegler、Konstantin Pogorelov、Kim V. Ånonsen、Thomas de Lange、Dag Johansen、Mattis Jeppsson、Kristin Ranheim Randel、Sigrun Eskeland 和 Pål Halvorsen。2018 年。深度神经网络的解剖，以改善医学图像分类和分类理解。在*计算机医学系统国际研讨会(CBMS'18)*上。363-368. DOI: [https://doi.org/10.1109/CBMS.2018.00070](https://doi.org/10.1109/CBMS.2018.00070) 导航到引用 1

- Rui Hou, Tao Jiang, Liuhang Zhang, Pengfei Qi, Jianbo Dong, Haibin Wang, Xiongli Gu, and Shujie Zhang. 2013. Cost effective data center servers. In _Proceedings of the International Symposium on High Performance Computer Architecture (HPCA’13)_. 179–187. DOI: [https://doi.org/10.1109/HPCA.2013.6522317](https://doi.org/10.1109/HPCA.2013.6522317) Navigate tocitation 1citation 2

> 鲁依侯、陶姜、刘航章、齐鹏飞、董建波、王海斌、顾雄力和张书杰。2013 年。成本效益的数据中心服务器。在国际高性能计算机体系结构研讨会(HPCA'13)上。179-187。DOI：[https://doi.org/10.1109/HPCA.2013.6522317](https://doi.org/10.1109/HPCA.2013.6522317) 导航引文 1 引文 2

- Jian Huang, Xiangyong Ouyang, Jithin Jose, Md Wasi-Ur-Rahman, Hao Wang, Miao Luo, Hari Subramoni, Chet Murthy, and Dhabaleswar K. Panda. 2012. High-performance design of hbase with RDMA over InfiniBand. In _Proceedings of the International Parallel and Distributed Processing Symposium (IPDPS’12)_. 774–785. DOI: [https://doi.org/10.1109/IPDPS.2012.74](https://doi.org/10.1109/IPDPS.2012.74) Navigate tocitation 1citation 2citation 3

> 黄健、欧阳翔勇、Jithin Jose、Md Wasi-Ur-Rahman、王浩、罗苗、Hari Subramoni、Chet Murthy 和 Dhabaleswar K. Panda。2012 年。使用 RDMA over InfiniBand 的 HBase 的高性能设计。在*国际并行和分布式处理研讨会(IPDPS'12)*上。774-785。DOI：[https://doi.org/10.1109/IPDPS.2012.74](https://doi.org/10.1109/IPDPS.2012.74) 导航引文 1 引文 2 引文 3

- Neo Jia and Kirti Wankhede. 2016. VFIO Mediated Devices. Retrieved from [https://www.kernel.org/doc/Documentation/vfio-mediated-device.txt](https://www.kernel.org/doc/Documentation/vfio-mediated-device.txt). Navigate tocitation 1

> Neo Jia 和 Kirti Wankhede。 2016。 VFIO 介导设备。 从[https://www.kernel.org/doc/Documentation/vfio-mediated-device.txt](https://www.kernel.org/doc/Documentation/vfio-mediated-device.txt)获取。 导航到引文 1

- Weihang Jiang, Jiuxing Liu, Hyun-Wook Jin, Dhabaleswar K. Panda, William Gropp, and Rajeev Thakur. 2004. High performance MPI-2 one-sided communication over InfiniBand. In _Proceedings of the International Symposium on Cluster Computing and the Grid (CCGrid’04)_. 531–538. DOI: [https://doi.org/10.1109/CCGrid.2004.1336648](https://doi.org/10.1109/CCGrid.2004.1336648) Navigate tocitation 1

> Weihang Jiang, Jiuxing Liu, Hyun-Wook Jin, Dhabaleswar K. Panda, William Gropp 和 Rajeev Thakur。2004 年。在 InfiniBand 上的高性能 MPI-2 单向通信。在*集群计算和网格国际研讨会(CCGrid'04)*上的论文。531-538. DOI：[https://doi.org/10.1109/CCGrid.2004.1336648](https://doi.org/10.1109/CCGrid.2004.1336648) 导航到引用 1

- Linux kernel development community. [n.d.]. NTB Drivers. Retrieved from [https://www.kernel.org/doc/html/latest/driver-api/ntb.html](https://www.kernel.org/doc/html/latest/driver-api/ntb.html). Navigate tocitation 1

> 社区开发的 Linux 内核。[n.d.]. NTB 驱动程序。从[https://www.kernel.org/doc/html/latest/driver-api/ntb.html](https://www.kernel.org/doc/html/latest/driver-api/ntb.html)获取。导航到引文 1

- Linux kernel development community. 2013. Linux Filesystems API. Retrieved from [https://www.kernel.org/doc/htmldocs/filesystems/index.html](https://www.kernel.org/doc/htmldocs/filesystems/index.html). Navigate tocitation 1

> 社区开发的 Linux 内核。2013。Linux 文件系统 API。从[https://www.kernel.org/doc/htmldocs/filesystems/index.html](https://www.kernel.org/doc/htmldocs/filesystems/index.html)获取。转到引用 1

- Linux kernel development community. 2013. VFIO—“Virtual Function I/O.” Retrieved from [https://www.kernel.org/doc/Documentation/vfio.txt](https://www.kernel.org/doc/Documentation/vfio.txt). Navigate tocitation 1

> 社区开发的 Linux 内核。2013 年。VFIO——“虚拟功能 I/O”。从[https://www.kernel.org/doc/Documentation/vfio.txt](https://www.kernel.org/doc/Documentation/vfio.txt)获取。转到引用 1

- Linux kernel development community. 2019. Linux IOMMU Support. Retrieved from [https://www.kernel.org/doc/Documentation/Intel-IOMMU.txt](https://www.kernel.org/doc/Documentation/Intel-IOMMU.txt). Navigate tocitation 1

> 社区开发的 Linux 内核。2019 年。Linux IOMMU 支持。从[https://www.kernel.org/doc/Documentation/Intel-IOMMU.txt](https://www.kernel.org/doc/Documentation/Intel-IOMMU.txt)获取。导航到引文 1

- Hyeong-Jun Kim, Young-Sik Lee, and Jin-Soo Kim. 2016. NVMeDirect: A user-space I/O framework for application-specific optimization on NVMe SSDs. In _Proceedings of the USENIX Workshop on Hot Topics in Storage and File Systems (HotStorage’16)_. 41–45. Navigate tocitation 1

> 黄振俊、李永植和金俊秀。2016 年。NVMeDirect：NVMe SSD 上应用特定优化的用户空间 I/O 框架。在《USENIX 存储和文件系统热点主题研讨会(HotStorage'16)》上。41-45。转到引文 1。

- KaiGai Kohei. 2016. GpuScan + SSD-to-GPUDirect DMA. Retrieved from [https://kaigai.hatenablog.com/entry/2016/09/08/003556](https://kaigai.hatenablog.com/entry/2016/09/08/003556). Navigate tocitation 1

> KaiGai Kohei. 2016. GPUScan + SSD-to-GPUDirect DMA。从[https://kaigai.hatenablog.com/entry/2016/09/08/003556](https://kaigai.hatenablog.com/entry/2016/09/08/003556)获取。转到引用 1。

- Lars Bjørlykke Kristiansen, Jonas Markussen, Håkon Kvale Stensland, Michael Riegler, Hugo Kohmann, Friedrich Seifert, Roy Nordstrøm, Carsten Griwodz, and Pål Halvorsen. 2016. Device lending in PCI express networks. In _Proceedings of the International Workshop on Network and Operating Systems Support for Digital Audio and Video (NOSSDAV’16)_. 10:1–10:6. DOI: [https://doi.org/10.1145/2910642.2910650](https://doi.org/10.1145/2910642.2910650) Navigate tocitation 1citation 2citation 3

> Lars Bjørlykke Kristiansen、Jonas Markussen、Håkon Kvale Stensland、Michael Riegler、Hugo Kohmann、Friedrich Seifert、Roy Nordstrøm、Carsten Griwodz 和 Pål Halvorsen。2016。PCI Express 网络中的设备借用。在《数字音频和视频网络和操作系统支持国际研讨会(NOSSDAV'16)》上。10：1-10：6。DOI：[https://doi.org/10.1145/2910642.2910650](https://doi.org/10.1145/2910642.2910650) 导航到引文 1 引文 2 引文 3

- Shuang Liang, Ranjit Noronha, and Dhabaleswar K. Panda. 2005. Swapping to remote memory over Infiniband: An approach using a high performance network block device. In _Proceedings of the IEEE International Conference on Cluster Computing (Cluster’05)_. 1–10. DOI: [https://doi.org/10.1109/CLUSTR.2005.347050](https://doi.org/10.1109/CLUSTR.2005.347050) Navigate tocitation 1citation 2

> - 双亮、兰吉特·诺伦尼亚和达巴勒斯瓦·K·潘达。2005 年。通过 Infiniband 将内存交换到远程内存：使用高性能网络块设备的一种方法。在 IEEE 国际集群计算会议(Cluster'05)上。1-10。DOI：[https://doi.org/10.1109/CLUSTR.2005.347050](https://doi.org/10.1109/CLUSTR.2005.347050)转到引文 1 引文 2

- Kevin Lim, Jichuan Chang, Trevor Mudge, Parthasarathy Ranganathan, Steven K. Reinhardt, and Thomas F. Wenisch. 2009. Disaggregated memory for expansion and sharing in blade servers. In _Proceedings of the the Annual International Symposium on Computer Architecture_( ISCA ’09). 267–278. DOI: [https://doi.org/10.1145/1555754.1555789](https://doi.org/10.1145/1555754.1555789) Navigate tocitation 1

> Kevin Lim、Jichuan Chang、Trevor Mudge、Parthasarathy Ranganathan、Steven K. Reinhardt 和 Thomas F. Wenisch。2009 年。刀片服务器中的分解存储器扩展和共享。在*计算机架构年度国际研讨会(ISCA'09)*上发表。267-278。DOI：[https://doi.org/10.1145/1555754.1555789](https://doi.org/10.1145/1555754.1555789) 导航至引文 1。

- Seung-Ho Lim, Ki-Woong Park, and Kwang-Ho Cha. 2019. Developing an OpenSHMEM model over a switchless PCIe non-transparent bridge interface. In _Proceedings of the International Parallel and Distributed Processing Symposium Workshops (IPDPSW’19)_. 593–602. DOI: [https://doi.org/10.1109/IPDPSW.2019.00104](https://doi.org/10.1109/IPDPSW.2019.00104) Navigate tocitation 1citation 2citation 3citation 4

> Seung-Ho Lim、Ki-Woong Park 和 Kwang-Ho Cha。2019。在无开关 PCIe 非透明桥接接口上开发 OpenSHMEM 模型。在*国际并行和分布式处理研讨会工作坊(IPDPSW'19)*上。593-602。DOI：[https://doi.org/10.1109/IPDPSW.2019.00104](https://doi.org/10.1109/IPDPSW.2019.00104) 导航到引文 1 引文 2 引文 3 引文 4

Seung-Ho Lim、Ki-Woong Park 和 Kwang-Ho Cha。2019 年。在无开关 PCIe 非透明桥接接口上开发 OpenSHMEM 模型。发表在《国际并行和分布式处理研讨会工作坊(IPDPSW'19)》上。593-602 页。DOI：[https://doi.org/10.1109/IPDPSW.2019.00104](https://doi.org/10.1109/IPDPSW.2019.00104) 导航到引文 1 引文 2 引文 3 引文 4

- Xiaoyi Lu, Nusrat S. Islam, Md. Wasi-Ur-Rahman, Jithin Jose, Hari Subramoni, Hao Wang, and Dhabaleswar K. Panda. 2013. High-performance design of Hadoop RPC with RDMA over InfiniBand. In _Proceedings of the International Conference on Parallel Processing (ICPP’13)_. 641–650. DOI: [https://doi.org/10.1109/ICPP.2013.78](https://doi.org/10.1109/ICPP.2013.78) Navigate tocitation 1

> Xiaoyi Lu, Nusrat S. Islam, Md. Wasi-Ur-Rahman, Jithin Jose, Hari Subramoni, Hao Wang 和 Dhabaleswar K. Panda。2013 年。利用 InfiniBand 上的 RDMA 进行高性能 Hadoop RPC 设计。在《并行处理国际会议(ICPP'13)》上发表。641-650。DOI：[https://doi.org/10.1109/ICPP.2013.78](https://doi.org/10.1109/ICPP.2013.78) 导航到引用 1

- Evangelos P. Markatos and George Dramitinos. 1996. Implementation of a reliable remote memory pager. In _Proceedings of the USENIX Annual Technical Conference (ATC’96)_. Navigate tocitation 1citation 2

> 张晓晖和乔治·德拉米蒂诺斯(Evangelos P. Markatos 和 George Dramitinos)。1996 年。可靠远程内存分页器的实施。在*美国电子信息交换网年度技术会议(ATC'96)*上。导航 tocitation 1citation 2

- Athanasios Theodore Markettos, Colin Rothwell, Brett F. Gutstein, Allison Pearce, Peter G. Neumann, Simon W. Moore, and Robert N. M. Watson. 2019. Thunderclap: Exploring vulnerabilities in operating system IOMMU protection via DMA from untrustworthy peripherals. In _Proceedings of the Network and Distributed System Security Symposium (NDSS’19)_. DOI: [https://doi.org/10.14722/ndss.2019.23194](https://doi.org/10.14722/ndss.2019.23194) Navigate tocitation 1

> Athanasios Theodore Markettos、Colin Rothwell、Brett F. Gutstein、Allison Pearce、Peter G. Neumann、Simon W. Moore 和 Robert N. M. Watson。2019 年。Thunderclap：通过来自不可信外设的 DMA 探索操作系统 IOMMU 保护中的漏洞。在*Network and Distributed System Security Symposium (NDSS'19)*的会议上。DOI：[https://doi.org/10.14722/ndss.2019.23194](https://doi.org/10.14722/ndss.2019.23194)转到引文 1。

- Jonas Markussen, Lars Bjørlykke Kristiansen, Rune Johan Borgli, Håkon Kvale Stensland, Friedrich Seifert, Michael Riegler, Carsten Griwodz, and Pål Halvorsen. 2020. Flexible device compositions and dynamic resource sharing in PCIe interconnected clusters using Device lending. _Cluster Comput._ 23 (2020), 1211–1234. Issue 2. DOI: [https://doi.org/10.1007/s10586-019-02988-0](https://doi.org/10.1007/s10586-019-02988-0) Navigate tocitation 1citation 2citation 3citation 4citation 5citation 6

> Jonas Markussen、Lars Bjørlykke Kristiansen、Rune Johan Borgli、Håkon Kvale Stensland、Friedrich Seifert、Michael Riegler、Carsten Griwodz 和 Pål Halvorsen。2020 年。使用设备出借在 PCIe 互联集群中实现灵活的设备组合和动态资源共享。_Cluster Comput._ 23(2020)，1211-1234。第 2 期。DOI：[https://doi.org/10.1007/s10586-019-02988-0](https://doi.org/10.1007/s10586-019-02988-0) 导航到引文 1 引文 2 引文 3 引文 4 引文 5 引文 6

- Jonas Markussen, Lars Bjørlykke Kristiansen, Håkon Kvale Stensland, Friedrich Seifert, Carsten Griwodz, and Pål Halvorsen. 2018. Flexible device sharing in PCIe clusters using device lending. In _Proceedings of the International Conference on Parallel Processing Companion (ICPPComp’18)_. Article 48 , 48:1–48:10. DOI: [https://doi.org/10.1145/3229710.3229759](https://doi.org/10.1145/3229710.3229759) Navigate tocitation 1citation 2citation 3citation 4citation 5citation 6

> Jonas Markussen、Lars Bjørlykke Kristiansen、Håkon Kvale Stensland、Friedrich Seifert、Carsten Griwodz 和 Pål Halvorsen。2018 年。使用设备出借在 PCIe 集群中实现灵活的设备共享。在《并行处理国际会议伴侣会议(ICPPComp'18)论文集》中，文章 48，48:1-48:10。DOI：https://doi.org/10.1145/3229710.3229759。

- Vijay Meduri. 2011. A Case for PCI Express as a High-Performance Cluster Interconnect. Retrieved from [https://www.hpcwire.com/2011/01/24/a_case_for_pci_express_as_a_high-performance_cluster_interconnect/](https://www.hpcwire.com/2011/01/24/a_case_for_pci_express_as_a_high-performance_cluster_interconnect/). Navigate tocitation 1citation 2citation 3

> Vijay Meduri，2011 年。支持 PCI Express 作为高性能集群互连的案例。从[https://www.hpcwire.com/2011/01/24/a_case_for_pci_express_as_a_high-performance_cluster_interconnect/](https://www.hpcwire.com/2011/01/24/a_case_for_pci_express_as_a_high-performance_cluster_interconnect/)检索。转到引文 1 引文 2 引文 3

- Microsemi. 2019. _Multi-Host Sharing of NVMe Drives and GPUs Using PCIe Fabrics_. Technical Report . Microsemi. Retrieved from [http://www.symmttm.com/document-portal/doc_download/1244483-multi-host-sharing-of-nvme-drives-and-gpus-using-pcie](http://www.symmttm.com/document-portal/doc_download/1244483-multi-host-sharing-of-nvme-drives-and-gpus-using-pcie). Navigate tocitation 1citation 2

> Microsemi. 2019. 使用 PCIe 网络共享 NVMe 驱动器和 GPU。技术报告。Microsemi。从[http://www.symmttm.com/document-portal/doc_download/1244483-multi-host-sharing-of-nvme-drives-and-gpus-using-pcie](http://www.symmttm.com/document-portal/doc_download/1244483-multi-host-sharing-of-nvme-drives-and-gpus-using-pcie)检索。转到引文 1 引文 2

- Ben-Yehuda Muli, Jon Mason, Orran Krieger, Jimi Xenidis, Leendert Van Doorn, Asit Mallick, Jun Nakijima, and Elsie Wahlig. 2006. Utilizing IOMMUs for virtualization in Linux and Xen. In _Proceedings of the Linux Symposium_. 71–85. Navigate tocitation 1

> 邦-耶胡莫利、乔恩·梅森、奥兰·克里格、吉米·克西尼迪斯、莱恩德特·范多恩、阿西特·马利克、中岛纯、埃尔西·瓦利格。2006 年。利用 IOMMU 进行 Linux 和 Xen 的虚拟化。在*Linux 研讨会*上。71-85。导航到引文 1。

- NVIDIA Corporation 2019. _GPUDirect RDMA Documentation_. NVIDIA Corporation. Retrieved from [https://docs.nvidia.com/cuda/gpudirect-rdma/index.html](https://docs.nvidia.com/cuda/gpudirect-rdma/index.html). Navigate tocitation 1

> NVIDIA 公司 2019。GPUDirect RDMA 文档。NVIDIA 公司。从[https://docs.nvidia.com/cuda/gpudirect-rdma/index.html](https://docs.nvidia.com/cuda/gpudirect-rdma/index.html)获取。导航到引文 1。

- NVIDIA Corporation 2020. _CUDA Toolkit Documentation v11.0.171_. NVIDIA Corporation. Retrieved from [http://docs.nvidia.com/cuda/](http://docs.nvidia.com/cuda/). Navigate tocitation 1

> NVIDIA Corporation 2020. CUDA 工具包文档 v11.0.171。NVIDIA Corporation。从[http://docs.nvidia.com/cuda/](http://docs.nvidia.com/cuda/)获取。导航到引用 1。

- NVM Express 2019. _NVM Express Base Specification_. NVM Express. Retrieved from [https://nvmexpress.org/wp-content/uploads/NVM-Express-1_3d-2019.03.20-Ratified.pdf](https://nvmexpress.org/wp-content/uploads/NVM-Express-1_3d-2019.03.20-Ratified.pdf). Navigate tocitation 1citation 2citation 3

> - NVM Express 2019，《NVM Express 基础规范》，NVM Express，从[https://nvmexpress.org/wp-content/uploads/NVM-Express-1_3d-2019.03.20-Ratified.pdf](https://nvmexpress.org/wp-content/uploads/NVM-Express-1_3d-2019.03.20-Ratified.pdf)获取。 导航引文 1 引文 2 引文 3。

- NVM Express 2019. _NVM Express Over Fabrics_. NVM Express. Retrieved from [https://nvmexpress.org/wp-content/uploads/NVMe-over-Fabrics-1.1-2019.10.22-Ratified.pdf](https://nvmexpress.org/wp-content/uploads/NVMe-over-Fabrics-1.1-2019.10.22-Ratified.pdf). Navigate tocitation 1citation 2

> NVM Express 2019。NVM Express Over Fabrics。NVM Express。从[https://nvmexpress.org/wp-content/uploads/NVMe-over-Fabrics-1.1-2019.10.22-Ratified.pdf](https://nvmexpress.org/wp-content/uploads/NVMe-over-Fabrics-1.1-2019.10.22-Ratified.pdf)检索。导航到引文 1 引文 2。

- Sinno Jialin Pan and Qiang Yang. 2010. A survey on transfer learning. _IEEE Trans. Knowl. Data Eng._ 22, 10 ( Oct. 2010), 1345–1359. DOI: [https://doi.org/10.1109/TKDE.2009.191](https://doi.org/10.1109/TKDE.2009.191) Navigate tocitation 1

> Sinno Jialin Pan 和 Qiang Yang，2010 年。转移学习的调查。_IEEE Trans. Knowl. Data Eng._ 22，10(2010 年 10 月)，1345-1359。 DOI：[https://doi.org/10.1109/TKDE.2009.191](https://doi.org/10.1109/TKDE.2009.191) 导航至引文 1

- Bo Peng, Haozhong Zhang, Jianguo Yao, Yaozu Dong, Yu Xu, and Haibing Guan. 2018. MDev-NVMe: A NVMe storage virtualization solution with mediated pass-through. In _Proceedings of the USENIX Annual Technical Conference (ATC’18)_. 665–676. Navigate tocitation 1

> - Bo Peng, Haozhong Zhang, Jianguo Yao, Yaozu Dong, Yu Xu 和 Haibing Guan。2018。MDev-NVMe：一种基于介导传递的 NVMe 存储虚拟化解决方案。在《美国电子信息学会年度技术会议(ATC'18)》论文集中，665-676 页。转到引用 1。

- Peripheral Component Interconnect Special Interest Group (PCI-SIG) 2008. _Multi-root I/O Virtualization and Sharing Specification_. Peripheral Component Interconnect Special Interest Group (PCI-SIG). Retrieved from [https://www.pcisig.com/specifications/iov/multi-root/](https://www.pcisig.com/specifications/iov/multi-root/). Navigate tocitation 1

> 中文：外围设备互连特别兴趣小组(PCI-SIG)2008 年。《多根 I / O 虚拟化和共享规范》。外围设备互连特别兴趣小组(PCI-SIG)。从[https://www.pcisig.com/specifications/iov/multi-root/](https://www.pcisig.com/specifications/iov/multi-root/)检索。转到引文 1

- Peripheral Component Interconnect Special Interest Group (PCI-SIG) 2009. _Address Translation Services Revision 1.1_. Peripheral Component Interconnect Special Interest Group (PCI-SIG). Retrieved from [https://www.pcisig.com/specifications/iov/ats/](https://www.pcisig.com/specifications/iov/ats/). Navigate tocitation 1citation 2

> PCI-SIG(外围设备组件互连特别兴趣小组)2009 年。《地址转换服务修订 1.1》。PCI-SIG(外围设备组件互连特别兴趣小组)。从[https://www.pcisig.com/specifications/iov/ats/](https://www.pcisig.com/specifications/iov/ats/)获取。导航参考文献 1 参考文献 2。

- Peripheral Component Interconnect Special Interest Group (PCI-SIG) 2010. _PCI Express 3.1 Base Specification_. Peripheral Component Interconnect Special Interest Group (PCI-SIG). Retrieved from [https://pcisig.com/specifications](https://pcisig.com/specifications). Navigate tocitation 1citation 2citation 3

> PCI-SIG(外围设备接口特殊兴趣小组)2010 年。_PCI Express 3.1 基本规范_。PCI-SIG(外围设备接口特殊兴趣小组)。从[https://pcisig.com/specifications](https://pcisig.com/specifications)检索。导航引文 1 引文 2 引文 3。

- Peripheral Component Interconnect Special Interest Group (PCI-SIG) 2010. _Single-root I/O Virtualization and Sharing Specification_. Peripheral Component Interconnect Special Interest Group (PCI-SIG). Retrieved from [https://www.pcisig.com/specifications/iov/single-root/](https://www.pcisig.com/specifications/iov/single-root/). Navigate tocitation 1citation 2

> 中文：外围设备互连特别兴趣小组(PCI-SIG)2010 年。_单根 I / O 虚拟化和共享规范_。外围设备互连特别兴趣小组(PCI-SIG)。从[https://www.pcisig.com/specifications/iov/single-root/](https://www.pcisig.com/specifications/iov/single-root/)检索。导航 tocitation 1citation 2

- Konstantin Pogorelov, Olga Ostroukhova, Mattis Jeppsson, Håvard Espeland, Carsten Griwodz, Thomas de Lange, Dag Johansen, Michael Riegler, and Pål Halvorsen. 2018. Deep learning and hand-crafted feature based approaches for polyp detection in medical videos. In _Proceedings of the International Symposium on Computer-Based Medical Systems (CBMS’18)_. 381–386. DOI: [https://doi.org/10.1109/CBMS.2018.00073](https://doi.org/10.1109/CBMS.2018.00073) Navigate tocitation 1

> Konstantin Pogorelov, Olga Ostroukhova, Mattis Jeppsson, Håvard Espeland, Carsten Griwodz, Thomas de Lange, Dag Johansen, Michael Riegler 和 Pål Halvorsen。2018。深度学习和基于手工特征的医学视频中肿瘤检测方法。在《计算机医学系统国际研讨会(CBMS'18)论文集》中，381-386 页。DOI：[https://doi.org/10.1109/CBMS.2018.00073](https://doi.org/10.1109/CBMS.2018.00073) 转到引用 1

- Konstantin Pogorelov, Kristin Ranheim Randel, Carsten Griwodz, Sigrun Losada Eskeland, Thomas de Lange, Dag Johansen, Concetto Spampinato, Duc-Tien Dang-Nguyen, Mathias Lux, Peter Thelin Schmidt, Michael Riegler, and Pål Halvorsen. 2017. KVASIR: A multi-class image dataset for computer aided gastrointestinal disease detection. In _Proceedings of the ACM Multimedia Systems Conference (MMSys’17)_. 164–169. DOI: [https://doi.org/10.1145/3083187.3083212](https://doi.org/10.1145/3083187.3083212) Navigate tocitation 1

> Konstantin Pogorelov、Kristin Ranheim Randel、Carsten Griwodz、Sigrun Losada Eskeland、Thomas de Lange、Dag Johansen、Concetto Spampinato、Duc-Tien Dang-Nguyen、Mathias Lux、Peter Thelin Schmidt、Michael Riegler 和 Pål Halvorsen。 2017。 KVASIR：一个多类图像数据集，用于计算机辅助胃肠道疾病检测。 在*ACM 多媒体系统会议(MMSys'17)*上。 164-169。 DOI：[https://doi.org/10.1145/3083187.3083212](https://doi.org/10.1145/3083187.3083212) 转到引文 1

- Konstantin Pogorelov, Michael Riegler, Sigrun Eskeland, Thomas de Lange, Dag Johansen, Carsten Griwodz, Peter Thelin Schmidt, and Pål Halvorsen. 2017. Efficient disease detection in gastrointestinal videos–global features versus neural networks. _Multimedia Tools Appl._ 76, 21 (2017), 22493–22525. DOI: [https://doi.org/10.1007/s11042-017-4989-y](https://doi.org/10.1007/s11042-017-4989-y) Navigate tocitation 1

> Konstantin Pogorelov、Michael Riegler、Sigrun Eskeland、Thomas de Lange、Dag Johansen、Carsten Griwodz、Peter Thelin Schmidt 和 Pål Halvorsen。2017 年。胃肠视频中有效的疾病检测——全局特征与神经网络。\_多媒体工具应用\_76，21(2017)，22493-22525。DOI：[https://doi.org/10.1007/s11042-017-4989-y](https://doi.org/10.1007/s11042-017-4989-y) 转到引文 1

- Konstantin Pogorelov, Michael Riegler, Jonas Markussen, Mathias Kux, Håkon Kvale Stensland, Thomas Lange, Carsten Griwodz, Pål Halvorsen, Dag Johansen, Peter Schmidt, and Sigrun Eskeland. 2016. Efficient processing of videos in a multi auditory environment using device lending of GPUs. In _Proceedings of the International Conference on Multimedia Systems (MMSys’16)_. 381–386. DOI: [https://doi.org/10.1145/2910017.2910636](https://doi.org/10.1145/2910017.2910636) Navigate tocitation 1

> Konstantin Pogorelov、Michael Riegler、Jonas Markussen、Mathias Kux、Håkon Kvale Stensland、Thomas Lange、Carsten Griwodz、Pål Halvorsen、Dag Johansen、Peter Schmidt 和 Sigrun Eskeland。 2016。使用 GPU 设备借用有效地处理多听觉环境中的视频。在《多媒体系统国际会议(MMSys'16)》论文集中，381-386 页。DOI：[https://doi.org/10.1145/2910017.2910636](https://doi.org/10.1145/2910017.2910636) 转到引文 1

- Murali Ravindran. 2008. Extending cabled PCI express to connect devices with independent PCI domains. In _Proceedings of the IEEE Systems Conference (SysCon’08)_. 1–7. DOI: [https://doi.org/10.1109/SYSTEMS.2008.4519048](https://doi.org/10.1109/SYSTEMS.2008.4519048) Navigate tocitation 1citation 2citation 3citation 4

> 马里·拉文德兰(Murali Ravindran)。2008。将有线 PCI Express 扩展到连接具有独立 PCI 域的设备。在 IEEE 系统会议(SysCon'08)上。1-7。DOI：[https://doi.org/10.1109/SYSTEMS.2008.4519048](https://doi.org/10.1109/SYSTEMS.2008.4519048) 导航引文 1 引文 2 引文 3 引文 4

- Carlos Reaño, Federico Silla, and José Duato. 2017. Enhancing the rCUDA remote GPU virtualization framework: From a prototype to a production solution. In _Proceedings of the International Symposium on Cluster, Cloud and Grid Computing (CCGRID’17)_. 695–698. DOI: [https://doi.org/10.1109/CCGRID.2017.42](https://doi.org/10.1109/CCGRID.2017.42) Navigate tocitation 1citation 2

> Carlos Reaño、Federico Silla 和 José Duato。2017 年。增强 rCUDA 远程 GPU 虚拟化框架：从原型到生产解决方案。在*集群、云和网格计算国际研讨会(CCGRID'17)*上。695-698。DOI：[https://doi.org/10.1109/CCGRID.2017.42](https://doi.org/10.1109/CCGRID.2017.42) 导航到引文 1 引文 2

- Jack Regula. 2004. _Using Non-Transparent Bridging in PCI Express Systems_. Whitepaper . PLX Technology/Broadcom. Retrieved from [https://www.digikey.no/no/pdf/b/broadcom/using-non-transparent-bridging-pci](https://www.digikey.no/no/pdf/b/broadcom/using-non-transparent-bridging-pci). Navigate tocitation 1citation 2citation 3citation 4

> 杰克·雷古拉(Jack Regula)。2004 年。《在 PCI Express 系统中使用非透明桥接》。白皮书。PLX Technology/Broadcom。从[https://www.digikey.no/no/pdf/b/broadcom/using-non-transparent-bridging-pci](https://www.digikey.no/no/pdf/b/broadcom/using-non-transparent-bridging-pci)获取。导航参考文献 1 参考文献 2 参考文献 3 参考文献 4。

- Davide Rosetti. 2014. Benchmarking GPUDirect RDMA on Modern Server Platforms. Retrieved from [https://developer.nvidia.com/blog/benchmarking-gpudirect-rdma-on-modern-server-platforms/](https://developer.nvidia.com/blog/benchmarking-gpudirect-rdma-on-modern-server-platforms/). Navigate tocitation 1citation 2

> Davide Rosetti，2014 年。在现代服务器平台上对 GPUDirect RDMA 进行基准测试。从[https://developer.nvidia.com/blog/benchmarking-gpudirect-rdma-on-modern-server-platforms/](https://developer.nvidia.com/blog/benchmarking-gpudirect-rdma-on-modern-server-platforms/)获取。导航到引文 1 引文 2。

- Andy Rudoff. 2017. Persistent memory programming. _USENIX; login:_ 42, 2 (2017), 34–40. Retrieved from [https://www.usenix.org/system/files/login/articles/login_summer17_07_rudoff.pdf](https://www.usenix.org/system/files/login/articles/login_summer17_07_rudoff.pdf). Navigate tocitation 1

> Andy Rudoff. 2017. 持久性内存编程。《USENIX；登录：》42，2(2017)，34-40。从[https://www.usenix.org/system/files/login/articles/login_summer17_07_rudoff.pdf](https://www.usenix.org/system/files/login/articles/login_summer17_07_rudoff.pdf)获取。转到引文 1。

- Kazuo Saito, Koji Anai, Keiju Igarashi, Takeshi Nishikawa, Ryoichi Himeno, and Kazuhiro Yoguchi. 1998. ATM bus system. U.S. patent No. 5,796,741 A. Navigate tocitation 1

> 齊藤一夫、穴井浩司、五十嵐慶寿、西川健、姫野良一、与口和弘。1998 年。ATM 公共汽車系統。美國專利號碼 5,796,741 A。請前往引用 1。

- Nikolay Sakharnykh. 2016. Beyond GPU Memory Limits with Unified Memory on Pascal. Retrieved from [https://developer.nvidia.com/blog/beyond-gpu-memory-limits-unified-memory-pascal/](https://developer.nvidia.com/blog/beyond-gpu-memory-limits-unified-memory-pascal/). Navigate tocitation 1

> 纳米德尔·萨哈尔尼克(Nikolay Sakharnykh)。2016 年。超越 Pascal GPU 内存限制的统一内存。从[https://developer.nvidia.com/blog/beyond-gpu-memory-limits-unified-memory-pascal/](https://developer.nvidia.com/blog/beyond-gpu-memory-limits-unified-memory-pascal/)获取。转到引文 1。

- Yizhou Shan, Yutong Huang, Yilun Chen, and Yiying Zhang. 2018. LegoOS: A disseminated, distributed OS for hardware resource disaggregation. In _Proceedings of the Conference on Operating Systems Design and Implementation (OSDI’18)_. 69–87. Navigate tocitation 1

> \*叶舟山、黄宇彤、陈一伦和张怡颖。 2018。 LegoOS：一个分散的、分布式的操作系统，用于硬件资源分散。 在操作系统设计和实施会议(OSDI'18)上。 69-87。 导航 tocitation 1。

- Cheol Shim, Kwang-Ho Cha, and Min Choi. 2018. Design and implementation of initial OpenSHMEM on PCIe NTB based cloud computing. _Cluster Comput._ 22 ( Feb. 2018), 1815–1826. DOI: [https://doi.org/10.1007/s10586-018-1707-0](https://doi.org/10.1007/s10586-018-1707-0) Navigate tocitation 1

> Cheol Shim、Kwang-Ho Cha 和 Min Choi。2018 年。基于 PCIe NTB 的云计算初始 OpenSHMEM 的设计与实现。_Cluster Comput._ 22(2018 年 2 月)，1815-1826。DOI：[https://doi.org/10.1007/s10586-018-1707-0](https://doi.org/10.1007/s10586-018-1707-0)转到引文 1

- Karen Simonyan and Andrew Zisserman. 2014. Very deep convolutional networks for large-scale image recognition. Retrieved from [https://arXiv:1409.1556](https://arXiv:1409.1556). Navigate tocitation 1

> Karen Simonyan 和 Andrew Zisserman。2014。非常深的卷积网络用于大规模图像识别。从[https://arXiv:1409.1556](https://arXiv:1409.1556)获取。转到引文 1。

- Mark J. Sullivan. 2010. _Intel Xeon Processor C5500/C3500 Series Non-Transparent Bridge_. Technical Report . Intel Corporation. Navigate tocitation 1citation 2

> 马克·J·沙利文(Mark J. Sullivan)。2010 年。_Intel Xeon 处理器 C5500/C3500 系列非透明桥_。技术报告。英特尔公司。导航 tocitation 1citation 2

- Jun Suzuki, Yoichi Hidaka, Hunichi Higuchi, Masaki Kan, and Takashi Yoshikawa. 2016. Disaggregation and sharing of I/O devices in cloud data centers. _IEEE Trans. Comput._ 65 ( Dec. 2016), 3013–3026. Issue 10. DOI: [https://doi.org/10.1109/TC.2015.2513759](https://doi.org/10.1109/TC.2015.2513759) Navigate tocitation 1

> 铃木纯、日高洋一、樋口興一、菅正樹、吉川孝. 2016. 云数据中心 I/O 设备的分解和共享. IEEE 计算机学报 65 (2016 年 12 月), 3013–3026. 第 10 期. DOI: [https://doi.org/10.1109/TC.2015.2513759](https://doi.org/10.1109/TC.2015.2513759) 导航参考文献 1

- Jun Suzuki, Yoichi Hidaka, Junichi Higuchi, Teruyuki Baba, Nobuharu Kami, and Takashi Yoshikawa. 2010. Multi-root share of single-root I/O virtualization (SR-IOV) compliant PCI express device. In _Proceedings of the IEEE Symposium on High Performance Interconnects_( HOTI ’10). 25–31. DOI: [https://doi.org/10.1109/HOTI.2010.21](https://doi.org/10.1109/HOTI.2010.21) Navigate tocitation 1

> 鈴木潤、日高洋一、樋口淳一、馬場輝行、神信晴、吉川隆. 2010. 單根 I/O 虛擬化(SR-IOV)兼容 PCI Express 裝置的多根共享. 在 IEEE 高性能互連研討會(HOTI '10)的論文集中. 25-31. DOI: [https://doi.org/10.1109/HOTI.2010.21](https://doi.org/10.1109/HOTI.2010.21) 前往引用 1

- Amir Taherkordi, Feroz Zahid, Yiannis Verginadis, and Geir Horn. 2018. Future cloud system designs: Challenges and research directions. _IEEE Access_ 6 (2018). DOI: [https://doi.org/10.1109/ACCESS.2018.2883149](https://doi.org/10.1109/ACCESS.2018.2883149) Navigate tocitation 1

> 阿米尔·塔赫科尔迪、费罗兹·扎希德、伊安尼斯·韦尔吉纳迪斯和吉尔·霍恩。2018 年。未来云系统设计：挑战和研究方向。IEEE Access 6(2018)。DOI：[https://doi.org/10.1109/ACCESS.2018.2883149](https://doi.org/10.1109/ACCESS.2018.2883149)转到引文 1

- Mellanox Technologies. [n.d.]. ConnectX-5 EN Single/Dual-Port Adapter Supporting 100Gb/s Ethernet. Retrieved from [https://www.mellanox.com/products/ethernet-adapters/connectx-5-en](https://www.mellanox.com/products/ethernet-adapters/connectx-5-en). Navigate tocitation 1

> Mellanox Technologies [n.d.]. ConnectX-5 EN 单/双端口适配器支持 100Gb/s 以太网。从[https://www.mellanox.com/products/ethernet-adapters/connectx-5-en](https://www.mellanox.com/products/ethernet-adapters/connectx-5-en)获取。转到引文 1。

- PLX Technologies. 2005. _Multi-Host System and Intelligent I/O Design with PCI Express_. Whitepaper . PLX Technology/Broadcom. Retrieved from [https://docs.broadcom.com/docs-and-downloads/pdf/technical/expresslane/NTB_Brief_April-05.pdf](https://docs.broadcom.com/docs-and-downloads/pdf/technical/expresslane/NTB_Brief_April-05.pdf). Navigate tocitation 1citation 2citation 3citation 4

> PLX 技术公司。2005 年。《PCI Express 的多主机系统和智能 I/O 设计》白皮书。PLX 技术/博通。从[https://docs.broadcom.com/docs-and-downloads/pdf/technical/expresslane/NTB_Brief_April-05.pdf](https://docs.broadcom.com/docs-and-downloads/pdf/technical/expresslane/NTB_Brief_April-05.pdf)获取。导航到引文 1 引文 2 引文 3 引文 4。

- Adam Thompson and Chris J. Newburn. 2019. GPUDirect Storage: A Direct Path Between Storage and GPU Memory. Retrieved from [https://developer.nvidia.com/blog/gpudirect-storage/](https://developer.nvidia.com/blog/gpudirect-storage/). Navigate tocitation 1

> Adam Thompson 和 Chris J. Newburn. 2019. GPUDirect 存储：存储和 GPU 内存之间的直接路径。从[https://developer.nvidia.com/blog/gpudirect-storage/](https://developer.nvidia.com/blog/gpudirect-storage/)获取。导航到引文 1。

- Animesh Trivedi, Bernard Metzler, and Patrick Stuedi. 2011. A case for RDMA in clouds. In _Proceedings of the Second Asia-Pacific Workshop on Systems (APSys’11)_. 17:1–17:5. DOI: [https://doi.org/10.1145/2103799.2103820](https://doi.org/10.1145/2103799.2103820) Navigate tocitation 1citation 2

> Animesh Trivedi、Bernard Metzler 和 Patrick Stuedi。 2011。 云中 RDMA 的案例。 在第二届亚太系统研讨会(APSys'11)上。 17：1-17：5。 DOI：[https://doi.org/10.1145/2103799.2103820](https://doi.org/10.1145/2103799.2103820) 导航到引文 1 引文 2

- Shin-Yeh Tsai and Yiying Zhang. 2019. A double-edged sword: Security threats and opportunities in one-sided network communication. In _Proceedings of the Workshop on Hot Topics in Cloud Computing (HotCloud’19)_. Navigate tocitation 1

> 张毅蔡欣晔。 2019。 双刃剑：单向网络通信中的安全威胁与机遇。 在《云计算热点议题研讨会(HotCloud'19)》的论文集中。 转到引用 1。

- Cheng-Chun Tu. 2014. _Memory-Based Rack Area Networking_. Ph.D. Dissertation . Stony Brook University. Navigate tocitation 1

> 钟俊·图(Cheng-Chun Tu)。2014 年。《基于内存的机架区域网络》。士学位论文。石溪大学。转到引文 1

- Cheng-Chun Tu and Tzi-cker Chiueh. 2018. Seamless fail-over for PCIe switched networks. In _Proceedings of the International Systems and Storage Conference (SYSTOR’18)_. 101–111. DOI: [https://doi.org/10.1145/3211890.3211895](https://doi.org/10.1145/3211890.3211895) Navigate tocitation 1citation 2citation 3citation 4

> 杜承春和崔子克. 2018. 基于 PCIe 交换网络的无缝故障转移. 在国际系统和存储会议(SYSTOR'18)上. 101-111. DOI: [https://doi.org/10.1145/3211890.3211895](https://doi.org/10.1145/3211890.3211895) 导航引文 1 引文 2 引文 3 引文 4

- Cheng-Chun Tu, Chao-tang Lee, and Tzi-cker Chiueh. 2013. Secure I/O device sharing among virtual machines on multiple hosts. _ACM SIGARCH Comput. Architect. News_ 41, 3 (2013), 108–119. DOI: [https://doi.org/10.1145/2508148.2485932](https://doi.org/10.1145/2508148.2485932) Navigate tocitation 1

> 陈俊图、李朝堂和崔子克. 2013. 在多个主机上的虚拟机之间安全共享 I/O 设备. ACM SIGARCH 计算机架构新闻 41, 3 (2013), 108-119. DOI: https://doi.org/10.1145/2508148.2485932 转到引文 1

- Cheng-Chun Tu, Chao-tang Lee, and Tzi-cker Chiueh. 2014. Marlin: A memory-based rack area network. In _Proceedings of the ACM/IEEE Symposium on Architectures for Networking and Communications Systems (ANCS’14)_. 125–136. DOI: [https://doi.org/10.1145/2658260.2658262](https://doi.org/10.1145/2658260.2658262) Navigate tocitation 1citation 2citation 3

> 陈俊图、李朝堂和齐克·球会。2014 年。Marlin：基于内存的机架区域网络。在 ACM / IEEE 网络和通信系统体系结构研讨会(ANCS'14)上发表。125-136。DOI：[https://doi.org/10.1145/2658260.2658262](https://doi.org/10.1145/2658260.2658262) 转到引文 1 引文 2 引文 3

- Akshay Venkatesh, Khaled Hamidouche, Sreeram Potluri, Davide Rosettig, Ching-Hsiang Chu, and Dhabaleswar K. Panda. 2017. MPI-GDS: High performance MPI designs with GPUDirect-aSync for CPU-GPU control flow decoupling. In _Proceedings of the International Conference on Parallel Processing (ICPP’17)_. 151–160. DOI: [https://doi.org/10.1109/ICPP.2017.24](https://doi.org/10.1109/ICPP.2017.24) Navigate tocitation 1

> Akshay Venkatesh、Khaled Hamidouche、Sreeram Potluri、Davide Rosettig、Ching-Hsiang Chu 和 Dhabaleswar K. Panda。2017 年。MPI-GDS：用 GPUDirect-aSync 实现 CPU-GPU 控制流解耦的高性能 MPI 设计。在《国际并行处理会议(ICPP'17)论文集》中，151-160 页。DOI：[https://doi.org/10.1109/ICPP.2017.24](https://doi.org/10.1109/ICPP.2017.24) 转到引文 1

- Akshay Venkatesh, Hari Subramoni, Khaled Hamidouche, and Dhabaleswar K. Panda. 2014. A high performance broadcast design with hardware multicast and GPUDirect RDMA for streaming applications on Infiniband clusters. In _Proceedings of the International Conference on High Performance Computing (HiPC’14)_. 1–10. DOI: [https://doi.org/10.1109/HiPC.2014.7116875](https://doi.org/10.1109/HiPC.2014.7116875) Navigate tocitation 1citation 2citation 3

> Akshay Venkatesh、Hari Subramoni、Khaled Hamidouche 和 Dhabaleswar K. Panda。2014 年。在 Infiniband 集群上，使用硬件多播和 GPUDirect RDMA 的高性能广播设计，用于流媒体应用程序。在《高性能计算国际会议(HiPC'14)》上。1-10。DOI：[https://doi.org/10.1109/HiPC.2014.7116875](https://doi.org/10.1109/HiPC.2014.7116875) 转到引文 1 引文 2 引文 3

- Heymian Wong. 2011. _PCI Express Multi-Root Switch Reconfiguration During System Operation_. Master's thesis. Massachusetts Institute of Technology. Navigate tocitation 1

> 他伊曼·王，2011。《系统运行期间 PCI Express 多根交换机重新配置》，硕士论文，麻省理工学院。转到引用 1

- Jian Yang, Juno Kim, Morteza Hoseinzadeh, Joseph Izraelevitz, and Steve Swanson. 2020. An empirical guide to the behavior and use of scalable persistent memory. In _Proceedings of the USENIX Conference on File and Storage Technologies (FAST’20)_. 169–182. Navigate tocitation 1

> 杨健、金俊奥、霍赛因扎德、伊兹拉耶利维茨和史蒂夫·斯旺森。2020 年。行为和可扩展持久内存使用的实证指南。在*文件和存储技术会议(FAST'20)*上。169-182。导航到引文 1。

- Ziye Yang, James R. Harris, Benjamin Walker, Daniel Verkamp, Changpeng Liu, Cunyin Chang, Gang Cao, Jonathan Stern, Vishal Verma, and Luse E. Paul. 2017. SPDK: A development kit to build high performance storage applications. In _Proceedings of the International Conference on Cloud Computing Technology and Science (CloudCom’17)_. 154–161. DOI: [https://doi.org/10.1109/CloudCom.2017.14](https://doi.org/10.1109/CloudCom.2017.14) Navigate tocitation 1citation 2citation 3

> Ziye Yang, James R. Harris, Benjamin Walker, Daniel Verkamp, Changpeng Liu, Cunyin Chang, Gang Cao, Jonathan Stern, Vishal Verma 和 Luse E. Paul。 2017。 SPDK：一个用于构建高性能存储应用程序的开发工具包。 在*CloudCom'17 国际云计算技术与科学会议论文集*中。 154-161。 DOI：[https://doi.org/10.1109/CloudCom.2017.14](https://doi.org/10.1109/CloudCom.2017.14) 转到引文 1 引文 2 引文 3

- Xiangliang Yu. 2016. NTB: Add support for AMD PCI-Express Non-Transparent Bridge. Retrieved from [https://lwn.net/Articles/672752/](https://lwn.net/Articles/672752/). Navigate tocitation 1citation 2

> 张亮宇。 2016。 NTB：为 AMD PCI-Express 非透明桥添加支持。 从[https://lwn.net/Articles/672752/](https://lwn.net/Articles/672752/)检索。 导航 tocitation 1citation 2

## Footnotes

- [1](#foot-fn1)Note that the term “borrower” is not always synonymous with the physical host using the device in every context, but may refer to an individual software process or a VM.

> 注意，“借用者”并不总是指使用设备的物理主机，而可能指一个软件进程或虚拟机。

- [2](#foot-fn2)This is often referred to as memory-mapped I/O (MMIO).

- [3](#foot-fn3)The PCIe standard also specifies optional support for relaxed ordering, but strict ordering is mandatory and used by default.

> PCIe 标准也指定了可选的宽松排序支持，但严格排序是强制执行的，并且默认使用。

- [4](#foot-fn4)Enabling the “Bus Master” bit in the command register enables DMA for a device.

> 在命令寄存器中启用“总线主”位可为设备启用 DMA。

- [5](#foot-fn5)NVMe also supports using MSI/MSI-X interrupts to indicate CQ events, but our implementation relies on completion polling alone.

> NVMe 也支持使用 MSI/MSI-X 中断来指示 CQ 事件，但我们的实现仅依赖于完成轮询。

- [6](#foot-fn6)Writes are simply dropped by the NTB. Read transactions will result in an unsupported request completion error, which by convention sets all requested bytes to 0xFF’s.

> 写入操作会被 NTB 直接忽略。读取事务会导致不支持的请求完成错误，按照惯例，所有请求的字节都会被设置为 0xFF。

- [7](#foot-fn7)A software process running on a GPU is called a “kernel” in CUDA. This should not be confused with the OS kernel.

> 在 CUDA 中，在 GPU 上运行的软件过程称为“内核”。这不应与操作系统内核混淆。

- [8](#foot-fn8)While it is possible to configure the PXH830 adapter cards in transparent mode rather than NTB mode, the One Stop Systems expansion chassis used in our tests uses a non-standard connector pin for the PCIe clock signal. In lieu of the possibility of putting the PXH830 in transparent mode, we therefore use HIB68-x16 adapters.

> 尽管可以将 PXH830 适配器卡配置为透明模式而不是 NTB 模式，但我们在测试中使用的 One Stop Systems 扩展机箱使用非标准连接器引脚用于 PCIe 时钟信号。因此，鉴于将 PXH830 放置在透明模式的可能性，我们因此使用 HIB68-x16 适配器。

- [9](#foot-fn9)Since an NVMe read operation involves a register write, several DMA transactions and interrupts, comparing similar hardware topologies would also reveal any latency overhead in the address translation mechanism of the NTBs as well.

> 由于 NVMe 读取操作涉及寄存器写入、多个 DMA 事务和中断，因此比较类似的硬件拓扑也可以揭示 NTB 地址转换机制中的延迟开销。

- [10](#foot-fn10)The maximum payload size for a device is configured by the system. While it can be configured individually for each device, it is usually configured to be the same for all devices in the PCIe tree due to several practical reasons, and is most commonly set to 128, 256, or 512 bytes.

> 设备的最大有效载荷大小由系统配置。虽然可以为每个设备单独配置，但由于几个实际原因，通常将 PCIe 树中的所有设备配置为相同的大小，最常见的设置为 128、256 或 512 字节。

- [11](#foot-fn11)The behavior of iperf2 is perhaps counter-intuitive. In most client/server applications, the client will typically request data from the server rather than the server acting as a receiver. We have used the same terminology as the program uses.

> \*[11](#foot-fn11)iperf2 的行为可能是反直觉的。在大多数客户端/服务器应用程序中，客户端通常会从服务器请求数据，而不是服务器作为接收者。我们使用了与程序相同的术语。

- [12](#foot-fn12)NVMe I/O commands are 64 bytes, so writing a command will automatically flush the Write-Combining Buffer on x86.

> \*[12](#foot-fn12)NVMe I/O 命令为 64 字节，因此在 x86 上编写命令将自动刷新 Write-Combining Buffer。

- [13](#foot-fn13)Multi-Root I/O Virtualization, see Section [9.1](#sec-56).

> \*[13](#足部-fn13)多根 I/O 虚拟化，参见[9.1](#sec-56)节。

- [14](#foot-fn14)In RDMA terminology, this is known as “zero-copy,” because the CPU is not involved in copying data. However, the authors argue that in the context of NVMe-oF, quite literally copying data from the NVMe to system memory on the target host, before sending it over the network, is actually not “zero-copy” at all.

> 在 RDMA 术语中，这被称为“零拷贝”，因为 CPU 不参与数据拷贝。然而，作者认为，在 NVMe-oF 的上下文中，从 NVMe 复制数据到目标主机的系统内存中，然后再通过网络发送，实际上并不是“零拷贝”。

- [15](#foot-fn15)Some IOMMUs support isolation _per application_ by using Protected Address Space ID, but as this also requires support in devices, our implementation does not currently support this.

> \*[15](#foot-fn15)有些 IOMMU 支持通过使用受保护的地址空间 ID 进行每个应用程序的隔离，但由于这也需要设备的支持，我们的实现目前不支持这一点。

J. Markussen is also with Simula Research Laboratory, Norway. P. Halvorsen also with Oslo Metropolitan University, Norway. H. K. Stensland is also with University of Oslo, Norway. C. Griwodz is also with SimulaMet, Norway. Authors’ addresses: J. Markussen, L. B. Kristiansen, and H. Kielland-Gyrud, Dolphin Interconnect Solutions AS, Nils Hansens vei 13, 0667 Oslo, Norway; emails: [jonas@dolphinics.com](mailto:jonas@dolphinics.com), [larsk@dolphinics.com](mailto:larsk@dolphinics.com), [halvor@dolphinics.com](mailto:halvor@dolphinics.com); P. Halvorsen, Simula Metropolitan, Pilestredet 52, 0167 Oslo, Norway; email: [paalh@simula.no](mailto:paalh@simula.no); H. K. Stensland, Simula Research Laboratory, PO Box 134, 1325 Lysaker, Norway; email: [haakonks@simula.no](mailto:haakonks@simula.no); C. Griwodz, Department of Informatics, University of Oslo, PO Box 1080, Blindern, 0316 Oslo, Norway; email: [griff@ifi.uio.no](mailto:griff@ifi.uio.no)

> J. Markussen 也在挪威 Simula 研究实验室工作。P. Halvorsen 也在挪威奥斯陆都市大学工作。H. K. Stensland 也在挪威奥斯陆大学工作。C. Griwodz 也在挪威 SimulaMet 工作。作者地址：J. Markussen，L. B. Kristiansen 和 H. Kielland-Gyrud，Dolphin Interconnect Solutions AS，Nils Hansens vei 13，0667 奥斯陆，挪威；电子邮件：[jonas@dolphinics.com](mailto:jonas@dolphinics.com)，[larsk@dolphinics.com](mailto:larsk@dolphinics.com)，[halvor@dolphinics.com](mailto:halvor@dolphinics.com)；P. Halvorsen，Simula Metropolitan，Pilestredet 52，0167 奥斯陆，挪威；电子邮件：[paalh@simula.no](mailto:paalh@simula.no)；H. K. Stensland，Simula Research Laboratory，PO Box 134，1325 Lysaker，挪威；电子邮件：[haakonks@simula.no](mailto:haakonks@simula.no)；C. Griwodz，信息学系，奥斯陆大学，PO Box 1080，Blindern，0316 奥斯陆，挪威；电子邮件：[griff@ifi.uio.no](mailto:griff@ifi.uio.no)

Permission to make digital or hard copies of part or all of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for third-party components of this work must be honored. For all other uses, contact the owner/author(s).

> 允许为个人或课堂使用而复制本作品的部分或全部内容，无需支付费用，但前提是不得以盈利或商业优势为目的进行复制或分发，并在第一页上注明本声明及完整引文。必须尊重本作品的第三方组件的版权。对于其他所有用途，请联系所有者/作者。

![](https://dl.acm.org/cms/attachment/9e0c7c02-c693-48b7-824b-57549ba37fba/cc-by.jpg)

[This work is licensed under a Creative Commons Attribution International 4.0 License](https://creativecommons.org/licenses/by/4.0/).

> 这项工作根据知识共享国际许可协议 4.0 版授权发布。

©2021 Copyright held by the owner/author(s).  
0734-2071/2021/07-ART2  
DOI: [https://doi.org/10.1145/3462545](https://doi.org/10.1145/3462545)

Publication History: Received July 2020; revised February 2021; accepted April 2021

> 发表历史：2020 年 7 月收到；2021 年 2 月修订；2021 年 4 月接受

---
