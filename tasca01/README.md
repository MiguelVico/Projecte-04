# T01 · DRP: Còpies de Seguretat — Estudi de Cas (Treball Cooperatiu)

## 📌 Descripció General

Aquesta tasca explora i posa en pràctica els conceptes essencials de les **còpies de seguretat** mitjançant una dinàmica cooperativa 1-2-4. L’objectiu és analitzar un cas realista d’empresa, identificar-ne les necessitats i dissenyar una **política de backup efectiva**.

## 🏢 Cas Pràctic: *Muntatges i Serveis Tècnics SL*

Empresa dedicada a la instal·lació i manteniment d'equips industrials.

### 🔧 Infraestructura Tècnica

* **Servidor de fitxers (Ubuntu Server)** amb:

  * Documents de projectes (300 GB, creixement moderat)
  * Bases de dades de comptabilitat i clients (20 GB, canvi constant)
  * Carpetes personals d’usuaris (100 GB)
* **10 equips clients Windows 10/11** amb fitxers temporals importants
* **Connexió a Internet**: Fibra 600 Mbps simètrica

### 🕒 Requisits de Recuperació

* **RTO:** < 4 hores per a Comptabilitat/Clients
* **RPO:**

  * General: fins a 24 h
  * Comptabilitat/Clients: màxim 4 h
* **Retenció mínima:** 1 mes

---

## 🧩 Fase 1: Treball Individual

Cada alumne respon:

1. **Què copiar?** Dades més crítiques i si cal copiar els equips clients.
2. **Periodicitat i tipus de còpia:** calendari (diari/setmanal/mensual) i tipus (completa, diferencial, incremental).
3. **Mitjans i ubicació:** tria del suport i aplicació de la regla **3-2-1**.

---

## 👥 Fase 2: Treball per Parelles

1. **Comparació i consens** de les respostes individuals.
2. **Creació d’un esquema 3-2-1 unificat**.

| Element             | Proposta de la Parella | Justificació |
| ------------------- | ---------------------- | ------------ |
| Dades Crítiques     |                        |              |
| Periodicitat (BD)   |                        |              |
| Tipus de Còpia (BD) |                        |              |
| Mitjà 1 (Local)     |                        |              |
| Mitjà 2 (Extern)    |                        |              |

---

## 🧑‍🤝‍🧑 Fase 3: Treball en Grup

1. **Presentació i debat** de les propostes de cada parella.
2. **Disseny final de la política de còpies** per a l’empresa.

### 📄 Document Final (A Lliurar)

Ha d’incloure:

#### 1) Dades objecte de còpia

* Quines dades es guarden
* Freqüència
* Distinció entre servidors / clients i crític / no crític

#### 2) Cronograma Setmanal Detallat

| Dia      | Dades | Tipus de Còpia | Mitjà |
| -------- | ----- | -------------- | ----- |
| Dilluns  |       |                |       |
| Dimarts  |       |                |       |
| ...      |       |                |       |
| Diumenge |       |                |       |

#### 3) Mitjans i ubicació (Regla 3-2-1)

* **Mitjà local** (ex: NAS, disc USB)
* **Mitjà extern** (ex: Cloud: Azure, Google Cloud...)
* **Ubicació fora de lloc** i responsable

#### 4) Estratègia de recuperació (RTO/RPO)

Com es garanteixen els límits fixats per a Comptabilitat/Clients.

---

## 📚 Materials de Suport

* Moodle 0226 Seguretat Informàtica — RA2.AA3
* INCIBE — *Copias de seguridad: guía de aproximación*
* Xataka — *Backup 3-2-1* (YouTube)

---

## 🎯 Objectius de l’Activitat

* Comprendre i aplicar polítiques de còpia de seguretat
* Treballar mitjançant una estructura cooperativa 1-2-4

---

## 🧠 Competències Treballades

* **Logística de sistemes microinformàtics**
* **Elaboració de documentació tècnica**

## 🛠 RA / CA Associats

* **0226.RA2:** Gestió d’emmagatzematge i integritat de la informació
* **CA 2.5 - 2.7:** Estratègies, freqüència i realització de còpies
* **C 2.4:** Còpies de seguretat i imatges de suport

---

## 🧩 Capacitats Clau

* Autonomia
* Innovació
* Relació interpersonal
* Organització
* Responsabilitat
* Treball en equip
* Resolució de problemes

