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

### **2. Instal·lació i configuració d'Ubuntu Desktop**

#### **2.1. Instal·lació d'Ubuntu Desktop**

##### **2.1.1. Preparació**
- **Descarrega la imatge ISO d'Ubuntu Desktop**:
  - Disponible a [https://ubuntu.com/download](https://ubuntu.com/download).
- **Crea una màquina virtual a VirtualBox**:
  - **Nom:** `client-nom-cognom`.
  - **Configuració de maquinari**:
    - **RAM:** Almenys 2 GB.
    - **Disc dur virtual:** Almenys 20 GB.
    - **Adaptador de xarxa:** Configura'l com a "Xarxa interna" amb el nom `LAN-nom-cognom`.
  - Munta la imatge ISO d'Ubuntu Desktop com a dispositiu òptic.

##### **2.1.2. Instal·lació del sistema operatiu**
1. **Arrenca la màquina virtual amb la ISO muntada**.
2. Selecciona l'idioma de la instal·lació (Català si està disponible).
3. Tria l'opció "Instal·la Ubuntu".
4. **Configuració durant la instal·lació**:
   - **Disposició del teclat:** Escull "Espanyol" o el que correspongui al teu teclat.
   - **Connexió a la xarxa:** Si no tens connexió directa, pots saltar aquest pas.
   - **Tipus d'instal·lació:**
     - Escull "Instal·lació mínima".
     - Marca l'opció "Instal·lar controladors de tercers".
   - **Particions del disc:** Escull "Esborra el disc i instal·la Ubuntu".
   - **Usuari inicial:** Introdueix un nom d'usuari i contrasenya. Configura el nom del dispositiu com `client-nom-cognom`.
5. Completa la instal·lació i reinicia la màquina.
6. Retira la imatge ISO abans de reiniciar.

---

### **2.2. Configuració de l'Ubuntu Desktop**

#### **2.2.1. Canviar el nom del host**
1. Obre un terminal.
2. Executa la següent comanda per canviar el nom del host:
   ```bash
   sudo hostnamectl set-hostname client-nom-cognom
   ```
3. Reinicia el sistema per aplicar el canvi:
   ```bash
   sudo reboot
   ```

---

#### **2.2.2. Solució a l'error "is not in the sudoers file"**

Aquest error indica que l'usuari actual `<nom_usuari>` no té permisos de **superusuari** (sudo). A continuació, es detallen els passos per solucionar-ho:

##### **1. Accedir al sistema amb privilegis**
- **Opció 1: Si tens un altre usuari amb permisos sudo**:
  - Inicia sessió amb aquest usuari utilitzant:
    ```bash
    su - usuari_amb_sudo
    ```

- **Opció 2: Si no tens cap altre usuari amb permisos sudo**:
  - Accedeix al **Recovery Mode**:
    1. Reinicia la màquina virtual.
    2. Al menú de GRUB, selecciona **Recovery Mode**.
    3. Tria l'opció **Root - Drop to root shell prompt**.

---

##### **2. Afegir l'usuari al grup sudo**
1. Si estàs utilitzant un usuari amb privilegis sudo, executa:
    ```bash
      sudo usermod -aG sudo <nom_usuari>
   ```

2. Si estàs en **Recovery Mode**, assegura't de remuntar el sistema en mode escriptura abans d'executar la comanda:

   ```bash
    mount -o remount,rw /
    usermod -aG sudo <nom_usuari>
   ```

---

##### **3. Verificar els canvis**
1. Reinicia el sistema per aplicar els canvis:
   ```bash
   reboot
   ```
2. Inicia sessió com a `<nom_usuari>` i comprova que tens permisos sudo amb:
   ```bash
    sudo whoami
   ```
   - Hauries de veure com a resposta:
     ```bash
      root
     ```

---

#### **Notes addicionals**
- Si el problema persisteix, revisa que el grup `sudo` estigui configurat correctament al fitxer `/etc/sudoers`.
- Per editar aquest fitxer de manera segura, utilitza:
  ```bash
  sudo visudo
  ```

Amb aquests passos, l'usuari `<nom_usuari>` tindrà permisos de superusuari correctament configurats.2. Reinicia la màquina virtual:
   ```bash
   sudo reboot
   ```

##### **2.2.2. Configuració de la xarxa interna**
1. **Assigna la xarxa interna**:
   - A VirtualBox, assegura't que el client utilitza l'adaptador de xarxa interna `LAN-nom-cognom`.

2. **Configura una IP estàtica**:
   - **Des de la interfície gràfica**:
     - Ves a Configuració → Xarxa → Opcions de connexió.(IPv4)
     - Configura els paràmetres:
       - **IP:** `192.168.1.20`
       - **Màscara:** `255.255.255.0`
       - **Passarel·la:** `192.168.1.1`
       - **DNS:** `8.8.8.8`
   - **Utilitzant Netplan** (opció avançada):
     1. Edita el fitxer de configuració:
        ```bash
        sudo nano /etc/netplan/01-netcfg.yaml
        ```
     2. Exemple de configuració:
        ```yaml
        network:
          version: 2
          ethernets:
            enp0s3:
              addresses:
                - 192.168.1.20/24
              routes:
                - to: default
                  via: 192.168.1.1
              nameservers:
                addresses:
                  - 8.8.8.8
        ```
     3. Aplica els canvis:
        ```bash
        sudo netplan apply
        ```

##### **2.2.3. Verificació de connectivitat**
1. **Comprova la connexió amb el servidor**:
   ```bash
   ping 192.168.1.10
   ```
2. **Comprova que pots connectar-te via SSH al servidor**:
   ```bash
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
     0 20 * * * /home/<nom_usuari>/backup-nom-cognom.sh
     ```

2. **Verificar que s’executa correctament**:
   - Comprova els logs del crontab:
     ```bash
     cat /var/log/syslog | grep CRON
     ```

---

#### **8. Opcional**

- Explora alternatives com TrueNAS o Duplicity per millorar el sistema de backup.

