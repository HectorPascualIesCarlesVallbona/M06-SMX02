# **🔍 Wireshark i Sniffers**

## **1️⃣ Wireshark**
- Wireshark permet comprovar el funcionament dels protocols de xarxa.
- Amb Wireshark NO podem avisar l'administrador si un servei cau.
- PCAP pot ser l’extensió dels arxius amb captura de paquets.

## **2️⃣ Sniffers**
Un **sniffer** és un programari o dispositiu de xarxa que captura i analitza els paquets de dades que circulen per una xarxa.
- **🔎 Funció principal:**
  - Monitoritzar el tràfic de xarxa.
  - Diagnosticar problemes de connexió.
  - Analitzar comunicacions entre dispositius.
  - Identificar possibles atacs de xarxa.
- **📌 Exemples:**
  - **Wireshark** (el més famós i usat per administradors de xarxa).
  - **tcpdump** (eina de línia de comandes).
  - **Ettercap**

## **3️⃣ Switch i Port de Mirroring**
### **Què és un Switch?**
Un **switch** és un dispositiu de xarxa que connecta diversos ordinadors i dispositius dins d’una xarxa local (LAN).
- **📌 Característiques:**
  - Aprèn quina **MAC address** hi ha en cada port i només envia els paquets als dispositius correctes.
  - Millor rendiment en comparació amb un hub (no aprèn).

### **Què és un Port de Mirroring?**
Un **port mirroring (SPAN - Switched Port Analyzer)** és una funcionalitat dels switches que permet **copiar tot el tràfic de la xarxa cap a un port concret** per ser monitoritzat per un sniffer com Wireshark.
- **📌 Per a què serveix?**
  - Monitoritzar el tràfic de xarxa.
  - Diagnosticar problemes o detectar intrusions.

---

# **Nagios i Monitorització de Xarxes**

## **1️⃣ Què és Nagios?**
Nagios és un **sistema de monitorització de xarxes i servidors** que permet supervisar l'estat de dispositius, serveis i aplicacions en temps real.
- **Funció principal:** Alertar en cas d'activitat de hackers.
- **📌 Característiques:**
  - Supervisa **servidors, switches, routers, bases de dades, aplicacions, etc.**
  - Detecta errors o caigudes abans que afectin els usuaris.
  - Notifica per **correu electrònic, SMS o notificacions** si un servei o servidor cau.

---

# **Protocol UDP**

## **1️⃣ Característiques de UDP**
- No té mecanismes de confirmació (**ACK**).
- No garanteix l’ordre dels paquets.
- No detecta ni corregeix errors.
- S’utilitza en **streaming, jocs online, videoconferències, DNS**.

## **2️⃣ Per què és no confiable?**
- No té confirmació de recepció.
- Els paquets poden arribar desordenats.
- No hi ha retransmissió en cas de pèrdua.

---

# **🛡️ Regles de Firewall en PfSense**

## **1️⃣ Regles Configurades**
| **🔧 Acció**  | **📡 Protocol** | **🌍 Origen**  | **🎯 Destí**  | **🔢 Port Destí** | **📌 Descripció** |
|------------|-------------|-------------|------------|---------------|----------------|
| ✅ **Allow**  | TCP         | 192.168.20.30 | *Any*      | 80, 443       | 🌐 Permetre accedir a qualsevol pàgina web des de l’ordinador **192.168.20.30** |
| ✅ **Allow**  | TCP         | !192.168.20.30 | *Any*      | 21            | 📁 Permetre accés FTP des de tota la LAN **excepte** l’ordinador **192.168.20.30** |
| ✅ **Allow**  | TCP         | *Any*         | *Any*      | 22            | 🔑 Permetre accés SSH des de tots els ordinadors de la LAN |
| ❌ **Block**  | TCP         | 192.168.20.30 | 192.168.20.66 | 22        | 🚫 **Blocar** accés SSH des de **192.168.20.30** cap al servidor **192.168.20.66** |
| ✅ **Allow**  | UDP         | *Any*         | *Any*      | 53            | 🌍 Permetre tràfic DNS per a resolució de noms de domini |
| 🚫 **Deny (Per defecte)** | *Any*  | *Any* | *Any* | *Any* | 🔒 **Blocar tot el tràfic que no coincideixi amb cap regla anterior** (Política Restrictiva) |

## **2️⃣ Explicació de l’Ordre de les Regles**
- Les regles més **específiques van primer**.
- Les regles més **generals després**.
- La política per defecte **bloca tot el tràfic no permès**.

---

# **Explicació de Ports Importants**

## **1️⃣ Port 80 i 443 (HTTP/HTTPS)**
- **80 (HTTP)**: Navegació web **no segura**.
- **443 (HTTPS)**: Navegació web **segura i xifrada** amb TLS/SSL.

## **2️⃣ Port 21 (FTP)**
- Protocol per **transferència de fitxers**.
- Necessari per establir connexió amb un servidor FTP.

## **3️⃣ Port 22 (SSH)**
- Serveix per **accedir remotament a servidors** amb una connexió segura.

## **4️⃣ Port 53 (DNS) i UDP**
- **Permet convertir noms de domini en adreces IP**.
- Fa servir **UDP perquè és més ràpid i no necessita connexió prèvia**.

---

# **📌 Conclusió**
- **Wireshark** és una eina de captura de paquets, però no envia alertes.
- **Nagios** és una eina de monitorització que envia alertes en temps real.
- **UDP és ràpid però no confiable**.
- **Els ports i les regles de firewall han d'estar ben configurats per garantir seguretat i funcionalitat.**

