
# T09: Servidor de Fitxers Linux amb NFS

## 📋 Descripció del Projecte

Aquest projecte consisteix en la implementació d'un servidor de fitxers centralitzat utilitzant **NFS (Network File System)** per a l'empresa fictícia **DevOptimize Solutions**, una startup de desenvolupament de programari que treballa exclusivament amb Linux.

### 🎯 Context del Client
El client pateix problemes crítics de gestió de codi font i actius:
- Còpies locals dels fitxers en cada desenvolupador
- Errors constants de versió
- Pèrdua d'eficiència en la col·laboració
- Necessitat d'un entorn centralitzat sense autenticació complexa

## 🎯 Objectius de la Tasca

### Objectius Tècnics
- Configurar un servidor NFS (NFSv3/NFSv4) a Ubuntu Server
- Crear usuaris i grups per simular l'entorn del client
- Establir recursos compartits amb control d'accés
- Configurar un client Linux per accedir als recursos
- Implementar muntatge manual i automàtic

### Objectius Pedagògics
- **RA4**: Gestió de recursos compartits del sistema
- **CA4.2**: Identificació de recursos del sistema a compartir
- **CA4.3**: Assignació de permisos als recursos compartits
- **CA4.6**: Establiment de nivells de seguretat per controlar l'accés

## 🏗️ Arquitectura del Sistema

### Components Principals
```
┌─────────────────────────────────────────────┐
│             SERVIDOR NFS                    │
│   IP: 192.168.56.108                        │
│                                             │
│   Directoris Exportats:                     │
│   ├── /srv/nfs/dev_projects (grups: devs)  │
│   └── /srv/nfs/admin_tools (grups: admins) │
│                                             │
│   Usuaris/Grups:                            │
│   ├── Grup: devs                            │
│   ├── Grup: admins                          │
│   ├── Usuari: dev01                         │
│   └── Usuari: admin01                       │
└─────────────────┬───────────────────────────┘
                  │
                  │ NFS (Network File System)
                  │
┌─────────────────▼───────────────────────────┐
│             CLIENT LINUX                    │
│   IP: 192.168.56.109                        │
│                                             │
│   Punts de Muntatge:                        │
│   ├── /mnt/dev_projects                    │
│   └── /mnt/admin_tools                     │
└─────────────────────────────────────────────┘
```

## 📁 Estructura del Repositori

```
tasca09/
├── img_t09/
│   ├── captura1.png
│   ├── captura2.png
│   ├── ...
│   └── captura40.png
├── Guia.md          # Documentació detallada del procés
└── README.md        # Aquest fitxer
```

## 🛠️ Tecnologies Utilitzades

- **Sistema Operatiu**: Ubuntu Server 20.04/22.04
- **Protocol**: NFS (Network File System)
- **Servei**: nfs-kernel-server
- **Eines**: 
  - `showmount`: Verificació d'exportacions
  - `mount`: Muntatge de recursos
  - `exportfs`: Gestió d'exportacions
  - `systemctl`: Control del servei

## 📝 Característiques Implementades

### ✅ Completat
- [x] Instal·lació i configuració del servidor NFS
- [x] Creació de grups i usuaris
- [x] Configuració de permisos Unix (chmod/chown)
- [x] Exportació de directoris via `/etc/exports`
- [x] Restriccions d'accés per IP/xarxa
- [x] Configuració del client NFS
- [x] Muntatge manual de recursos
- [x] Configuració de muntatge automàtic (`/etc/fstab`)
- [x] Proves de connectivitat i escriptura
- [x] Verificació amb entorn gràfic

### 🔧 Permisos Configurats
```
/srv/nfs/dev_projects:
  • Propietari: root
  • Grup: devs
  • Permisos: 770 (rwxrwx---)

/srv/nfs/admin_tools:
  • Propietari: root
  • Grup: admins
  • Permisos: 770 (rwxrwx---)
```

## 📚 Competències Treballades

### Competències Professionals
- **f**: Instal·lar, configurar i mantenir serveis multiusuari
- **g**: Realitzar proves funcionals i diagnosticar disfuncions
- **o**: Utilitzar mitjans de consulta per resoldre supòsits

### Competències Clau
- **Autonomia**: Planificació i execució independent
- **Innovació**: Implementació de solucions eficients
- **Resolució de problemes**: Diagnòstic i solució d'errors
- **Organització**: Documentació estructurada del procés

## 🚀 Procediment Resumit

### Fase 1: Preparació del Servidor
1. Actualització del sistema
2. Instal·lació de `nfs-kernel-server`
3. Creació de grups (`devs`, `admins`)
4. Creació d'usuaris (`dev01`, `admin01`)
5. Creació de directoris compartits
6. Assignació de permisos

### Fase 2: Configuració NFS
1. Edició de `/etc/exports`
2. Configuració d'accés per xarxa
3. Reinici del servei NFS
4. Habilitació d'inici automàtic

### Fase 3: Configuració del Client
1. Instal·lació de `nfs-common`
2. Verificació de connectivitat
3. Creació de punts de muntatge
4. Muntatge manual de recursos
5. Configuració de `/etc/fstab`

### Fase 4: Proves i Verificació
1. Creació de fitxers de prova
2. Verificació d'accés
3. Comprovació amb `showmount`
4. Validació gràfica

## 📊 Resultats Obtinguts

### Mètriques de Connectivitat
- **Ping**: 0% pèrdua de paquets
- **Latència**: 0.460 ms (mitjana)
- **Estabilitat**: Connexió persistent

### Control d'Accés
- **Accés per xarxa**: `192.168.56.0/24`
- **Permisos Unix**: Propietari + Grup
- **Permisos NFS**: `rw,sync,no_subtree_check`

## 🔒 Consideracions de Seguretat

### Implementades
- Restricció per rang IP
- Permisos Unix granulars
- Separació de rols (devs/admins)
- Ús de `no_subtree_check` per a eficiència

### Recomanacions Futures
- Implementar NFSv4 amb Kerberos
- Configurar firewall (UFW/iptables)
- Establir quotes de disc
- Implementar monitorització

## 📖 Aprendentatges Clau

### Tècnics
- Configuració de serveis d'xarxa en Linux
- Gestió de permisos Unix i NFS
- Diagnòstic de problemes de connectivitat
- Automatització de muntatges

### Professionals
- Interpretació de requeriments del client
- Documentació de processos tècnics
- Proves de concepte amb entorns reals
- Solució de problemes en xarxes Linux

## 🤝 Contribucions

Aquesta tasca ha estat desenvolupada seguint les bones pràctiques de:
- **Documentació Ubuntu**: Configuració oficial NFS
- **SomeBooks.es**: Guies detallades d'instal·lació
- **Material del curs**: AA1. NFS de l'UD5

## 📄 Llicència

Aquest projecte és part del material acadèmic del mòdul **Sistemes Operatius en Xarxa** i s'utilitza amb finalitats educatives.

## 👥 Crèdits

**Autor**: Miquel Vico Guardiola  
**Curs**: SMX2 - Sistemes Microinformàtics i Xarxes  
**Data**: 10/12/2025  

