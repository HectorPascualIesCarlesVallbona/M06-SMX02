# INSTAL.LAR GUEST ADDITIONS

### **1. Instal·la les Guest Additions**
Les **Guest Additions** són un conjunt d'eines que permeten funcions avançades, com ara el copiar i enganxar, la integració de carpetes compartides i el redimensionament automàtic de la pantalla.

#### **Com instal·lar les Guest Additions:**
1. Obre VirtualBox.
2. Selecciona la màquina virtual on vols activar el copiar i enganxar.
3. Accedeix al menú de la màquina virtual amb aquesta encesa:
   - **Dispositius** → **Inserir imatge de CD de les Guest Additions**.
4. Dins de la màquina virtual, obre un terminal i executa:
   ```bash
   sudo apt update
   sudo apt install build-essential dkms linux-headers-$(uname -r)
   ```
5. Munta el CD de les Guest Additions:
   ```bash
   sudo mount /dev/cdrom /media/cdrom
   ```
6. Instal·la les Guest Additions:
   ```bash
   sudo /media/cdrom/VBoxLinuxAdditions.run
   ```
7. Reinicia la màquina virtual:
   ```bash
   sudo reboot
   ```

---

### **2. Habilita el copiar i enganxar bidireccional**
1. A VirtualBox, tanca la màquina virtual.
2. Selecciona la màquina virtual i accedeix a les **Configuracions**.
3. Ves a **General** → **Avançat**.
4. A la secció **Compartició de porta-retalls**, selecciona **Bidireccional**.
5. Fes clic a **D'acord** per guardar els canvis.
6. Torna a iniciar la màquina virtual.

---

### **3. Comprova la funcionalitat**
1. Obre un fitxer de text o un terminal a la màquina virtual.
2. Intenta copiar text des de l'amfitrió (host) i enganxar-lo a la màquina virtual.
3. També prova de copiar text des de la màquina virtual i enganxar-lo a l'amfitrió.

---

### **Si encara no funciona:**
- **Actualitza VirtualBox:** Assegura't que tens l'última versió de VirtualBox instal·lada.
- **Comprova els permisos:** Assegura't que el teu usuari a la màquina virtual té permisos suficients per executar les Guest Additions.
- **Desactiva i torna a activar la funció de copiar i enganxar:** Revisa les opcions a VirtualBox i reinicia tant l'amfitrió com la màquina virtual.
