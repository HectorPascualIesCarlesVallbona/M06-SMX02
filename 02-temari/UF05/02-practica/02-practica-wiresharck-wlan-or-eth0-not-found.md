# **No apareixen interfícies de xarxa físiques (com `wlan0` o `eth0`)**, només opcions de captura remota (`Cisco remote capture`, `SSH remote capture`, etc.).
- Això pot indicar que:
  1. **Wireshark no té permisos per capturar paquets en interfícies físiques.**
  2. **El teu usuari no forma part del grup `wireshark`.**
  3. **No tens `dumpcap` configurat correctament.**
  4. **L'adaptador de xarxa no és compatible amb mode monitor.**

---

## ✅ **Solució: Activar la captura en interfícies de xarxa**
### **1️⃣ Comprovar que Wireshark pot veure les interfícies**
Executa aquest comandament per veure les interfícies disponibles:
```bash
sudo wireshark -D
```
Si `wlan0` o `eth0` no apareixen, el problema és d'accés o permisos.

---

### **2️⃣ Donar permisos a Wireshark**
Executa aquests comandaments per permetre la captura sense `sudo`:
```bash
sudo dpkg-reconfigure wireshark-common
sudo usermod -aG wireshark $USER
sudo chmod +x /usr/bin/dumpcap
sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap
```
Després **reinicia la sessió** o l'ordinador perquè els canvis tinguin efecte.

---

### **3️⃣ Comprovar si `dumpcap` pot capturar paquets**
Executa:
```bash
sudo dumpcap -D
```
Si apareixen interfícies com `wlan0` o `eth0`, significa que ara Wireshark hauria de veure-les.

---

### **4️⃣ Tornar a obrir Wireshark i provar la captura**
Obre **Wireshark** i comprova si ara pots veure `wlan0` o `eth0`. Si vols capturar trànsit Wi-Fi, assegura’t que el teu adaptador **suporta mode monitor**.

