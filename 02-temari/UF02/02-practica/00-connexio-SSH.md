# CONNEXIÓ SSH

### **Per què canvien el nom d'usuari i el host?**

1. **Nom d'usuari:**
   - Quan et connectes amb **SSH**, estàs accedint com a un usuari configurat al servidor, que pot ser diferent del teu usuari local.
   - Per exemple, si al client estàs loguejat com **`hpascual_client`** però et connectes al servidor com **`client-hpascual`**, el terminal mostrarà aquest darrer nom.

2. **Nom del host (hostname):**
   - El **hostname** del terminal també canvia perquè ara reflecteix el nom de la màquina del servidor al qual t'has connectat.
   - Si el nom del servidor és **`sbak-hpascual`**, aquest serà el que apareixerà al prompt després de connectar-te.

---

### **Exemple pràctic**

1. **Abans de connectar-te:**
   Si ets al client:
   ```bash
   hpascual_client@client-hpascual:~$
   ```

2. **Després de connectar-te al servidor amb SSH:**
   Si t'has connectat amb:
   ```bash
   ssh client-hpascual@192.168.1.10
   ```
   El prompt canviarà a:
   ```bash
   client-hpascual@sbak-hpascual:~$
   ```

---

### **Com saber on ets?**

1. **Comprova l'usuari i host actual:**
   Executa:
   ```bash
   whoami
   hostname
   ```
   - Això et dirà l'usuari actiu i el hostname de la màquina on estàs treballant.

2. **Verifica la IP del sistema:**
   Per confirmar que estàs treballant al servidor:
   ```bash
   hostname -I
   ```
   Hauria de mostrar la IP del servidor (per exemple, `192.168.1.10`).

---

### **Com saber a quin sistema et connectes?**

Si vols confirmar a quin usuari o sistema et connectaràs abans d'executar `ssh`, revisa la comanda que utilitzes. Per exemple:
```bash
ssh client-hpascual@192.168.1.10
```
- **`client-hpascual`**: Usuari amb el qual et connectaràs al servidor.
- **`192.168.1.10`**: IP o hostname del servidor al qual et connectes.

