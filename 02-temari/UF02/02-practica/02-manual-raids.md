## **REPTE: SISTEMES RAID EN WINDOWS**

### **0. Preparació inicial**

1. **Configura la màquina virtual:**
   - Crea una màquina virtual amb **Windows Server** a **VirtualBox**:
     - Assigna 4 GB de RAM i 2 CPUs.
     - Afegeix al controlador SATA el [disc dur virtual de Windows Server](https://go.microsoft.com/fwlink/p/?linkid=2195172&clcid=0x409&culture=en-us&country=us)
     - Configura el controlador SATA amb almenys 5 ports disponibles.
   - Afegeix 3 discos (VHD) SATA de 25 GB:
     - **Ruta:** Configuració → Emmagatzematge → Controlador SATA → Afegir disc dur → Crear nou → Creixement dinàmic (Deixa la casella Pre-allocate Full Size desmarcada) → Assigna els noms `elteunom1`, `elteunom2` i `elteunom3`.

2. **Crea una instantània inicial:**
   - Nom: **“Inicial”**.

---

## **1. Gestió d'emmagatzemament en Windows**

### **Instruccions:**
1. Inicia el sistema i obre l’eina de gestió de discos:
   - **Ruta:** Inici → Herramientas Administrativas → Administración de equipos → Administración de discos.

2. Inicialitza els discos en estil GPT:
   - Clic dret als discos no assignats → “Inicializar disco” → Selecciona GPT.

3. Comprova que els discos apareixen com a **espai no assignat** o **unallocated**.

4. Verifica que els discos no són visibles al sistema (Inici → Equipo o Start → This PC).

### **Preguntes:**
- **Els discos apareixen al sistema?**
  - **Resposta:** No, perquè encara no tenen particions ni sistema de fitxers aplicat.
- **Per què no es poden utilitzar directament?**
  - **Resposta:** Els discos necessiten ser particionats i formatats amb un sistema de fitxers per ser accessibles.

---

## **2. Disc distribuït**

### **Instruccions:**
1. Crea un volum distribuït:
   - Botó dret al primer disc → “Nuevo volumen distribuido”.
   - Selecciona el primer i el segon disc, assignant la meitat de l’espai al segon disc.
   - Assigna la unitat **E:** i el nom **Nom-Cognom**.

2. Format el volum:
   - Selecciona **NTFS** com a sistema de fitxers.

3. Amplia el volum:
   - Afegeix tot l’espai restant del segon disc i el tercer disc complet.

4. Comprova l’espai disponible i afegeix 20-30 fitxers al volum E.

### **Preguntes:**
- **Què és una unitat d’assignació o “clúster”?**
  - **Resposta:** És la unitat mínima d’emmagatzematge que el sistema de fitxers pot assignar a un fitxer. Pot tenir mides des de 512 bytes fins a diversos megabytes.
- **Quina mida total té el volum? Per què?**
  - **Resposta:** La mida total serà la suma dels espais utilitzats dels discos seleccionats.

---

## **3. RAID 0**

### **Instruccions:**
1. Elimina els volums existents o restaura l’instantània inicial.

2. Crea un volum RAID 0 amb 2 discos:
   - Botó dret al primer disc → “Nuevo volumen seccionado”.
   - Selecciona 12 GB de cada disc.
   - Assigna la unitat **F:** i el nom **Nom-Cognom**.

3. Crea un volum RAID 0 amb 3 discos:
   - Selecciona espais del primer, segon i tercer disc.
   - Assigna la unitat **H:**.

4. Extreu un disc i comprova l’accessibilitat del volum.

### **Preguntes:**
- **Es pot accedir a les dades després de perdre un disc?**
  - **Resposta:** No, RAID 0 no té tolerància a fallades, per tant, la pèrdua d’un disc implica la pèrdua de totes les dades.

---

## **4. RAID 1 (Mirall)**

### **Instruccions:**
1. Elimina els volums existents o restaura l’instantània inicial.

2. Crea un volum RAID 1:
   - Botó dret al primer disc → “Nuevo volumen reflejado”.
   - Selecciona 12 GB de dos discos.
   - Assigna la unitat **E:** i el nom **Nom-Cognom**.

3. Trenca el mirall i verifica els discos restants.

4. Simula una fallada eliminant un disc.

5. Reconstrueix el mirall amb un nou disc.

### **Preguntes:**
- **Es pot accedir a les dades després de perdre un disc?**
  - **Resposta:** Sí, RAID 1 manté una còpia exacta de les dades.
- **Quant triga la sincronització del mirall?**
  - **Resposta:** Depèn de la mida del volum i del rendiment del maquinari.

---

## **5. RAID 5 (Bandes amb paritat)**

### **Instruccions:**
1. Elimina els volums existents i afegeix els discos necessaris.

2. Crea un volum RAID 5:
   - Botó dret al primer disc → “Nuevo volumen RAID 5”.
   - Selecciona tres discos.
   - Assigna la unitat **Z:** i el nom **Nom-Cognom**.

3. Simula una fallada eliminant un disc.

4. Reconstrueix el RAID 5 amb un nou disc.

### **Preguntes:**
- **Es pot accedir a les dades després d’una fallada?**
  - **Resposta:** Sí, RAID 5 utilitza paritat per reconstruir dades amb un disc fallit.
- **Què passa amb les dades modificades durant la reconstrucció?**
  - **Resposta:** Es mantenen consistents gràcies al mecanisme de paritat.

s informació o ajustos, avisa’m!