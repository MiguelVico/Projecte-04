# T06: Accés Remot - Escriptori Remot (RDP)

## 📋 Descripció de la Tasca

**Títol:** T06 Accés Remot - Escriptori Remot (RDP)  
**Modalitat:** Tasca individual  
**Curs:** 0227 Serveis de Xarxa  

### Context
Com a consultors d'EverPia, no només gestionem servidors pel darrere (backend), sinó que també donem suport directe als usuaris finals. Quan un client ens truca i diu "No em funciona el programa X" o "M'ha sortit un error a la pantalla", no n'hi ha prou amb una terminal de comandes. Necessitem **veure el que ells veuen i prendre el control** del seu ratolí i teclat per resoldre problemes en temps real.

### La Missió
Crear la **documentació oficial** que rebran els futurs becaris per realitzar tasques de suport remot gràfic. Aquesta documentació servirà com a guia per establir connexions d'Escriptori Remot utilitzant màquines virtuals en una Prova de Concepte (PoC) interna.

### Tecnologia Principal: RDP (Remote Desktop Protocol)
- **L'estàndard de Microsoft** per administrar equips Windows
- **Ara també disponible a Linux** (Gràcies a GNOME, l'escriptori gràfic que incorpora Zorin OS)
- Ens permet connectar-nos a un equip Zorin de la mateixa manera que ho faríem a un Windows 11

## 🎯 Objectius

### Objectius Específics
- Configurar el servei RDP per permetre la connexió remota amb entorn gràfic
- Establir connexions segures entre equips Linux i Windows
- Documentar el procés de forma clara i accessible

### Competències Treballades
- **a)** Determinar la logística associada a operacions d'instal·lació, configuració i manteniment
- **f)** Instal·lar, configurar i mantenir serveis multiusuari en entorn de xarxa local

### Resultats d'Aprenentatge (RA)
- **0227.RA6:** Gestiona mètodes d'accés remot descrivint-ne les característiques i instal·lant-hi els serveis corresponents

### Criteris d'Avaluació (CA)
- **6.3** Instal·la un servei d'accés remot en mode gràfic
- **6.4** Comprova el funcionament d'ambdós mètodes
- **6.6** Realitza proves d'accés remot entre sistemes de diferent naturalesa
- **6.7** Realitza proves d'administració remota entre sistemes de diferent naturalesa

## 📁 Estructura del Repositori

```
tasca06/
│
├── README.md                    # Aquest document
├── Guia.md                      # Guia tècnica completa amb totes les passes
├── img_t06/                     # Carpeta amb totes les captures de pantalla
│   ├── captura1.png
│   ├── captura2.png
│   ├── captura3.png
│   ├── ... (fins a captura15.png)
│   └── terminal_update.png
└── ...
```

## 🛠️ Contingut de la Guia Tècnica

La [Guia.md](Guia.md) inclou:

1. **Configuració del servidor RDP a Windows 11**
   - Activació de l'escriptori remot
   - Configuració d'usuaris permesos
   - Obtenció de la informació de connexió

2. **Configuració del servidor RDP a Zorin OS (Linux)**
   - Actualització del sistema
   - Activació del servei d'escriptori remot
   - Configuració de paràmetres de connexió

3. **Connexió des de diferents clients**
   - Client Windows → Servidor Windows/Linux
   - Client Linux (Remmina) → Servidor Windows/Linux
   - Resolució de problemes de certificats

4. **Verificació del funcionament**
   - Comprovació de control remot
   - Transferència de fitxers
   - Ús de recursos compartits

5. **Resolució de problemes comuns**
   - Errors de connexió
   - Problemes de certificats
   - Rendiment de la connexió

## 🚀 Per Començar

### Requisits Previs
- Màquina virtual amb Windows 11
- Màquina virtual amb Zorin OS
- Coneixements bàsics de terminal Linux
- Accés a la configuració del sistema dels dos SO

### Temporització
- **Hores estimades:** 3 hores
- **Data de lliurament:** Consultar Moodle

## 📚 Materials de Suport

- [Moodle 0227 Serveis de Xarxa - UD4.AA3 Escriptoris Remots](https://moodle.institutmontilivi.cat)
- Documentació oficial de Microsoft RDP
- Wiki de Remmina (client RDP per Linux)

## 👥 Competències Clau Desenvolupades

| Competència | Nivell |
|------------|---------|
| **Autonomia** | ⭐⭐⭐⭐ |
| **Innovació** | ⭐⭐⭐ |
| **Relació interpersonal** | ⭐⭐⭐⭐ |
| **Organització del treball** | ⭐⭐⭐⭐⭐ |
| **Responsabilitat** | ⭐⭐⭐⭐⭐ |
| **Resolució de problemes** | ⭐⭐⭐⭐ |

## 📞 Aplicacions Pràctiques

Aquests coneixements són essencials per:
- **Suport tècnic remot** a usuaris amb problemes gràfics
- **Administració de servidors Windows** amb interfície gràfica
- **Assistència en temps real** per errors que requereixen interacció visual
- **Formació a distància** mostrant procediments en pantalla

## 🎓 Justificació Tècnica

L'elecció del RDP com a protocol principal es justifica per:
- **Ubiquitat** en entorns Windows empresarials
- **Integració nativa** amb els sistemes Microsoft
- **Suport creixent** en ecosistemes Linux
- **Eficiència** en l'ús del ample de banda
- **Seguretat** integrada amb xifrat

## 📤 Lliurament

**Producte final a entregar:**
- Carpeta del repositori amb tot el contingut
- `README.md` amb descripció i justificació tècnica
- `Guia.md` amb la guia tècnica completa
- Totes les captures de pantalla necessàries

**Format de lliurament:**
- Enllaç a la carpeta del repositori GitHub
- Lliurar a la tasca corresponent del Moodle

---

*"La rapidesa amb què ens connectem a l'equip d'un client per resoldre una incidència definirà la nostra qualitat com a servei."*
