## L'error **"Destination Host Unreachable"**.

## ✅ **1️⃣ Comprovar si el Servidor té rutes correctes**
A la màquina **Servidor (`10.0.2.15`)**, executa:
```bash
ip route
```
📌 **Hauries de veure alguna cosa similar a això:**
```
default via 10.0.2.1 dev enp0s3
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15
```
Si la línia `default via 10.0.2.1` **no apareix**, afegeix-la manualment:
```bash
sudo ip route add default via 10.0.2.1
```
Ara torna a provar **el ping des del Servidor cap al Client**:
```bash
ping -c 4 10.0.2.20
```
Si el ping funciona, intenta fer-ho des del Client cap al Servidor.

---

## ✅ **2️⃣ Comprovar si la interfície `enp0s3` està UP**
A la màquina **Servidor**, executa:
```bash
ip a
```
📌 **Ha d'aparèixer alguna cosa així per `enp0s3`:**
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP
```
🔹 **Si no està en `UP`**, aixeca la interfície manualment:
```bash
sudo ip link set enp0s3 up
```

Ara torna a provar **el ping** des del Client:
```bash
ping -c 4 10.0.2.15
```

---

## ✅ **3️⃣ Comprovar si el Firewall del Servidor bloqueja el trànsit**
A la màquina **Servidor**, executa:
```bash
sudo ufw status
```
Si el Firewall està activat, obre el trànsit ICMP i SSH:
```bash
sudo ufw allow 22/tcp
sudo ufw allow proto icmp
sudo ufw reload
```
Ara prova de fer el **ping des del Client** de nou:
```bash
ping -c 4 10.0.2.15
```

---

## ✅ **4️⃣ Comprovar si VirtualBox està bloquejant la connexió**
Si encara no funciona:
1. **Obre VirtualBox**.
2. Ves a **Configuració → Xarxa** de la màquina **Servidor (`firewall-servidor`)**.
3. Comprova que:
   - L’**Adaptador 1** està en **NAT**.
   - L’**Adaptador 2** està en **Xarxa Interna (si n'hi ha)**.
4. Reinicia la màquina virtual.

