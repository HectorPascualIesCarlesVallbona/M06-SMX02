## RECUPERACIÓ DE DADES AMB TESTDISK

### **TESTDISK (5 punts)**

1. **Afegiu la imatge disc1.vdi a Ubuntu Desktop:**  
   - Obriu VirtualBox i afegiu el fitxer `disc1.vdi` a la configuració de la màquina virtual.

2. **Creació d'una imatge binària amb dd:**  
   - Comanda:  
     ```bash
        sudo dd if=/dev/sdb of=imatge-elmeunom.img bs=4M status=progress
     ```
     *Has de cercar on es troba l'imatge, en el meu cas a `/dev/sdb`*

   - Verificació:  
     ```bash
        ls -lh imatge-elmeunom.img
     ```

3. **Càlcul del hash per verificar la integritat:**  
   - Comanda per calcular el hash:  
     ```bash
        sudo sha256sum imatge-elmeunom.img
     ```
   - Compara amb l'original per verificar que són iguals. Detecta la ruta del disk1 en la teva MV. En el meu cas és `/dev/sdb`
      ```bash
         sudo sha256sum /dev/sdb
      ```

4. **Informació de particions amb fdisk:**  
   - Comanda:  
      ```bash
         fdisk -l imatge-elmeunom.img
      ```
   - Observa si hi ha particions llistades.
      ```bash
         sudo parted imatge-elmeunom.img print
      ```

5. **Muntatge manual de particions:**  
   - Intentar muntar:  
      ```bash
         sudo mount -o loop,ro imatge-elmeunom.img /mnt
      ```

6. **Instal·lació de TestDisk i anàlisi de la imatge:**  
   - Instal·lació:  
     ```bash
     sudo apt install testdisk
     ```
   - Execució:  
     ```bash
     sudo testdisk imatge-elmeunom.img
     ```
   - Crea un fitxer de logs quan ho sol·liciti.

7. **Selecció del tipus de particionat (GPT o MBR).**

8. **Ús de l'opció [Analyse] per trobar particions i recuperació amb [Write].**  
   - Especificar quins sistemes d'arxius es troben (NTFS, EXT4, etc.).
   - Selecciona la partició que vols recuperar (per exemple, la NTFS).
   - Prem Enter per continuar.
   - Selecciona l'opció [Write] per escriure la taula de particions i fer-les accessibles.
   - Continua per cadasquna de les particions que apareixen i fes `sudo reboot`.

9. **Verificació amb fdisk després de la recuperació.**  
   - Comprovar si apareixen les particions restaurades.
      ```bash
         sudo fdisk imatge-elmeunom.img
      ```
   - Selecciona `p` a les opcions per veure les particions

10. **Exploració de fitxers amb [Advanced] i [List].**
   - Selecciona partició i dona-li a `List`. Et mostrarà carpetes, si entres veuràs fitxers dintre
   - Fitxers esborrats -> Apareixen en vermell o prefixats amb una `D`.
   - Selecciona el fitxer esborrat -> pren `c`per copiar el fitxer
   - TestDisk et demanarà una ubicació on desar-lo (trieu un directori com /home/hector/recuperats).
   - Confirma i TestDisk copiarà el fitxer recuperat.
   - Si vols recuperar diversos fitxers a la vegada: Prem a per seleccionar tots els fitxers del directori i copiar-los.

   💡 Hi ha una partició `NO NAME` que està corrupta. No cerqueu ara res a dintre. Entreu en tots els directoris i subdirectoris restants.

11. **Informe de fitxers trobats:**

    - Ús de `tree` per visualitzar:  
      ```bash
         tree /ruta_on_s_han_guardat
      ```

12. **Avaluació del material incriminatori:**  
    - Comprovar l’existència de documents sensibles que indiquin activitat sospitosa.

---

### **PHOTOREC, FOREMOST, BINWALK (4 punts)**

1. **Intent de muntatge de la imatge:**  
   - Canviar extensió a `.iso`:  
      ```bash
         mv imatge-elmeunom.img imatge-elmeunom.iso
      ```
   - Muntatge:  
      ```bash
         sudo mount -o loop imatge-elmeunom.iso /mnt
      ```
      Si ja existeix un punt de muntatge no passa d'aquest punt

   - Amb l'explorador d'arxius mireu quins fitxers podem veure i quins no. Els que puguem veure són els que no va esborrar el propietari del disc.

   - Desmunteu la `iso`. Si tens problemes per desmuntar, força-ho manualment afegint la ruta on ho tens muntat
      
      Ruta a dispositius loop asociats
      ```bash
         losetup -a
      ```

      Seleccionem ruta d'on tenim la nostra `iso` apuntant. En el meu cas `/dev/loop16`
      ```bash
         sudo rm /dev/loop16
      ```


2. **Recuperació de dades amb Photorec:**  
   Fixeu-vos que torna a la `img`i no la `iso`
   - Execució:  
      ```bash
         sudo photorec imatge-elmeunom.img
      ```

3. **Recuperació amb Foremost:**  
   Fixeu-vos que torna a la `img`i no la `iso`
   - Execució:  
      ```bash
         foremost -i imatge-elmeunom.img -o output
      ```
   Si teniu problemes amb aquest command canvieu `output`-> `output_nou`

   - El programa ha detectat un fitxer relacionat amb un document de Microsoft Word (document.xml.rels).
   - Pases a fer després de la recuperació_

      - Revisar el contingut de la carpeta `output_nou`:

         ```bash
         ls -lh output_nou
         ```

      - Intentar obrir els fitxers per veure si són vàlids:

         ```bash
         libreoffice output_nou/document.docx
         ```

      - Si els fitxers recuperats estan corruptes, pots intentar una altra eina com:

         ```bash
         photorec imatge-hpascual.img
         ```

      - Si necessites un tipus de fitxer específic, pots executar Foremost amb filtres:

         ```bash
         foremost -t jpg,pdf,docx -i imatge-hpascual.img -o output_especific


4. **Ús de Binwalk per anàlisi de fitxers ocults:**  
   - Execució:  
     ```bash
     binwalk -e imatge-elmeunom.img
     ```

5. **Comparació entre les eines (Taula de comparació):**

6. **Nous arxius trobats i possibles proves addicionals.**  
   - Avaluació de documents que podrien ser proves addicionals.

---

### **DESTRUCCIÓ DE LES DADES AMB SEGURETAT (1 punt)**

1. **Destrucció amb "dd" o "shred":**  
   - Destrucció total del disc:  
     ```bash
     sudo dd if=/dev/zero of=/dev/<ruta_disk> bs=4M status=progress
     ```
   - Esborrat segur de fitxers individuals:  
     ```bash
     shred -u fitxer_sensible.txt
     ```

2. **Verificació de la destrucció amb TestDisk i Foremost:**  
   - Intentar recuperar amb:  
     ```bash
     sudo testdisk imatge-elmeunom.img
     foremost -i imatge-elmeunom.img -o output
     ```
   - Evidenciar que ja no es poden recuperar fitxers.
