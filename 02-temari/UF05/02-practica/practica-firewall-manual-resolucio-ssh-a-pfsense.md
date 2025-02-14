# **Connexió SSH a pfSense (192.168.1.1)**
Si vols connectar-te via **SSH** al pfSense des del client Ubuntu, has de comprovar aquests punts:

#### **1. Habilita SSH a pfSense**
Per defecte, pfSense no té el servei SSH activat. Per activar-lo:
1. Entra a la interfície web de pfSense (`https://192.168.1.1`).
2. Ves a **System → Advanced → Secure Shell**.
3. **Marca l'opció**: `Enable Secure Shell (sshd)`.
4. Guarda els canvis i aplica'ls.

#### **2. Comprova la regla de firewall**
Per defecte, el firewall pot estar bloquejant connexions SSH. Per permetre SSH:
1. Ves a **Firewall → Rules → LAN**.
2. Afegeix una nova regla (`Add`):
   - **Protocol**: TCP
   - **Port Destí**: 22 (SSH)
   - **Acció**: Pass
   - **Source**: `any` (o una IP específica si vols restringir accés)
3. Guarda i aplica els canvis.

#### **3. Torna a provar la connexió SSH**
Des de l'Ubuntu client:
```bash
ssh admin@192.168.1.1
```
O si tens un usuari específic:
```bash
ssh hector@192.168.1.1
```
Si és la primera connexió, et demanarà acceptar la clau del servidor (`yes`).

#### **4. Si encara falla**
- **Verifica que el servei està actiu** a pfSense:
  ```bash
  service sshd status
  ```
- **Comprova que no està bloquejat pel firewall** (`Firewall → Logs`).
- **Fes un test de connexió**:
  ```bash
  telnet 192.168.1.1 22
  ```
  Si no respon, encara hi ha un bloqueig en el firewall.
