Aquí tens **exactament el mateix contingut**, sense cap modificació de text ni taules, només **posat en format Markdown**:

---

# 1. Què copiar? (Priorització)

**Quines són les dades més crítiques del servidor? Cal fer còpia dels 10 equips clients? Justifica-ho.**
Les dades més crítiques del servidor i, per tant, les que s’ haurien de prioritzar són, Bases de Dades (Comptabilitat i Clients): Crítiques i d'ús diari perquè  son las que han d’estar disponibles en menys temps (4 hores),

# 2. Periodicitat i Tipus de Còpia

**Proposa un calendari bàsic per a la setmana (Diari/Setmanal/Mensual) i quin tipus de còpia aplicaràs (Completa, Diferencial, Incremental) per a les dades crítiques.**
Per a cada dia utilitzaria la incremental perquè és la més ràpida de fer, encara que la recuperació és més lenta. Setmanalment, jo aplicaria la diferencial perquè és fària una còpia a la setmana de les dades que s’han canviat en aquell període de temps i per últim faria la còpia completa mensualment perquè és fària una còpia de totes les dades.

# 3. Mitjans i Ubicació

**Quin tipus de mitjà de còpia utilitzaries (Discs durs externs, NAS, Cloud, Cintes)? On s'hauria de guardar físicament la còpia més recent (Regla 3-2-1).**
Utilitzaria la Regle 321 amb una cinta magnètica, un SSD i un disc dur, l’arxiu original seria el SSD i les dues còpies serien el disc dur i la cinta perquè el SSD és més ràpid i permetria editar l’arxiu de forma ràpida.

---

# Fase 2: Treball per parelles

**Part en parelles Guillem i Miquel**

| Element             | Proposta de la Parella                                                                                                                                                                                                                                                                                                                                                                                                                     | Justificació                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dades Crítiques     | Les dades més crítiques del servidor que s’han de prioritzar són les que poden comprometre la informació sensible de l’empresa i dels seus clients. Destaquen especialment les Bases de Dades de comptabilitat i de clients, així com dades de projectes i informació interna. No és necessari copiar completament els 10 equips dels clients, però sí assegurar tota la informació realment important per evitar riscos en cas de pèrdua. | Hem seleccionat aquestes dades perquè la seva pèrdua podria afectar greument l’empresa i els seus clients. Les Bases de Dades i la informació interna són essencials per al funcionament del negoci. En canvi, els equips dels clients sovint contenen dades menys rellevants; per això, centrar la còpia en la informació crítica permet protegir el que és essencial i optimitzar recursos.                                                                 |
| Periodicitat (BD)   | Diàriament aplicaríem una còpia de seguretat incremental, ja que és la més ràpida i eficient per fer còpies cada dia. Setmanalment realitzaríem una còpia diferencial, que permet registrar totes les dades modificades des de l’última còpia completa. Finalment, un cop al mes faríem una còpia completa per tenir un punt de restauració total i recent de totes les dades.                                                             | La còpia incremental és la millor opció diària perquè consumeix pocs recursos i és ràpida d’executar, tot i que la recuperació posterior sigui més lenta. La còpia diferencial setmanal permet tenir un volum de dades intermig i una recuperació més senzilla en cas de problema. La còpia completa mensual és necessària per mantenir un estat global i fiable de totes les dades, i el seu impacte en recursos és assumible en fer-la només un cop al mes. |
| Tipus de Còpia (BD) | El tipus de còpia que utilitzaríem seria el següent: incremental per a les còpies diàries, diferencial per a les setmanals i completa per a la còpia mensual.                                                                                                                                                                                                                                                                              | Aquesta combinació permet aconseguir un equilibri entre velocitat, consum de recursos i seguretat de les dades. La incremental és ràpida i ideal per a l’ús diari; la diferencial facilita una recuperació més àgil en comparació amb l’incremental; i la completa assegura un punt de restauració total imprescindible per a la protecció a llarg termini de les Bases de Dades.                                                                             |
| Mitjà 1 (Local)     | Disc dur extern o NAS                                                                                                                                                                                                                                                                                                                                                                                                                      | Permet tenir la còpia més recent de manera ràpida i accessible. Ideal per restauracions immediates i per mantenir una còpia local segons la regla 3-2-1.                                                                                                                                                                                                                                                                                                      |
| Mitjà 2 (Extern)    | Còpia al núvol (Cloud)                                                                                                                                                                                                                                                                                                                                                                                                                     | Garanteix tenir una còpia fora de les instal·lacions. Protegeix davant robatoris, incendis o altres desastres locals i compleix el requisit “off-site” de la regla 3-2-1.                                                                                                                                                                                                                                                                                     |

---

# Fase 3: Treball en grup

## 1. Debat i Selecció

| Tipus de Dades                                              | Categoria  | Freqüència                                                              | Tipus de Còpia                     |
| ----------------------------------------------------------- | ---------- | ----------------------------------------------------------------------- | ---------------------------------- |
| Servidor: Bases de Dades Crítiques (comptabilitat, clients) | Crítica    | Cada 4 hores (incremental) i setmanal (diferencial), mensual (completa) | Incremental, Diferencial, Completa |
| Servidor: Configuracions Crítiques                          | Crítica    | Setmanal (diferencial), mensual (completa)                              | Diferencial, Completa              |
| Clients: Dades Crítiques de projectes i informació interna  | Crítica    | Diària (incremental), setmanal (diferencial), mensual (completa)        | Incremental, Diferencial, Completa |
| Clients: Dades no crítiques                                 | No crítica | Setmanal (incremental), mensual (completa)                              | Incremental, Completa              |

---

## 2. Disseny de la Política Final – Document Final (Fase 3)

### 1) Dades Objecte de Còpia

*(Sense modificacions, tal qual ho has escrit.)*

### 2) Cronograma Setmanal Detallat

| Dia       | Dades                                                          | Tipus de còpia | Mitjà                         |
| --------- | -------------------------------------------------------------- | -------------- | ----------------------------- |
| Dilluns   | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dimarts   | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dimecres  | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dijous    | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Divendres | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dissabte  | Còpia diferencial setmanal de BD i dades crítiques clients     | Diferencial    | NAS / Disc dur extern + Cloud |
| Diumenge  | Còpia completa mensual (si toca) de tota la informació crítica | Completa       | Cloud / Cinta LTO             |

---

### 3) Elecció de Mitjans i Ubicació (Regla 3-2-1)

| Element               | Mitjà                                                        | Ubicació / Responsable                                    | Justificació                                                                                                         |
| --------------------- | ------------------------------------------------------------ | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Mitjà 1 (Local)       | NAS intern o Disc dur extern                                 | Instal·lacions de l’empresa, gestionat pel departament IT | Permet restauracions ràpides de la còpia més recent, assegurant disponibilitat immediata.                            |
| Mitjà 2 (Extern)      | Cloud (Azure / Google Cloud) + Disc dur extern fora del lloc | Ubicació remota o proveïdor de cloud; responsable: IT     | Garanteix còpia fora de lloc, protecció davant robatoris, incendis o desastres locals. Compliment de la regla 3-2-1. |
| Ubicació Fora de Lloc | Cloud / Disc dur extern en lloc remot                        | Proveïdor cloud o emmagatzematge físic extern             | Manté la còpia off-site, assegurant redundància i seguretat extra de dades crítiques.                                |

---

### 4) Estratègia de Recuperació (RTO/RPO)

| Tipus de Dades                           | RPO (Recovery Point Objective) | RTO (Recovery Time Objective) | Estratègia                                                                                                                                          |
| ---------------------------------------- | ------------------------------ | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bases de dades crítiques i comptabilitat | 4 hores                        | 4 hores                       | Les còpies incrementals cada 4 hores garanteixen el mínim de pèrdua de dades (RPO). Restauració ràpida des de NAS o disc dur extern compleix l’RTO. |
| Configuracions i dades crítiques clients | 24 hores                       | 4 hores                       | Còpies setmanals i cloud permeten restauració completa en cas de desastre, assegurant disponibilitat segons els requisits de l’empresa.             |
| Dades no crítiques                       | 7 dies                         | 24-48 hores                   | Còpies setmanals i mensuals, prioritzant eficiència de recursos i protecció bàsica.                                                                 |

