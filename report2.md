## Report of Paper Reading - 2018214208 岳力  
Scalable Kernel TCP Design and Implementation for Short-Lived Connections
Xiaofeng Lin, Yu Chen, Xiaodong Li, Junjie Mao, Jiaquan He, Wei Xu, Yuanchun Shi

2019年4月28日
- Summary of major innovations
    - Presents Fastsocket, 
    - Key designation:
        1. Global Listen Table → Local Listen Table 
        1. Global Established Table → Local Established Table
        1. Receive Flow Deliver
        1. Fastsocket-aware VFS

- What the problems the paper mentioned?
    - 充分发挥一个EL X8古董计算机的性能，为所在大学提供多进程计算服务
    - 作为一段非常早期的操作系统相关工作，难度被低估、人手有限、经验不足
- How about the important related works/papers?
    - 本文没有引reference......
- What are some intriguing aspects of the paper?
    - 首次提出分层设计操作系统的概念，影响深远
- How to test/compare/analyze the results?
    - 能否正确的完成多进程任务？
    - 能否测CPU利用率？
- How can the research be improved?
    - 加入多用户(multi-access)
    - 系统使用ALGOL编写，运行效率值得怀疑？
    - 编写过程没有太考虑留debug接口（当时是不是还没有Debug/Release版本的概念）
    - 文中实现了比较原始的软件MMU，现在应该都是硬件加速的
- If you write this paper, then how would you do?
    - 加入系统层次结构图
    - 给每一层加示例代码/伪代码
    - 更详细地介绍硬件（在当代人看来这个计算机的部件十分陌生）
- Did you test the results by yourself? If so,What’s your test Results?
    - No
    - 原文只分享了一些设计中遇到的困难、解决方法和经验，没有对系统本身做Evaluation
- Give the survey paper list in the same area of the paper your reading.
    - 还没有读其它操作系统相关文献