# Report 2 of Paper Reading - 2018214208 岳力  
**Scalable Kernel TCP Design and Implementation for Short-Lived Connections**  
Xiaofeng Lin, Yu Chen, Xiaodong Li, Junjie Mao, Jiaquan He, Wei Xu, Yuanchun Shi  
Published in **ASPLOS '16**  

## Summary of major innovations  
- Presents **Fastsocket**, a BSD Socket-compatible and scalable kernel socket design, facing new application scenarios (Weibo):
    - Growth of network bandwidth
    - Increasing CPU cores
    - More short-lived connections for apps
- Key designation:
    1. Global Listen Table → Local Listen Table 
    1. Global Established Table → Local Established Table
    1. Receive Flow Deliver
    1. Fastsocket-aware VFS
## What the problems the paper mentioned?
- TCB Management  
TCB (TCP Control Block) (sockets in linux) are managed in 2 global hash tables, *ListenTable* for passive conns, and *Established Table* for both active and passive conns. Bothtable is **global** originnally, shared by all CPU cores.
- Lack of Connection Locality  
While a Multi-core computer receiving packets. The incoming package will trigger NIC(Network Interface Controller) interrupt, which processed by the CPU core handle thatinterrupt. Somehow, applications are responsible for further processing, which could berunning on another core.  
- Additional Overhead for Compatibility  
Applications use sockets as FD (File Descripter) via VFS (Virtual File System).  
## How about the important related works/papers?
- TCB Management  
    - Listen Table
        - SO_REUSEPORT and Megapipe
    - Established Table
- Lack of Connection Locality  
- Additional Overhead for Compatibility  

LOTS OF related work in section 5.  
## What are some intriguing aspects of the paper?
- Full Partition of TCB Management  
    - Local Listen Table  
        ```listen()``` is replaced with ``` local_listen()```, creating and inserting socket into ```Local Listen Table``` while still maintaining the originnal ```Global Listen Table```. For incoming packets which has registed local listen socket, **Fast Path** is avaliable that no need of take lock of ```Global Listen Table```. While other packets are still taking **Slow Path**, which rarely occurs in practice.
    - Local Established Table  
        Similar to listen table, **Fastsocket** allocates ```Local Established Table``` for each CPU core, in order to store established connections.
        - Q: Is there **Global Established Table** ?  
- Complete Connection Locality  
    Send incoming packets to the corresponding handler CPU directly.  
    - Active Connection  
    To make sure that a same CPU core processing both receiving and sending for same port, **Fastsocket** maps ports to CPUs using a hash function: c=hash(p<sub>src</sub>), where **c** is CPU core id and **p<sub>src</sub>** is the source port.
    - Passive Connection
    The hash function works for active connections only, a list of procedure is introduced to determine if a incoming packet is passive or active.  
        - Q: Active? Passive?  
- Fastsocket-aware VFS  
    Special fastpath for short-lived connections.  
    - ```dentry``` and ```inode``` are useless for VFS, but introduce tremendous synchronization overhead.  
    - Simply remove them would cause incompatibility with legacy application. Thus **Fastsocket** keeps minimum states and functions for them, a small price to pay.  
## How to test/compare/analyze the results?
- Does **Fastsocket** **benefit** real applications?
    The results show that their throughputs with Fastsocket outperform the base Linux by **267%** and **621%** respectively on a 24-core machine.
- How does **Fastsocket** **scale**, compared to the state-of-theart Linux TCP kernel stack implementations, especially for short-lived TCP connection handling?
    The results show that the throughput with Fastsocket on a 24-core system scales to 20x while Linux 3.13 with SO REUSEPORT achieves only 12x.
## How can the research be improved?
- No idea
## If you write this paper, then how would you do?
- Nice paper already
## Did you test the results by yourself? If so,What’s your test Results?
- No
- Open sourced: https://github.com/fastos/fastsocket
## Give the survey paper list in the same area of the paper your reading.
- NONE
