# Iran Internet Shutdown – Juni 2025  
## Eine tiefgehende technische und datenbasierte forensische Analyse von Irans Strategie der Netzisolierung im Kriegsfall (IR-GFW)

---

## Zusammenfassung

Im Juni 2025 führte der Iran eine beispiellose landesweite Internetsperre durch, die mit eskalierenden militärischen Konflikten einherging und zu einer nahezu vollständigen Trennung vom globalen Internet führte. Dieses Papier liefert eine umfassende forensische Untersuchung der technischen Architektur der Abschaltung und nutzt dabei Telemetriedaten aus verschiedenen Quellen, darunter NetBlocks, Cloudflare Radar, Kentik und IODA. Wir analysieren Manipulationen im BGP-Routing, DPI-Filtermethoden, VPN-Blockierungstechniken und die strategische Implementierung von nationalen Whitelists. Darüber hinaus werden die geopolitischen und sozioökonomischen Auswirkungen diskutiert sowie langfristige Konsequenzen für die digitale Souveränität Irans erörtert.

---

## 1. Einführung

Im Sommer 2025 kam es zu einer deutlichen Eskalation des israelisch-iranischen Konflikts, die in gezielten Luftangriffen auf kritische iranische militärische und nukleare Infrastruktur gipfelte. Als Reaktion darauf führten die iranischen Behörden eine koordinierte landesweite Netzisolierung durch, die hier als IR-GFW (Iranian Great Firewall) bezeichnet wird. Im Gegensatz zur anhaltenden Zensur-Firewall Chinas fungierte die IR-GFW als digitale Belagerung im Kriegsfall, die mehrschichtige Filterung und den Rückzug von Infrastruktur kombinierte, um nahezu vollständige digitale Isolation zu erreichen. Diese Analyse aggregiert Telemetriedaten unabhängiger Messstellen, um den Zeitverlauf und die technische Umsetzung der Abschaltung zu rekonstruieren.

---

## 2. Methodik & Datenquellen

- **NetBlocks**: Globale Internet-Beobachtungsstelle, die Konnektivitätsstörungen mittels Traceroutes, DNS-Analysen und Web-Probing überwacht.  
- **Cloudflare Radar**: Analyseplattform für Internetverkehr, die globale und regionale IP-Verkehrstrends sowie Anomalien erfasst.  
- **Kentik**: Netzwerk-Analyseplattform mit Fokus auf BGP-Monitoring, Flussdaten und Peering-Statistiken.  
- **IODA (Internet Outage Detection and Analysis)**: Forschungsprojekt des Georgia Institute of Technology, das mittels aktiver Probing-Techniken Netzwerkausfälle erkennt.  

Die Daten wurden vom 10. bis 25. Juni 2025 gesammelt, um Basiswerte vor der Krise, den Beginn der Sperre sowie Phasen der teilweisen Wiederherstellung abzudecken.

---

## 3. Zeitverlauf & Ereignisrekonstruktion

| Datum      | Ereignisbeschreibung                             | Netzwerkmetriken                            |
|------------|-------------------------------------------------|---------------------------------------------|
| 13. Juni   | Israelische Luftangriffe auf iranische Ziele    | ~54% Rückgang des externen Transitverkehrs laut Kentik |
| 14.–16. Juni | Zunehmende Netzwerkdegradation               | Routing-Instabilität und sporadische DPI-Resets |
| 17. Juni   | Beginn des vollständigen Netzzusammenbruchs     | NetBlocks meldet ~97% Verlust der Konnektivität |
| 18.–19. Juni | Stabilisierung des Netz-Blackouts             | Nur nationaler Intranet-Verkehr (~3% des globalen Niveaus) |
| 21. Juni   | Temporäre teilweise Wiederherstellung (~2 Stunden) | Anstieg des Cloudflare-Verkehrs, anschließend erneut Reset |
| 22.–25. Juni | Anhaltender Blackout                          | VPN-Verkehr stark gestört, zahlreiche TCP-Resets |

---

## 4. Technische Analyse

### 4.1 BGP-Routing-Manipulation

- Iranische ISPs zogen globale BGP-Präfixe zurück, wodurch iranische ASes aus globalen Routing-Tabellen entfernt wurden.  
- Rückzüge konzentrierten sich auf Tier-1-Upstream-Provider (z. B. Level 3, Tata Communications), um internationale Peerings zu kappen.  
- Teilweise Routing-Ankündigungen blieben intern bestehen, um den Zugang zum National Information Network (NIN) sicherzustellen.  

### 4.2 Deep Packet Inspection & Filterung

- DPI-Geräte an nationalen IXPs richteten sich gegen VPN-Protokolle (OpenVPN, WireGuard) und verschlüsselte DNS-Verfahren (DoH/DoT).  
- TCP-Reset-Injektionen beendeten verdächtige Sessions abrupt.  
- SNI-Filterung (Server Name Indication) blockierte Zugriffe auf verbotene externe Dienste bei TLS-Handshakes.  
- DNS-Poisoning leitete externe Domains entweder auf interne IPs oder ins Nichts (Blackholing) um.  

### 4.3 Maßnahmen gegen VPNs & Proxies

- Aktives Probing von verschleiertem VPN-Traffic (z. B. Obfs4, Shadowsocks) führte zu vermehrten Verbindungsabbrüchen.  
- Verhaltensbasierte Fingerprinting-Techniken erkannten unübliche Handshake-Muster und lösten Resets aus.  
- Berichten zufolge kamen ML-basierte DPI-Klassifizierer zum Einsatz, um Umgehungstools dynamisch zu identifizieren.  

### 4.4 Whitelisting & Intrusion Detection

- Striktes Whitelisting ließ ausschließlich Traffic zu genehmigten IP-Bereichen und Diensten zu.  
- Intrusion-Detection-Systeme überwachten in Echtzeit nach anomalen Mustern und blockierten diese umgehend.  
- Kritische inländische Plattformen (E-Commerce, Social Media, Banking) wurden über ein geschlossenes nationales Netz geleitet.

---

## 5. Datenvisualisierung

### 5.1 NetBlocks Connectivity Index

![NetBlocks traffic chart](./data/netblocks_traffic.png)

- Zeigt einen drastischen Rückgang der Netzreichweite von 100 % auf etwa 3 % innerhalb von vier Tagen.  
- Zeitlich korreliert mit gemeldeten militärischen Aktionen.

### 5.2 IODA Active Probing Metrics

![IODA chart](./data/ioda_google_access.png)

- Probing von Google DNS zeigt zeitweise teilweise Wiederherstellung, gefolgt von erneutem Blackout.  
- Bestätigt DNS-Poisoning und selektive Blockierungen.

### 5.3 Cloudflare Traffic Analytics

![Cloudflare chart](./data/cloudflare_drop.png)

- Regionale Verkehrsdaten stimmen mit globaler Telemetrie überein und zeigen einen Verlust von 95–97 % des Traffics.  
- Der Anstieg am 21. Juni deutet auf ein kurzzeitiges Fenster wiederhergestellter Konnektivität hin.

---

## 6. Geopolitische & Sozioökonomische Auswirkungen

- **Die Kommunikationssperre behinderte massiv die Koordination der Zivilbevölkerung während der Krise.**  
- **Finanzdienstleistungen wie die Bank Sepah und die Kryptowährungsbörse Nobitex setzten externe Operationen aus, was zu Liquiditätsengpässen führte.**  
- **Internationale Organisationen wie Amnesty International und Reporter ohne Grenzen verurteilten die Sperre als Verletzung digitaler Rechte und aus humanitären Gründen.**  
- **Die verschärfte Zensur wirft Bedenken hinsichtlich der künftigen digitalen Governance Irans und eines möglichen anhaltenden “Splinternet”-Szenarios auf.**

---

## 7. Gegenmaßnahmen & Externe Interventionen

- Elon Musks Starlink-Satelliten milderten die Auswirkungen des Blackouts teilweise in Grenzregionen.  
- Iranische Behörden versuchten, Satellitenempfang in urbanen Zentren durch Störsender zu blockieren.  
- Untergrund-Mesh-Netzwerke und Peer-to-Peer-Apps verzeichneten einen leichten Anstieg der Nutzung.

---

## 8. Fazit & Ausblick

Die iranische Internetsperre im Juni 2025 markiert eine neue Evolutionsstufe staatlicher Netzwerkkontrolle, die eine Kriegsstrategie mit Backbone-Isolation kombiniert. Dieses Ereignis signalisiert einen Paradigmenwechsel hin zu nationaler digitaler Souveränität durch gezielte technische Abkopplung vom globalen Internet. Künftige Konflikte könnten ähnliche Maßnahmen sehen – mit weitreichenden Folgen für Internet-Governance, Cybersicherheit und bürgerliche Freiheiten weltweit.

---

## 9. Quellen

- NetBlocks. „Iran Internet Shutdown Report, Juni 2025.“ [NetBlocks.org](https://netblocks.org)  
- Cloudflare Radar. „Regional Traffic Analytics.“ [Cloudflare.com/radar](https://radar.cloudflare.com)  
- Kentik Labs. „BGP and Flow Data Analysis, Juni 2025.“ [Kentik.com](https://kentik.com)  
- IODA Project. „Internet Outage Detection and Analysis.“ [Ioda.gatech.edu](https://ioda.gatech.edu)  
- Amnesty International. „Iran: Digital Rights Violations during 2025 Conflict.“  
- Reporter ohne Grenzen. „Internet Blackouts and Human Rights.“  
- The Verge, TechCrunch, Wired — diverse Artikel über die Iran-Abschaltung
