# Iran Internet Shutdown – June 2025  
## An In-Depth Technical & Data-Driven Forensic Analysis of Iran’s Wartime Network Isolation Strategy (IR-GFW)

---

## Abstract

In June 2025, Iran implemented an unprecedented nationwide internet blackout coinciding with escalating military conflict, resulting in a near-total severance from the global internet. This paper provides a comprehensive forensic examination of the shutdown’s technical architecture, leveraging cross-source telemetry including NetBlocks, Cloudflare Radar, Kentik, and IODA data. We analyze BGP routing manipulations, DPI filtering methodologies, VPN blocking techniques, and the strategic deployment of domestic network whitelisting. Furthermore, we discuss the geopolitical and socio-economic ramifications and speculate on the long-term implications for Iran’s digital sovereignty.

---

## 1. Introduction

The summer of 2025 witnessed a marked intensification in the Israeli-Iranian conflict, culminating in targeted airstrikes against critical Iranian military and nuclear infrastructure. In response, Iranian authorities executed a coordinated nationwide network isolation, herein referred to as IR-GFW (Iranian Great Firewall). Unlike China’s persistent censorship firewall, IR-GFW functioned as an emergency wartime digital siege, combining multi-layered filtering and infrastructure withdrawal to impose near-complete digital blackout. This analysis aggregates telemetry from independent measurement entities to reconstruct the shutdown timeline and technical implementation.

---

## 2. Methodology & Data Sources

- **NetBlocks**: Global internet observatory monitoring connectivity disruptions via traceroutes, DNS resolution, and web probing.  
- **Cloudflare Radar**: Traffic analytics platform providing global and regional IP traffic trends and anomaly detection.  
- **Kentik**: Network analytics platform leveraging BGP monitoring, flow data, and peering statistics.  
- **IODA (Internet Outage Detection and Analysis)**: Research project from Georgia Tech using active probing to detect network outages.  

Data was collated from June 10 to June 25, 2025, to capture pre-conflict baselines, blackout onset, and partial recovery phases.

---

## 3. Timeline & Event Reconstruction

| Date       | Event Description                                   | Network Metrics                             |
|------------|---------------------------------------------------|---------------------------------------------|
| June 13    | Israeli airstrikes on Iranian targets             | ~54% drop in external transit via Kentik    |
| June 14-16 | Progressive network degradation                    | Routing instability and intermittent DPI resets |
| June 17    | Full-scale network collapse begins                 | NetBlocks reports ~97% connectivity loss    |
| June 18-19 | Network blackout stabilizes                         | Domestic intranet traffic only (~3% global) |
| June 21    | Temporary partial restoration (~2 hours)           | Spike in Cloudflare traffic noted, then reset |
| June 22-25 | Persistent blackout continues                       | VPN traffic disrupted, heavy TCP resets     |

---

## 4. Technical Analysis

### 4.1 BGP Route Manipulation

- Iranian ISPs withdrew global BGP prefixes causing global routing tables to drop Iranian ASes from the internet backbone.  
- Route withdrawals focused on Tier-1 upstream providers (e.g., Level 3, Tata Communications) to sever international peering.  
- Partial route announcements persisted internally to maintain National Information Network (NIN) accessibility.  

### 4.2 Deep Packet Inspection & Filtering

- DPI appliances deployed at national IXPs targeted VPN protocols (OpenVPN, WireGuard) and encrypted DNS (DoH/DoT).  
- TCP Reset Injection techniques forcibly closed suspicious sessions.  
- SNI (Server Name Indication) filtering applied to TLS handshakes to block access to banned external services.  
- DNS poisoning redirected external domains to internal IPs or blackholes.  

### 4.3 VPN & Proxy Countermeasures

- Active probing of obfuscated VPN traffic (Obfs4, Shadowsocks) led to increased connection failures.  
- Behavioral fingerprinting techniques detected non-standard handshake patterns, triggering resets.  
- Reported use of ML-based DPI classifiers to dynamically identify circumvention tools.  

### 4.4 Whitelisting & Intrusion Detection

- Complete whitelisting enforced; only traffic destined for government-approved IP ranges and services permitted.  
- Intrusion Detection Systems (IDS) monitored for anomalous patterns, with real-time blocking rules.  
- Critical domestic platforms (e-commerce, social media, banking) routed through closed national network.

---

## 5. Data Visualization

### 5.1 NetBlocks Connectivity Index

![NetBlocks traffic chart](./data/netblocks_traffic.png)

- Shows sharp decline in network reachability from 100% to ~3% over four days.  
- Correlates temporally with reported military actions.

### 5.2 IODA Active Probing Metrics

![IODA chart](./data/ioda_google_access.png)

- Google DNS probing demonstrates intermittent partial restoration followed by reversion to blackout state.  
- Confirms DNS poisoning and selective blocking.

### 5.3 Cloudflare Traffic Analytics

![Cloudflare chart](./data/cloudflare_drop.png)

- Regional traffic data corroborates global telemetry with 95–97% traffic loss.  
- Spike during June 21 indicates transient window of restored connectivity.

---

## 6. Geopolitical & Socioeconomic Impacts

- **Communication blackout severely impaired civilian coordination during crisis.**  
- **Financial services including Bank Sepah and Nobitex cryptocurrency exchange suspended external operations, resulting in liquidity crises.**  
- **International condemnation by Amnesty International and Reporters Without Borders cited breach of digital rights and humanitarian concerns.**  
- **Increased censorship precedent raises concerns about Iran’s future governance of the digital sphere and potential for prolonged “splinternet” isolation.**

---

## 7. Countermeasures & External Interventions

- Elon Musk’s Starlink satellites partially mitigated blackout impact for border regions with satellite terminals.  
- Jamming attempts by Iranian authorities curtailed satellite reception in urban centers.  
- Underground mesh networks and peer-to-peer applications saw marginal usage upticks.

---

## 8. Conclusion & Outlook

The June 2025 Iran internet shutdown represents an evolution in state-level network control, employing a wartime strategy that fuses traditional censorship with backbone-level isolation. This event signals a paradigm shift toward national digital sovereignty through technical splintering of the global internet. Future conflicts may see similar approaches employed, with implications for internet governance, cybersecurity, and civil liberties worldwide.

---

## 9. References

- NetBlocks. "Iran Internet Shutdown Report, June 2025." [NetBlocks.org](https://netblocks.org)  
- Cloudflare Radar. "Regional Traffic Analytics." [Cloudflare.com/radar](https://radar.cloudflare.com)  
- Kentik Labs. "BGP and Flow Data Analysis, June 2025." [Kentik.com](https://kentik.com)  
- IODA Project. "Internet Outage Detection and Analysis." [Ioda.gatech.edu](https://ioda.gatech.edu)  
- Amnesty International. "Iran: Digital Rights Violations during 2025 Conflict."  
- Reporters Without Borders. "Internet Blackouts and Human Rights."  
- The Verge, TechCrunch, Wired — various articles on Iran blackout  
