## Report of Paper Reading - 2018214208 岳力  
**Scalable Kernel TCP Design and Implementation for Short-Lived Connections**  
Xiaofeng Lin, Yu Chen, Xiaodong Li, Junjie Mao, Jiaquan He, Wei Xu, Yuanchun Shi  
Published in **ASPLOS '16**  

2019年4月28日
### Summary of major innovations  
- Presents **Fastsocket**, a BSD Socket-compatible and scalable kernel socket design, facing new application scenarios (Weibo):
    - Growth of network bandwidth
    - Increasing CPU cores
    - More short-lived connections for apps
- Key designation:
    1. Global Listen Table → Local Listen Table 
    1. Global Established Table → Local Established Table
    1. Receive Flow Deliver
    1. Fastsocket-aware VFS
### What the problems the paper mentioned?
- TCB Management  
TCB (TCP Control Block) (sockets in linux) are managed in 2 global hash tables, *ListenTable* for passive conns, and *Established Table* for both active and passive conns. Bothtable is **global** originnally, shared by all CPU cores.
- Lack of Connection Locality  
While a Multi-core computer receiving packets. The incoming package will trigger NIC(Network Interface Controller) interrupt, which processed by the CPU core handle thatinterrupt. Somehow, applications are responsible for further processing, which could berunning on another core.  
- Additional Overhead for Compatibility  
Applications use sockets as FD (File Descripter) via VFS (Virtual File System).  
### How about the important related works/papers?
- TCB Management  
    - Listen Table
        - SO_REUSEPORT and Megapipe
    - Established Table
        - 
- Lack of Connection Locality  

- Additional Overhead for Compatibility  
### What are some intriguing aspects of the paper?
- 首次提出分层设计操作系统的概念，影响深远
### How to test/compare/analyze the results?
- 能否正确的完成多进程任务？
- 能否测CPU利用率？
### How can the research be improved?
- TODO
### If you write this paper, then how would you do?
- TODO
### Did you test the results by yourself? If so,What’s your test Results?
- No
- Open sourced: https://github.com/fastos/fastsocket
### Give the survey paper list in the same area of the paper your reading.
- NONE
