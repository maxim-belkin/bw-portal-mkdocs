# []{#BWUserDoc-BasicInfoonBlueWaters-BriefBlueWatersSystemSummary}Brief Blue Waters System Overview

## []{#BWUserDoc-BasicInfoonBlueWaters-Overview}Blue Waters

Blue Waters is a Cray XE6/XK7 system consisting of more than 22,500 XE6
compute nodes (each containing two AMD Interlagos processors) augmented
by more than 4200 XK7 compute nodes (each containing one AMD Interlagos
processor and one NVIDIA GK110 \"Kepler\" accelerator) in a single
Gemini interconnection fabric. This configuration enables extremely
large simulations on hundreds of thousands of traditional CPUs for
science and engineering discovery, while also supporting development and
optimization of cutting-edge applications capable of leveraging the
compute power of thousands of GPUs.

### []{#BWUserDoc-BasicInfoonBlueWaters-Nodes}Blue Waters Nodes

Blue Waters is equipped with three node types: traditional compute nodes
(XE6), accelerated compute nodes (XK7), and service nodes. Each node
type is introduced below.

[]{#BWUserDoc-BasicInfoonBlueWaters-TraditionalComputeNodes%28XE6%29}
Traditional Compute Nodes (XE6)

The XE6 dual-socket nodes are populated with 2 AMD Interlagos model 6276
CPU processors (one per socket) with a nominal clock speed of at least
2.3 GHz and 64 GB of physical memory. The Interlagos architecture
employs the AMD Bulldozer core design in which two integer cores share a
single floating point unit. In describing Blue Waters, we refer to the
Bulldozer compute unit as a single compute \"core\" and consider the
Interlagos processors as having 8 (floating point) cores each, although
this processor has been described elsewhere as having 16 cores.  The
Bulldozer core has 16KB/64KB data/instruction L1 caches, 2 MB shared L2
and instruction support for [SSSE3, SSE4.1, SSE4.2, AES-NI, PCLMULQDQ ,
AVX, XOP, and
FMA4](http://blogs.amd.com/work/2010/11/22/following-instructions/).
Each core is able to complete up to 8 floating point operations per
cycle. The architecture supports 8 cores per socket with two dies, each
die containing 4 cores forming a NUMA domain. The 4 cores of a NUMA
domain share an 8 MB L3 cache.  Access times for shared memory on the
other die in the same socket are somewhat longer than access times
within the same die. See chapters 1 and 2 in the AMD document [Software
Optimization Guide for AMD Family
15hProcessors](http://support.amd.com/us/Processor_TechDocs/47414.pdf)
for additional technical information on the 62xx processor.

[![](/liferay-content/image-gallery/Documentation%20images/XE6_Node.png){style="height:auto; width:50%"}](/liferay-content/image-gallery/Documentation%20images/XE6_Node.png)

[]{#BWUserDoc-BasicInfoonBlueWaters-GPUenabledComputeNodes%28XK7%29}GPU-enabled
Compute Nodes (XK7)

Accelerator nodes are equipped with one Interlagos model 6276 CPU
processor and one [NVIDIA GK110 \"Kepler\"
accelerator](http://www.nvidia.com/content/PDF/kepler/NVIDIA-Kepler-GK110-Architecture-Whitepaper.pdf)
[K20X](http://developer.download.nvidia.com/GTC/inside-tesla-kepler-k20-family.pdf).
The CPU acts as a host processor to the accelerator.  Currently the
NVIDIA accelerator will not directly interact with the Gemini
interconnect so that data needs to be moved to a node containing an
accelerator, and that data may be accessed by the accelerator as mapped
memory, or through DMA transfers to accelerator device memory. Each XK7
node has 32 GB of system memory while the accelarator has 6 GB of
memory. The Kepler GK110 implementation includes 14 Streaming
Multiprocessor (SMX) units and six 64?bit memory controllers. Each of
the SMX units feature 192 single?precision CUDA cores. The Kepler
architecture supports a unified memory request path for loads and
stores, with an L1 cache per SMX multiprocessor. Each SMX has 64 KB of
on-chip memory that can be configured as 48 KB of Shared memory with 16
KB of L1 cache, or as 16 KB of shared memory with 48 KB of L1 cache. The
Kepler also features 1536KB of dedicated L2 cache memory. Single-Error
Correct Double-Error Detect (SECDED) ECC code is enabled by default.

![](/liferay-content/image-gallery/Documentation%20images/XK_node_characteristics.png){style="height:auto; width:50%"}
![](/liferay-content/image-gallery/Documentation%20images/XK6_Node.jpg){style="height:auto; width:50%"}

The Cray XK currently supports exclusive mode for access to the
accelerator. NVIDIA\'s Proxy (part of \"Hyper-Q\") manages access for
other contexts or multiple tasks of the user application via the
[CRAY_CUDA_MPS (formerly CRAY_CUDA_PROXY) environment
setting](/accelerator-jobs-on-xk-nodes).

[]{#BWUserDoc-BasicInfoonBlueWaters-I%2FOandServiceNodes%28XIO%29}I/O
and Service Nodes (XIO)

Each Cray XE6/XK6 XIO blade contains four service nodes. Service nodes
use AMD Opteron \"Istanbul\" six-core processors, with 16 GB of DDR2
memory, and the same Gemini interconnect processors as compute nodes.
XIO nodes take one of four roles as Service nodes:

-   **esLogin Nodes:** These nodes provide user access and log in
    services for the system. Login nodes are network attached esLogin
    servers. Users access the system through these nodes, each running
    the full Linux operating system.
-   **PBS MOM Nodes**: These are nodes which run the jobs (where the PBS
    script gets executed and aprun is launched) and where interactive
    batch jobs (qsub -I) place a user. These nodes are in the Gemini
    high speed network (HSN).
-   **Network Service Node:** Each Network service node contains one
    Infiniband QDR IB card that can be connected to external networking
    connections.
-   **LNET Router Nodes:** Manage file system metadata and transfer data
    to and from storage devices and applications. These nodes use
    Infiniband QDR cards as the interface to the storage network.
-   **Boot Node/System Database Node (SDB):** Each Cray XE6/XK7 system
    requires one or more boot node and one or more SDB node. These nodes
    contain one 4-Gbit FC HBA and one GigE PCIe card. The FC HBA
    connects to the RAID and the GigE card connects to the System
    Management Workstation of the HSS. A redundant boot and SDB node are
    also configured to provide resiliency.

### []{#BWUserDoc-BasicInfoonBlueWaters-Interconnect}Interconnect

The Blue Waters system employs the Cray Gemini interconnect, which
implements a 3D torus topology. Note that there are 2 compute nodes per
gemini hub. The torus is periodic or re-entrant in all directions (paths
wrap around). A schematic of the general use of the high speed network
(HSN) is shown in the following figure detailing a simplified view of
the location of all node types in the torus. In reality the IO and
service nodes are not confined to a single \"plane\" in the torus.

[![](/liferay-content/image-gallery/Documentation%20images/torus_general.png){style="height:auto; width:50%"}](/liferay-content/image-gallery/Documentation%20images/torus_general.png)

The (x,y,z) locations of the Gemini routers can be plotted to show a
particular view of the torus. In the image below the Blue Waters torus
is shown with gray dots representing the location of XE nodes, red
spheres representing the location of XK nodes and blue spheres
representing service nodes. Note that the system should be viewed as
periodic or re-entrant and that there is no unique origin and no corners
in a torus.

[![](/liferay-content/image-gallery/Documentation%20images/bw_torus.png){style="height:auto; width:50%"}](/liferay-content/image-gallery/Documentation%20images/bw_torus.png)

The native messaging protocol for the interconnect is Cray\'s Generic
Network Interface (GNI). A second messaging protocol, the Distributed
Shared Memory Application (DMAPP) interface, is also supported. GNI and
DMAPP provide low-level communication services to user-space software.
GNI directly exposes the communications capabilities of the Gemini while
DMAPP supports a logically shared, distributed memory (DM) programming
model and provides remote memory access (RMA) between processes within a
job in a one-sided manner. For more information please see the [Using
the GNI and DMAPP
APIs](http://docs.cray.com/books/S-2446-3103/S-2446-3103.pdf) document.

### []{#BWUserDoc-BasicInfoonBlueWaters-DataStorage}Data Storage

[]{#BWUserDoc-BasicInfoonBlueWaters-Onlinestorage}Online storage

Blue Waters provides three different file systems built with the Lustre
file system technology.

The three file systems are provided to the users as follows:

-   **/u** b storage for home directories
-   **/projects** b storage for project home directories
-   **/scratch** b high performance, high capacity transient storage
    for applications

All three file systems on Blue Waters are built using Cray Sonexion 1600
Lustre appliances. The Cray Sonexion 1600 appliances provide the basic
storage building block for the Blue Waters I/O architecture and are
referred to as a \"Scalable Storage Unit\" (SSU). Each SSU is RAID
protected and is capable of providing up to 5.35 GB/s of IO performance
and \~120TB of usable disk space.

The **/u** and **/projects** are configured with 18 (eighteen) SSUs
each, to provide 2 PB usable storage and 96 GB/s IO performance.

The **/scratch** file system uses 180 (one hundred eighty) SSUs to
provide 21.6PB of usable disk storage and 963 GB/s IO performance. This
file system can provide storage for up to 2 million file system objects.

Permissions for `/home`, `/project/sciteam/psn` and
`/scratch/sciteam/user` are set to prevent unintentional sharing. Please
see the [File and Directory Access Control
List page](https://bluewaters.ncsa.illinois.edu/facl)  for more
information.  Sharing by grpup  in `/project/sciteam/psn` is also
available in scratch as` /scratch/sciteam/PREFIX_psn `where prefix will
be one of `EOT`, `GS`, `ILL`. Please contact 

\
[]{#BWUserDoc-BasicInfoonBlueWaters-DataStorage}Usage Guidelines

Blue Waters uses a one-time password system for logging into accounts
based on  Duo Moble. You will need to either install the Duo app on a
smart phone or request a Duo hardtoken during the identity creation
process. .

[Queues](/queues-and-scheduling-policies) and [quotas](/storage#quotas)
discussed in the User Guide.

### []{#BWUserDoc-BasicInfoonBlueWaters-DataStorage}Compiling and Linking

The Blue Waters development environment include four compiler suites:

-   Cray Compilation Environment (CCE) : `PrgEnv-cray`
-   PGI Compiler Suite : `PrgEnv-pgi`
-   Intel Compilers : `PrgEnv-intel`
-   GNU Compilers : `PrgEnv-gnu`

The default programming environment is the Cray (`PrgEnv-cray`) compiler
suite. Other compilers may be selected using the module swap command
with either `PrgEnv-pgi` or `PrgEnv-gnu`.

The programming environments provide wrappers for compiling Fortran
(ftn), C (cc), or C++ (CC) programs, so that similar compiler commands
can be used no matter which compiler environment is selected. These
wrapper scripts also check for the presense of other loaded modules and
take the appropriate measures to add include paths and libraries with
compiling and linking.

See the [Compiling page in the User Guide](/compiling) for more
information.

## []{#Reference}Reference

Links to Cray, AMD, Nvidia & compiler vendor developer sites with a list
of recommended reading:

-   Processor architecture
    -   [Software Optimization Guide for AMD Family 15h
        Processors](/liferay-content/document-library/amd_0_47414_15h.pdf)
    -   [AMD64 Architecture Programmer\'s Manual Volume 1: Application
        Programming](/liferay-content/document-library/amd_1_24592_APM.pdf)
    -   [AMD64 Architecture Programmer\'s Manual Volume 2: System
        Programming](/liferay-content/document-library/amd_2_24593.pdf)
    -   [AMD64 Architecture Programmer\'s Manual Volume 3:
        General-Purpose and System
        Instructions](/liferay-content/document-library/amd_3.pdf)
    -   [AMD64 Architecture Programmer\'s Manual Volume 4: 128-Bit and
        256-Bit Media
        Instructions](/liferay-content/document-library/amd_4_26568.pdf)
    -   [AMD64 Architecture Programmer\'s Manual Volume 5: 64-Bit Media
        and x87 Floating Point
        Instructions](/liferay-content/document-library/amd_5_26569.pdf)

```{=html}
<!-- -->
```
-   Accelerator architecture
    -   [NVIDIA Kepler GK110 Architecture
        Whitepaper](http://www.nvidia.com/content/PDF/kepler/NVIDIA-Kepler-GK110-Architecture-Whitepaper.pdf)
    -   [Nvidia Kepler K20X Board
        Design](http://www.nvidia.com/content/PDF/kepler/Tesla-K20X-BD-06397-001-v05.pdf)
    -   [NVIDIA K20 and K20X Performance Technical
        Brief](http://www.nvidia.com/docs/IO/122874/K20-and-K20X-application-performance-technical-brief.pdf)
    -   I[nside
        Kepler](http://developer.download.nvidia.com/GTC/inside-tesla-kepler-k20-family.pdf)
    -   [NVIDIA Tesla Kepler Family
        Overview](http://www.nvidia.com/content/tesla/pdf/Tesla-KSeries-Overview-LR.pdf)
    -   [PGI Accelerator Compilers \[OMP-like
        directives\]](http://www.pgroup.com/resources/accel.htm)
    -   [PGI CUDA
        Fortran](http://www.pgroup.com/resources/cudafortran.htm)
    -   [Nvidia CUDA Toolkit c, c++, math
        libs](http://developer.nvidia.com/cuda-toolkit)
        -   [CUDA C Programming
            Guide](http://developer.download.nvidia.com/compute/DevZone/docs/html/C/doc/CUDA_C_Programming_Guide.pdf)
        -   [CUDA C Best Practices (ch. 4 config. optimization tips ,
            ch. 8 multi-gpu and MPI tips, see appendix A for
            recommendations)](http://developer.download.nvidia.com/compute/DevZone/docs/html/C/doc/CUDA_C_Best_Practices_Guide.pdf)
-   Interconnect (gemini) related docs
    -   [Gemini-based Cielo performance evaluation at
        Sandia](http://www.cs.sandia.gov/~ktpedre/copyrighted-papers/lspp2011.pdf)
    -   [Optimizing Global Arrays for
        Gemini](http://www.epcc.ed.ac.uk/wp-content/uploads/2010/12/Vairavan%20Murugappan.pdf)
-   Compiler documentation
    -   [Cray Application Developer\'s Environment User\'s Guide: Using
        Compilers](http://docs.cray.com/cgi-bin/craydoc.cgi?mode=Show;q=;f=/books/S-2396-601/html-S-2396-601//chapter-xpb5m1kn-brbethke.html)
    -   [Cray C and C++ Reference
        Manual](http://docs.cray.com/cgi-bin/craydoc.cgi?mode=View;id=S-2179-74;idx=books_search;this_sort=title;q=;type=books;title=Cray%20C%20and%20C%2b%2b%20Reference%20Manual)
    -   [AMD Compiler Options Quick Reference Guide for Valencia and
        Interlagos ](http://developer.amd.com/Assets/CompilerOptQuickRef-62004200.pdf)
    -   Refer to
        <http://developer.amd.com/tools/open64/Pages/default.aspx> for
        latest information on AMD compilers.
