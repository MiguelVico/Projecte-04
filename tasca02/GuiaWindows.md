# T02: Guia Tècnica - Duplicati a Windows

## 1. Introducció
En aquesta guia tècnica vegem com configurar còpies de seguretat amb **Duplicati** a Windows 11. La idea és fer còpies del perfil de l'usuari cada hora a un disc secundari i a les 18:00 a Google Drive, seguint l'esquema 3-2-1.

---

## 2. Descarrega i instal·lació de Duplicati

### 2.1 Descarrega
Primer anem a la pàgina oficial de Duplicati i descarreguem la versió per Windows.

![Descàrrega de Duplicati](/tasca02/img_windows/captura1.png)

**Anàlisi**: A la captura veiem la pàgina de descàrrega amb les opcions per Mac i Windows. Seleccionem la versió Windows 64bit.

---

### 2.2 Instal·lació
Executem el fitxer `.msi` descarregat i seguim l'assistent d'instal·lació.

![Assistent d'instal·lació](/tasca02/img_windows/captura2.png)

**Anàlisi**: L'assistent ens demana si volem executar Duplicati després de la instal·lació. Marquem l'opció i cliquem *Finish*.

---

## 3. Configuració inicial de Duplicati

### 3.1 Interfície inicial
En obrir Duplicati per primera vegada, ens trobem amb aquesta pantalla:

![Interfície inicial sense tasques](/tasca02/img_windows/captura3.png)

**Anàlisi**: La interfície mostra que no hi ha tasques programades. Veiem dues seccions principals: *Backups* (per crear còpies) i *Restores* (per restaurar).

---

### 3.2 Crear nova còpia de seguretat
Cliquem a *Add +* per començar a configurar una còpia nova.

![Crear nova còpia](/tasca02/img_windows/captura4.png)

**Anàlisi**: Ens dóna dues opcions: crear una còpia nova o importar-ne una des d'un fitxer. Seleccionem *Add a new backup*.

També podem veure la mateixa pantalla amb més detall en una altra captura:

![Nova còpia de seguretat](/tasca02/img_windows/captura5.png)

---

## 4. Configuració dels orígens de dades

### 4.1 Selecció de carpetes origen
Ara hem de triar quines carpetes volem incloure a la còpia.

![Selecció de carpetes origen](/tasca02/img_windows/captura6.png)

**Anàlisi**: Aquí podem navegar pel sistema de fitxers i seleccionar carpetes concretes. En el nostre cas, seleccionarem la carpeta de l'usuari (perfil).

---

### 4.2 Vista detallada de carpetes
En una altra captura veiem més opcions d'origen:

![Dades d'origen](/tasca02/img_windows/captura7.png)

**Anàlisi**: Es mostren les unitats i carpetes disponibles. També hi ha opcions per afegir rutes directes, aplicar filtres i excluir fitxers.

---

## 5. Configuració de la programació

### 5.1 Freqüència de les còpies
Aquí definim cada quant temps es farà la còpia.

![Programació de la còpia](/tasca02/img_windows/captura8.png)

**Anàlisi**: Configurem que es repeteixi cada 1 hora i triem tots els dies de la setmana. Això és per la còpia al disc secundari.

---

## 6. Configuració de les opcions

### 6.1 Opcions generals
Duplicati ens permet personalitzar aspectes com la mida dels volums i la retenció de còpies.

![Opcions de configuració](/tasca02/img_windows/captura9.png)

**Anàlisi**: Veiem opcions com la mida dels volums (50 MB) i la política de retenció (mantenir totes les còpies).

---

### 6.2 Criptatge
És important protegir les nostres còpies amb contrasenya.

![Configuració de criptatge](/tasca02/img_windows/captura12.png)

**Anàlisi**: Configurem el criptatge AES-256 amb una contrasenya. Ens recorden que la hem de guardar bé, perquè si la perdem perdrem accés a les còpies.

---

## 7. Configuració del destí a Google Drive

### 7.1 Connexió amb Google Drive
Per a la còpia de les 18:00 a cloud, configurem Google Drive com a destí.

![Configuració de Google Drive](/tasca02/img_windows/captura13.png)

**Anàlisi**: Aquí veiem com s'ha configurat la connexió amb Google Drive, amb l'ID d'autorització ofuscat per seguretat.

---

## 8. Execució i monitorització

### 8.1 Llista de còpies configurades
Un cop configurat, ja apareix la nostra còpia a la llista.

![Llista de còpies](/tasca02/img_windows/captura10.png)

**Anàlisi**: Veiem "backup tasca" amb el destí Local. Ens diu que s'executarà en una hora.

---

### 8.2 Execució amb èxit
Després d'executar-se, podem veure els resultats.

![Execució exitosa](/tasca02/img_windows/captura11.png)

**Anàlisi**: La còpia s'ha executat correctament (fa uns segons). Veiem les dades d'origen (1,75 GB) i destí (968,25 MB) comprimits.

---

# T02: Guia Tècnica - Duplicati a Windows (Continuació)

## 10. Configuració avançada de programació

### 10.1 Programació automàtica
Podem afinar més quan s'executen les còpies.

![Configuració avançada de programació](/tasca02/img_windows/captura15.png)

**Anàlisi**: Aquí configurem que si es passa la data programada, s'executi tan aviat com sigui possible. També veiem la propera execució programada per al 9/12/2025 a les 13:00.

---

## 11. Configuració de volums i retenció

### 11.1 Mida dels volums
Duplicati divideix les còpies en volums per facilitar-ne la gestió.

![Configuració de volums](/tasca02/img_windows/captura16.png)

**Anàlisi**: Configurem la mida dels volums i la política de retenció. En aquest cas, es mantindran totes les còpies de seguretat, de manera que no s'esborrarà res automàticament.

---

## 12. Selecció de dades origen més específica

### 12.1 Selecció detallada de carpetes
Ara veiem una altra vista per triar quines carpetes exactes volem incloure.

![Selecció detallada de dades](/tasca02/img_windows/captura14.png)

**Anàlisi**: Es mostren totes les carpetes disponibles per seleccionar. També hi ha opcions per mostrar fitxers ocults i aplicar filtres.

---

## 13. Verificació i execució de còpies

### 13.1 Verificació de còpia a Google Drive
Quan s'executa la còpia a Google Drive, podem veure el procés de verificació.

![Verificació de còpia a Google Drive](/tasca02/img_windows/captura17.png)

**Anàlisi**: Es mostra "Verifying backend data..." mentre comprova les dades a Google Drive. Veiem les dues còpies: "backup tasca" (local) i "backup 2" (Google Drive).

---

## 14. Exploració del contingut de les còpies

### 14.1 Vista del contingut de Documents
Des de la interfície de Duplicati podem veure què hi ha dins les còpies.

![Contingut de Documents](/tasca02/img_windows/captura18.png)

**Anàlisi**: Es mostra el contingut de la carpeta Documents dins de la còpia de seguretat. Això ens permet verificar quins fitxers s'han inclòs.

---

### 14.2 Selecció de fitxers per restaurar
Quan anem a restaurar, podem triar quins fitxers concretes volem recuperar.

![Selecció de fitxers per restaurar](/tasca02/img_windows/captura19.png)

**Anàlisi**: Aquí estem seleccionant fitxers concrets de la còpia per restaurar. Veiem carpetes com Documents, Downloads i fitxers com prova1.txt.

---

## 15. Restauració de dades

### 15.1 Opcions de restauració
Abans de restaurar, podem triar on volem que vagin els fitxers.

![Opcions de restauració](/tasca02/img_windows/captura20.png)

**Anàlisi**: Podem triar entre restaurar a la ubicació original o triar una nova ubicació. També podem navegar per les carpetes per triar on volem recuperar els fitxers.

---

### 15.2 Restauració completada
Un cop finalitzada la restauració, ens mostra el resultat.

![Restauració completada](/tasca02/img_windows/captura21.png)

**Anàlisi**: La restauració s'ha completat correctament. La secció de notificacions indica que no hi ha més notificacions pendents.

---

## 16. Gestió de múltiples còpies

### 16.1 Llista de totes les còpies
Ara tenim dues còpies configurades i podem veure-les totes.

![Llista completa de còpies](/tasca02/img_windows/captura23.png)

**Anàlisi**: Veiem les dues còpies:
- **backup tasca**: Local, amb 1.92 GB d'origen i 1.42 GB de destí (comprimit)
- **backup 2**: Google Drive, amb 2.68 GB d'origen

També veiem que tenim 5 versions de la còpia local i 1 versió de la còpia a Google Drive.

---

## 17. Comprovació a Google Drive

### 17.1 Vista des de Google Drive
Podem comprovar que les còpies realment s'han pujat a Google Drive.

![Carpeta a Google Drive](/tasca02/img_windows/captura24.png)

**Anàlisi**: Des del navegador, veiem la carpeta "PROVA_T02" dins de Google Drive on s'emmagatzemen les còpies. La carpeta es va modificar fa poc (6:52).

---

## 18. Configuració de restauració avançada

### 18.1 Opcions per fitxers existents
Quan restauram, podem triar com gestionar fitxers que ja existeixin.

![Opcions per fitxers existents](/tasca02/img_windows/captura25.png)

**Anàlisi**: Podem triar entre sobreescriure els fitxers existents o guardar versions diferents amb marca de temps al nom del fitxer.

---

## 19. Finalització del procés

### 19.1 Logs de restauració
Després de tot el procés, podem revisar els logs per veure què ha passat.

![Logs de restauració](/tasca02/img_windows/captura26.png)

**Anàlisi**: Els logs mostren quan i què s'ha fet durant la restauració. En aquest cas no hi ha més notificacions, tot ha anat bé.

---

