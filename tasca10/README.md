# T10: Servidor d'Impressió Linux amb CUPS

## 📋 Descripció de la Tasca
Aquesta tasca té com a objectiu configurar un **servidor d'impressió centralitzat** utilitzant **CUPS (Common UNIX Printing System)** en un entorn Ubuntu Server, i compartir una impressora virtual amb un client Zorin OS. És una **Prova de Concepte (PoC)** que demostra com es pot optimitzar i simplificar la gestió d'impressores en una xarxa empresarial.

## 🎯 Context
A la consultora **EverPia**, ens han demanat una solució per centralitzar la impressió als departaments de **DevOptimize Solutions**, que utilitzen una barreja de clients Linux (Zorin OS) i servidors Ubuntu. Actualment, tenen problemes amb:
- Drivers incompatibles
- Costos de tòner descontrolats
- Confusió en l'enviament de treballs d'impressió

La solució proposta és un **servidor d'impressió centralitzat** que gestioni totes les impressores des d'un únic punt.

## 🖨️ Escenari de Treball
- **Màquina 1 (Servidor):** Ubuntu Server
  - Interfície NAT (per accés a Internet)
  - Interfície Host-Only (xarxa interna amb el client)
- **Màquina 2 (Client):** Zorin OS Desktop
  - Mateixa configuració de xarxa que el servidor

## 📝 Objectius de la PoC
1. **Instal·lar CUPS** al servidor Ubuntu
2. **Configurar una impressora virtual** cups-pdf (simula una impressora de xarxa)
3. **Configurar CUPS** per escoltar a totes les interfícies i permetre administració remota
4. **Compartir la impressora** via el frontal web de CUPS
5. **Configurar el client Zorin** per afegir la impressora compartida
6. **Realitzar proves d'impressió** des del client
7. **Verificar** que els PDFs es generen correctament al servidor

## 🛠️ Competències Treballades
- **Instal·lar, configurar i mantenir serveis multiusuari** en xarxa local
- **Realitzar proves funcionals** i diagnosticar disfuncions
- **Utilitzar mitjans de consulta** per resoldre problemes nous

## 📈 Resultats d'Aprenentatge (RA)
- **RA4:** Gestiona els recursos compartits del sistema, interpretant especificacions i determinant nivells de seguretat
- **CA4.4:** Comparteix impressores en xarxa
- **C4.4:** Configuració d'impressores compartides en xarxa

## 📁 Estructura del Repositori
```
tasca10/
│
├── img_T10/
│   ├── captura1.png
│   ├── captura2.png
│   ├── ...
│   └── captura29.png
│
├── Guia.md          # Documentació detallada pas a pas
└── README.md        # Aquest fitxer
```

## 🚀 Procediment Resumit
### Al Servidor:
1. Configuració de xarxa (NAT + Host-Only)
2. Actualització del sistema
3. Instal·lació de CUPS i cups-pdf
4. Configuració de `cupsd.conf`
5. Afegir usuari al grup `lpadmin`
6. Configuració via interfície web

### Al Client:
1. Afegir impressora compartida des de Settings
2. Prova d'impressió des d'aplicacions
3. Verificació del funcionament

## 📊 Materials de Suport
- **UD5. AA1. CUPS** (Material propi al Moodle)
- **Vídeo:** [Instalación de servidor de impresión en cups para linux](https://www.youtube.com/watch?v=FNwSTrOSgZQ)
- **Documentació Ubuntu:** [Network File System (NFS)](https://documentation.ubuntu.com/server/how-to/networking/install-nfs/)
- **Tutorial:** [How To Install CUPS Print Server on Ubuntu 24.04 LTS](https://idroot.us/install-cups-print-server-ubuntu-24-04/)

## ✅ Resultat Esperat
Un servidor CUPS funcionant que:
- Comparteix una impressora virtual PDF a la xarxa
- Accepta treballs d'impressió des de clients Linux
- Genera PDFs al servidor com a prova d'impressió
- Redueix la complexitat de gestió d'impressores

## 👥 Autoria
**Tasca individual** realitzada com a part del mòdul de Sistemes Operatius en Xarxa.

## 📅 Data de Lliurament
- **Producte final:** Carpeta al repositori amb README.md i Guia.md
- **Format d'entrega:** Enllaç a la tasca del Moodle

---

> **Nota:** Aquesta PoC demostra que és possible implementar un servidor d'impressió centralitzat amb programari lliure, reduint costos i millorant l'eficiència en entorns empresarials mixtos Linux.
