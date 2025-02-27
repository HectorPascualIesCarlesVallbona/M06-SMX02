# **Trobar l’adreça MAC del router (gateway) a Wireshark**  

Ara mateix tens aplicat el filtre `arp`, que només mostra **trànsit ARP** (Address Resolution Protocol). Aquest protocol s'utilitza per descobrir l'adreça MAC associada a una IP dins de la mateixa xarxa.

---

## **Com fer-ho**

1. **Busca una línia on aparegui una petició ARP amb el missatge:**  
   ```
   Who has 10.0.2.1? Tell 10.0.2.15
   ```
   - **10.0.2.1** sol ser l’adreça del **router (gateway)** en una xarxa NAT de VirtualBox.
   - **10.0.2.15** és probablement la IP de la teva màquina virtual.

2. **Fixa’t en la resposta ARP corresponent**, on el router respon:
   ```
   10.0.2.1 is at XX:XX:XX:XX:XX:XX
   ```
   - L’adreça **MAC del router és la que apareix després de "is at"**.

3. També pots mirar **la columna "Destination"** en les respostes ARP:  
   - **El router hauria de tenir una MAC fixa a la teva xarxa**.

---

## **Exemple**
Si tens aquesta entrada:
```
    Who has 10.0.2.1? Tell 10.0.2.15
```
I després:
```
    10.0.2.1 is at 52:54:00:12:35:03
```
L'adreça MAC del router és `52:54:00:12:35:03`**.
