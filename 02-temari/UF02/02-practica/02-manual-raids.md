# **REPTE: SISTEMES RAID EN WINDOWS**

## **0. Preparació inicial**
### **0.1 Configura la màquina virtual**
- Crea una màquina virtual amb **Windows Server** a **VirtualBox**:
  - Assigna 4 GB de RAM i 2 CPUs.
  - Afegeix al controlador SATA el [disc dur virtual de Windows Server](https://go.microsoft.com/fwlink/p/?linkid=2195172&clcid=0x409&culture=en-us&country=us).
  - Configura el controlador SATA amb almenys 5 ports disponibles.
- Afegeix 3 discos (VHD) SATA de 25 GB cadascun:
  - **Ruta:** Configuració → Emmagatzematge → Controlador SATA → Afegir disc dur → Crear nou → Creixement dinàmic (deixa la casella Pre-allocate Full Size desmarcada).
  - Assigna els noms `elteunom1`, `elteunom2` i `elteunom3`.

### **0.2 Crea una instantània inicial**
- Nom: **"Inicial"**.

---

## **1. Gestió d'emmagatzemament en Windows**
### **1.1 Inicia el sistema**
- Obre l’eina de gestió de discos:
  - **Ruta:** Inici → Herramientas Administrativas → Administración de equipos → Administración de discos.

### **1.2 Inicialitza els discos**
- Fes clic dret als discos no assignats → **Inicializar disco** → Selecciona **GPT**.
- Si ja estan inicialitzats, no apareixerà aquesta opció.

### **1.3 Verificacions**
- Comprova que els discos apareixen com a **espai no assignat** o **unallocated**.
- Verifica que els discos no són visibles al sistema (Inici → Equipo o Start → This PC).

### **1.4 Preguntes**
1. **Els discos apareixen al sistema?**
   - **Resposta:** No, perquè encara no tenen particions ni sistema de fitxers aplicat.
2. **Per què no es poden utilitzar directament?**
   - **Resposta:** Els discos necessiten ser particionats i formatats amb un sistema de fitxers per ser accessibles.

---

## **2. Disc distribuït**
### **2.1 Crear un volum distribuït**
1. Fes clic dret al `Disk1` → **Nuevo volumen distribuido** o **New spanned volume**.
2. Selecciona:
   - El primer disc complet.
   - La meitat de l’espai del segon disc.
3. Assigna:
   - **Unitat:** E:
   - **Nom:** Nom-Cognom.
   - **Sistema de fitxers:** NTFS.

### **2.2 Ampliar el volum**
1. Fes clic dret sobre el volum **E:** → **Amplia volumen** (o **Extend Volume**).
2. Selecciona:
   - La resta de l’espai del segon disc.
   - Tot el tercer disc.
3. Fes clic a **Next** → **Finish**.

### **2.3 Comprova l’espai disponible i afegeix fitxers**
- Afegeix 20-30 fitxers utilitzant **Command Prompt**:
  ```bash
  fsutil file createnew E:\fitxer1.txt 1073741824
  fsutil file createnew E:\fitxer2.txt 1073741824
  fsutil file createnew E:\fitxer3.txt 1073741824
  ```

---

## **3. RAID 0**
### **3.1 Restaurar l'estat inicial**
- Elimina els volums existents o restaura l’instantània inicial.

### **3.2 Crear un volum RAID 0 amb 2 discos**
1. Fes clic dret al `Disk1` → **Nuevo volumen seccionado** o **New Striped Volume**.
2. Selecciona **12 GB per a cada disc**.
3. Assigna:
   - **Unitat:** F:
   - **Nom:** Nom-Cognom.

### **3.3 Crear un volum RAID 0 amb 3 discos**
1. Fes clic dret al `Disk1` → **Nuevo volumen seccionado** o **New Striped Volume**.
2. Selecciona espais dels 3 discos.
3. Assigna:
   - **Unitat:** H:
   - **Nom:** Nom-Cognom.

### **3.4 Comprova i afegeix fitxers**
- Afegeix 20-30 fitxers al volum **H:** amb el mateix mètode que al volum **E:**.

### **3.5 Simula una fallada**
1. A VirtualBox:
   - Accedeix a **Configuració (Settings)** → **Emmagatzematge (Storage)**.
   - Extreu el **Disk1** seleccionant **Remove Disk from Virtual Drive**.
2. Inicia la màquina virtual i comprova que el volum **H:** no és accessible.

---

### **4. RAID 1 (Mirall)**

#### **4.1 Crear un volum mirall amb dos discos**
1. **Restaura o prepara els discos:**
   - Si tens volums creats, elimina els volums **E:** i **H:** o restaura l’instantània anomenada **"abans raid1"**.
   - Assegura't que els discos **Disk 1** i **Disk 2** tenen espai sense assignar.

2. **Crear el volum mirall:**
   - Obre **Gestió de discos**.
   - Fes clic dret sobre l’espai no assignat de **Disk 1** → selecciona **Nuevo volumen reflejado** (**New Mirrored Volume**).
   - Selecciona **Disk 1** i **Disk 2**.
   - Assigna **12 GB** de cada disc al volum.
   - Tria la lletra **E:** i posa com a nom del volum el teu **Nom-Cognom**.
   - Finalitza el procés i espera que el volum s’inicialitzi.

3. **Captura:**
   - Fes una captura de pantalla del volum creat.

---

#### **4.2 Verificar la mida del volum**
1. **Obre Inicio → Equipo (This PC):**
   - Comprova que el volum **E:** està disponible.
2. **Quina mida té el volum E?:**
   - La mida serà de **12 GB**, ja que el volum mirall reflecteix el mateix espai a tots dos discos.
3. **Captura:**
   - Fes una captura de pantalla del volum **E:** al gestor d’emmagatzematge o a **Equipo**.

---

#### **4.3 Recrear el volum amb 25 GB**
1. **Elimina el volum existent:**
   - A **Gestió de discos**, fes clic dret sobre el volum **E:** i selecciona **Eliminar volumen** (**Delete Volume**).

2. **Crea un volum mirall nou:**
   - Repeteix el procés de creació del volum, però assigna **25 GB** de cadascun dels dos discos (**Disk 1** i **Disk 2**).
   - Assigna la lletra **E:** i el nom del volum com el teu **Nom-Cognom**.

3. **Captura:**
   - Fes una captura de pantalla del volum **E:** al gestor d’emmagatzematge o a **Equipo**.

---

#### **4.4 Estructura de directoris al volum E**
1. **Crea una estructura de directoris:**
   - Al volum **E:**, crea una estructura amb 3 nivells. Exemple:
     ```
     E:\
     ├── Projectes
     │   ├── Practiques
     │   │   └── arxiu1.txt
     │   ├── Apunts
     │   │   └── arxiu2.txt
     ├── Dades
         └── arxiu3.txt
     ```
   - Afegeix arxius teus (pràctiques o apunts).

2. **Captura:**
   - Fes una captura de pantalla mostrant la jerarquia de directoris creada.

---

#### **4.5 Trencar el mirall**
1. **Trenca el volum mirall:**
   - A **Gestió de discos**, fes clic dret sobre el volum **E:** → selecciona **Break Mirrored Volume** (**Trencar volum reflectit**).
   - Ara els discos tindran còpies independents de les dades.

2. **Verificacions:**
   - Accedeix a **Inicio → Equipo** i comprova:
     - Quants volums tens disponibles.
     - Les lletres assignades als volums.
     - Si tots dos contenen les mateixes dades.

3. **Captura:**
   - Fes una captura de pantalla dels volums després de trencar el mirall.

---

#### **4.6 Refer el mirall amb un tercer disc**
1. **Reconstrueix el mirall:**
   - Fes clic dret sobre el volum **E:** → selecciona **Add Mirror** (**Afegir mirall**).
   - Tria el **Disk 3** per reconstruir el mirall.
   - El sistema començarà la sincronització automàticament.

2. **Observa la reconstrucció:**
   - Durant la sincronització, apareixerà l’estat **"Volviendo a sincronizar"**.
   - Comprova el temps que triga i observa que depèn:
     - De la mida del volum.
     - De la velocitat d’escriptura dels discos.

3. **Captura:**
   - Fes una captura de pantalla durant la reconstrucció.

---

#### **4.7 Simular una fallada**
1. **Simula la fallada d’un disc:**
   - Atura abruptament la màquina virtual:
     - Pots tancar VirtualBox directament o eliminar el disc **Disk 3** de la controladora SATA sense apagar l'equip.

2. **Inicia la màquina virtual i comprova:**
   - Accedeix al **Gestor d’emmagatzematge** i verifica l’estat del volum:
     - **Quin missatge apareix respecte el disc fallit?**
     - **Es pot accedir al volum E?:**

3. **Captures:**
   - Fes una captura de pantalla del volum **E:** i del missatge d'error del disc fallit.

---


### **5. RAID 5 (Bandes amb Paritat)**

#### **5.1 Preparació**

1. **Feu un snapshot abans de començar:**
   - Crea una instantània de l'estat actual del sistema i anomena-la **"abans raid5"**.

2. **Eliminar volums de l’exercici anterior:**
   - Obre **Gestió de discos** i elimina els volums **E:**, **H:**, o altres que hagis creat.
   - Si no hi ha espais no assignats, elimina tots els volums existents dels discos 1, 2 i 3.

3. **Afegir el Disc 3 a la màquina virtual:**
   - A **VirtualBox**, afegeix el **Disk 3** a la controladora SATA.
   - Assegura't que és el mateix disc utilitzat en exercicis anteriors.

---

#### **5.2 Crear el volum RAID 5**

1. **Crear el volum:**
   - Obre **Gestió de discos**.
   - Converteix **Disk3** en **basic disc** si no està operatiu.
   - Fes clic dret a l'espai no assignat del primer disc (**Disk 1**) → selecciona **Nuevo volumen RAID 5** (**New RAID 5 Volume**).
   - Selecciona els tres discos disponibles (Disk 1, Disk 2 i Disk 3).
   - Assigna **Z:** com a lletra de volum.
   - Posa el teu **Nom-Cognom** com a nom del volum.
   - Formata amb **NTFS**.

2. **Afegeix dades:**
   - Al volum **Z:**, crea 20 o 30 carpetes i fitxers. Pots utilitzar fitxers de pràctiques, apunts, o crear-los amb el següent comandament:
     ```bash
      fsutil file createnew Z:\fitxer1.txt 1048576
      fsutil file createnew Z:\fitxer2.txt 1048576
      fsutil file createnew Z:\fitxer3.txt 1048576
     ```

3. **Captura:**
   - Fes una captura de pantalla del volum creat i de l’estructura de carpetes.

---

#### **5.3 Comprovar mida i espai disponible**

1. **Quantes unitats hi ha?**
   - Ves a **Inicio → Equipo** (**This PC**) i comprova que només apareixen les unitats **C:** i **Z:**.

2. **Quina mida té el volum Z:?**

3. **Quant d’espai hem perdut? Per què?**

4. **Captura:**

---

#### **5.4 Simular una fallada**

1. **Apaga l’equip abruptament:**
   - Tanca VirtualBox sense apagar correctament la màquina virtual o utilitza el botó **Power Off**.

2. **Elimina el Disc 3:**
   - A **VirtualBox**, accedeix a **Configuració → Emmagatzematge → Controlador SATA**.
   - Selecciona el **Disk 3** i fes clic a **Remove Disk from Virtual Drive**.

3. **Inicia l'equip i verifica:**
   - Obre **Gestió de discos** i comprova l’estat del volum RAID 5:
     - Quin missatge apareix respecte al disc fallit?
     - El volum **Z:** és accessible?

4. **Captures:**
   - Fes una captura del gestor d’emmagatzematge i qualsevol error relacionat amb el volum **Z:**.

---

#### **5.5 Modificar dades i reconstruir el volum**

1. **Modifica dades del volum Z::**
   - Accedeix al volum **Z:** i edita alguns dels fitxers existents (pots canviar el contingut amb un editor de text).

2. **Afegir un nou disc substitut (Disc 5):**
   - A **VirtualBox**, afegeix un **Disk5** com a substitut del **Disk 3**. **NOTA**: la capacitat del **Disk5** ha de ser més gran que la resta
   - Inicia la màquina virtual.

3. **Reconstrueix el volum RAID 5:**
   - Obre **Gestió de discos**.
   - Fes clic dret sobre el volum RAID 5 en estat degradat → Selecciona **Add Disk** i tria el **Disk 5**. Primer cal eliminar qualsevol referència, **Disk 3** en el nostre cas, al disc que falta abans de poder afegir-ne un de nou.
   - Espera que el sistema reconstrueixi el volum. L'estat canviarà a **"Rebuilding"** o **"Volviendo a sincronizar"**.

4. **Verifica els fitxers modificats:**
   - Accedeix al volum **Z:** i comprova si els fitxers modificats s’han mantingut després de la reconstrucció.

5. **Captures:**
   - Fes captures de pantalla del procés de reconstrucció i del volum final.

---

#### **5.6 Simular una fallada múltiple**

1. **Elimina el Disk 1:**
   - A **VirtualBox**, accedeix a **Configuració → Emmagatzematge → Controlador SATA**.
   - Selecciona el **Disk 1** i elimina'l.

2. **Inicia l’equip i verifica:**
   - Obre **Gestió de discos** i comprova si el volum **Z:** és accessible amb dues fallades.
   - **Què passa amb les dades?**

---

#### **5.7 Conclusions**

1. **Tolerància a fallades:**
   - RAID 5 pot suportar la fallada d’un sol disc gràcies a la paritat, però perd totes les dades si dos discos fallen simultàniament.

2. **Espai perdut:**
   - L’espai equivalent a un disc es dedica a emmagatzemar informació de paritat.

3. **Temps de reconstrucció:**
   - Depèn de la mida del volum i la velocitat del sistema.

4. **RAID 5 vs altres RAID:**
   - Ofereix un bon equilibri entre redundància i espai disponible, però no és tan robust com RAID 1 ni tan ràpid com RAID 0.



