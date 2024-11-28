# BACKUP EXTENSION

## **Què és el "bit de backup"?**
- El **bit de backup** és un marcador (o flag) que alguns sistemes de fitxers assignen a cada arxiu per indicar si ha estat modificat o no des de l'última còpia de seguretat.
- Quan es crea o modifica un fitxer, el bit de backup es "marca" o s'activa.
- Quan es fa una còpia de seguretat, aquest bit es pot "desmarcar" (esborrar), deixant clar que ja s’ha fet una còpia de seguretat del fitxer.


### **"Esborra bit de backup, però no el té en compte en copiar"**
1. **Esborra el bit de backup**:
   - El sistema de còpies "desmarca" aquest bit després de fer la còpia, indicant que ja s’ha fet una còpia de seguretat del fitxer.

2. **No el té en compte en copiar**:
   - Això significa que el sistema fa una còpia de seguretat de tots els fitxers, independentment de si el bit està marcat o no.
   - Es pot donar en còpies completes, on es copia tot el contingut, sense dependre del bit de backup.


### **Per què és important?**
Aquest comportament és típic de les **còpies completes**, ja que:
- No confia en el bit de backup per decidir què copiar, sinó que copia tot.
- Es garanteix que no es perdi cap fitxer, encara que el bit no estigui configurat correctament.



### **Exemple pràctic**
- Imagina que tens un sistema amb 10.000 fitxers.
  - 1. Es fa una còpia completa de tots els fitxers (còpia completa).
  - 2. El sistema esborra el bit de backup per indicar que aquests fitxers ja estan copiats.
  - 3. Tot i esborrar el bit, en la següent còpia completa, el sistema tornarà a copiar **tots els fitxers**, sense importar l'estat del bit.  


## **Què són les cintes magnètiques?**
Són dispositius d’emmagatzematge que utilitzen una cinta de material magnètic per guardar dades. Les cintes s’allotgen dins de cartutxos o cassets dissenyats per ser utilitzats amb un lector o gravador de cintes.

### **Ús principal**
Emmagatzemar grans volums de dades de forma econòmica i fiable durant llargs períodes de temps.


