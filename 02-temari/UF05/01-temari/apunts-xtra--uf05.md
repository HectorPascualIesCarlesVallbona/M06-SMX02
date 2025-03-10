- Wiresharck permet comprovar el funcionament dels protocols de xarxa.
- Si posem un sniffer a la xarxa Ethernet: Cal connectar a un port de mirroring del switch
- El programari d'inventari de xarxa: Permet saber si ha canviat el programari de l’equip. 
- Nagios té com a funció: Alertar en cas d'activitat de hackers.
- Protocol UDP: És un protocol no confiable.
- Amb Wireshark NO podem: Avisar l'administrador si un servei cau.
- PCAP pot ser: l’extensió dels arxius amb captura de paquets.

---

### **Què és un Sniffer, un Switch i un Port de Mirroring del Switch?**

---

## **1️⃣ Què és un Sniffer?**
Un **sniffer** és un programari o dispositiu de xarxa que captura i analitza els paquets de dades que circulen per una xarxa.  
📌 **Funció principal:**  
- Monitoritzar el tràfic de xarxa.
- Diagnosticar problemes de connexió.
- Analitzar comunicacions entre dispositius.
- Identificar possibles atacs de xarxa.

✅ **Exemples de sniffers coneguts:**  
- **Wireshark** (el més famós i usat per administradors de xarxa).  
- **tcpdump** (eina de línia de comandes).  
- **Ettercap** (també pot realitzar atacs de Man-in-the-Middle).  

🔴 **Perill dels sniffers**:  
- Si s’utilitzen sense autorització, poden capturar contrasenyes i dades sensibles.  
- Un atacant podria usar un sniffer per fer espionatge de la xarxa (network sniffing).  

---

## **2️⃣ Què és un Switch?**
Un **switch** és un dispositiu de xarxa que connecta diversos ordinadors i dispositius dins d’una xarxa local (LAN).  
📌 **Característiques principals:**  
- **Intelligent forwarding**: A diferència d’un hub, un switch **aprèn quina MAC address hi ha en cada port** i només envia els paquets als dispositius correctes.  
- **Millor rendiment**: Evita que el tràfic sigui vist per tots els dispositius de la xarxa, reduint la congestió.  

✅ **Exemple:**  
- Un switch de 8 ports permet connectar 8 ordinadors en una xarxa local.  

🔴 **Limitació per a sniffers:**  
- Com que el switch només envia paquets al dispositiu destinatari, un **sniffer normal no pot veure tot el tràfic de la xarxa**.  

---

## **3️⃣ Què és un Port de Mirroring del Switch?**
Un **port mirroring (SPAN - Switched Port Analyzer)** és una funcionalitat dels switches que permet **copiar tot el tràfic de la xarxa cap a un port concret**.

📌 **Per a què serveix?**  
- Permet que un **sniffer** (com Wireshark) vegi tot el tràfic de la xarxa sense limitacions.  
- Es fa servir per a **monitoritzar la xarxa** i detectar problemes o intrusions.  

✅ **Exemple d’ús:**  
- Si un administrador vol veure tot el tràfic de xarxa que passa per un switch, configura un **port mirroring** per duplicar el tràfic d’un altre port i enviar-lo a un ordinador amb **Wireshark**.

---

## **📌 Resum**
- Un **sniffer** com **Wireshark** captura el tràfic de xarxa.
- Un **switch** només envia els paquets als dispositius corresponents.
- Un **port mirroring** permet copiar el tràfic de diversos ports cap a un ordinador amb un **sniffer**.

---

### **Què és Nagios?** 🖥️🔍

Nagios és un **sistema de monitorització de xarxes i servidors** que permet supervisar l'estat de dispositius, serveis i aplicacions en temps real. És una eina molt utilitzada per **administradors de sistemes i xarxes** per garantir el correcte funcionament dels sistemes informàtics.

---

## **📌 Funcionalitats principals de Nagios**
✅ **Monitorització de xarxes i sistemes:**  
- Supervisa servidors, switches, routers, bases de dades, aplicacions, etc.
- Detecta errors o caigudes abans que afectin els usuaris.

✅ **Alertes en temps real:**  
- Notifica per **correu electrònic, SMS o notificacions** si algun servei o servidor cau.
- Evita que problemes passin desapercebuts.

✅ **Supervisió de recursos:**  
- Controla ús de **CPU, RAM, disc, xarxa, processos actius**, etc.
- Permet detectar sobrecàrregues o possibles fallades.

✅ **Històric i generació d'informes:**  
- Guarda un registre de l'estat dels sistemes per analitzar **tendències** i **anticipar problemes futurs**.

✅ **Escalabilitat i personalització:**  
- Permet afegir **plugins** per ampliar funcionalitats.
- Suporta grans infraestructures amb molts dispositius.

---

## **📌 Com funciona Nagios?**
1️⃣ **Els "hosts" (servidors, routers, switches, etc.) envien informació a Nagios.**  
2️⃣ **Nagios compara l'estat dels dispositius amb regles predefinides.**  
3️⃣ **Si detecta un problema, genera una alerta immediata.**  
4️⃣ **Els administradors reben notificacions i poden actuar abans que un problema es converteixi en una fallada crítica.**  

---

### **Per què el protocol UDP és considerat no confiable?** 🛜❌✅

---

### **📌 Què és UDP?**
**UDP (User Datagram Protocol)** és un protocol de la capa de **Transport** del model **TCP/IP** que permet l'enviament de dades **sense establir una connexió prèvia** i sense garantir que arribin correctament.

---

### **🚀 Característiques principals de UDP**
✅ **Velocitat alta**: No espera confirmació dels paquets enviats.  
✅ **Baixa latència**: Ideal per aplicacions en temps real.  
❌ **No garanteix entrega**: Els paquets poden perdre's o arribar desordenats.  
❌ **No hi ha retransmissió**: Si un paquet es perd, UDP no el reenviarà.  

---

### **📌 Per què UDP és "no confiable"?**
📌 **1️⃣ No té mecanismes de confirmació (ACK)**  
- Amb TCP, cada paquet enviat espera una confirmació (**ACK - Acknowledgment**).  
- Amb **UDP, el receptor no envia confirmació**, així que l’emissor no sap si el paquet ha arribat.  

📌 **2️⃣ No garanteix l’ordre dels paquets**  
- UDP **envia els paquets individualment**, i poden arribar desordenats.  
- TCP, en canvi, reordena els paquets per garantir la seqüència correcta.  

📌 **3️⃣ No detecta ni corregeix errors**  
- **TCP detecta errors i sol·licita la retransmissió de dades incorrectes**.  
- **UDP simplement envia dades sense verificar-ne la integritat**.  

📌 **4️⃣ No gestiona congestió de xarxa**  
- TCP **s’adapta** a la velocitat de la xarxa per evitar saturació.  
- UDP **segueix enviant paquets al màxim ritme possible**, sense importar la qualitat de la xarxa.

---

Wireshark **no pot avisar l'administrador si un servei cau** perquè és un **sniffer** que captura i analitza el tràfic de xarxa, però **no monitoritza serveis ni envia alertes**.  

🔍 **Per detectar caigudes de serveis**, s'utilitzen eines com **Nagios, Zabbix o PRTG**, que supervisen servidors i serveis en temps real i envien notificacions en cas de fallada.

---

**PCAP (Packet Capture)** és el format d’arxiu estàndard per **emmagatzemar captures de paquets de xarxa** realitzades per eines com **Wireshark, tcpdump i Tshark**.  

📂 **Fitxers PCAP** tenen l’extensió **.pcap** i contenen **dades crues** dels paquets capturats, permetent analitzar comunicacions de xarxa amb detall.

---

- Taula exemple **PfSense**

| **Acció**  | **Protocol** | **Origen**  | **Destí**  | **Port Destí** | **Descripció** |
|------------|-------------|-------------|------------|---------------|----------------|
| **Allow**  | TCP         | 192.168.20.30 | *Any*      | 80, 443       | Permetre accedir a qualsevol pàgina web des de l’ordinador **192.168.20.30** |
| **Allow**  | TCP         | !192.168.20.30 | *Any*      | 21            | Permetre accés FTP des de tota la LAN **excepte** l’ordinador **192.168.20.30** |
| **Allow**  | TCP         | *Any*         | *Any*      | 22            | Permetre accés SSH des de tots els ordinadors de la LAN |
| **Block**  | TCP         | 192.168.20.30 | 192.168.20.66 | 22        | **Blocar** accés SSH des de **192.168.20.30** cap al servidor **192.168.20.66** |
| **Allow**  | UDP         | *Any*         | *Any*      | 53            | Permetre tràfic DNS per a resolució de noms de domini |
| **Deny (Per defecte)** | *Any*  | *Any* | *Any* | *Any* | **Blocar tot el tràfic que no coincideixi amb cap regla anterior** (Política Restrictiva) |


#### **3. Explicació**
1. La **primera regla** permet a **192.168.20.30** accedir a **pàgines web** (HTTP i HTTPS) (ports 80 i 443).
2. La **segona regla** permet accedir a **FTP** des de qualsevol ordinador **excepte** **192.168.20.30**.
3. La **tercera regla** permet accedir a **SSH** (port 22) des de qualsevol host de la LAN.
4. La **quarta regla** **bloca** específicament l’accés SSH de **192.168.20.30** cap al servidor **192.168.20.66**.
5. La **cinquena regla** permet el tràfic **DNS** (port 53 UDP) perquè qualsevol ordinador de la LAN pugui resoldre noms de domini.
6. **Finalment**, la política per defecte **denega qualsevol altre tràfic** que no coincideixi amb les regles anteriors.

---

Els **ports 80 i 443** són els ports estàndard per accedir a pàgines web:

- **Port 80 (HTTP - HyperText Transfer Protocol)**:  
  - S’utilitza per a la navegació web sense xifrar (protocol HTTP).  
  - Quan escrius una adreça com `http://exemple.com`, el navegador es comunica amb el servidor a través del port **80**.  
  - **Desavantatge**: No ofereix seguretat, ja que la informació viatja sense xifrar.

- **Port 443 (HTTPS - HyperText Transfer Protocol Secure)**:  
  - S’utilitza per a la navegació web segura (protocol HTTPS).  
  - Quan accedeixes a `https://exemple.com`, la comunicació entre el teu navegador i el servidor està xifrada mitjançant **TLS/SSL**.  
  - **Avantatge**: Protegeix les dades sensibles (ex. contrasenyes, dades bancàries, etc.).

---

### **Per què cal permetre aquests ports?**
Quan un usuari vol navegar per Internet:
1. Si accedeix a una web **sense xifrar**, la connexió passa pel **port 80**.
2. Si accedeix a una web **segura (HTTPS)**, la connexió passa pel **port 443**.
3. Si bloquegem aquests ports, l’usuari **no podrà obrir cap pàgina web**.

### **Per què ambdós ports i no només HTTPS (443)?**
- **Moltes webs encara permeten HTTP (port 80)**, i algunes redirigeixen automàticament a HTTPS.
- Si només es permet el **port 443**, es pot restringir la navegació web només a llocs segurs.
- Si un usuari vol accedir a qualsevol web, cal tenir **tots dos ports oberts**.

---

El **port 21** s'utilitza per a **FTP (File Transfer Protocol)**, que és el protocol estàndard per a la transferència de fitxers entre un client i un servidor.

---

### **Per què el port 21 en la regla del firewall?**
- **FTP funciona mitjançant el port 21** en mode **control**.  
- Quan un usuari inicia una sessió FTP amb un servidor, la connexió inicial es fa mitjançant aquest port.  
- Sense permetre el **port 21**, no es podria establir la connexió FTP.

---

El **port 22** s'utilitza per al **protocol SSH (Secure Shell)**, que permet connexions segures i xifrades a servidors remots.

---

### **Per què el port 22 en la regla del firewall?**
- **SSH és un protocol de comunicació remota segur**, molt utilitzat per:
  - **Administració de servidors** (gestió remota de sistemes Linux i Unix).
  - **Transferència segura de fitxers** mitjançant `SCP` o `SFTP`.
  - **Execució de comandes a distància** en un terminal.

- Sense permetre el **port 22**, **no es podrien establir connexions SSH** a servidors de la xarxa.

---

### **Per què el port 53 i per què UDP en DNS?**

---

## **1️⃣ Per què el port 53?**
El **port 53** és l'estàndard per a **DNS (Domain Name System)**, el servei que permet convertir **noms de domini (ex. google.com) en adreces IP** (ex. 142.250.180.14).  
- **Sense el port 53 o el servei DNS, els ordinadors no podrien navegar per Internet mitjançant noms de domini** i haurien d'introduir les adreces IP manualment.

---

## **2️⃣ Per què UDP en DNS?**
El protocol **DNS pot funcionar amb UDP o TCP**, però **normalment usa UDP** perquè:
✅ **És més ràpid i eficient**:  
- **UDP (User Datagram Protocol)** és un protocol **sense connexió**, és a dir, **envia una sol·licitud i no espera confirmació** abans de rebre una resposta.  
- Això fa que **les consultes DNS siguin més ràpides**, ja que el servidor no ha de gestionar una connexió prèvia amb cada petició.

✅ **Les consultes DNS són petites**:  
- La majoria de respostes DNS tenen **menys de 512 bytes**, així que poden ser enviades en **un sol paquet UDP** sense fragmentació.

---

### **Per què l'ordre de les regles en el firewall és important?**

Els **firewalls processen les regles en ordre seqüencial** de dalt a baix. Quan un paquet de dades arriba al firewall:
1. **Comprova la primera regla**: Si coincideix, aplica l’acció (Acceptar, Rebutjar o Bloquejar).
2. **Si no coincideix, passa a la següent regla.**
3. **Si arriba al final de la llista i no coincideix amb cap regla, es denega per defecte** (política restrictiva).

📌 **Conclusió:**  
- Les **regles més específiques** han d’anar abans de les més generals.  
- Si una regla general apareix abans d’una de més específica, aquesta última **mai s’executarà**.

---

### **Per què aquest ordre?**
1️⃣ **Primera regla** (HTTP/HTTPS per a 192.168.20.30) → S'ha de posar primer perquè és **molt específica**. Si es col·loqués després d'una regla general, podria ser sobreescrita.  

2️⃣ **Segona regla** (Permet FTP per a tothom excepte 192.168.20.30) → Va després perquè conté una **excepció** (`!192.168.20.30`). Si es posés després d'una regla general de bloqueig, no funcionaria.  

3️⃣ **Tercera regla** (SSH permès per a tothom) → Ha d’anar abans de la quarta, ja que estableix una norma general.  

4️⃣ **Quarta regla** (Bloca SSH només per a 192.168.20.30 cap a 192.168.20.66) → Ha d’anar **després** de la regla general de SSH perquè la regla general permet SSH a tothom, i aquí fem una excepció.  

5️⃣ **Cinquena regla** (Permetre DNS) → És una regla **necessària per a la navegació**, però no depèn de les anteriors, així que pot anar després.  

6️⃣ **Última regla (Política per defecte: tot blocat)** → Aquesta sempre ha d’estar **al final**, ja que evita que es permeti trànsit no autoritzat.

---

### **Exemple d'error en l'ordre**
🔴 **Si poséssim la regla "Blocar SSH de 192.168.20.30" abans de la regla "Permetre SSH per a tota la LAN"**, aquesta última la sobreescriuria i **no es bloquejaria l’accés SSH de 192.168.20.30** al servidor.

📌 **Moral de la història:** L'ordre de les regles **determina com funciona el firewall**. S’ha de posar primer allò **més específic** i després les normes generals. Sempre acabar amb una regla **deny all** per bloquejar el que no estigui explícitament permès.


