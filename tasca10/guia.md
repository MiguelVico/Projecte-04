# Guia de Configuració del Servidor d'Impressió CUPS

Aquesta guia explica pas a pas com configurar un servidor d'impressió CUPS a Ubuntu Server i compartir una impressora virtual amb un client Zorin OS. Tot això forma part d'una prova de concepte per demostrar la centralització d'impressores en una xarxa.

---

## 1. Configuració inicial de la xarxa a VirtualBox

Abans de res, hem de configurar correctament les interfícies de xarxa de la màquina virtual del servidor per assegurar-nos que pot comunicar-se amb l'exterior i amb el client.

### Configuració de la interfície NAT
Aquesta interfície permet al servidor accedir a internet per descarregar paquets.

![Configuració de xarxa NAT](/tasca10/img_T10/captura1.png)

**Anàlisi:**  
Aquí veiem que l'**Adaptador 1** està configurat en mode **Xarxa NAT**. Això permet que la màquina virtual surti a internet mitjançant la IP de la màquina host. El nom de la xarxa és `NatNetwork` i l'adaptador virtual és un `Intel PRO/1000 MT Desktop`. El cable està connectat.

### Configuració de la interfície Host-Only
Aquesta interfície crearà una xarxa privada entre el servidor i el client (Zorin OS), sense sortir a internet.

![Configuració de xarxa Host-Only](/tasca10/img_T10/captura2.png)

**Anàlisi:**  
L'**Adaptador 2** està habilitat i connectat al **Adaptador de només l'amfitrió (Host-Only)**. Això crearà una xarxa interna entre màquines virtuals. El nom de l'adaptador a la màquina host és `VirtualBox Host-Only Ethernet Adapter`. Aquesta serà la xarxa on el servidor CUPS escoltarà les peticions d'impressió.

---

## 2. Actualització del sistema

Abans d'instal·lar res, sempre és bona pràctica actualitzar els repositoris i el sistema.

```bash
sudo apt update && sudo apt upgrade -U
```

![Comanda d'actualització](/tasca10/img_T10/captura3.png)

**Anàlisi:**  
Executant aquesta comanda, descarreguem la llista més recent de paquets disponibles (`update`) i després actualitzem tots els paquets instal·lats a les seves versions més recents (`upgrade -U`). És un pas bàsic per evitar problemes de compatibilitat.

Després de l'actualització, el sistema ens mostra un resum dels paquets actualitzats i reinicia els serveis necessaris.

![Sortida de l'actualització](/tasca10/img_T10/captura4.png)

**Anàlisi:**  
Aquí podem veure com el sistema configura i reinicia diversos serveis després de l'actualització, com `netplan.io`, `cloud-init`, `dbus`, etc. Tot ha acabat sense errors.

---

## 3. Instal·lació de CUPS

Ara instal·lem el paquet **CUPS** (Common UNIX Printing System), que serà el nostre servidor d'impressió.

```bash
sudo apt install cups
```

![Instal·lació de CUPS](/tasca10/img_T10/captura5.png)

**Anàlisi:**  
La comanda `apt install cups` instal·larà no només CUPS sinó també totes les seves dependències, com ara `avahi-daemon` (per descobriment de serveis a la xarxa), `cups-client`, `cups-filters`, etc. Ens demana confirmació per descarregar uns 41.3 MB i ocupar uns 149 MB de disc. Premem **S** per continuar.

---

## 4. Verificació de l'estat del servei CUPS

Un cop instal·lat, comprovem que el servei CUPS estigui actiu i funcionant.

```bash
sudo systemctl status cups
```

![Estat del servei CUPS](/tasca10/img_T10/captura6.png)

**Anàlisi:**  
El servei `cups.service` està **actiu (running)**, **habilitat (enabled)** i porta uns 1 minut i 19 segons en marxa. El procés principal té el PID 6325. Això significa que CUPS s'ha instal·lat correctament i està llest per rebre configuracions.

---

## 5. Instal·lació de la impressora virtual CUPS-PDF

Com que no tenim una impressora física per fer proves, instal·lem una impressora virtual que “imprimeixi” documents a arxius PDF.

```bash
sudo apt install cups-pdf
```

![Instal·lació de cups-pdf](/tasca10/img_T10/captura8.png)

**Anàlisi:**  
El sistema instal·la el paquet `printer-driver-cups-pdf` (que substitueix `cups-pdf`). Només ocupa 25.5 kB i 2.1 MB de disc. Un cop instal·lat, actualitza automàticament les definicions de la impressora PDF i reinicia els serveis relacionats. Ara ja tenim una impressora virtual PDF disponible al sistema.

---

## 6. Reinici del servei CUPS i configuració

Després d'instal·lar nous components, reiniciem CUPS per assegurar-nos que carregui tots els canvis.

```bash
sudo systemctl restart cups && sudo systemctl status cups
```

![Reinici i estat de CUPS](/tasca10/img_T10/captura7.png)

**Anàlisi:**  
Reiniciem el servei amb `restart` i immediatament comprovem el seu estat amb `status`. Veiem que el servei s'ha reiniciat correctament i està actiu des de les `17:04:26 UTC`. Tot correcte.

També podem comprovar-ho amb:

```bash
sudo systemctl restart cups
systemctl status cups
```

![Alt reinici de CUPS](/tasca10/img_T10/captura12.png)

**Anàlisi:**  
El resultat és el mateix: servei actiu i funcionant des de les `17:10:48 UTC`. El procés principal ara és el PID 6923.

---

## 7. Configuració del fitxer `cupsd.conf`

Ara hem de configurar CUPS perquè escolti peticions no només des de la pròpia màquina (`localhost`) sinó també des de la xarxa (des del client Zorin).

Primer, fem una còpia de seguretat del fitxer de configuració per si la liem.

```bash
sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.bak
```

![Còpia de seguretat](/tasca10/img_T10/captura9.png)

**Anàlisi:**  
Així tenim una còpia de seguretat anomenada `cupsd.conf.bak` per poder restaurar la configuració original si alguna cosa surt malament.

Ara editem el fitxer de configuració:

```bash
sudo nano /etc/cups/cupsd.conf
```

![Edició del fitxer cupsd.conf](/tasca10/img_T10/captura10.png)

**Anàlisi:**  
Obrim el fitxer amb l'editor `nano`. El que veiem a continuació és el contingut del fitxer.

![Contingut de cupsd.conf](/tasca10/img_T10/captura11.png)

**Anàlisi:**  
Aquí és on hem de fer els canvis clau:

1. **Permetre l'accés des de la xarxa:**
   - Trobar la línia `Listen /run/cups/cups.sock` i afegir a sota:
     ```
     Listen 192.168.56.113:631
     ```
     (Aquesta és la IP de la interfície Host-Only del servidor, que veurem després).

2. **Permetre l'administració remota i la compartició:**
   - A la secció `<Location /admin>` i `<Location />`, podem ajustar les regles d'accés. Per defecte només permet `Allow @LOCAL` (localhost). Podríam afegir la xarxa local, per exemple `Allow 192.168.56.0/24`.

Per aquesta guia, assumirem que els canvis necessaris ja s'han fet per permetre l'accés des del client.

---

## 8. Accés a la interfície web d'administració de CUPS

CUPS ofereix una interfície web per configurar impressores i gestionar treballs. Hi accedim mitjançant el navegador a l'adreça:

```
https://192.168.56.113:631
```

![Accés a la interfície web](/tasca10/img_T10/captura13.png)

**Anàlisi:**  
Aquesta és la IP del servidor a la xarxa Host-Only (la mateixa que hem posat a la configuració de `cupsd.conf`). El port `631` és el port per defecte de CUPS. Utilitzem `https` per una connexió segura.

---

## 9. Interfície web de CUPS

Un cop dintre, veurem la pàgina d'inici de CUPS.

![Pàgina d'inici de CUPS](/tasca10/img_T10/captura14.png)

**Anàlisi:**  
És la pàgina principal de CUPS 2.4.7. Des d'aquí podem navegar a **Administration** per afegir impressores, gestionar treballs i configurar el servidor.

---

## 10. Configuració del servidor a la interfície web

A la secció d'administració, podem ajustar opcions importants com compartir impressores i permetre administració remota.

![Configuració del servidor](/tasca10/img_T10/captura15.png)

**Anàlisi:**  
Aquí activem les opcions:
- **Share printers connected to this system**: Per compartir les impressores amb la xarxa.
- **Allow remote administration**: Per administrar CUPS des d'altres màquines.
- **Allow printing from the Internet**: Si volem (per aquesta prova no és necessari).
- **Allow users to cancel any job**: Opcional, depèn de la política.

Després fem clic a **Change Settings** per desar.

---

## Resum fins aquí

1. Hem configurat la xarxa del servidor amb NAT (per internet) i Host-Only (per la xarxa interna).
2. Hem actualitzat el sistema.
3. Hem instal·lat CUPS i l'impressora virtual cups-pdf.
4. Hem verificat que el servei CUPS funciona.
5. Hem configurat CUPS per escoltar a la IP de la xarxa interna.
6. Hem accedit a la interfície web d'administració.
7. Hem activat la compartició d'impressores i l'administració remota.

Ara el servidor està llest per rebre treballs d'impressió del client Zorin OS.

---

# Guia de Configuració del Servidor d'Impressió CUPS (Part 2)

Ara continuem amb la configuració del client i la prova d'impressió. Ja tenim el servidor preparat, ara toca configurar el client Zorin OS perquè pugui imprimir al servidor i fer les proves.

---

## 11. Permisos d'administració de CUPS

Per poder afegir i gestionar impressores des de l'usuari normal, hem d'afegir l'usuari al grup `lpadmin`.

```bash
sudo usermod -aG lpadmin usuari
```

![Afegir usuari al grup lpadmin](/tasca10/img_T10/captura16.png)

**Anàlisi:**  
Amb aquesta comanda afegim l'usuari `usuari` al grup `lpadmin`. Aquest grup té permisos per administrar impressores a CUPS sense necessitat de fer `sudo` per a tot. És important tancar sessió i tornar-la a obrir perquè els canvis de grup tinguin efecte.

---

## 12. Afegir la impressora al servidor via interfície web

Ara anem a la interfície web de CUPS (`https://192.168.56.113:631`) i afegim la impressora virtual.

### Pas 1: Iniciem l'addició de impressora

A la pàgina d'Administració, fem clic a **Add Printer**.

![Pantalla inicial d'addició d'impressora](/tasca10/img_T10/captura17.png)

**Anàlisi:**  
Veiem que ja detecta la impressora local **CUPS-PDF (Virtual PDF Printer)**. Seleccionem aquesta i continuem.

### Pas 2: Configuració bàsica de la impressora

![Configuració del nom i descripció](/tasca10/img_T10/captura18.png)

**Anàlisi:**  
Posem:
- **Name:** `Virtual_PDF_Printer` (sense espais ni caràcters especials)
- **Description:** `Virtual PDF Printer`
- **Location:** `Server`
- **Connection:** `cups-pdf:/` (això indica que és la impressora virtual PDF)
- **Sharing:** ✅ Share This Printer (important perquè el client la pugui veure)

### Pas 3: Selecció del fabricant (Make)

![Selecció del fabricant](/tasca10/img_T10/captura19.png)

**Anàlisi:**  
Seleccionem **Generic** com a fabricant, ja que és una impressora virtual genèrica.

### Pas 4: Selecció del model

![Selecció del model](/tasca10/img_T10/captura20.png)

**Anàlisi:**  
Triem **Generic CUPS-PDF Printer (no options) (en)**. Aquest és el controlador per a la impressora virtual PDF.

### Pas 5: Confirmació i finalització

![Impressora afegida correctament](/tasca10/img_T10/captura21.png)

**Anàlisi:**  
Ens mostra un missatge de confirmació: **"Printer Virtual_PDF_Printer has been added successfully."**. Ja tenim la impressora afegida al servidor.

---

## 13. Configuració del client Zorin OS

Ara canviem a la màquina client (Zorin OS) per afegir la impressora compartida.

### Pas 1: Accedim a la configuració d'impressores

A Zorin, anem a **Settings** → **Printers**.

![Configuració d'impressores a Zorin](/tasca10/img_T10/captura22.png)

**Anàlisi:**  
Veiem que ja apareix una impressora disponible: **Virtual_PDF_Printer_usuari**. Això és la impressora que hem compartit des del servidor. Seleccionem-la i fem clic a **Add**.

### Pas 2: Verificació de la impressora afegida

![Impressora afegida al client](/tasca10/img_T10/captura23.png)

**Anàlisi:**  
Ara la impressora apareix com a **Virtual_PDF_Printer_usuari Ready**. El model és **CUPS v1.1** i la ubicació és **Server**. Està preparada per rebre treballs.

---

## 14. Prova d'impressió des del client

Ara farem una prova imprimint un document des del client.

### Pas 1: Obrim un document i intentem imprimir

Obrim **LibreOffice Impress** (o qualsevol altra aplicació) i fem clic a **Print**.

![Diàleg d'impressió a LibreOffice](/tasca10/img_T10/captura24.png)

**Anàlisi:**  
A la finestra d'impressió, seleccionem la impressora **CUPS-PDF-Printer** (que és la que hem afegit). Configurem les opcions (pàgines, còpies, etc.) i fem clic a **Print**.

---

## 15. Verificació al servidor

Després d'imprimir, comprovem al servidor si s'ha generat el PDF.

### Pas 1: Llistem les impressores al servidor

Tornem a la interfície web de CUPS i anem a **Printers**.

![Llista d'impressores al servidor](/tasca10/img_T10/captura25.png)

**Anàlisi:**  
Veiem que hi ha dues impressores:
1. **PDF** (la impressora PDF local del sistema)
2. **Virtual_PDF_Printer** (la que hem creat i compartit)

Totes dues estan **Idle** (inactives) perquè ja ha acabat el treball.

### Pas 2: Comprovem els treballs completats

Anem a la secció **Jobs** a la interfície web.

![Llista de treballs completats](/tasca10/img_T10/captura27.png)

**Anàlisi:**  
Aquí veiem un treball completat:
- **ID:** `Virtual_PDF_Printer-1`
- **State:** `completed at Wed Dec 17 17:38:52 2025`
- Podríem fins i tot reimprimir el treball amb **Reprint Job** si volguéssim.

### Pas 3: Detalls de la impressora

Si entrem a la impressora **Virtual_PDF_Printer**:

![Detalls de la impressora](/tasca10/img_T10/captura26.png)

**Anàlisi:**  
Veiem que l'estat és **Idle, Accepting Jobs, Shared**. També hi ha opcions com **Print Test Page**, **Pause Printer**, etc. Tot funciona correctament.

### Pas 4: Cercar el PDF generat

La impressora cups-pdf desa els PDFs a una carpeta específica. Normalment és a `/home/usuari/PDF` o `/var/spool/cups-pdf/`.

```bash
cd ~/PDF
ls -l
```

![Carpeta PDF amb el document generat](/tasca10/img_T10/captura28.png)

**Anàlisi:**  
Primer fem `ls -al` a l'inici i veiem que hi ha una carpeta **PDF**. Després entrem i hi veiem:

```bash
cd ~/PDF
ls -l
```

![Contingut de la carpeta PDF](/tasca10/img_T10/captura29.png)

**Anàlisi:**  
I aquí està! El fitxer **home_usuari_PDF.pdf** amb 5123 bytes, creat el `dic 17 17:38`. Aquest és el document que hem "imprès" des del client Zorin. El nom pot variar, però és el PDF resultant.

---

## Resum final

✅ **Tot ha funcionat correctament:**

1. **Servidor configurat:** CUPS instal·lat, impressora virtual afegida, compartició activada.
2. **Xarxa funcionant:** Host-Only connecta servidor i client.
3. **Client configurat:** Ha afegit la impressora compartida sense problemes.
4. **Prova realitzada:** S'ha imprès un document des del client.
5. **Resultat verificat:** Al servidor s'ha generat el PDF corresponent.

---

## Comandes principals utilitzades (resum)

```bash
# Servidor
sudo apt update && sudo apt upgrade -U
sudo apt install cups
sudo apt install cups-pdf
sudo systemctl status cups
sudo systemctl restart cups
sudo usermod -aG lpadmin usuari
sudo nano /etc/cups/cupsd.conf  # (per configurar Listen a la IP de la xarxa)

# Verificació
cd ~/PDF
ls -l
```

---

## Conclusió

Hem demostrat amb èxit la **Prova de Concepte (PoC)**. Un servidor Ubuntu amb CUPS pot gestionar una impressora virtual PDF i compartir-la a la xarxa, permetent que clients com Zorin OS hi puguin imprimir sense necessitat de drivers complexos ni impressores físiques. Això redueix costos, simplifica la gestió i centralitza el control d'impressió.

---
