# **RAID resum**

Aquí tens l'índex clickable generat per a tot el document:

---

## **Índex**

1. [Introducció als sistemes RAID](#introducció-als-sistemes-raid)
2. [RAID 0: Velocitat, però sense seguretat](#raid-0-velocitat-però-sense-seguretat)
3. [RAID 1: Seguretat, però sense velocitat](#raid-1-seguretat-però-sense-velocitat)
4. [RAID 5: Velocitat i seguretat equilibrades](#raid-5-velocitat-i-seguretat-equilibrades)
5. [Diferències](#diferències)
6. [Càlcul del volum total per configuracions RAID](#càlcul-del-volum-total-per-configuracions-raid)
   - [RAID 0](#raid-0)
   - [RAID 3](#raid-3)
   - [RAID 5](#raid-5)
   - [RAID de Windows (Spanned Volume)](#raid-de-windows-spanned-volume)
7. [Com funciona la paritat](#com-funciona-la-paritat)
   - [Per què és útil la paritat?](#per-què-és-útil-la-paritat)
   - [Exemple senzill amb 3 discos (RAID 5)](#exemple-senzill-amb-3-discos-raid-5)
8. [Què és XOR?](#què-és-xor)
   - [Taula de veritat del XOR](#taula-de-veritat-del-xor)
   - [Exemple pas a pas](#exemple-pas-a-pas)
   - [Exemple pràctic](#exemple-pràctic)
9. [Com es recupera la informació amb XOR](#com-es-recupera-la-informació-amb-xor)
   - [Exemple pràctic amb 3 discos (RAID 5)](#exemple-pràctic-amb-3-discos-raid-5)
10. [Què és l’striping?](#què-és-lstriping)
    - [Exemple amb 2 discos (RAID 0)](#exemple-amb-2-discos-raid-0)
    - [Què és l’ample de stripe?](#què-és-lample-de-stripe)
    - [Avantatges i inconvenients del striping](#avantatges-i-inconvenients-del-striping)
    - [Aplicació pràctica](#aplicació-pràctica)

---

## **Introducció als sistemes RAID**
- RAID significa **Redundant Array of Independent Disks** (Conjunt Redundant de Discos Independents).
- S’utilitzen per millorar la velocitat, la redundància (seguretat de dades) o una combinació de totes dues.
- Tipus comuns: RAID 0, RAID 1, i RAID 5.

---

## **RAID 0: Velocitat, però sense seguretat**
- **Com funciona**:
  - Divideix les dades en trossos i les distribueix entre tots els discos (striping).
  - No hi ha còpia de seguretat: si falla un disc, es perden totes les dades.
- **Avantatge**:
  - Molt ràpid.
- **Inconvenient**:
  - No té tolerància a fallades.
- **Exemple gràfic**:
    - D1: A, C
    - D2: B, D

---

## **RAID 1: Seguretat, però sense velocitat**
- **Com funciona**:
    - Les dades es dupliquen exactament en dos discos (mirroring).
    - Si un disc falla, l’altre conserva totes les dades.
- **Avantatge**:
  - Alta seguretat de dades.
- **Inconvenient**:
  - Necessita el doble d’espai (dos discos emmagatzemen el mateix).
- **Exemple gràfic**:
  - D1: A, B, C
    - D2: A, B, C

---

## **RAID 5: Velocitat i seguretat equilibrades**
- **Com funciona**:
  - Requereix almenys 3 discos.
  - Les dades es distribueixen (striping) amb informació de paritat (P), que permet reconstruir les dades si falla un disc.
  - Si falla un disc, les dades es poden recuperar gràcies a la paritat.
- **Avantatge**:
  - Combina velocitat i seguretat.
- **Inconvenient**:
  - Pot ser lent per escriure (cal calcular la paritat).
- **Exemple gràfic**:
    - D1: A, B, P
    - D2: C, P, D
    - D3: P, E, F
  - Paritat (P) es calcula sumant (o XOR) dades dels altres discos.

---

## **Diferències**

| **RAID** | **Avantatges**          | **Inconvenients**         | **Discos mínims** |
|----------|-------------------------|---------------------------|-------------------|
| RAID 0   | Velocitat alta          | Sense seguretat           | 2                 |
| RAID 1   | Alta seguretat          | Necessita espai duplicat  | 2                 |
| RAID 5   | Equilibri velocitat i seguretat | Complexitat en escriure | 3                 |

---

## **Càlcul del volum total per configuracions RAID**

Quan els discos que formen un RAID tenen diferents capacitats d'emmagatzematge, el volum total del RAID es calcula basant-se en el disc amb **menor capacitat**. Això es deu a la necessitat d'una distribució uniforme de les dades entre els discos, especialment en configuracions que utilitzen **striping** o **paritat**.

Aquí tens la secció corregida perquè sigui compatible amb qualsevol visualitzador Markdown:

---

### **RAID 0 (Striping)**
- **Volum efectiu**: El volum total és igual al nombre de discos multiplicat per la capacitat del disc **més petit**.
- **Exemple**:
  - Discos: 1 TB, 2 TB i 3 TB.
  - Volum total = 3 × 1 TB = 3 TB.
- **Raó**: Cada bloc de dades es divideix en stripes iguals, i només es pot utilitzar la capacitat que tots els discos poden contribuir uniformement.

---

### **RAID 1 (Mirroring)**
- **Volum efectiu**: Igual a la capacitat del disc **més petit**, ja que les dades es dupliquen exactament en cada disc.
- **Exemple**:
  - Discos: 1 TB, 2 TB.
  - Volum total = 1 TB (el disc de 2 TB només utilitzarà 1 TB per igualar el disc més petit).
- **Raó**: La duplicació de dades requereix que tots els discos emmagatzemin la mateixa quantitat.

---

### **RAID 5 (Striping amb paritat)**
- **Volum efectiu**: (N - 1) × capacitat del disc més petit, on N és el nombre total de discos.
- **Exemple**:
  - Discos: 1 TB, 2 TB i 3 TB.
  - Volum total = (3 - 1) × 1 TB = 2 TB.
- **Raó**: La paritat i les dades s’han de distribuir uniformement, de manera que només es pot utilitzar la capacitat comuna entre tots els discos.

---

### **RAID de Windows (Spanned Volume)**
- **Volum efectiu**: La capacitat total és la suma de tots els discos, independentment de les mides.
- **Exemple**:
  - Discos: 1 TB, 2 TB i 3 TB.
  - Volum total = 1 + 2 + 3 = 6 TB.
- **Raó**: Aquest tipus no utilitza paritat ni striping, per la qual cosa pot aprofitar completament la capacitat de cada disc. **No té tolerància a fallades.**

---

### **Nota important**
En configuracions RAID amb discos de diferents capacitats:
1. **Optimització**: És recomanable utilitzar discos de mides iguals per maximitzar la capacitat total.
2. **Desavantatge**: En RAID 0, 1 i 5, la capacitat de discos més grans es "perd" i queda sense utilitzar.

---

## **Com funciona la paritat?**

La **paritat** és un mecanisme utilitzat en els sistemes RAID (com RAID 5) per garantir que les dades es puguin recuperar si falla un dels discos. Es basa en un càlcul matemàtic senzill que compara les dades emmagatzemades en els discos per crear un "bloc de paritat". Aquest bloc permet reconstruir les dades perdudes en cas d'error.


1. **Creació del bloc de paritat**:
   - Es calcula a partir dels blocs de dades dels altres discos.
   - S'utilitza una operació XOR (o exclusiu), que té aquestes propietats:
     - 1 ⊕ 1 = 0
     - 0 ⊕ 0 = 0
     - 1 ⊕ 0 = 1
     - 0 ⊕ 1 = 1
   - **Exemple**:
     - Bloc 1: `1010` (disc 1)
     - Bloc 2: `1100` (disc 2)
     - Bloc de paritat: `0110` (resultat de `1010 XOR 1100`).

---

2. **Recuperació de dades perdudes**:
   - Si falla un disc, les dades es poden reconstruir combinant els blocs de dades restants i el bloc de paritat.
   - Exemple:
     - Si falla el disc 1:
       - XOR del Bloc 2 (1100) i el Bloc de paritat (0110) dóna 1010, que són les dades perdudes.

---

### **Per què és útil la paritat?**
- Permet emmagatzemar dades redundants sense duplicar-les completament (com en RAID 1).
- Només ocupa l’espai d’un disc per emmagatzemar la paritat, independentment del nombre de discos (per exemple, en un RAID 5 amb 3 discos, 1 es dedica a la paritat).

---

### **Exemple senzill amb 3 discos (RAID 5):**
- **Discos amb dades**:
  - Disc 1: A (1010)
  - Disc 2: B (1100)
  - Disc 3: P (0110) → Bloc de paritat calculat de A XOR B.
- **Si falla el Disc 1**:
  - XOR de B (1100) i P (0110) = A (1010), recuperant les dades perdudes.

Per explicar l'operació **XOR** de manera senzilla, pots pensar-hi com una comparació que diu: "Si els valors són diferents, el resultat és 1; si són iguals, el resultat és 0."

---

## **Què és XOR?**
- XOR significa "eXclusive OR" (O exclusiu).
- Treballa comparant bits (els 0s i 1s) un per un.

---

### **Taula de veritat del XOR**
| **Bit A** | **Bit B** | **Resultat XOR** |
|-----------|-----------|------------------|
| 0         | 0         | 0                |
| 0         | 1         | 1                |
| 1         | 0         | 1                |
| 1         | 1         | 0                |

**Regla fàcil**: Si són **diferents**, el resultat és **1**. Si són **iguals**, el resultat és **0**.

---

### **Exemple pas a pas**
Imagina que tens dos nombres binaris:

- Nombre A: **1010**
- Nombre B: **1100**

Ara fem el XOR bit a bit (compara cada columna):

1. **Primera columna**:  
   - A: **1**, B: **1** → Iguals → XOR = **0**.

2. **Segona columna**:  
   - A: **0**, B: **1** → Diferents → XOR = **1**.

3. **Tercera columna**:  
   - A: **1**, B: **0** → Diferents → XOR = **1**.

4. **Quarta columna**:  
   - A: **0**, B: **0** → Iguals → XOR = **0**.

**Resultat final del XOR**: **0110**

---

### **Exemple pràctic**
Pots utilitzar una metàfora visual amb llums:
- A i B són dos interruptors (ON = 1, OFF = 0).
- XOR diu: "La llum només s’encén (1) si els interruptors estan en posicions diferents."

Recuperar informació amb un XOR en un sistema com RAID 5 és possible perquè el XOR permet "recalcular" el valor perdut utilitzant els valors restants i el bloc de paritat.

---

## **Com es recupera la informació amb XOR?**
1. Quan un disc falla, el sistema encara té:
   - Els blocs de dades dels altres discos.
   - El bloc de **paritat** calculat amb XOR.

2. Com que el XOR té una propietat reversible, pots recuperar el valor del bloc perdut simplement comparant els blocs restants i la paritat.

---

### **Exemple pràctic amb 3 discos (RAID 5):**

#### **Dades originals:**
- **Disc 1:** Bloc A (1010)
- **Disc 2:** Bloc B (1100)
- **Disc 3:** Paritat P (calculada com A XOR B = 0110)

---

#### **Passa una fallada en el Disc 1**:
Ara falta el Bloc A (1010). Però tenim:
- Bloc B (1100).
- Paritat P (0110).

---

#### **Càlcul per recuperar el Bloc A**:
Utilitzem la fórmula del XOR per recuperar el bloc perdut:
```
A = P ⊕ B
```

1. **Paritat P**: `0110`  
2. **Bloc B**: `1100`  

**Pas a pas del XOR:**
- Primera columna: 0 ⊕ 1 = 1  
- Segona columna: 1 ⊕ 1 = 0  
- Tercera columna: 1 ⊕ 0 = 1  
- Quarta columna: 0 ⊕ 0 = 0  

**Resultat recuperat**: `1010`

---

### **Per què funciona això?**
A causa de la propietat reversible del XOR:
- Si tens qualsevol dos valors (A i P, o B i P), pots obtenir el tercer (B o A) perquè el XOR elimina el valor conegut i deixa el que falta.

---

Els **stripes** en RAID són els trossos de dades en què es divideix la informació per distribuir-la entre diversos discos d’un conjunt RAID. Aquest concepte és clau en configuracions com **RAID 0** i **RAID 5**, que utilitzen la tècnica de **striping**.

---

## **Què és l’striping?**
- **Striping** és la tècnica de dividir les dades en blocs (stripes) i emmagatzemar cada bloc en un disc diferent.
- L’objectiu és augmentar la velocitat d’escriptura i lectura, ja que els discos treballen en paral·lel.

---

### **Exemple amb 2 discos (RAID 0):**
Imagina que tens un fitxer dividit en 4 parts (A, B, C, D). Aquestes parts es distribueixen en **stripes** entre dos discos:

```
Disk 1: [ A ] [ C ]
Disk 2: [ B ] [ D ]
```

Quan necessites accedir al fitxer complet:
- Els discos 1 i 2 treballen junts per llegir simultàniament **A i B** o **C i D**.  
- Això fa que el rendiment sigui molt més ràpid comparat amb un únic disc.

---

### **Què és l’ample de stripe?**
L’ample de stripe és la mida de cada tros o bloc de dades que es distribueix a cada disc.  
- **Exemple:** Si l’ample de stripe és 64 KB, cada disc contindrà blocs de 64 KB de dades.

---

### **Avantatges i inconvenients del striping:**
| **Avantatges**                | **Inconvenients**              |
|--------------------------------|--------------------------------|
| Millora la velocitat           | Sense redundància (en RAID 0) |
| Permet utilitzar diversos discos junts | Falla d’un disc = dades perdudes |

---

### **Aplicació pràctica:**
El striping s’utilitza especialment en entorns on la velocitat és crítica, com ara edició de vídeo o bases de dades amb alta demanda d’I/O. En RAID 5, es combina amb paritat per afegir tolerància a fallades.

Les sigles **XOR** signifiquen **eXclusive OR**, que en català seria **O exclusiu**. Es tracta d’una operació lògica en què el resultat és **1** (vertader) només si un dels dos operands és **diferent** de l’altre. Si ambdós són iguals (0 i 0, o 1 i 1), el resultat és **0** (fals).

---

## **XOR**:
- **OR** (o lògic): Una operació que dona resultat **1** si almenys un dels dos operands és **1**.
- **eXclusive** (exclusiu): Es refereix al fet que el resultat només és **1** si exactament un dels operands és **1**, excloent el cas en què tots dos són **1**.

---

### **Comparativa XOR vs OR**
| **Operació** | **Operand A** | **Operand B** | **Resultat** |
|--------------|---------------|---------------|--------------|
| OR           | 0             | 0             | 0            |
|              | 0             | 1             | 1            |
|              | 1             | 0             | 1            |
|              | 1             | 1             | 1            |
| XOR          | 0             | 0             | 0            |
|              | 0             | 1             | 1            |
|              | 1             | 0             | 1            |
|              | 1             | 1             | 0            |

**Nota**: XOR és especialment útil en informàtica perquè té propietats que permeten la recuperació i verificació de dades, com passa en sistemes RAID i algorismes de xifratge.




