# T05: Accés Remot – Guia de Configuració i Ús SSH  
**Destinat a: nous becaris de la consultora**

Hola! Si estàs llegint això és perquè t’acabes d’incorporar a l’equip i et toca aprendre a gestionar servidors de forma remota. No et preocupis, aquí tens tot explicat pas a pas, amb les comandes que necessites i captures per veure com es fa. Això és el que fem diàriament per administrar servidors Linux i Windows de manera segura via **SSH**.

---

## 1. Configuració inicial de la xarxa a Linux  
Abans de connectar-nos remotament, hem de tenir la xarxa ben configurada. Això ho fem amb `netplan`.

**Comanda utilitzada:**  
```bash
sudo netplan apply
```

**Explicació:**  
Aquesta comanda aplica els canvis de configuració de xarxa que hem definit a `/etc/netplan/`. És com dir-li al sistema: “Ara sí, activa la nova configuració de xarxa que t’he donat”.

![Configuració de xarxa amb netplan](/tasca05/img_T05/captura1.png)

---

## 2. Actualització del sistema  
Sempre és bona pràctica tenir el sistema actualitzat abans de configurar serveis nous.

**Comanda utilitzada:**  
```bash
sudo apt upgrade && sudo apt update -y
```

**Explicació:**  
- `sudo apt update` actualitza la llista de paquets disponibles.  
- `sudo apt upgrade` actualitza els paquets instal·lats a les noves versions.  
- El `-y` fa que contesti “sí” automàticament a tot, per no haver d’estar pendent.

![Actualització del sistema](/tasca05/img_T05/captura3.png)

---

## 3. Configuració del servidor SSH  
Per poder connectar-nos de forma remota, hem de configurar el servidor SSH. Ho fem editant el fitxer de configuració.

**Comanda per editar el fitxer:**  
```bash
sudo nano /etc/ssh/sshd_config
```

**Canvis principals que hem fet:**  
- `PermitRootLogin prohibit-password`: no permet l’accés directe amb contrasenya a l’usuari root (més segur).  
- `AllowUsers usuari`: només l’usuari “usuari” pot connectar-se via SSH.  
- `StrictModes yes`: el sistema comprova els permisos dels fitxers clau abans d’acceptar connexions.

![Configuració de sshd_config](/tasca05/img_T05/captura4.png)  
![Part de la configuració SSH](/tasca05/img_T05/captura5.png)

---

## 4. Connexió SSH des d’un client Linux  
Ara provem de connectar-nos des d’un altre Linux a la nostra màquina servidora.

**Comanda utilitzada:**  
```bash
ssh -D 9876 usuari@192.168.56.106
```

**Explicació:**  
- `ssh`: comanda per connectar via Secure Shell.  
- `-D 9876`: obre un túnel SOCKS al port local 9876 (útil per redirigir tràfic).  
- `usuari@192.168.56.106`: usuari i IP del servidor al que volem connectar-nos.

**Anàlisi de la captura:**  
En la primera connexió, el sistema ens avisa que no reconeix la màquina remota i ens mostra la seva empremta digital (fingerprint) per verificar-ne l’autenticitat. Això és normal i és part de la seguretat de SSH.

![Primera connexió SSH](/tasca05/img_T05/captura7.png)

---

## 5. Instal·lació de Wireshark (opcional per anàlisi de xarxa)  
Wireshark és una eina que ens permet capturar i analitzar paquets de xarxa. És útil per depurar connexions.

**Descàrrega des del lloc oficial:**  
A la pàgina de Wireshark podem veure les versions estables i escollir la que coincideixi amb el nostre sistema (Windows, macOS, Ubuntu, etc.).

![Descàrrega de Wireshark](/tasca05/img_T05/captura8.png)  
![Instal·lador de Wireshark](/tasca05/img_T05/captura9.png)  
![Completant la instal·lació](/tasca05/img_T05/captura10.png)  
![Instal·lació finalitzada](/tasca05/img_T05/captura11.png)

**Explicació:**  
Wireshark s’instal·la amb un assistent senzill. Cal assegurar-se de tancar l’aplicació abans de començar la instal·lació. En el procés, també instal·la **Npcap** (per a Windows), que permet capturar tràfic de xarxa.

---

## 6. Ús de Wireshark per analitzar tràfic SSH  
Wireshark ens permet veure el tràfic de xarxa en temps real. És útil per comprovar que les connexions SSH es fan correctament i que tot el tràfic està xifrat.

**Procés:**  
1. Obrim Wireshark i seleccionem la interfície de xarxa correcta (Ethernet).  
2. Apliquem un filtre `ssh` per veure només el tràfic SSH.  
3. Iniciem la captura i fem una connexió SSH des d’un client.

![Cerca de Wireshark al menú d'inici](/tasca05/img_T05/captura12.png)  
![Interfície principal de Wireshark](/tasca05/img_T05/captura13.png)  
![Configuració de proxy a Windows](/tasca05/img_T05/captura14.png)  
![Captura de tràfic SSH](/tasca05/img_T05/captura16.png)

**Anàlisi de la captura 16:**  
A la taula de Wireshark es veuen paquets entre `192.168.56.106` (servidor) i `10.0.2.15` (client).  
- **Protocol**: SSHv2 (versió 2, més segura).  
- **Info**: “Server: Encrypted packet” / “Client: Encrypted packet” → confirmen que el tràfic està xifrat.  
- **Ports**: el port origen/destinació és el 22 (estàndard SSH), tot i que a la captura no es veu directament perquè Wireshark agrupa sota “SSHv2”.  

Això demostra que la connexió és segura i que no es transmet informació en clar.

---

## 7. Connexió SSH des de Windows amb PowerShell  
A Windows podem utilitzar **PowerShell** (que ja inclou el client SSH) per connectar-nos al servidor Linux.

**Comanda utilitzada:**  
```powershell
ssh usuari@192.168.56.106
```

**Explicació:**  
Igual que a Linux, `ssh` seguit de l’usuari i la IP del servidor. La primera vegada et demanarà la contrasenya de l’usuari remot.

![Connexió SSH des de PowerShell](/tasca05/img_T05/captura15.png)

**Anàlisi de la captura:**  
Un cop acceptada la contrasenya, el servidor ens dóna la benvinguda i mostra informació del sistema (càrrega, ús de memòria, actualitzacions pendents). Això confirma que la connexió ha estat exitosa i que estem dins del servidor Ubuntu.

---

## 8. Autenticació amb claus SSH (més segura que contrasenya)  
Per millorar la seguretat, podem configurar autenticació amb claus públiques/privades. Així no caldrà introduir contrasenya cada vegada.

### 8.1 Generar les claus a Windows  
**Comanda utilitzada:**  
```powershell
ssh-keygen -t rsa
```

**Explicació:**  
- `ssh-keygen` és l’eina per generar parells de claus.  
- `-t rsa` especifica el tipus de clau (RSA).  
- Es generen dos fitxers a `C:\Users\usuari\.ssh\`:  
  - `id_rsa` (clau privada – NO la comparteixis mai).  
  - `id_rsa.pub` (clau pública – aquesta sí que la copiem al servidor).

![Generació de claus SSH a Windows](/tasca05/img_T05/captura17.png)  
![Llistat de claus generades](/tasca05/img_T05/captura18.png)

### 8.2 Copiar la clau pública al servidor Linux  
**Comanda utilitzada:**  
```powershell
scp .\.ssh\id_rsa.pub usuari@192.168.56.106:id_rsa.pub
```

**Explicació:**  
`scp` (Secure Copy) copia el fitxer de la clau pública al directori home del servidor remot.

![Còpia de la clau pública al servidor](/tasca05/img_T05/captura19.png)

### 8.3 Configurar la clau al servidor  
Des del servidor Linux, afegim la clau pública al fitxer `authorized_keys`.

**Comandes utilitzades:**  
```bash
mkdir .ssh
touch .ssh/authorized_keys
cat id_rsa.pub >> .ssh/authorized_keys
```

**Explicació:**  
- Creem el directori `.ssh` si no existeix.  
- Creem/actualitzem el fitxer `authorized_keys`.  
- Afegim el contingut de la clau pública al fitxer.  
Això permetrà que el client Windows es connecti sense contrasenya.

![Crear directori .ssh al servidor](/tasca05/img_T05/captura20.png)  
![Afegir la clau a authorized_keys](/tasca05/img_T05/captura21.png)

### 8.4 Provar la connexió sense contrasenya  
Ara, en connectar-nos des de Windows, no demanarà contrasenya (si la clau privada està ben configurada).

![Connexió SSH sense contrasenya](/tasca05/img_T05/captura22.png)

**Anàlisi de la captura:**  
Es connecta directament i mostra el missatge de benvinguda. Això confirma que l’autenticació per clau ha funcionat correctament.

---

## 📌 Resum final  
Amb aquests passos has après:  
1. **Configurar la xarxa** amb `netplan`.  
2. **Actualitzar el sistema** amb `apt`.  
3. **Configurar el servidor SSH** per accés segur.  
4. **Connectar-te des d’un client Linux** amb `ssh`.  
5. **Instal·lar i utilitzar Wireshark** per anàlisis de xarxa.  
6. **Connectar-te des de Windows** amb PowerShell.  
7. **Configurar autenticació amb claus SSH** per més seguretat i comoditat.  

Ara ja pots administrar servidors Linux tant des de clients Linux com Windows, amb autenticació segura i possibilitat de depurar la connexió amb Wireshark.

**Recorda:**  
- Les claus privades mai es comparteixen.  
- Els túnels SOCKS són útils per accedir a recursos interns de manera segura.  
- Sempre verifica el tràfic amb Wireshark si sospites de problemes de connexió.

Amb això ja tens la base per ser operatiu des del primer dia a la consultora. 🚀
