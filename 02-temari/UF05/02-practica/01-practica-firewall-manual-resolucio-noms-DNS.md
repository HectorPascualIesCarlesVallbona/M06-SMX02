# **Solució per al problema de DNS**

## **1. Comprova que pfSense té un DNS configurat**
Vés a **System → General Setup** a la interfície web de pfSense i revisa la configuració dels servidors DNS. Assegura’t que hi ha un servidor DNS configurat, per exemple:
   - **8.8.8.8** (Google)

Desmarca l'opció **DNS Server Override** si vols forçar que pfSense utilitzi els servidors que has configurat manualment.

## **2. Comprova que pfSense permet el tràfic DNS**
A **Firewall → Rules → LAN**, assegura’t que hi ha una regla que permet **tràfic DNS (port 53, TCP/UDP)** cap a pfSense o cap als servidors DNS externs.

Si no hi ha cap regla explícita, afegeix una nova:
   - **Protocol:** TCP/UDP
   - **Port de destí:** 53 (DNS)
   - **Destinació:** any (o un servidor DNS específic)

## **3. Reinicia el servei de DNS de pfSense**
A la consola de pfSense, executa:
```bash
    pfctl -d
    service unbound restart
    pfctl -e
```
Això desactiva i torna a activar el tallafocs i reinicia **Unbound**, el servei de resolució de DNS intern de pfSense.

#### **4. Comprova la resolució de noms des d’Ubuntu**
A la màquina Ubuntu, executa:
```bash
nslookup google.com 8.8.8.8
```
- Si funciona, significa que el problema era que Ubuntu estava consultant un servidor DNS incorrecte.
- Si no funciona, revisa que la màquina Ubuntu estigui agafant correctament la configuració de DNS.

Per canviar-ho manualment, pots editar el fitxer de configuració de resolució de noms:
```bash
sudo nano /etc/resolv.conf
```
I afegir:
```
nameserver 8.8.8.8
nameserver 1.1.1.1
```
Després, fes una prova amb:
```bash
nslookup google.com
```
