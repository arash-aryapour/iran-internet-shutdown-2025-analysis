# 伊朗互联网封锁 – 2025年6月  
## 伊朗战时网络隔离策略 (IR-GFW) 的深入技术与数据取证分析

---

## 摘要

2025年6月，伊朗在军事冲突升级期间，实施了一场史无前例的全国性互联网封锁，几乎与全球互联网完全隔绝。本文通过分析来自 NetBlocks、Cloudflare Radar、Kentik 和 IODA 的多源遥测数据，对此次封锁的技术架构进行了全面取证研究。我们详细探讨了 BGP 路由操纵、深度包检测（DPI）过滤方法、VPN 阻断技术以及国内网络白名单策略的部署。此外，还讨论了此次事件的地缘政治和社会经济影响，并对伊朗数字主权的长期走向进行了推测。

---

## 1. 引言

2025年夏季，以色列与伊朗的冲突急剧升级，以色列对伊朗关键军事及核设施发动了精准空袭。作为回应，伊朗当局执行了全国性的网络隔离行动，本文称之为 IR-GFW（伊朗防火长城）。与中国的持续网络审查系统不同，IR-GFW 是一场战时紧急的数字封锁，通过多层过滤及撤回网络基础设施，实现了几乎完全的数字黑暗。本分析汇总了多家独立监测机构的遥测数据，重构了封锁的时间线及技术实现。

---

## 2. 方法与数据来源

- **NetBlocks**：全球互联网观测组织，通过 traceroute、DNS 解析及网页探测来监测连接中断。  
- **Cloudflare Radar**：提供全球及区域 IP 流量趋势和异常检测的流量分析平台。  
- **Kentik**：基于 BGP 监控、流量数据及互联统计的网络分析平台。  
- **IODA (互联网中断检测与分析)**：乔治亚理工学院的研究项目，通过主动探测检测网络中断。  

数据采集时间为 2025 年 6 月 10 日至 6 月 25 日，覆盖冲突前基线、封锁开始及部分恢复阶段。

---

## 3. 时间线与事件重建

| 日期       | 事件描述                                 | 网络指标                                |
|------------|----------------------------------------|-----------------------------------------|
| 6 月 13 日 | 以色列对伊朗目标发动空袭               | Kentik 报告外部流量下降约 54%         |
| 6 月 14-16 日 | 网络状况逐步恶化                       | 路由不稳定，偶发 DPI 重置             |
| 6 月 17 日 | 大规模网络崩溃开始                      | NetBlocks 报告连接丧失约 97%         |
| 6 月 18-19 日 | 网络封锁稳定                          | 仅限国内内联网流量 (~3% 全球)        |
| 6 月 21 日 | 短暂部分恢复（约两小时）               | Cloudflare 流量短暂回升后再次下降     |
| 6 月 22-25 日 | 持续封锁                              | VPN 流量中断，TCP 重置频繁             |

---

## 4. 技术分析

### 4.1 BGP 路由操纵

- 伊朗 ISP 撤销了全球 BGP 前缀，导致全球路由表将伊朗 AS 移出互联网骨干。  
- 路由撤回集中针对一级运营商（如 Level 3、Tata Communications），切断国际互联。  
- 局部路由通告在国内维持国家信息网络 (NIN) 的可用性。

### 4.2 深度包检测（DPI）与过滤

- 部署在国家级 IXPs 的 DPI 设备针对 VPN 协议（OpenVPN、WireGuard）及加密 DNS（DoH/DoT）实施检测。  
- TCP Reset 注入技术强制关闭可疑会话。  
- TLS 握手的 SNI（服务器名称指示）过滤阻止访问受禁止的外部服务。  
- DNS 污染将外部域名重定向到内部 IP 或黑洞地址。

### 4.3 VPN 与代理对抗措施

- 对混淆 VPN 流量（如 Obfs4、Shadowsocks）进行主动探测，导致连接失败率上升。  
- 行为指纹识别检测非常规握手模式，触发连接重置。  
- 据报告，采用基于机器学习的 DPI 分类器动态识别翻墙工具。

### 4.4 白名单与入侵检测

- 实施全面白名单，仅允许访问政府批准的 IP 范围与服务。  
- 入侵检测系统 (IDS) 实时监测异常流量，并执行动态阻断规则。  
- 关键国内平台（电商、社交媒体、银行）通过封闭的国家网络路由。

---

## 5. 数据可视化

### 5.1 NetBlocks 连通性指数

![NetBlocks 流量图](./data/netblocks_traffic.png)

- 显示四天内网络可达率从 100% 急剧下降至约 3%。  
- 与报道的军事行动时间高度一致。

### 5.2 IODA 主动探测指标

![IODA 图表](./data/ioda_google_access.png)

- Google DNS 探测显示间歇性部分恢复，随后再次进入封锁状态。  
- 验证了 DNS 污染及选择性封锁的存在。

### 5.3 Cloudflare 流量分析

![Cloudflare 图表](./data/cloudflare_drop.png)

- 区域流量数据与全球遥测数据一致，显示 95–97% 流量损失。  
- 6 月 21 日出现短暂流量高峰，表明短时间恢复连接。

---

## 6. 地缘政治与社会经济影响

- **通讯封锁严重影响危机期间民众的协调与响应。**  
- **包括 Sepah 银行及 Nobitex 加密货币交易所在内的金融服务暂停对外业务，引发流动性危机。**  
- **国际特赦组织及无国界记者组织谴责伊朗侵犯数字权利，引发人道主义关切。**  
- **审查先例加剧对伊朗未来数字治理以及长期“网络碎片化”孤立的担忧。**

---

## 7. 应对措施与外部干预

- 埃隆·马斯克的 Starlink 卫星部分缓解边境地区的封锁影响。  
- 伊朗当局进行干扰，削弱城市地区的卫星接收。  
- 地下 Mesh 网络及点对点应用出现小幅增长。

---

## 8. 结论与展望

2025 年 6 月伊朗互联网封锁标志着国家层面网络控制的新阶段，其战时策略将传统审查与骨干级网络隔离相结合。此事件预示着通过技术手段分割全球互联网，迈向国家数字主权的新范式。未来冲突中，类似策略或将更频繁出现，对全球互联网治理、网络安全及公民自由产生深远影响。

---

## 9. 参考文献

- NetBlocks. “Iran Internet Shutdown Report, June 2025.” [NetBlocks.org](https://netblocks.org)  
- Cloudflare Radar. “Regional Traffic Analytics.” [Cloudflare.com/radar](https://radar.cloudflare.com)  
- Kentik Labs. “BGP and Flow Data Analysis, June 2025.” [Kentik.com](https://kentik.com)  
- IODA Project. “Internet Outage Detection and Analysis.” [Ioda.gatech.edu](https://ioda.gatech.edu)  
- Amnesty International. “Iran: Digital Rights Violations during 2025 Conflict.”  
- Reporters Without Borders. “Internet Blackouts and Human Rights.”  
- The Verge、TechCrunch、Wired – 多篇关于伊朗互联网封锁的报道
