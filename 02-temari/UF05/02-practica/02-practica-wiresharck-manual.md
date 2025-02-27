# **Introducció a Wireshark**
Wireshark és un **analitzador de tràfic de xarxa** que permet capturar i examinar els paquets que circulen per una xarxa. És una eina fonamental en ciberseguretat i administració de xarxes.

### **Requisits previs**
1. **Sistema operatiu**: Windows/Linux/MacOS (es recomana Linux per funcionalitats avançades).
2. **Wireshark instal·lat**. Si no el tens, instal·la’l:
   - A **Ubuntu/Debian**:
     ```bash
        sudo apt update && sudo apt install wireshark -y
     ```
   - A **Windows**, descarrega’l de [wireshark.org](https://www.wireshark.org/download.html) i segueix les instruccions.
3. **Accés a una xarxa**: Per fer captures en viu.

---

### **Configuració inicial**
1. Obre **Wireshark**.
2. A la pantalla principal, selecciona la **targeta de xarxa activa**:
   - Si fas servir Wi-Fi: selecciona **wlan0** o **enp0s3**.
   - Si fas servir Ethernet: selecciona **eth0**.
3. Fes clic a **Start** per iniciar la captura.

[si no apareix wlan0, enp0s3 o eth0](wlan-or-eth0-not-found.md)

---

### **1️. Captura de paquets ICMP (Ping i Traceroute)**
**📌 Objectiu:** Analitzar com funciona el protocol **ICMP** mitjançant **ping** i **traceroute**.

1. Obre un terminal i executa:
   ```bash
      ping -c 4 www.google.es
   ```
   **Preguntes a respondre**: Respon preguntes

2. Ara prova **traceroute**:
   ```bash
      traceroute www.google.es 
   ```
   **Preguntes a respondre**: Respon preguntes

---

### **2️. Anàlisi del protocol DNS**
1. Esborra la memòria cau DNS:
   ```bash
      sudo resolvectl flush-caches
   ```
2. Executa:
   ```bash
      nslookup www.xtec.cat
   ```
3. Filtra Wireshark per **dns**:
   - Obre Wireshark i selecciona la interfície de xarxa correcta (per exemple, enp0s3).
   - A la barra de filtres (a dalt de tot), **Analizar -> Aplicar como filtro -> Selected**, i escriu
   ```bash
      dns
   ```
   **Preguntes a respondre**: Respon preguntes

---

### **3️. Anàlisi del protocol ARP**
1. Filtra Wireshark per **arp**:
   ```bash
      arp
   ```
2. Troba l’adreça MAC del **router (gateway)**.
[find mac gateway](find-mac-gateway.md)

---

### **4️. Anàlisi d'una captura (captura1.pcapng)**
Obre el fitxer de captura **captura1.pcapng** i respon:

Per passar el fitxer del host a la teva MV, executa des de la teva MV:
```bash
   scp nom_usuari_host@IP_DEL_HOST:/ruta_absoluta_on_tens_imatge_al_host/captura1.pcapng /ruta_absoluta_on_vols_copiar_imatge_a_la_MV/
```

**Sessió FTP**
1. Filtra per FTP:
   ```
   ftp
   ```
2. Troba:
   - **Usuari i contrasenya**.
   - **Fitxers descarregats**.
   - **Contingut del fitxer descarregat**.

---

**Sessió Telnet**
1. Filtra per Telnet:
   ```
   telnet
   ```
2. Mostra:
   - Comandes introduïdes per l’usuari.
   - Text retornat pel servidor.

---

**Sessió SSH**
1. Filtra per SSH:
   ```
   ssh
   ```

---

### **5️. Captura de correu SMTP**
Per passar el fitxer del host a la teva MV, executa des de la teva MV:
```bash
   scp nom_usuari_host@IP_DEL_HOST:/ruta_absoluta_on_tens_imatge_al_host/captura2.pcapng /ruta_absoluta_on_vols_copiar_imatge_a_la_MV/
```

#### **1. Trobar el missatge enviat via SMTP**
1. **Filtrar només el protocol SMTP**:
   ```
   smtp
   ```
2. **Trobar el paquet amb el contingut del missatge**:
   - Busca un paquet que contingui l'ordre `DATA`, que indica l'inici del cos del missatge.
   - Després d'aquest, hi haurà el text del correu enviat.
3. **Veure el contingut complet**:
   - Clic dret sobre el paquet SMTP > **Follow TCP Stream**.
   - Aquí es pot llegir el missatge i qualsevol fitxer adjunt.
4. **Extreure el fitxer adjunt** (si n'hi ha):
   - A **File > Export Objects > HTTP/SMTP** (segons com estigui enviat).

---

#### **2. Instal·lar i utilitzar NetworkMiner**
NetworkMiner no sempre suporta `.pcapng`, per tant, cal convertir-lo:
1. **Amb editcap (eina de Wireshark)**:
   ```bash
      editcap captura2.pcapng captura2.pcap
   ```
2. **Carregar el fitxer a NetworkMiner**:
   - Obrir **NetworkMiner** i carregar el fitxer `captura2.pcap`.

---

#### **3. Anàlisi amb NetworkMiner**
1. **Número d’imatges i tipus**:
   - A la pestanya **Images**, veuràs les imatges capturades a la xarxa.
   - Fes una captura de pantalla i anota el tipus (PNG, JPEG, etc.).
   
2. **Número de fitxers capturats**:
   - A la pestanya **Files**, veuràs quins fitxers han estat transferits.
   - Guarda els noms i formats.

3. **Credencials trobades**:
   - A la pestanya **Credentials**, veuràs possibles usuaris i contrasenyes capturades (de protocols insegurs com FTP, Telnet, SMTP).

4. **Dominis registrats**:
   - A la pestanya **DNS**, NetworkMiner mostra els dominis resolts.

5. **Número d’hosts registrats**:
   - A la pestanya **Hosts**, es mostren totes les IPs capturades en la xarxa.

6. **Informació addicional rellevant**:
   - Cerca si hi ha tràfic sospitós, protocols insegurs, fitxers xifrats o algun altre detall destacable.

---

## RESUM
- NetworkMiner permet extreure imatges, fitxers, credencials i informació de DNS/hosts de manera més visual.
- La conversió de `.pcapng` a `.pcap` amb `editcap` és necessària si NetworkMiner no obre el fitxer directament.
