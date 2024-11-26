### REPTE: Sistemes RAID en Windows

---

#### **Instruccions generals**
1. **Nom i cognoms al document**: Copieu l'enunciat i responeu a sota. Ressalteu les preguntes.
2. **Noms personalitzats**: Els noms dels equips, usuaris, carpetes, etc., han d'incloure el vostre nom i cognoms. Això ha d'aparèixer a totes les captures.
3. **Consultes i respostes**:
   - Utilitzeu els apunts del mòdul, manuals, fitxers d'ajuda ("man") i Internet com a últim recurs.
4. **Captures de pantalla**:
   - Feu servir "FlameShot" per afegir cercles o fletxes a les captures.
   - Assegureu-vos que les captures tenen context.
5. **Instantànies en VirtualBox**:
   - Creeu una instantània abans de cada acció i anomeneu-la "Abans de (l'acció que aneu a fer)".
6. **Comprovacions**:
   - Verifiqueu que tots els canvis produeixin l'efecte desitjat.

---

#### **Advertències legals i d'ús**
- **L'ús de la informació ha de ser responsable i ètic.** No utilitzeu mai aquests coneixements en entorns de producció o amb finalitats il·lícites.

---

### **Objectius**
- Comprendre les tecnologies d'emmagatzematge distribuït i redundant configurant RAID en Windows Server.

---

### **Referències**
- Informació sobre discos dinàmics:
  - [Hardzone](https://hardzone.es/reportajes/que-es/disco-dinamico-basico/)
  - [ProfesionalReview](https://www.profesionalreview.com/2020/05/23/disco-dinamico-que-es/)
- Disc dur virtual Windows Server:
  - [Enllaç de descàrrega](https://go.microsoft.com/fwlink/p/?linkid=2195172&clcid=0x409&culture=en-us&country=us)
- Vídeo explicatiu sobre RAID a Windows Server:  
  [YouTube](https://www.youtube.com/watch?v=3XO-GrcyxHg)

---

### **1. Preparació inicial**
1. Configureu una màquina virtual amb Windows Server des d'un disc virtual proporcionat pel professor.
2. Creeu 3 discos SATA de 25 GB cadascun:
   - **Pasos**:
     - Aneu a **Emmagatzematge -> Controlador SATA -> Afegir disc dur -> Crear nou**.
     - Configureu com a discs de creixement dinàmic i anomeneu-los `elteunom1`, `elteunom2`, `elteunom3`.

> **Nota**: Aneu a les propietats de la màquina virtual i augmenteu el nombre de ports SATA a un mínim de 5 per habilitar el **hot-plug**.

> **Esquema de configuració inicial**:
>
> ![Esquema inicial](imgs/raid-windows-e8ba1eef.png)

---

### **2. Configuració i gestió d'emmagatzematge a Windows**
1. Inicialitzeu els discos en format GPT.
2. Aneu a **Inici -> Herramientas Administrativas -> Administración de equipos -> Almacenamiento -> Administración de discos**.
3. Comproveu que els discos no es veuen al sistema fins que es formatin.

---

### **3. Exercici 1: Disc distribuït (2 punts)**
- **Objectiu**: Crear un disc distribuït sense tolerància a fallades.

1. Creeu un volum distribuït utilitzant el primer i el segon disc (meitat d'espai al segon disc).
2. Configureu-lo amb la lletra E: i anomeneu-lo amb el vostre nom.
3. Verifiqueu:
   - El volum utilitza tot el primer disc i meitat del segon.
   - La mida total del volum i el sistema d'arxius (captures requerides).
4. Amplieu el volum E: per incloure tot el segon disc i, posteriorment, el tercer.
5. Reduïu el volum E: i creeu un volum nou (F:) amb la resta de l'espai del segon disc.
6. Extregueu el segon disc i comproveu l'accés a les dades.

---

### **4. Exercici 2: RAID 0 (2 punts)**
- **Objectiu**: Crear un volum seccionat (bandes sense paritat).

1. Creeu un volum RAID 0 amb 2 discos.
2. Configureu un altre volum RAID 0 utilitzant els 3 discos restants.
3. Verifiqueu la mida del volum i els espais no assignats (captures requerides).
4. Simuleu una fallada extraient el primer disc i comproveu l'accés a les dades.

---

### **5. Exercici 3: RAID 1 (Mirall) (3 punts)**
- **Objectiu**: Crear un volum mirall i simular fallades.

1. Creeu un volum RAID 1 amb 2 discos.
2. Trenca el mirall i crea un de nou amb un disc diferent.
3. Simuleu una fallada eliminant un disc en calent i observeu el comportament del sistema (captures requerides).
4. Verifiqueu si es poden crear miralls amb discos de mides diferents.

---

### **6. Exercici 4: RAID 5 (3 punts)**
- **Objectiu**: Crear un volum RAID 5 amb bandes i paritat.

1. Configureu un volum RAID 5 amb 3 discos i assigna-li la lletra Z.
2. Simuleu una fallada extraient un dels discos i reconstruïu el RAID amb un nou disc.
3. Verifiqueu l'accés a les dades abans i després de la reconstrucció.

