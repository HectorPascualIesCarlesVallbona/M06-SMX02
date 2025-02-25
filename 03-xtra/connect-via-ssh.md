# CONNECT FROM HOST TO VM
Això ens serveix per a poder fer operacions de copiar i pegar text executant de manera remota la MV a la nostra màquina (host)

1. **Comprova la IP de la MV**  
   A la màquina virtual, executa:
   ```bash
        ip a
   ```
   Busca la línia que mostra `inet X.X.X.X` associada a la interfície de xarxa activa (per exemple, `enp0s3`). Aquesta serà la IP de la MV.

2. **Activa el servidor SSH a la MV** (si no ho has fet ja)  
   A la MV, instal·la i activa SSH:
   ```bash
        sudo apt update
        sudo apt install openssh-server -y
        sudo systemctl enable --now ssh
   ```

3. **Comprova que el servei SSH està actiu**  
   A la MV, executa:
   ```bash
        sudo systemctl status ssh
   ```
   Si apareix com `active (running)`, vol dir que està funcionant correctament.

4. **Configura VirtualBox perquè la connexió funcioni**
   - Si la xarxa de la MV és **"NAT"**, hauràs de configurar un **port forwarding** a VirtualBox per poder accedir des de l'host. Ho pots fer des de la configuració de VB o directament per la terminal:
        - Apaga la MV.
        - Des de VB: 
            - A VirtualBox, selecciona la màquina virtual.
            - Ves a **Configuració > Xarxa > Avançat > Regles de redirecció de ports**.
            - Afegeix una nova regla:
            - **Nom**: SSH
            - **Protocol**: TCP
            - **Port host**: 2222 (pots posar-ne un altre si vols)
            - **Adreça host**: deixa-ho buit
            - **Port convidat**: 22
            - **Adreça convidat**: deixa-ho buit
        - Des de terminal del host:
            `sudo VBoxManage modifyvm "nom_de_la_teva_mv" --natpf1 "guestssh,tcp,,2222,,22"`


5. **Accedeix des de la teva màquina real (host)**
   - Obre terminal del host i executa:
     ```bash
        ssh -p 2222 usuari@127.0.0.1
     ```

6. **Si la connexió funciona**, veuràs que accedeixes a la MV i podràs gestionar-la des del teu sistema real sense necessitat d'obrir la consola de VirtualBox.
