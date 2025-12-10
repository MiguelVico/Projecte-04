# Guia: Configuració d'un Servidor NFS i Client Linux

Aquest document explica pas a pas com es va configurar un servidor NFS (Network File System) i un client Linux per a la startup DevOptimize Solutions. L'objectiu era centralitzar fitxers per evitar problemes de versions entre desenvolupadors.

---

## 1. Preparació del sistema

Abans de començar, hem actualitzat el sistema per assegurar-nos que tot estigui al dia.

**Comanda:**
```bash
sudo apt update && sudo apt upgrade -y
```
![Actualització del sistema](/tasca09/img_t09/captura1.png)

*Anàlisi:*
Aquí només s'executen les comandes d'actualització del sistema. No hi ha resposta visible, però la comanda `-y` confirma automàticament totes les preguntes.

---

## 2. Configuració de xarxa

Es va configurar la interfície de xarxa per assegurar connectivitat.

**Fitxer de configuració:**
```yaml
version: 2
ethernets:
  enpos3:
    dhcp4: true
  enpos6:
    dhcp4: true
```
![Configuració de xarxa](/tasca09/img_t09/captura2.png)

*Anàlisi:*
Es configura la xarxa amb DHCP per a dues interfícies (`enpos3` i `enpos6`). Això permet obtenir una IP automàticament del router.

---

## 3. Instal·lació del servidor NFS

Instal·lem el paquet necessari per a configurar el servidor NFS.

**Comanda:**
```bash
sudo apt install nfs-kernel-server
```
![Instal·lació del servidor NFS](/tasca09/img_t09/captura3.png)

*Anàlisi:*
El sistema mostra els paquets que s'instal·laran (`nfs-kernel-server`, `nfs-common`, `rpcbind`, etc.) i demana confirmació. Es veu que es necessiten 569 kB de descàrrega i 2.022 kB d'espai addicional.

---

## 4. Creació de grups

Es creen els grups `devs` i `admins` per organitzar els usuaris.

**Comandes:**
```bash
sudo groupadd devs
sudo groupadd admins
```
![Creació del grup devs](/tasca09/img_t09/captura4.png)
![Creació del grup admins](/tasca09/img_t09/captura5.png)

*Anàlisi:*
Aquestes comandes creen dos grups al sistema: `devs` per a desenvolupadors i `admins` per a administradors. No hi ha sortida visible si tot va bé.

---

## 5. Creació d'usuaris

Es creen usuaris i s'assignen als grups corresponents.

**Comandes:**
```bash
sudo useradd -m -s /bin/bash -g devs dev@1
sudo useradd -m -s /bin/bash -g admins admin@1
```
![Creació d'usuaris](/tasca09/img_t09/captura6.png)

*Anàlisi:*
Es creen dos usuaris: `dev@1` i `admin@1`, amb el seu directori home (`-m`), shell bash (`-s /bin/bash`) i assignats als seus grups (`-g`). Més endavant se'ls assignarà contrasenya.

---

## 6. Assignació de contrasenyes

Es defineixen contrasenyes per als nous usuaris.

**Comandes:**
```bash
sudo passwd admin@1
sudo passwd dev@1
```
![Assignació de contrasenyes](/tasca09/img_t09/captura16.png)

*Anàlisi:*
Aquí es canvia la contrasenya dels usuaris. El sistema avisa si la contrasenya és massa curta, però finalment s'accepta.

---

## 7. Creació de directoris compartits

Es preparen les carpetes que es compartiran via NFS.

**Comandes:**
```bash
sudo mkdir -p /srv/nfs/dev_projects
sudo mkdir -p /srv/nfs/admin_tools
```
![Creació de directoris](/tasca09/img_t09/captura7.png)

*Anàlisi:*
Es creen dos directoris dins de `/srv/nfs/`: un per a projectes de desenvolupament i un altre per a eines d'administració.

---

## 8. Assignació de permisos i propietat

Es canvia el propietari i els permisos dels directoris.

**Comandes:**
```bash
sudo chown root:devs /srv/nfs/dev_projects
sudo chown root:admins /srv/nfs/admin_tools
sudo chmod 770 /srv/nfs/dev_projects
sudo chmod 770 /srv/nfs/admin_tools
```
![Canvi de propietari i permisos](/tasca09/img_t09/captura7.png)

*Anàlisi:*
- `chown`: assigna el propietari `root` i el grup corresponent a cada carpeta.
- `chmod 770`: dona permisos de lectura, escriptura i execució al propietari i al grup, però cap permís als altres usuaris.

---

## 9. Configuració de les exportacions NFS

Es defineix quines carpetes es comparteixen i amb quines regles.

**Comanda per editar el fitxer:**
```bash
sudo nano /etc/exports
```
![Edició del fitxer exports](/tasca09/img_t09/captura9.png)

**Contingut afegit:**
```
/srv/nfs/dev_projects 192.168.56.109(rw,sync,no_subtree_check)
/srv/nfs/admin_tools 192.168.56.109(rw,sync,no_subtree_check)
```
![Configuració d'exportacions](/tasca09/img_t09/captura10.png)

*Anàlisi:*
Aquí s'indica que els dos directoris es comparteixen amb la IP `192.168.56.109` amb permisos de lectura i escriptura (`rw`), sincronització (`sync`) i sense comprovació de subarbres (`no_subtree_check`).

---

## 10. Reinici i activació del servei NFS

S'apliquen els canvis i s'activa el servei perquè s'iniciï automàticament.

**Comandes:**
```bash
sudo exportfs -Fa
sudo systemctl restart nfs-kernel-server
sudo systemctl enable nfs-kernel-server
```
![Reinici del servei NFS](/tasca09/img_t09/captura11.png)
![Activació del servei NFS](/tasca09/img_t09/captura12.png)

*Anàlisi:*
- `exportfs -Fa`: força la reexportació de totes les carpetes.
- `systemctl restart`: reinicia el servei NFS per aplicar canvis.
- `systemctl enable`: activa l'inici automàtic del servei en arrencar el sistema.

---

## 11. Verificació de les exportacions al servidor

Es comprova que les carpetes estiguin ben exportades.

**Comanda:**
```bash
sudo showmount -e
```
![Verificació d'exportacions al servidor](/tasca09/img_t09/captura13.png)

*Anàlisi:*
Es llisten les carpetes exportades i la xarxa amb permisos: `192.168.56.0/24`. Això vol dir que totes les IPs d'aquesta xarxa poden accedir.

---

## 12. Preparació del client NFS

Des del client, instal·lem els paquets necessaris per muntar recursos NFS.

**Comanda:**
```bash
sudo apt install nfs-common -y
```
![Instal·lació de nfs-common al client](/tasca09/img_t09/captura17.png)

*Anàlisi:*
S'instal·la el paquet `nfs-common`, que conté les eines necessàries per a muntar recursos NFS des d'un client Linux.

---

## 13. Prova de connectivitat entre client i servidor

Es fa un ping per verificar que el client pot arribar al servidor.

**Comanda:**
```bash
ping 192.168.56.108
```
![Prova de ping al servidor](/tasca09/img_t09/captura18.png)

*Anàlisi:*
El client fa ping a la IP `192.168.56.108` (el servidor). Es reben 4 paquets amb temps molt baixos (entre 0.5 i 1.24 ms), cosa que indica una connexió xarxa estable i propera.

---

## 14. Verificació de les exportacions des del client

Es comprova des del client quines carpetes ofereix el servidor.

**Comanda:**
```bash
showmount -e 192.168.56.108
```
![Verificació d'exportacions des del client](/tasca09/img_t09/captura19.png)

*Anàlisi:*
El servidor respon amb la llista de recursos exportats i la IP específica del client (`192.168.56.109`) que té accés. Això confirma que el servidor està compartint correctament.

---

## 15. Muntatge dels recursos al client

Es crea un directori local al client i es munta el recurs NFS.

**Comanda:**
```bash
sudo mkdir -p /mnt/admin_tools
```
![Creació del directori local al client](/tasca09/img_t09/captura20.png)

*Anàlisi:*
Es crea un directori local (`/mnt/admin_tools`) on es muntarà la carpeta remota. Això permet accedir als fitxers compartits com si fossin locals.

---

# Guia: Configuració d'un Servidor NFS i Client Linux (Part 2)

Continuació de la configuració del servidor NFS i client Linux per a DevOptimize Solutions. En aquesta part, completem el muntatge dels recursos, la creació de fitxers de prova, la configuració permanent i la verificació final.

---

## 16. Modificació de les exportacions per tota la xarxa

S'actualitza el fitxer `/etc/exports` del servidor per permetre l'accés des de tota la xarxa `192.168.56.0/24`.

**Contingut actualitzat del fitxer exports:**
```
/srv/nfs/dev_projects 192.168.56.0/24(rw,sync,no_subtree_check)
/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,no_subtree_check)
```
![Configuració d'exportacions per tota la xarxa](/tasca09/img_t09/captura23.png)

*Anàlisi:*
Ara s'ha canviat la IP específica `192.168.56.109` per la xarxa sencera `192.168.56.0/24`. Això vol dir que qualsevol dispositiu d'aquesta xarxa podrà accedir als recursos compartits.

---

## 17. Reexportació i reinici del servei NFS

S'apliquen els canvis al servidor reiniciant el servei NFS.

**Comandes:**
```bash
sudo exportfs -na
sudo systemctl restart nfs-kernel-server
sudo exportfs -u
sudo systemctl enable nfs-kernel-server
```
![Reinici del servei NFS després dels canvis](/tasca09/img_t09/captura24.png)

*Anàlisi:*
- `exportfs -na`: mostra totes les exportacions actuals sense aplicar-les.
- `systemctl restart`: reinicia el servei per aplicar canvis.
- `exportfs -u`: desmunta totes les exportacions.
- `systemctl enable`: assegura que el servei s'iniciï en arrencar el sistema.

---

## 18. Verificació de connectivitat client-servidor

Es torna a verificar que el client pugui arribar al servidor amb un ping prolongat.

**Comanda:**
```bash
ping 192.168.56.108
```
![Ping prolongat al servidor](/tasca09/img_t09/captura25.png)

*Anàlisi:*
Es fan 10 pings amb èxit, sense pèrdua de paquets. Els temps de resposta són molt baixos (mitjana de 0.460 ms), confirmant una connexió estable i ràpida.

---

## 19. Actualització del sistema client

Abans de continuar, s'actualitza el client per assegurar que tot estigui al dia.

**Comanda:**
```bash
sudo apt upgrade
```
![Actualització del client](/tasca09/img_t09/captura26.png)

*Anàlisi:*
El sistema detecta 23 paquets per actualitzar (incloent cups, brave-browser, libwebkit, etc.) i demana confirmació. Es necessiten 203 MB de descàrrega.

---

## 20. Verificació de les exportacions des del client (amb la nova configuració)

Es comprova que el client vegi les exportacions amb la configuració actualitzada de xarxa.

**Comanda:**
```bash
sudo showmount -e 192.168.56.108
```
![Verificació d'exportacions amb xarxa /24](/tasca09/img_t09/captura27.png)

*Anàlisi:*
Ara el servidor mostra que les carpetes estan disponibles per a tota la xarxa `192.168.56.0/24`, confirmant el canvi fet anteriorment.

---

## 21. Creació del punt de muntatge al client

Si no existeix, es crea el directori local per muntar el recurs.

**Comanda:**
```bash
sudo mkdir -p /mnt/admin_tools
```
![Creació del directori admin_tools al client](/tasca09/img_t09/captura28.png)

*Anàlisi:*
Es crea el directori `/mnt/admin_tools` si no existeix. La opció `-p` evita errors si ja existeix.

---

## 22. Muntatge manual del recurs NFS

Es munta la carpeta remota `admin_tools` al directori local.

**Comanda:**
```bash
sudo mount -t nfs 192.168.56.108:/srv/nfs/admin_tools /mnt/admin_tools
```
![Muntatge manual del recurs admin_tools](/tasca09/img_t09/captura29.png)

*Anàlisi:*
Amb aquesta comanda s'estableix la connexió NFS entre el servidor i el client. La carpeta remota `/srv/nfs/admin_tools` ara apareix com a local a `/mnt/admin_tools`.

---

## 23. Prova de desmuntatge i remuntatge

Per comprovar que el muntatge funciona correctament, es desmunta i es torna a muntar.

**Comandes:**
```bash
sudo umount /mnt/admin_tools
sudo mount -t nfs 192.168.56.108:/srv/nfs/admin_tools /mnt/admin_tools
```
![Desmuntatge i remuntatge del recurs](/tasca09/img_t09/captura30.png)
![Remuntatge del recurs](/tasca09/img_t09/captura31.png)

*Anàlisi:*
- `umount`: desmunta el recurs (no es mostra sortida si va bé).
- `mount`: el torna a muntar. Aquest pas confirma que la connexió és persistent i reutilitzable.

---

## 24. Creació de fitxers de prova

Des del client, es creen fitxers dins del recurs muntat per verificar els permisos d'escriptura.

**Comandes:**
```bash
sudo touch /mnt/admin_tools/test1.txt
sudo touch /mnt/admin_tools/test2.txt
```
![Creació de fitxers de prova](/tasca09/img_t09/captura22.png)
![Creació del segon fitxer de prova](/tasca09/img_t09/captura32.png)

*Anàlisi:*
Es creen dos fitxers buits dins de la carpeta compartida. Com que es requereix `sudo`, es demana la contrasenya de l'usuari. Això demostra que el client té permisos d'escriptura al recurs.

---

## 25. Muntatge del segon recurs (dev_projects)

Es repeteix el procés per a la carpeta de projectes de desenvolupament.

**Comandes:**
```bash
sudo mkdir -p /mnt/dev_projects
sudo mount -t nfs 192.168.56.108:/srv/nfs/dev_projects /mnt/dev_projects
```
![Muntatge del recurs dev_projects](/tasca09/img_t09/captura33.png)

*Anàlisi:*
Ara es munta la segona carpeta compartida (`dev_projects`). Amb això, el client té accés als dos recursos del servidor.

---

## 26. Visualització dels recursos muntats des de l'explorador gràfic

Es comprova que les carpetes apareguin a l'entorn gràfic del client.

**Visualització a l'explorador:**
![Carpetes muntades a /mnt](/tasca09/img_t09/captura34.png)
![Contingut d'admin_tools a l'explorador](/tasca09/img_t09/captura35.png)
![Vista general de les carpetes muntades](/tasca09/img_t09/captura36.png)
![Vista compacta de les carpetes muntades](/tasca09/img_t09/captura37.png)

*Anàlisi:*
Les captures mostren com l'entorn gràfic (probablement Zorin OS) mostra les dues carpetes muntades (`admin_tools` i `dev_projects`) dins de `/mnt`. També es veuen els fitxers de prova creats anteriorment (`test1.txt` i `test2.txt`).

---

## 27. Configuració del muntatge automàtic (fstab)

Perquè els recursos es muntin automàticament en arrencar el sistema, s'afegeixen al fitxer `/etc/fstab`.

**Comanda per editar:**
```bash
sudo nano /etc/fstab
```
![Edició del fitxer fstab](/tasca09/img_t09/captura38.png)

**Línies afegides:**
```
192.168.56.108:/srv/admin_tools /mnt/admin_tools nfs defaults 0 0
192.168.56.108:/srv/dev_projects /mnt/dev_projects nfs defaults 0 0
```
![Configuració del fstab](/tasca09/img_t09/captura39.png)

*Anàlisi:*
- **Origen**: la IP del servidor i la ruta exportada.
- **Punt de muntatge**: les carpetes locals on es muntaran.
- **Tipus**: `nfs`.
- **Opcions**: `defaults` (inclou rw, sync, etc.).
- **Dump i pass**: `0 0` (no es fa còpia de seguretat ni verificació).

---

## 28. Verificació final dels muntatges

Es comprova que els recursos estiguin muntats correctament amb totes les opcions.

**Comanda:**
```bash
mount | grep nfs
```
![Verificació detallada dels muntatges NFS](/tasca09/img_t09/captura40.png)

*Anàlisi:*
La sortida mostra:
- Ambdues carpetes muntades amb NFS versió 4.2
- IP del servidor: `192.168.56.108`
- IP del client: `192.168.56.109`
- Opcions: `rw` (lectura/escriptura), `relatime`, `hard` (intents repetits en error), `proto=tcp`
- Totes les opcions confirmen un muntatge estable i funcional

---

## Resum final complet

### **Què hem aconseguit:**
1. **Servidor NFS configurat** amb dues carpetes compartides (`dev_projects` i `admin_tools`)
2. **Control d'accés per xarxa** (només la xarxa `192.168.56.0/24`)
3. **Usuaris i grups** creats per simular l'entorn de la startup
4. **Permisos Linux** assignats (770 per a propietari+grup)
5. **Client configurat** per accedir als recursos
6. **Muntatge manual i automàtic** configurat
7. **Proves d'escriptura** realitzades amb èxit
8. **Verificació de connectivitat** i funcionament

### **Beneficis per DevOptimize Solutions:**
- **Centralització real**: tots els desenvolupadors treballen sobre els mateixos fitxers
- **Control de versions** millorat (no més còpies locals contradictòries)
- **Seguretat bàsica** mitjançant permisos Unix i restriccions de xarxa
- **Accés transparent** (els fitxers apareixen com a locals als usuaris)

### **Pròxims passos (si es volgués ampliar):**
- Configurar NFSv4 amb Kerberos per a autenticació forta
- Implementar quotes de disc
- Configurar còpies de seguretat automàtiques
- Monitoritzar l'ús del servidor

El sistema ja està llest per ser usat en producció pels desenvolupadors i administradors de DevOptimize Solutions! 🚀
