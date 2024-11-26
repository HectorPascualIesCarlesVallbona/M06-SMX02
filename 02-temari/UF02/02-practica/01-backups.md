Aquí tens la pràctica completada amb les imatges col·locades als punts corresponents. 

---

### REPTE: Còpia de seguretat en Windows i Linux

**_MOLT IMPORTANT:_ CAL LLEGIR AMB ATENCIÓ A CADA ENUNCIAT**

---

#### **Instruccions generals**
1. **Nom i cognoms al document**: Copieu l'enunciat i responeu a sota. Ressalteu les preguntes.
2. **Noms personalitzats**: Els noms dels equips, usuaris, carpetes, etc. han d'incloure el vostre nom i cognoms. Això ha d'aparèixer a totes les captures.
3. **Consultes i respostes**:
   - Utilitzeu els apunts del mòdul, manuals, fitxers d'ajuda ("man") i Internet com a últim recurs.
4. **Captures de pantalla**:
   - Feu servir "FlameShot" per afegir cercles o fletxes.
   - Eviteu mini-captures; cal un context clar.
5. **Instantànies en VirtualBox**:
   - Creeu una instantània abans de començar cada acció i anomeneu-la "Abans de (l'acció que aneu a fer)".
6. **Comprovacions**:
   - Verifiqueu que tots els canvis produeixin l'efecte desitjat.

---

#### **Advertències legals i d'ús**
- L'ús d'aquesta informació ha de ser **responsable i ètic**. No utilitzeu mai aquests coneixements en entorns de producció o amb finalitats il·lícites. 

---

### **Objectius**
- Aprendre a utilitzar eines Linux per realitzar i recuperar còpies de seguretat.

---

### **Referències**
- Dejà Dup (Linux): [Enllaç](https://wiki.gnome.org/Apps/DejaDup/)
- Les 15 millors aplicacions de backup per Linux: [Enllaç](https://www.neoguias.com/mejores-aplicaciones-backup-linux)

---

### **Còpia de seguretat en mode consola amb SCP**

#### **Configuració inicial**
1. **Màquina virtual Ubuntu Server**:
   - Nom de host: `sbak-nom-cognom` (`hostnamectl set-hostname nou-hostname`).
   - Xarxa interna: "LAN-nom-cognom".
   - Adreça IP privada: Configureu-la amb `netplan`.
   - Servei SSH: Habiliteu-lo.

2. **Màquina virtual Ubuntu Desktop**:
   - Nom de host: `client-nom-cognom`.
   - Xarxa interna: "LAN-nom-cognom".
   - Adreça IP privada: Configureu-la a través de la interfície gràfica.
   - Comprovacions: Verifiqueu la connexió amb `ping` i `ssh`.

> **Esquema del servidor i client de backups:**
>
> ![Esquema lògic](imgs/03.backup-23-24-e27aead7.png)

3. **Alternativa si no teniu Ubuntu Desktop instal·lat**:
   - Utilitzeu l'equip físic com a client i connecteu-lo a la xarxa "vboxnet0" (Host-Only).
   - Configureu una IP per DHCP al servidor.

> **Esquema de configuració alternativa:**
>
> ![Esquema alternativa](imgs/2425-backup-e3c04645.png)

---

#### **Configuració d'accessos sense contrasenya**
1. **Generació de claus SSH**:
   - Executeu: `ssh-keygen -t rsa`.
   - Deixeu els valors per defecte.
   - Resultat: Dos fitxers generats (`~/.ssh/id_rsa` i `~/.ssh/id_rsa.pub`).

2. **Còpia de la clau pública al servidor**:
   - Utilitzeu: `ssh-copy-id ubak-nom-cognom@sbak-nom-cognom`.
   - Verifiqueu el fitxer `~/.ssh/authorized_keys` al servidor.

---

#### **Configuració del servidor de backups**
1. Creeu un usuari de backups:
   - Nom: `ubak-nom-cognom`.
   - Directori: `/backup`.
   - Permisos: Escriure en aquest directori.

2. Comproveu que podeu accedir al servidor sense contrasenya i copiar fitxers amb `scp`.

---

#### **Creació d’un script de còpies de seguretat**
1. Creeu un script:
   - Nom del fitxer: `backup-nom-cognom.sh`.
   - Drets d'execució: 
     ```bash
     chmod u+x backup-nom-cognom.sh
     ```
   - Contingut:
     1. Comprimir amb `zip`.
     2. Xifrar amb `gpg`:
        ```bash
        gpg --batch -c --passphrase 'password' -o fitxer-xifrat.gpg fitxer.zip
        ```
     3. Copiar al servidor amb `scp`:
        ```bash
        scp fitxer-xifrat.gpg ubak-nom-cognom@sbak-nom-cognom:/backup
        ```
     4. Esborrar fitxers temporals.
     5. Afegir un missatge:
        ```bash
        echo "Backup realitzat nom-cognom $(date)"
        ```

2. Executeu l’script i comproveu el resultat.

---

#### **Restauració de còpies**
1. Copieu un fitxer del servidor al client amb `scp`.
2. Desxifreu i descomprimiu el fitxer en una carpeta temporal.

---

#### **Automatització amb crontab**
1. Programeu l’execució diària de l’script:
   - Editeu `crontab -e` i afegiu:
     ```bash
     0 20 * * * /path/to/backup-nom-cognom.sh
     ```
2. Comproveu que l'script s'executa correctament.

---

#### **Opcional (3 punts extres)**
1. Configureu un servidor de backups amb TrueNAS.  
   - [Informació de TrueNAS](https://www.truenas.com/truenas-community-edition/)

2. Utilitzeu Duplicity per fer còpies incrementals.  
   - [Duplicity en Ubuntu Server](http://somebooks.es/copias-de-seguridad-en-ubuntu-server-18-04-lts-con-duplicity-parte-i/)

