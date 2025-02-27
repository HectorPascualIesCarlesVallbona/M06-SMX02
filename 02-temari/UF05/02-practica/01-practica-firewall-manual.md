# **MANUAL DE PRÀCTICA: TALLAFOCS PERSONALS I CORPORATIUS**

## **1. TALLAFOCS PERSONAL AMB GUI (Ubuntu + Gufw)**

### **1.1 Instal·lació de Gufw al servidor**
1. Obre una terminal a la màquina virtual Ubuntu (Servidor).
2. Instal·la Gufw:
   ```bash
        sudo apt update
        sudo apt install gufw -y
   ```
3. Obre l'aplicació **Gufw** des del menú d'aplicacions o amb:
   ```bash
        sudo gufw
   ```
4. Assegura't que estigui desactivat inicialment.

---

### **1.2 Instal·lació d'eines al client**
1. Crea una altra màquina virtual Ubuntu (Client).
2. Instal·la **nmap**:
   ```bash
        sudo apt update
        sudo apt install nmap -y
   ```

---

### **1.3 Instal·lació de serveis al servidor**
1. A la màquina **Servidor**, instal·la **Apache** i **OpenSSH Server**:
   ```bash
        sudo apt install apache2 openssh-server -y
   ```
2. Comprova que els serveis estan en execució:
   ```bash
        sudo systemctl status apache2
        sudo systemctl status ssh
   ```

---

### **1.4 Comprovació de serveis des del client**
1. Obre un navegador web al **Client** i accedeix a la IP del **Servidor**:
   ```
        http://IP_DEL_SERVIDOR
   ```
   *Si Apache està en marxa, hauria d'aparèixer la pàgina per defecte d'Apache*.

   Si no surt res és perquè no existeix IP a client o servidor.
   Per definir una IP: *15 per servidor, 20 per client*
   ```bash
        sudo ip addr add 10.0.2.15/24 dev enp0s3
        sudo ip route add default via 10.0.2.1
    ```

2. Comprova que pots connectar via SSH:
   ```bash
        ssh usuari@IP_DEL_SERVIDOR
   ```
   Si dona error tipus que no troba `destination host` o `unreachable` => [error-unreachable.md](./practica-firewall-error-unreachable.md)

---

### **1.5 Escaneig de ports amb nmap**
Des del **Client**, executa:
```bash
    nmap IP_DEL_SERVIDOR
```
Hauries de veure els ports **80 (HTTP)** i **22 (SSH)** oberts.

---

### **1.6 Activació del Firewall**
1. A la màquina **Servidor**, obre **Gufw** i activa'l.
2. Configura **"Incoming: Deny"** i **"Outgoing: Allow"**.
3. Comprova que el **Client** ja no pot accedir als serveis web ni SSH.

---

### **1.7 Creació de regles**
1. **Permetre Apache (HTTP)**:
   - A **Gufw**, ves a **Regles** → Prem **"+"** → Selecciona **Apache**.
2. **Permetre SSH**:
   - A **Gufw**, ves a **Regles** → Prem **"+"** → Selecciona **SSH**.
3. Comprovar que les regles s’han aplicat:
    ```bash
        sudo ufw status verbose
    ```
4. sudo netplan apply

---

### **1.8 Comprovacions finals**
1. Al **Client**, executa `nmap IP_DEL_SERVIDOR` i comprova que els serveis tornen a estar accessibles.
2. Prova de connectar via SSH i accedir a la web.

---

✅ **FITA 1: Demana validació al professor abans de continuar.**

---

## **2. CONFIGURACIÓ DE PFSENSE (FIREWALL CORPORATIU)**

### **2.1 Creació de la màquina virtual PfSense**
1. Crea una nova màquina virtual a **VirtualBox**.
2. Assigna:
   - **Interfície WAN**: NAT
   - **Interfície LAN**: Xarxa interna
3. Instal·la **PfSense** seguint aquests passos:
   - Acceptar termes
   - Instal·lar => continuar amb les dades predefinides
   - Configurar **WAN** (DHCP)
   - Configurar **LAN** (IP: 192.168.100.1)

---

### **2.2 Configuració de la LAN**
0. Treu la iso de l'unitat òptica i reinicia.
1. Obre la consola de **PfSense**, a les opcions escull `shell` prenen el número `8` i executa:
   ```bash
        ping 8.8.8.8
   ```
   Si hi ha resposta, la xarxa està bé.
2. Crea una màquina virtual Ubuntu amb *xarxa interna*.
3. Comprova connectivitat:
   ```bash
        ping 192.168.100.1 # mateixa IP que la LAN de PfSense configurada
   ```
   Si necessites modificar la IP de la LAN de PfSense:
   - Ves al `Shell`a la pantalla d'inici de la màquina amb PfSense
   - Asignes tú mateix una IP estàtica
      ```bash
         ifconfig em1 192.168.1.1/24 up
      ```
   - Comproves que sigui correcta
      ```bash
         ifconfig em1
      ```
   - Proves el ping a la MV Ubuntu amb la nova IP
      ```bash
         ping 192.168.1.1
      ```
4. Accedeix a **https://192.168.100.1** o **https://192.168.1.1** i inicia sessió amb:
      ```
         usuari: admin
         password: pfsense
      ```

---

✅ **FITA 2: Demana validació al professor abans de continuar.**

---

## **3. PROVES I CONFIGURACIÓ DEL FIREWALL**

### **3.1 Política permissiva (Blocar allò no desitjat)**
*Tots els passos es fan a pfSense**, excepte el pas 2 i 3, que es fan a **Ubuntu Client**.

#### **3.1.1 Activar DHCP a la LAN** (pfSense)
1. Accedeix a la interfície web de pfSense:  
   `https://192.168.1.1`
2. Ves a **Services** → **DHCP Server**.
3. Activa **Enable DHCP server on LAN interface**.
4. Assigna un rang d'IP, per exemple:
   - **Range Start**: `192.168.1.100`
   - **Range End**: `192.168.1.150`
5. Desa la configuració i reinicia el servei DHCP.

#### **3.1.2 Comprovar que l'Ubuntu Client té una IP vàlida** (Ubuntu Client)
1. Obre un terminal a Ubuntu i executa:
   ```bash
      ip a
   ```
2. Comprova que l'adreça IP està dins del rang `192.168.1.100 - 192.168.1.150`.
3. Si no té IP, força la petició DHCP:
   ```bash
      dhclient -v
   ```

#### **3.1.3 Provar d'accedir a Internet** (Ubuntu Client)
1. Comprova la connexió fent un ping a Google:
   ```bash
      ping 8.8.8.8
   ```
2. Si hi ha resposta, la configuració és correcta.
3. Prova d'accedir a una pàgina web per confirmar.

### **3.1.4 Blocar el tràfic ICMP (ping)** (pfSense)
1. Accedeix a **Firewall** → **Rules** → **LAN**.
2. Fes clic a **Add Rule**.
3. Configura la regla:
   - **Action**: `Block`
   - **Protocol**: `ICMP`
   - **Source**: `any`
   - **Destination**: `any`
   - **Description**: "Block ICMP (ping)"
4. Desa la configuració i aplica els canvis.

### **3.1.5 Comprovar que no es pot fer ping** (Ubuntu Client)
1. Torna a provar de fer `ping` a Google:
   ```bash
      ping 8.8.8.8
   ```
2. Si la configuració és correcta, el `ping` hauria de fallar amb un error com:
   ```
      Destination Host Unreachable
   ```

🔹 **Si encara pots fer ping, prova de netejar la memòria cau del firewall a pfSense:**
```bash
   pfctl -F all
   pfctl -d
   pfctl -e
```
---

### **3.2 Política restrictiva (Permetre només el necessari)**
1. Accedeix a la interfície web de **pfSense** (`https://192.168.1.1`).
2. Ves a **Firewall** → **Rules** → **LAN**.
3. **Elimina o desactiva totes les regles existents**:
   - Fes clic a la icona de la paperera 🗑️ per eliminar cada regla.
   - Alternativament, fes clic a la icona de "⚫" per desactivar-les.
4. Desa els canvis i aplica la configuració.

#### **3.2.1 Permetre només tràfic essencial**
Ara s’han de crear regles específiques per permetre únicament HTTP, DNS i SSH.

📍 **Afegeix aquestes regles a:**  
👉 **Firewall** → **Rules** → **LAN** → **Add Rule**

##### 🔹 **Permetre tràfic HTTP i HTTPS (navegació web)**
1. **Action**: `Pass`
2. **Protocol**: `TCP`
3. **Source**: `any`
4. **Destination**: `any`
5. **Destination Port Range**:
   - From: `HTTP (80)`
   - To: `HTTPS (443)`
6. **Description**: `Allow HTTP/HTTPS Traffic`
7. Desa i aplica els canvis.



##### 🔹 **Permetre tràfic DNS (resolució de noms)**
1. **Action**: `Pass`
2. **Protocol**: `TCP/UDP`
3. **Source**: `any`
4. **Destination**: `any`
5. **Destination Port Range**:
   - From: `DNS (53)`
   - To: `DNS (53)`
6. **Description**: `Allow DNS Traffic`
7. Desa i aplica els canvis.

##### 🔹 **Permetre tràfic SSH**
1. **Action**: `Pass`
2. **Protocol**: `TCP`
3. **Source**: `any`
4. **Destination**: `any`
5. **Destination Port Range**:
   - From: `SSH (22)`
   - To: `SSH (22)`
6. **Description**: `Allow SSH Traffic`
7. Desa i aplica els canvis.

#### **3.2.2 Comprovar la configuració**
✅ **Comprovar navegació a Internet (HTTP/HTTPS)**  
1. Obre un navegador i intenta accedir a `https://www.google.com`.
2. Si la configuració és correcta, la pàgina s’hauria de carregar.  
👉 Si no funciona, segueix aquestes [pases](./practica-firewall-manual-resolucio-noms-DNS.md) 

✅ **Comprovar resolució DNS**  
Executa:
```bash
   nslookup google.com
```

✅ **Comprovar connexió SSH**
Des d’un altre dispositiu (o des del mateix Ubuntu Client), executa:
```bash
   ssh usuari@192.168.1.1
```
👉 Si no funciona, segueix aquestes [pases](./practica-firewall-manual-resolucio-ssh-a-pfsense.md) 

❌ **Prova ping**
```bash
   ping 8.8.8.8
```

❌ **Prova de connectar per FTP**
```bash
   ftp 192.168.1.1
```

---

✅ **FITA 3: Demana validació al professor abans de continuar.**

---

## **4. CONFIGURACIÓ D'UNA DMZ (3-LEG MODEL)**

### **4.1 Creació de la DMZ**

### **1. Afegir una tercera interfície a pfSense a VirtualBox**
1. A VirtualBox, selecciona la màquina virtual de **pfSense**.
2. Ves a **Configuració** → **Xarxa**.
3. Afegeix una **tercera interfície de xarxa**:
   - Activa **Adaptador 3**.
   - Configura’l com a **Xarxa interna**.
   - Assigna el nom: **DMZ_ELMEUNOM** (substitueix *ELMEUNOM* pel teu nom real).

4. Desa els canvis i reinicia la màquina virtual de **pfSense**.

---

### **2. Assignar i configurar la interfície a pfSense**
1. Accedeix a **pfSense Web UI** (`https://192.168.1.1`).
2. Ves a **Interfaces → Assignments**.
3. Hauria d'aparèixer una interfície nova disponible (`vtnet2` o `em2`, depenent de VirtualBox).
4. Fes clic a **Add** per afegir-la.
5. Un cop afegida, fes clic sobre la interfície nova (haurà rebut un nom com **OPT1**).
6. Activa-la marcant **Enable Interface**.
7. Assigna-li els paràmetres següents:
   - **Nom**: `DMZ`
   - **IPv4 Configuration Type**: **Static IPv4**
   - **IPv4 Address**: `192.168.200.1/24`
8. Desa els canvis i reinicia pfSense.

---

### **3. Comprovar la configuració**
- A **Status → Interfaces**, comprova que la interfície **DMZ** apareix activa.
- A la consola de pfSense (`Shell` o via SSH), executa:
  ```bash
      ifconfig
  ```
  i verifica que `em2` (o el nom que li hagi assignat pfSense) té l’adreça `192.168.200.1`.

---

### **4.2 Creació del servidor DMZ**

### **1. Creació d'una màquina virtual Ubuntu Server**
1. A **VirtualBox**, crea una nova màquina virtual amb:
   - **Nom**: Ubuntu-DMZ.
   - **Tipus**: Linux.
   - **Versió**: Ubuntu (64-bit).
   - **Memòria RAM**: 2 GB (o més si ho necessites).
   - **Disc dur**: 10 GB mínim (en format VDI, expansible).

2. A **Configuració** → **Xarxa**:
   - Configura **Adaptador 1** com a **Xarxa interna**.
   - Assigna-li el nom **DMZ_ELMEUNOM** (substitueix *ELMEUNOM* pel teu nom real).

3. Desa els canvis i inicia la instal·lació de **Ubuntu Server**.

---

### **2. Configurar una IP estàtica**
Un cop iniciada la màquina Ubuntu Server, configura la xarxa de la DMZ.

1. Obre el fitxer de configuració de **Netplan**:
   ```bash
      sudo nano /etc/netplan/00-installer-config.yaml
   ```
2. Modifica el fitxer per establir una IP estàtica:
   ```yaml
   network:
      ethernets:
         enp0s3:
            addresses:
              - 192.168.200.10/24
            routes:
              - to: default
                via: 192.168.200.1
            nameservers:
              addresses: [8.8.8.8, 1.1.1.1]
      version: 2

   ```
   **Nota:** Assegura’t que `ens3` és la interfície correcta. Pots comprovar-ho amb:
   ```bash
      ip a
   ```

3. Desa els canvis.
4. Aplica la configuració amb:
   ```bash
      sudo netplan apply
   ```

---

### **3. Verificació de la connectivitat**
1. Comprova que la IP s'ha assignat correctament:
   ```bash
   ip a
   ```
   Hauries de veure `192.168.200.10/24` assignada a `ens3`.

2. Comprova la connectivitat amb pfSense:
   ```bash
   ping 192.168.200.1
   ```
   Si reps resposta, vol dir que la xarxa interna **DMZ_ELMEUNOM** està configurada correctament.

3. Comprova la resolució DNS:
   ```bash
   nslookup google.com
   ```
   o
   ```bash
   dig google.com
   ```
   Si el DNS no resol, revisa que pfSense tingui el **DNS Resolver** actiu i ben configurat.

---

---

### **4.3 Configuració de serveis**
1. Instal·la **Apache**, **SSH** i **FTP**:
   ```bash
        sudo apt install apache2 openssh-server vsftpd -y
   ```
2. Bloqueja la comunicació DMZ → LAN.
3. Comprova que pots accedir als serveis des de fora.

---

✅ **FITA 4: Demana validació al professor abans de finalitzar.**

