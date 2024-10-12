# Seguretat Passiva (Equips) - Seguretat física

## Índex

- [Seguretat Passiva (Equips) - Seguretat física](#seguretat-passiva-equips---seguretat-física)
  - [Índex](#índex)
  - [1. Emplaçament de les Instal·lacions](#1-emplaçament-de-les-installacions)
  - [2. Aïllament](#2-aïllament)
  - [3. Condicions Ambientals](#3-condicions-ambientals)
  - [4. Subministrament Elèctric i Comunicacions](#4-subministrament-elèctric-i-comunicacions)
  - [5. CPD Redundant](#5-cpd-redundant)
  - [6. Mesures de Prevenció d'Incendis](#6-mesures-de-prevenció-dincendis)
  - [7. Mesures de Seguretat Física](#7-mesures-de-seguretat-física)
  - [8. Estàndard per a Centres de Dades](#8-estàndard-per-a-centres-de-dades)
  - [Referències](#referències)

---

## 1. Emplaçament de les Instal·lacions

- **CPD**: El Centre de Processament de Dades (CPD), també conegut com a Data Center o sala freda, és l'espai on es centralitzen els servidors d'una organització.
- **Baixa Visibilitat**: Situar el CPD en llocs amb poca visibilitat per evitar cridar l'atenció. Cal evitar cartells o logos que indiquin la presència d'un CPD.
- **Ubicació i Accés**: Idealment a la primera planta d'un edifici per evitar riscos d'inundació (si fos al soterrani), d'atacs (planta baixa) o de foc que pugi (plantes superiors). Accés molt controlat, amb passadissos amples per facilitar el transport de grans ordinadors.
- **Factors Externs**: Proximitat als serveis d'emergència com la policia, bombers i serveis sanitaris. També és recomanable situar el CPD en zones amb baixa taxa de criminalitat i allunyat d'activitats industrials perilloses.
- **Sala**: Disposició de fals terra i sostre per facilitar la distribució de cables i la ventilació. Ha de tenir alçada suficient per apilar equips en racks.
- **Desastres Naturals**: Ubicar el CPD en llocs amb baixos riscos d'inundació, terratrèmols, tornados o despreniments.

## 2. Aïllament

- **Temperatura**: Els servidors generen molta calor, de manera que cal mantenir una ventilació constant i un control estricte de la temperatura.
- **Humitat**: Cal regular la humitat per evitar corrosió (humitat alta) o electricitat estàtica (humitat baixa). Es poden usar deshumidificadors quan sigui necessari.
- **Interferències Electromagnètiques**: Evitar ubicar el CPD a prop d'equips que generin interferències com generadors, fluorescents o maquinària industrial.
- **Soroll**: Els ventiladors dels servidors poden generar molt soroll, de manera que és necessari un aïllament acústic adequat per protegir els treballadors que es troben prop del CPD.

## 3. Condicions Ambientals

- **Climatització**: Els CPD solen tenir sistemes de climatització redundants per mantenir la temperatura al voltant dels 22 graus amb un 50% d'humitat. El sistema filtra l'aire en circuit tancat per evitar pols i contaminants.
- **Passadissos Calents-Freds**: Els servidors es col·loquen en blocs per formar passadissos amb els ventiladors orientats cap a la mateixa direcció, el que facilita l'extracció de l'aire calent.
- **Altres Sistemes de Refrigeració**: Alguns centres utilitzen sistemes de refrigeració per immersió, submergint els servidors en líquid refrigerant ignífug, o bé refrigeració directa a la CPU amb líquids.

## 4. Subministrament Elèctric i Comunicacions

- **Redundància**: Comptar amb dues empreses proveïdores tant d’electricitat com de telecomunicacions (usant tecnologies diferents com fibra i ADSL per diversificar).
- **Electricitat**: Els circuits que alimenten els equips del CPD han d'estar separats de la resta de circuits de l'empresa per evitar interrupcions.
- **SAI (Sistema d'Alimentació Ininterrompuda)**: Dispositius amb bateries que asseguren el subministrament d’energia en cas de fallades elèctriques.
- **Generadors**: En sistemes crítics (com els d'hospitals) es recomana l'ús de generadors elèctrics que permetin seguir funcionant durant un tall de llarga durada.

## 5. CPD Redundant

- **Centre de Respaldo (CR)**: Grans empreses poden comptar amb un altre CPD físicament allunyat per evitar que una mateixa contingència afecti els dos centres.
- **Funció Stand-by**: El CR només entra en servei si el CPD principal falla.
- **Línies de Comunicació Ampla**: Per mantenir la sincronització entre els dos centres de dades.
- **Commutació**: Procediments documentats per poder fer el canvi de servei entre CPDs de manera ràpida.

## 6. Mesures de Prevenció d'Incendis

- **Sistemes d'Extinció**:
  - Manuals: Inclouen extintors i mànegues d’aigua.
  - Automàtics: Dispersors d’aigua o gasos com CO₂ (treu oxigen) o haló (interfereix en la combustió).
- **Sistemes Detectors**:
  - Sensors de fum: Sistemes òptics que detecten variacions de llum degut a fum.
  - Sensors de temperatura: Detecten augments sobtats de calor.
- **Sistemes Manuals**: Alarmes d'incendi que es poden activar manualment i dispersors de gasos o aigua.

## 7. Mesures de Seguretat Física

- **Valoració de Riscos**: S'ha d'avaluar el nivell d'amenaça per decidir quines mesures de seguretat instal·lar. Cal trobar un equilibri entre seguretat i productivitat.
- **Classificació de Mesures de Seguretat Física**:
     1. **Dissuasives**: Elements visibles de seguretat que serveixen per dissuadir atacants (alarma, vigilància, il·luminació nocturna, tanques).
     2. **Dificultar l'Accés a Personal no Autoritzat**:
        - Mètodes físics com cadenats i panys.
        - Controls d’accés biomètrics, targetes intel·ligents, tecles numèrics, tanques i mantraps (habitacions amb autenticació robusta).
     3. **Detecció d'Intrusos**:
        - Sensors interns (detecció de canvis ambientals).
        - Sensors externs (presència i moviment).
        - Han de tenir subministrament elèctric independent per assegurar funcionament en cas de tall.

## 8. Estàndard per a Centres de Dades

- **Norma TIA 942**: Especifica els requisits estàndard per construir CPDs, amb diferents nivells TIER que indiquen la resistència i fiabilitat del centre.
- **Nivells TIER**: Classifiquen els CPDs segons la seva capacitat de resistència davant fallades, on el TIER I és el nivell més bàsic i el TIER IV el més avançat.

---

## Referències

- Casos d’èxit de CPD i vídeos relacionats amb els Data Centers (Google, ABAST).
- Informació sobre els nivells TIER i l'actualització de la norma TIA 942.
