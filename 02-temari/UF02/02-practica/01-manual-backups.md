# **REPTE: CÒPIA DE SEGURETAT EN SERVIDOR LINUX**

### **1. Instal·lació i configuració d'Ubuntu Server**

#### **1.1. Inici de la instal·lació**
- Selecciona l'idioma (Català, si està disponible).
- Selecciona l'opció "Instal·lar Ubuntu Server".

#### **1.2. Configuració bàsica**
- **Idioma del sistema**: Selecciona l'idioma que prefereixis.
- **Disposició del teclat**: Configura el teclat (normalment, "Espanyol").
- **Nom de la màquina (hostname)**:
  - Introdueix `sbak-nom-cognom`.

#### **1.3. Creació de l'usuari inicial**
- Introdueix un nom d'usuari i contrasenya per accedir al sistema.
- **Nota**: Pots posar el teu nom complet com a usuari inicial. Més tard crearàs l'usuari específic per al backup.

#### **1.4. Particions del disc**
- Escull una de les opcions:
  - "Use an entire disk" (automàtic).
  - "Manual" (personalitzat).
- Assegura't que el disc seleccionat és l'adequat. Si només hi ha un, confirma i continua.

#### **1.5. Selecció de paquets i serveis**
- **Configuració del servidor SSH**:
  - Activa l'opció "Instal·lar SSH server" (imprescindible per aquesta pràctica).
- Deixa la resta de paquets desmarcats per mantenir el sistema lleuger.

#### **1.6. Finalització de la instal·lació**
- Quan acabi, el sistema reiniciarà automàticament.
- Retira el mitjà d'instal·lació (USB o ISO).
- Un cop arranqui, introdueix el nom d'usuari i contrasenya creats durant la instal·lació.

---

#### **1.7. Configuració de la màquina després de la instal·lació**

##### **1.7.1. Canviar el nom del host**
1. Comanda per configurar el nom del host:
   ```bash
   sudo hostnamectl set-hostname sbak-nom-cognom
   ```
2. Comprova el canvi:
   ```bash
   hostnamectl
   ```
3. Reinicia la màquina virtual per aplicar els canvis:
   ```bash
   sudo reboot
   ```

##### **1.7.2. Configuració de la xarxa interna**
1. **A VirtualBox**:
   - Assigna la xarxa interna amb el nom `LAN-nom-cognom`.
   - Executa la màquina virtual. Tingues paciència, pot trigar a carregar.
   - Quan hagi acabat el procés, introdueix els credencials creats durant la instal·lació.

2. **Configura una IP estàtica amb Netplan**:
   - Edició del fitxer de configuració:
     ```bash
     sudo nano /etc/netplan/01-netcfg.yaml
     ```
   - Exemple de configuració:
     ```yaml
     network:
       version: 2
       ethernets:
         enp0s3:
           addresses:
             - 192.168.1.10/24
           routes:
             - to: default
               via: 192.168.1.1
           nameservers:
             addresses:
               - 8.8.8.8
     ```
     **Notes importants**:
     - Cada nivell d'identació ha de tenir exactament 2 espais (no tabulacions).
     - Les llistes (p. ex. `addresses:`) han de començar amb un guió (`-`) seguit d'un espai.

   - Aplica els canvis:
     ```bash
     sudo netplan apply
     ```

##### **1.7.3. Configuració del servidor SSH**
1. **Instal·lar el servei SSH**:
   ```bash
   sudo apt update && sudo apt install -y openssh-server
   ```
2. **Verificar que està actiu**:
   ```bash
   sudo systemctl status ssh
   ```
3. **Si el servei no està actiu**:
   - **Activa el servei SSH perquè s'iniciï automàticament en cada reinici**:
     ```bash
     sudo systemctl enable ssh
     ```
   - **Inicia el servei manualment**:
     ```bash
     sudo systemctl start ssh
     ```
   - **Comprova que el servei està funcionant**:
     ```bash
     sudo systemctl status ssh
     ```
     - Hauries de veure una línia amb:
       ```
       Active: active (running)
       ```
---

#### **2. Configuració del client de còpies de seguretat (Ubuntu Desktop)**

1. **Crear una màquina virtual Ubuntu Desktop**:
   - **Nom del host:**
        ```bash
        sudo hostnamectl set-hostname client-nom-cognom
        sudo reboot
        ```

2. **Configurar la xarxa interna**:
   - Assigna la xarxa interna `LAN-nom-cognom` a la màquina.
   - Configura una IP estàtica des de la interfície gràfica o mitjançant Netplan, seguint el mateix procediment que el servidor.

3. **Verificació de connectivitat**:
   - Des del client:
     ```bash
     ping 192.168.1.10
     ssh nom-cognom@192.168.1.10
     ```

---

#### **3. Configuració d’autenticació SSH sense contrasenya**

1. **Generar un parell de claus SSH al client**:
   - Executa:
     ```bash
     ssh-keygen -t rsa
     ```
   - Accepta el nom de fitxer per defecte (`~/.ssh/id_rsa`) i prem ENTER quan demani contrasenya.

2. **Copiar la clau pública al servidor**:
   - Executa:
     ```bash
     ssh-copy-id ubak-nom-cognom@192.168.1.10
     ```

3. **Prova de connexió SSH sense contrasenya**:
   - Connecta’t al servidor:
     ```bash
     ssh ubak-nom-cognom@192.168.1.10
     ```

---

#### **4. Preparar el servidor per a les còpies de seguretat**

1. **Crear l’usuari de backup**:
   ```bash
   sudo adduser ubak-nom-cognom
   ```

2. **Crear el directori per als backups**:
   ```bash
   sudo mkdir /backup
   sudo chown ubak-nom-cognom:ubak-nom-cognom /backup
   ```

---

#### **5. Creació i execució d’un script de còpia de seguretat**

1. **Crear l’script al client**:
   - Obre l’editor:
     ```bash
     nano backup-nom-cognom.sh
     ```
   - Contingut de l’script:
     ```bash
     #!/bin/bash
     echo "Comprimint fitxers..."
     zip -r backup_$(date +%Y%m%d).zip ~/Documents ~/Pictures ~/Desktop

     echo "Xifrant fitxer..."
     gpg --batch --passphrase 'algunpassword' -c backup_$(date +%Y%m%d).zip

     echo "Copiant fitxer al servidor..."
     scp backup_$(date +%Y%m%d).zip.gpg ubak-nom-cognom@192.168.1.10:/backup

     echo "Eliminant fitxers temporals..."
     rm backup_$(date +%Y%m%d).zip backup_$(date +%Y%m%d).zip.gpg

     echo "Backup completat $(date)"
     ```
   - Dona-li permisos d’execució:
     ```bash
     chmod u+x backup-nom-cognom.sh
     ```

2. **Executar l’script i verificar els resultats**:
   ```bash
   ./backup-nom-cognom.sh
   ```

---

#### **6. Restaurar els arxius**

1. **Copiar el backup del servidor al client**:
   ```bash
   scp ubak-nom-cognom@192.168.1.10:/backup/backup_YYYYMMDD.zip.gpg ~/backup_restore/
   ```

2. **Desxifrar i descomprimir**:
   - Desxifra:
     ```bash
     gpg --batch --passphrase 'algunpassword' backup_YYYYMMDD.zip.gpg
     ```
   - Descomprimeix:
     ```bash
     unzip backup_YYYYMMDD.zip
     ```

---

#### **7. Automatització amb crontab**

1. **Configurar l’automatització**:
   - Edició del crontab:
     ```bash
     crontab -e
     ```
   - Afegir la línia:
     ```
     0 20 * * * /path/to/backup-nom-cognom.sh
     ```

2. **Verificar que s’executa correctament**:
   - Comprova els logs del crontab:
     ```bash
     cat /var/log/syslog | grep CRON
     ```

---

#### **8. Opcional**

- Explora alternatives com TrueNAS o Duplicity per millorar el sistema de backup.

