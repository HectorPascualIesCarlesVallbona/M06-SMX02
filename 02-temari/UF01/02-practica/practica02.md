# PRACTICA 02 - KEEPASS

[link a pràctica](https://gitlab.com/pautome/m6-smxpub-2324/-/blob/main/UF1-seguretat-passiva/Reptes/02-gestor-contrasenyes/gestor-contrasenyes.md?ref_type=heads)

## procés detallat

1. **Instal·lació de KeePassX a Linux**
    - Obre el terminal a la màquina física.
    - Executa la comanda següent per instal·lar KeePassX:

    ```bash
        sudo apt-get install keepassx
    ```

    - Si et demana la contrasenya d'administrador, introdueix-la per continuar amb la instal·lació.
    - Un cop finalitzada la instal·lació, obre l’aplicació des del menú d'aplicacions o executa keepassx al terminal.

2. **Creació d'un nou fitxer de contrasenyes**
    - A l’aplicació KeePassX, ves a File -> New Database per crear un nou fitxer de contrasenyes.
    - Selecciona una ubicació per desar el fitxer (idealment a una carpeta de Dropbox, Google Drive, Mega, etc., per facilitar la sincronització entre dispositius).
    - Assigna una contrasenya mestra segura per protegir la base de dades. Aquesta contrasenya hauria de ser llarga i complexa, combinant lletres, números i símbols.
    - Desa el fitxer amb extensió ***.kdbx.***

3. **Creació de grups de contrasenyes**
    - A la barra lateral esquerra de KeePassX, fes clic dret a Root i selecciona Add Group.
    - Crea els grups següents:

    ```bash
        email
        e-learning
        programació
    ```

   - Afegeix altres grups segons les teves necessitats o categories de contrasenyes que vulguis gestionar.
  
4. **Afegeix comptes de serveis amb contrasenyes segures**
    - Per cada servei, fes clic dret al grup corresponent i selecciona Add Entry:
    - Per al compte Gmail del Vallbona, selecciona el grup email.
    - Per al compte del Moodle i Cisco Networking Academy, selecciona e-learning.
    - Per al compte GitLab, selecciona programació.
    - Completa els camps:

    ```bash
        - Nom d’entrada: Nom del servei o compte.
        - Nom d’usuari: Nom d´usuari associat.
        - URL: Adreça URL del servei, si cal.
        - Contrasenya: Fes clic al botó de generació de contrasenyes i configura-la amb els següents criteris:
            - Llargada de 15 caràcters.
            - Caràcters en minúscula, majúscula, símbols especials, guions i números.
        - Opcionalment, afegeix una data de caducitat per recordar-te de canviar-la periòdicament.
    ```

5. **Fer servir les contrasenyes**
    - Selecciona el registre desitjat i fes clic dret per obrir el menú d'opcions.
    - Fes clic a URL -> Open URL per obrir la pàgina web del servei.
    - Si no s'obre automàticament, comprova la configuració de KeePassX anant a Extras -> Ajustes i ajusta la configuració del navegador.
    - Amb l'opció Copy Username i Copy Password, copia les credencials i enganxa-les al navegador.

6. **Instal·lació a Windows**
    - Inicia la màquina virtual amb Windows.
    - Descarrega i instal·la la versió corresponent de KeePassX per a Windows.
    - Copia el fitxer ***.kdbx*** des de Linux a Windows, per exemple, a través d’una carpeta compartida de VirtualBox o accedint directament a la carpeta de Dropbox.
    - Obre el fitxer de la base de dades des de KeePassX a Windows i verifica que totes les entrades es carreguen correctament.

7. **Investigació de gestors de contrasenyes alternatius**
    - Cerca altres gestors de contrasenyes (1Password, Bitwarden, LastPass, etc.) i fes una comparació de les seves funcionalitats amb KeePass.
    - Identifica si aquests programes ofereixen característiques addicionals o avantatges que puguin ser més útils per les teves necessitats.

8. **(Opcional) Cracking del fitxer "CrackThis.kdb"**
    - Si has trobat el fitxer ***CrackThis.kdb***, intenta accedir-hi per investigar la seguretat de KeePass.
    - Assegura't de fer això amb finalitats educatives, i recorda els aspectes ètics de l'activitat.
  
9. **(Opcional) Instal·lació a dispositius mòbils**
    - Si vols sincronitzar les contrasenyes també al teu dispositiu mòbil, instal·la una aplicació compatible amb KeePass.
    - Configura la sincronització amb Dropbox o un altre servei en el núvol per accedir al fitxer .kdbx des del mòbil i utilitzar les mateixes contrasenyes a tots els dispositius.
    - Amb aquests passos, podràs cobrir tots els aspectes del repte i verificar que l’eina funciona correctament a cada entorn. Recorda anotar les teves passes i fer captures de pantalla per documentar el procés.
