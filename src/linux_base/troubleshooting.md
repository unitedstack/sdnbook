# 故障排查

----

## 网络故障排查思路

一般地，网络故障排查可以遵循以下几步：

    1. Find out at what level of TCP/IP stack(or some other stack) occurs 
       the problem.
    2. Understand what is the correct system behavior, and what is 
       deviation from normal system state.
    3. Try to express the problem in one sentence or in several words
    4. Using obtained information from buggy system, your own experience 
       and experience of other people(google, various forum, etc.), 
       try to solve the problem until success(or failure).
    5. If you fails, ask other people about help or some advice.
    

 * 首先，确定发生的网络故障属于 TCP / IP 的那一层；
 * 其次，理解在正常情况下，系统的状态应该是怎么样的；
 * 然后，使用最 `准确` 的语言描述问题；
 * 然后，在故障系统中尽可能多的获取有用信息，凭借自己的力量去解决；
 * 最后，求助别人。

多数情况下，当我们遇到了网络问题，第一反应就是求助别人，这时往往我们并没有
深入研究该问题，甚至连故障现象都描述不清，就直接求助别人。这时往往被求助的人
由于没有获取到完整的故障描述、故障现象和重现方法，导致被求助人要从零开始分析
问题，导致使用更多的人力资源。正确的网络排障过程应该是，发现问题者首先要
自己确定发生故障的原因，对于无法定位的问题，网上查询是否有相似问题和解决方案。
对于自己实在无法解决的问题，在求助他人之前，一定可以用最准确最精确的语言描述出
来故障现象以及重现方法。在解决问题后，一定要有总结经验的过程。


解决网络问题可以如下表示：

    SolveNetworkProblem(information_about_system_state,
                        my_experience,
                        people_experience)


 * information_about_system_state: 网络故障的信息、重现方法和系统状态
 * my_experience: 发现故障者自身的排障过程
 * people_experience: 包含两方面，网上搜索的过程和求助他人的过程





注：以上部分内容来自 [Linux network troubleshooting and debugging](http://unix.stackexchange.com/questions/50098/linux-network-troubleshooting-and-debugging)
