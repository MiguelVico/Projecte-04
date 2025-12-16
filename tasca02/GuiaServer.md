# T02: Guia Tècnica - Duplicity al servidor Linux

## 1. Introducció
En aquesta part configurem còpies de seguretat amb **Duplicity** a Ubuntu Server. La idea és fer còpies completes diàries i incrementals, i automatitzar-ho amb cron. Seguint la política 3-2-1, aquesta seria la còpia local.

---

## 2. Preparació del disc secundari

### 2.1 Afegir el disc a VirtualBox
Primer hem d'afegir un disc dur virtual de 10 GB a la nostra màquina.

![Selector de disc dur a VirtualBox](/tasca02/img_server/captura27.png)

**Anàlisi**: A la interfície de VirtualBox, seleccionem "Afegeix" per afegir un disc nou. Creem un disc VDI de 10 GB per emmagatzemar les còpies.

---

### 2.2 Verificació dels discos
Un cop afegit el disc, comprovem que el sistema el reconeix.

![Llistat de discs amb lsblk](/tasca02/img_server/captura28.png)

**Anàlisi**: Amb `lsblk` veiem tots els discs. Identifiquem:
- `sda`: Disc principal del sistema (256 GB)
- `sdb`: Disc secundari afegit (10 GB)
- El disc `sdb` encara no té particions ni sistema de fitxers.

---

### 2.3 Creació de la partició
Ara creem una partició al disc secundari.

![Creació de partició amb parted](/tasca02/img_server/captura29.png)

**Anàlisi**: Executem `sudo parted /dev/sdb` per entrar a l'eina parted. Aquí crearem una partició que ocupi tot el disc.

---

### 2.4 Comprovació del sistema de fitxers
Abans de formatar, mirem quin sistema de fitxers tenim.

![Comprovació de sistemes de fitxers](/tasca02/img_server/captura30.png)

**Anàlisi**: Amb `lsblk -f` veiem que `sdb` no té cap sistema de fitxers (`FSTYPE` buit). El disc principal `sda` té particions amb vfat i ext4.

---

## 3. Instal·lació de les eines necessàries

### 3.1 Instal·lació de Duplicity i XFS
Instal·lem les eines que necessitarem.

![Instal·lació de duplicity i xfsprogs](/tasca02/img_server/captura31.png)

**Anàlisi**: Executem `sudo apt install duplicity xfsprogs`. El sistema ens diu que ja estan instal·lats a les versions més recents.

---

### 3.2 Verificació de xfsprogs
Comprovem específicament que xfsprogs està instal·lat.

![Verificació de xfsprogs](/tasca02/img_server/captura39.png)

**Anàlisi**: Confirmem que `xfsprogs` ja està instal·lat en la versió 5.13.0.

---

## 4. Formatació del disc

### 4.1 Error inicial de formatació
Provem de formatar el disc però encara no hem creat la partició.

![Error al formatar sense partició](/tasca02/img_server/captura32.png)

**Anàlisi**: El comandament `sudo mkfs.xfs -f /dev/sdb1` falla perquè `/dev/sdb1` no existeix. Primer cal crear la partició amb parted.

**Solució**: Dins de parted executem:
```
(parted) mklabel gpt
(parted) mkpart primary 0% 100%
(parted) quit
```

---

### 4.2 Creació del punt de muntatge
Creem la carpeta on muntarem el disc.

![Creació del punt de muntatge](/tasca02/img_server/captura33.png)

**Anàlisi**: Executem:
- `sudo mkdir -p /media/backup`: Crea la carpeta
- `sudo mount /dev/sdb1 /media/backup`: Munta la partició (després de crear-la i formatar-la)

---

## 5. Creació d'usuaris i dades de prova

### 5.1 Creació del primer usuari
Creem un usuari per tenir dades de prova.

![Creació d'usuari1](/tasca02/img_server/captura34.png)

**Anàlisi**: Amb `sudo adduser usuari1` creem un nou usuari. El sistema ens demana contrasenya (minim 8 caràcters) i daddes addicionals.

---

### 5.2 Creació del segon usuari
Creem un segon usuari.

![Creació d'usuari2](/tasca02/img_server/captura35.png)

**Anàlisi**: El procés és idèntic al primer usuari. Ara tenim dos usuaris nous amb les seves carpetes home.

---

### 5.3 Navegació a carpetes d'usuari
Ens movem a la carpeta d'un usuari per crear arxius.

![Canvi a carpeta home](/tasca02/img_server/captura36.png)

**Anàlisi**: Amb `cd /home/client` entrem a una carpeta d'usuari. En realitat, després de crear usuari1 i usuari2, les carpetes serien `/home/usuari1` i `/home/usuari2`.

---

### 5.4 Creació d'arxius de prova
Creem arxius de 10 MB per tenir dades per fer còpia.

![Creació d'arxius amb fallocate](/tasca02/img_server/captura37.png)

**Anàlisi**: Executem `fallocate -l 10M fitxerX.bin` per crear 4 arxius de 10 MB cada un. Això ens donarà 40 MB de dades per a les còpies.

---

## 6. Configuració de Duplicity

### 6.1 Establiment de la passphrase
Configurem la variable d'entorn per a la contrasenya de xifrat.

![Exportació de la passphrase](/tasca02/img_server/captura38.png)

**Anàlisi**: `export PASSPHRASE="contrasenya_forta"` estableix la contrasenya que Duplicity usarà per xifrar les còpies. És important que sigui forta!

---

### 6.2 Primera execució de Duplicity
Provem de fer una còpia per veure com funciona.

![Primera execució de duplicity](/tasca02/img_server/captura40.png)

**Anàlisi**: Executem `sudo duplicity /home file:///media/backup/duplicity`. El sistema ens demana la passphrase GnuPG (que ja hem establert). Com és la primera còpia, no hi ha cap còpia prèvia.

---

## 7. Desmuntatge del disc
Un cop feta la còpia, desmuntem el disc per seguretat.

![Desmuntatge del disc](/tasca02/img_server/captura41.png)

**Anàlisi**: Amb `sudo umount /media/backup` desmuntem el disc. Això assegura que les dades no es corrompin i que el disc no estigui sempre accessible.

---

## 8. Automatització amb scripts

### 8.1 Creació del script de còpia completa
Creem un script per automatitzar les còpies completes.

![Creació del script fullbackup.sh](/tasca02/img_server/captura42.png)

**Anàlisi**: Obrim l'editor nano per crear `/usr/local/bin/fullbackup.sh`.

---

### 8.2 Contingut del script de còpia completa
Aquest és el codi del script.

![Contingut de fullbackup.sh](/tasca02/img_server/captura43.png)

**Anàlisi**: El script fa:
1. Defineix variables (dispositiu, punt de muntatge, etc.)
2. Exporta la passphrase
3. Crea el punt de muntatge si no existeix
4. Munta el disc si no està muntat
5. Executa Duplicity amb opció de còpia completa
6. Desmunta el disc

---

### 8.3 Permisos d'execució
Donem permisos d'execució al script.

![Donar permisos al script](/tasca02/img_server/captura44.png)

**Anàlisi**: Amb `sudo chmod +x /usr/local/bin/fullbackup.sh` fem que el script sigui executable.

---

## 9. Programació amb cron

### 9.1 Configuració del cron per a còpies completes
Configurem cron per executar el script els diumenges a les 23:00.

![Configuració de crontab](/tasca02/img_server/captura45.png)

**Anàlisi**: Executem `sudo crontab -e` i afegim:
```
0 23 * * 0 /usr/local/bin/fullbackup.sh
```
Això vol dir: minuts 0, hora 23, qualsevol dia del mes, qualsevol mes, diumenge (0).

---

### 9.2 Script de còpies incrementals
Creem un segon script per a còpies incrementals diàries.

![Contingut d'incrementalbackup.sh](/tasca02/img_server/captura46.png)

**Anàlisi**: Aquest script és similar al de còpia completa però sense l'opció `--full-if-older-than`. S'executarà de dilluns a dissabte a les 23:00.

---

# T02: Guia Tècnica - Duplicity al servidor Linux (Continuació)

## 10. Configuració del script incremental (continuació)

### 10.1 Permisos del script incremental
Després de crear el script incremental, li donem permisos d'execució.

![Permisos per a incrementalbackup.sh](/tasca02/img_server/captura47.png)

**Anàlisi**: Amb `sudo chmod +x /usr/local/bin/incrementalbackup.sh` fem que el segon script també sigui executable. Ara ja tenim els dos scripts preparats.

---

## 11. Configuració final del cron

### 11.1 Edició del crontab
Configurem les dues tasques al cron.

![Primera vista del crontab](/tasca02/img_server/captura48.png)

**Anàlisi**: Veiem el crontab amb les dues línies afegides:
- `0 23 * * 0 /usr/local/bin/fullbackup.sh` (diumenges a les 23:00)
- `0 23 * * 1-6 /usr/local/bin/incrementalbackup.sh` (de dilluns a dissabte a les 23:00)

---

### 11.2 Crontab corregit
Aquí veiem una versió millorada de la configuració.

![Crontab final](/tasca02/img_server/captura49.png)

**Anàlisi**: La configuració definitiva és:
- `0 23 * * 0`: Diumenges a les 23:00 (còpia completa)
- `0 23 * * 1-6`: De dilluns a dissabte a les 23:00 (còpia incremental)

Els números dels dies de la setmana són: 0=diumenge, 1=dilluns, 2=dimarts, etc.

---

## 12. Verificació del contingut de les còpies

### 12.1 Exploració del disc de còpies
Quan muntem el disc, podem veure què hi ha dins.

![Contingut del disc de còpies](/tasca02/img_server/captura50.png)

**Anàlisi**: Des del gestor d'arxius veiem la carpeta `duplicity` dins de `/media/backup`. És on es guarden totes les còpies.

---

### 12.2 Detall dels fitxers de còpia
Una mirada més detallada als fitxers.

![Fitxers de duplicity](/tasca02/img_server/captura51.png)

**Anàlisi**: Veiem els fitxers que Duplicity ha creat:
- `duplicity-full...`: Còpia completa
- `duplicity-inc...`: Còpia incremental
- `.manifest`, `.vol...`: Fitxers de dades
- `.signatures`: Signatures per detectar canvis

---

## 13. Comprovació des de terminal

### 13.1 Muntatge manual
Abans de comprovar, muntem el disc manualment.

![Muntatge del disc](/tasca02/img_server/captura52.png)

**Anàlisi**: Executem `sudo mount /dev/sdb1 /media/backup` per muntar el disc si no ho està ja.

---

### 13.2 Llistat dels fitxers
Comprovem els fitxers des del terminal.

![Llistat des de terminal](/tasca02/img_server/captura53.png)

**Anàlisi**: Amb `ls /media/backup/duplicity` veiem els mateixos fitxers que abans, però des de la línia de comandes.

---

## 14. Estat de les còpies amb Duplicity

### 14.1 Comprovació de l'estat
Duplicity té una comanda per veure l'estat de les còpies.

![Comandament collection-status](/tasca02/img_server/captura54.png)

**Anàlisi**: Executem `duplicity collection-status file:///media/backup/duplicity` per veure informació detallada de les còpies.

---

### 14.2 Resultat de l'estat
Aquesta és la informació que ens dóna Duplicity.

![Resultat de collection-status](/tasca02/img_server/captura55.png)

**Anàlisi**: Veiem informació molt útil:
- **Última còpia completa**: 9 de desembre de 2025 a les 13:17:49
- **Cadena de còpies**:
  - Còpia completa: 9 de desembre 13:17:49 (1 volum)
  - Còpia incremental: 9 de desembre 13:22:00 (1 volum)
- **Total**: 2 conjunts de còpia, 2 volums
- **Cap còpia orfe o incompleta**: Tot està correcte

---

## 15. Resum Final del Procés

### **Tot el que hem fet**:

1. **Preparació del disc**:
   - Afegir disc de 10 GB a VirtualBox
   - Crear partició amb parted
   - Formatar amb XFS
   - Crear punt de muntatge `/media/backup`

2. **Instal·lació i configuració**:
   - Instal·lar Duplicity i xfsprogs
   - Crear usuaris de prova (usuari1, usuari2)
   - Generar arxius de 10 MB per testejar

3. **Scripts d'automatització**:
   - `fullbackup.sh`: Còpia completa els diumenges
   - `incrementalbackup.sh`: Còpies incrementals de dilluns a dissabte
   - Ambdós scripts: Munten, fan còpia, desmunten

4. **Programació amb cron**:
   - Diumenges 23:00 → Còpia completa
   - Dilluns-Dissabte 23:00 → Còpia incremental

5. **Verificació**:
   - Comprovar fitxers al disc de còpies
   - Veure estat amb `duplicity collection-status`
   - Confirmar que hi ha còpies completes i incrementals

### **Aspectes de seguretat**:
- ✅ El disc està desmuntat per defecte
- ✅ Les còpies estan xifrades amb GPG
- ✅ Passphrase emmagatzemada de manera segura
- ✅ Logs guardats a `/var/log/`
- ✅ Disc formatat amb XFS (bo per grans fitxers)

### **Com restaurar si cal**:
Per restaurar arxius esborrats accidentalment:
```bash
export PASSPHRASE="contrasenya_forta"
sudo mount /dev/sdb1 /media/backup
duplicity restore file:///media/backup/duplicity /ruta/on/restaurar
sudo umount /media/backup
```

---

## 16. Conclusió
Ara el servidor Linux està protegit amb còpies automàtiques:
- **Còpia completa setmanal** (diumenges)
- **Còpies incrementals diàries** (dilluns a dissabte)
- **Tot automatitzat** amb cron
- **Disc desmuntat** quan no s'usa (més segur)

Combinat amb la part de Windows (Duplicati + Google Drive), tenim un pla de còpies complet que segueix l'esquema 3-2-1 per a l'empresa "Muntatges i Serveis Tècnics SL".
