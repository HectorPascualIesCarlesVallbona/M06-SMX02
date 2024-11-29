### REPTE: Còpia de Seguretat en Linux

### **1. Instal·lació d'Ubuntu Server**

1. **Inici de la instal·lació**:
   - Selecciona l'idioma (Català, si està disponible).
   - Selecciona l'opció "Instal·lar Ubuntu Server".

2. **Configuració bàsica**:
   - **Idioma del sistema**: Selecciona l'idioma que prefereixis.
   - **Disposició del teclat**: Configura el teclat (normalment, "Espanyol").
   - **Nom de la màquina (hostname)**:
     - Quan ho demani, introdueix `sbak-nom-cognom`.

3. **Creació de l'usuari inicial**:
   - Introdueix un nom d'usuari i contrasenya per accedir al sistema.
   - **ATENCIÓ**: Pots posar el teu nom complet com a usuari inicial, ja que després crearàs l'usuari específic per al backup.

4. **Configuració de la xarxa**:
   - Selecciona "Configuració manual".
   - Introdueix una IP estàtica de classe C, per exemple:
     - Adreça IP: `192.168.1.10`
     - Màscara de xarxa: `/24`
     - Passarel·la (Gateway): `192.168.1.1`
     - DNS: `8.8.8.8`

5. **Particions del disc**:
   - Escull "Use an entire disk" o "Manual" si vols personalitzar.
   - Assegura't que el disc seleccionat és l'adequat. Si només hi ha un, confirma i continua.

6. **Selecció de paquets i serveis**:
   - **Configuració del servidor SSH**:
     - Activa l'opció "Instal·lar SSH server" (és important per aquesta pràctica).
   - Deixa la resta de paquets desmarcats per mantenir el sistema lleuger. Instal·larem el que calgui més tard.

7. **Finalització de la instal·lació**:
   - Quan acabi, el sistema reiniciarà. Retira el mitjà d'instal·lació (USB o ISO).
   - Un cop arranqui, introdueix el nom d'usuari i contrasenya creats durant la instal·lació.

---


#### **1. Configuració del servidor de còpies de seguretat (Ubuntu Server)**

1. **Crear una màquina virtual (VM) Ubuntu Server**:
   - **Nom del host:** 
     ```bash
     sudo hostnamectl set-hostname sbak-nom-cognom
     ```
   - **Reinicia la màquina virtual per aplicar el canvi**:
     ```bash
     sudo reboot
     ```

2. **Configurar la xarxa interna**:
   - A VirtualBox:
     - Assigna la xarxa interna amb el nom `LAN-nom-cognom`.
   - Configura una IP estàtica amb Netplan:
     - Edició del fitxer de configuració de la xarxa:
       ```bash
       sudo nano /etc/netplan/01-netcfg.yaml
       ```
       Exemple de configuració:
       ```yaml
       network:
         version: 2
         ethernets:
           enp0s3:
             addresses:
               - 192.168.1.10/24
             gateway4: 192.168.1.1
             nameservers:
               addresses:
                 - 8.8.8.8
       ```
     - Aplica els canvis:
       ```bash
       sudo netplan apply
       ```

3. **Configurar SSH**:
   - Instal·lar el servei:
     ```bash
     sudo apt update && sudo apt install -y openssh-server
     ```
   - Verificar que està en funcionament:
     ```bash
     sudo systemctl status ssh
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

