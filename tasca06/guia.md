# Guia Tècnica: Configuració i Ús d'Escriptori Remot (RDP)

## Introducció

Hola! En aquesta guia t'explicaré com configurar i utilitzar l'accés remot gràfic mitjançant el protocol RDP (Remote Desktop Protocol). Això és super útil quan has d'ajudar algú amb un problema al seu ordinador i necessites veure la seva pantalla i controlar el seu equip des del teu. Ho farem tant amb Windows com amb Linux (Zorin OS).

## 1. Configuració del Servidor RDP a Windows 11

### Pas 1: Activar l'Escriptori Remot

Primer, al Windows que farem de servidor (l'ordinador al que ens volem connectar), hem d'activar l'escriptori remot:

![Configuració inicial escriptori remot](/tasca06/img_t06/captura1.png)

Anem a **Configuració > Sistema > Escriptori remot** i activem l'opció.

### Pas 2: Afegir Usuaris Permesos

Un cop activat, hem d'afegir quins usuaris podran connectar-se remotament:

![Afegir usuaris per accés remot](/tasca06/img_t06/captura2.png)

Fem clic a "Seleccionar usuaris" i afegim els comptes que necessitem. Els administradors sempre podran connectar-se, però podem afegir usuaris estàndard si volem.

### Pas 3: Obtenir la Informació de Connexió

Ara necessitem saber com connectar-nos a aquest equip:

![Informació de connexió del servidor](/tasca06/img_t06/captura3.png)

Apuntem el **nom de l'equip** (en aquest cas `DESKTOP-6R6TBC8`) o la seva adreça IP. També necessitarem un nom d'usuari i contrasenya vàlids d'aquest equip.

---

## 2. Configuració del Servidor RDP a Zorin OS (Linux)

### Pas 1: Actualitzar el Sistema

Abans de res, obrim una terminal i actualitzem el sistema:

```bash
sudo apt update && sudo apt upgrade -y
```

![Actualització del sistema Zorin](/tasca06/img_t06/captura4.png)

**Anàlisi:** Amb aquesta comanda actualitzem la llista de paquets i després actualitzem tots els paquets instal·lats. El `-y` fa que confirmi automàticament totes les preguntes durant l'actualització. És important tenir el sistema actualitzat abans d'instal·lar nous serveis.

### Pas 2: Activar Escriptori Remot

A Zorin OS, l'escriptori remot ja ve integrat. L'activem anant a:

![Configuració escriptori remot Zorin](/tasca06/img_t06/captura5.png)

**Configuració > Compartir > Escriptori Remot**

Aquí podem:
- Activar/desactivar l'escriptori remot
- Configurar l'accés amb contrasenya
- Veure el nom de l'equip i l'adreça de connexió

### Pas 3: Configurar la Connexió

![Detalls configuració remot Zorin](/tasca06/img_t06/captura6.png)

Configurem:
- **Nom de l'equip:** `usuari-VirtualBox`
- **Adreça de connexió:** `ms-rd://usuari-VirtualBox.local`
- **Usuari:** `usuari`
- **Contrasenya:** (la que vulguem)

També podem activar el protocol VNC legacy si ho necessitem per connectar-nos amb clients més antics.

---

## 3. Connexió des d'un Client Windows

### Pas 1: Obrir Client d'Escriptori Remot

Al client Windows (des d'on ens volem connectar), obrim l'aplicació "Connexió a escriptori remot":

![Client escriptori remot Windows](/tasca06/img_t06/captura7.png)

### Pas 2: Configurar la Connexió

![Configuració connexió client](/tasca06/img_t06/captura8.png)

Introduïm:
- **Equip:** El nom o IP del servidor (per exemple, `usuari-VirtualBox.local`)
- **Usuari:** El nom d'usuari del servidor

Podem guardar la configuració per fer-ho més ràpid la propera vegada.

### Pas 3: Establir la Connexió

En connectar-nos, ens demanarà les credencials:

![Autenticació connexió](/tasca06/img_t06/captura9.png)

Si és la primera vegada que ens connectem, ens pot sortir un avís de seguretat sobre el certificat. Si estem dins d'una xarxa de confiança, podem continuar.

---

## 4. Connexió des d'un Client Linux (Remmina)

### Pas 1: Instal·lar Remmina

Remmina és el client d'escriptori remot més popular per Linux. El podem trobar a la botiga d'aplicacions de Zorin o instal·lar-lo des de terminal:

```bash
sudo apt install remmina remmina-plugin-rdp
```

![Interfície Remmina](/tasca06/img_t06/captura10.png)

### Pas 2: Crear Nova Connexió

A Remmina, creem una nova connexió RDP:

![Nova connexió Remmina](/tasca06/img_t06/captura11.png)

Configurem:
- **Protocol:** RDP
- **Servidor:** Adreça del servidor
- **Usuari i contrasenya**
- Podem guardar la connexió per al futur

### Pas 3: Gestionar Certificats de Seguretat

![Alerta certificat seguretat](/tasca06/img_t06/captura12.png)

Quan ens connectem per primera vegada, Remmina ens avisarà sobre problemes amb el certificat de seguretat. Això és normal en connexions locals. Si confiem en el servidor, podem acceptar el certificat i marcar "No tornar a preguntar".

---

## 5. Connexió entre Diferents Sistemes

### Windows → Linux
![Connexió Windows a Linux](/tasca06/img_t06/captura13.png)

Utilitzem el client d'escriptori remot de Windows i ens connectem a l'adreça del servidor Linux. Recorda que al Linux hem d'haver activat prèviament l'escriptori remot.

### Linux → Windows
![Connexió Linux a Windows](/tasca06/img_t06/captura14.png)

Des de Remmina al Linux, ens connectem a l'adreça IP o nom del servidor Windows. Necessitem un usuari i contrasenya vàlids del Windows.

### Linux → Linux
També podem connectar-nos entre Linux si tots dos tenen el servei d'escriptori remot activat.

---

## 6. Verificació del Funcionament

Un cop connectats, hauríem de poder:

1. **Veure l'escriptori remot** del servidor
2. **Controlar ratolí i teclat** (si ho hem permès a la configuració)
3. **Transferir fitxers** en algunes configuracions
4. **Utilitzar els recursos** com si estiguéssim físicament davant de l'equip

![Escriptori remot connectat](/tasca06/img_t06/captura15.png)

**Anàlisi:** A la captura podem veure com des d'un client ens hem connectat correctament a un servidor Windows 11 i estem veient el seu escriptori. El client mostra que estem connectats al `DESKTOP-GR6TBC8.local` i podem interactuar amb tot el sistema com si hi estiguéssim físicament.

---

## Resolució de Problemes Comuns

### Problema 1: No puc connectar-me
- **Comprova** que l'escriptori remot està activat al servidor
- **Verifica** que el firewall permet el tràfic RDP (port 3389)
- **Assegura't** que l'usuari i contrasenya són correctes

### Problema 2: Error de certificat
- A l'entorn de prova, pots acceptar el certificat no vàlid
- A producció, configura certificats vàlids

### Problema 3: Connexió lenta
- Redueix la qualitat gràfica des de la configuració del client
- Assegura't d'estar a la mateixa xarxa local o tenir bona connexió

---

## Conclusions

Amb aquesta configuració, ja pots:
- ✅ Donar suport remot a usuaris Windows des de qualsevol lloc
- ✅ Administrar servidors Linux amb interfície gràfica
- ✅ Crear connexions creuades entre diferents sistemes operatius

Recorda que per entorns de producció has d'ajustar la seguretat: utilitzar VPNs, certificats vàlids, i limitar els accessos només als usuaris necessaris.

El RDP és una eina super poderosa per al suport tècnic, i ara que saps com configurar-ho tant a Windows com a Linux, pots ajudar a usuaris amb problemes sense necessitat d'estar físicament al seu costat! 💻🚀
