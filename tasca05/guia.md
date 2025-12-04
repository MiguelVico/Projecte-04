# 📡 T05: Accés Remot - Guia Completa de Configuració SSH

## 🎯 Descripció
Aquesta guia explica com configurar connexions SSH segures per administrar servidors remotament, tant des de clients Linux com Windows. És la documentació oficial per a nous becaris de la consultora.

---

## 1. Configuració inicial de xarxa i sistema

### 1.1 Configuració de xarxa amb Netplan
Abans de res, cal assegurar-nos que el servidor té una IP fixa o correctament configurada.

**Comanda:**
```bash
sudo netplan apply
```

**Explicació:**
Aplica la configuració de xarxa definida a `/etc/netplan/`. És com dir "activa els canvis que t'he dit".

![Configuració de xarxa amb netplan](/tasca05/img_t05/captura1.png)
*La IP està configurada amb DHCP4: true*

---

### 1.2 Actualització del sistema
Sempre actualitzem abans de configurar serveis nous.

**Comanda:**
```bash
sudo apt upgrade && sudo apt update -y
```

**Explicació:**
- `update`: actualitza la llista de paquets disponibles
- `upgrade`: actualitza els paquets instal·lats
- `-y`: diu "sí" automàticament a tot

![Actualització del sistema](/tasca05/img_t05/captura3.png)
*El sistema s'actualitza automàticament*

---

## 2. Instal·lació i configuració del servidor SSH

### 2.1 Instal·lació del servei SSH
**Comanda:**
```bash
sudo apt install openssh-server -y
```

**Explicació:**
Instal·la el paquet `openssh-server`, que ens permetrà acceptar connexions SSH d'altres màquines.

---

### 2.2 Configuració del fitxer sshd_config
**Comanda per editar:**
```bash
sudo nano /etc/ssh/sshd_config
```

**Canvis importants que fem:**
```bash
#PermitRootLogin prohibit-password  # Root no pot fer SSH amb contrasenya
#AllowUsers usuari                  # Només l'usuari 'usuari' pot connectar-se
#StrictModes yes                    # Comprova permisos dels fitxers clau
```

![Configuració de sshd_config](/tasca05/img_t05/captura4.png)
*Fitxer de configuració complet del SSH*

![Part de la configuració SSH](/tasca05/img_t05/captura5.png)
*Secció específica d'autenticació i usuaris*

**Explicació:**
- `PermitRootLogin prohibit-password`: incrementa seguretat, root només pot entrar amb claus
- `AllowUsers usuari`: limita els usuaris que poden connectar-se
- Reiniciem el servei després dels canvis: `sudo systemctl restart sshd`

---

### 2.3 Gestió de l'usuari root (CONFIGURACIÓ NOVA)

#### 2.3.1 Posar contrasenya a root
Tot i que no permetrem accés remot a root, necessitem tenir-lo configurat per emergències locals.

**Comanda:**
```bash
sudo passwd root
```

**Procés:**
```
[sudo] password for usuari: (introduïm la nostra contrasenya)
New password: (posem una contrasenya forta per a root)
Retype new password: (repetim la mateixa contrasenya)
```

**Explicació:**
Així tenim l'usuari root amb contrasenya per si necessitem accedir localment (per exemple, si hi ha problemes amb el nostre usuari normal).

#### 2.3.2 Verificació d'accés local vs remot (CONFIGURACIÓ NOVA)

**Test d'accés local (HA DE FUNCIONAR):**
```bash
su -
```
O bé:
```bash
sudo -i
```
*Haurem d'introduir la contrasenya de root que acabem de posar.*

**Test d'accés remot (HA DE FALLAR):**
```bash
ssh root@192.168.56.106
```

**Resultat esperat:**
```
root@192.168.56.106: Permission denied (publickey).
```

**Explicació:**
Aquesta prova demostra que:
1. ✅ **Root SÍ pot accedir localment** (per emergències)
2. ❌ **Root NO pot accedir remotament via SSH** (per seguretat)
3. ✅ La nostra configuració `PermitRootLogin prohibit-password` funciona correctament

---

## 3. Primera connexió SSH

### 3.1 Connexió des de Linux
**Comanda:**
```bash
ssh -D 9876 usuari@192.168.56.106
```

**Explicació:**
- `ssh`: comanda per connectar via Secure Shell
- `-D 9876`: crea un túnel SOCKS al port 9876 (més endavant l'explicarem)
- `usuari@192.168.56.106`: usuari i IP del servidor

![Primera connexió SSH](/tasca05/img_t05/captura7.png)

**Anàlisi de la captura:**
El sistema ens diu: "The authenticity of host '192.168.56.106 (192.168.56.106)' can't be established."
- Això és **normal** en la primera connexió
- El servidor ens mostra la seva empremta digital (fingerprint) SHA256
- Demana confirmació per continuar (escrivim `yes`)
- Aquesta empremta es guarda a `~/.ssh/known_hosts` per futures connexions

---

## 4. Connexió des de Windows

### 4.1 Connexió bàsica amb PowerShell
**Comanda:**
```powershell
ssh usuari@192.168.56.106
```

![Connexió SSH des de PowerShell](/tasca05/img_t05/captura15.png)

**Anàlisi:**
- Demana la contrasenya de l'usuari remot
- Un cop dins, mostra informació del sistema (Ubuntu 24.04.3 LTS)
- Veiem stats del sistema: càrrega, memòria, actualitzacions pendents
- Confirmació: estem dins del servidor remot

---

## 5. Configuració de túnels SSH

### 5.1 Túnel SOCKS
**Comanda:**
```bash
ssh -D 9876 usuari@192.168.56.106
```

**Explicació:**
Crea un túnel SOCKS al port local 9876 que redirigeix tot el tràfic a través del servidor SSH.

**Configuració al navegador (Windows):**
1. Anar a Configuració → Xarxa → Configuració del proxy
2. Posar adreça: `127.0.0.1` (localhost)
3. Port: `9876`
4. Tipus: SOCKS v5

![Configuració de proxy a Windows](/tasca05/img_t05/captura14.png)
*Així tot el nostre tràfic web passa pel túnel segur*

---

### 5.2 Anàlisi del túnel amb Wireshark (CONFIGURACIÓ NOVA)

#### 5.2.1 Instal·lació de Wireshark
**Descàrreguem des del lloc oficial:**

![Descàrrega de Wireshark](/tasca05/img_t05/captura8.png)
![Instal·lador de Wireshark](/tasca05/img_t05/captura9.png)
![Completant la instal·lació](/tasca05/img_t05/captura10.png)
![Instal·lació finalitzada](/tasca05/img_t05/captura11.png)

**Procés:**
1. Baixem la versió 4.6.2 per Windows
2. Executem l'instal·lador
3. Acceptem tots els passos (inclòs Npcap per capturar paquets)
4. Finalitzem la instal·lació

#### 5.2.2 Captura i anàlisi de tràfic

**Passos:**
1. Obrim Wireshark
2. Seleccionem la interfície de xarxa correcta
3. Filtrem per `ssh` o `port 22`
4. Iniciem la captura
5. Fem una connexió SSH

![Cerca de Wireshark al menú d'inici](/tasca05/img_t05/captura12.png)
![Interfície principal de Wireshark](/tasca05/img_t05/captura13.png)

![Captura de tràfic SSH](/tasca05/img_t05/captura16.png)

**Anàlisi de la captura:**
A la taula veiem:
- **Origen/Destinació**: `192.168.56.106` ↔ `10.0.2.15`
- **Protocol**: `SSHv2` (versió 2, la més segura)
- **Info**: "Server: Encrypted packet" / "Client: Encrypted packet"
- **Conclusió**: Tot el tràfic està xifrat, no es pot llegir el contingut

**Per què és important:**
- Verifiquem que la connexió és realment segura
- Confirmem que es fa servir SSHv2 (no l'antiga versió 1)
- Veiem que els paquets són molt petits (només control, no dades grans)

---

## 6. Transferència d'arxius amb SCP

### 6.1 Enviar arxiu del client al servidor
**Comanda des de Windows:**
```powershell
scp .\.ssh\id_rsa.pub usuari@192.168.56.106:id_rsa.pub
```

![Còpia de la clau pública al servidor](/tasca05/img_t05/captura19.png)

**Explicació:**
- `scp`: Secure Copy, com el `cp` però a través de SSH
- Transfereix l'arxiu de manera segura (xifrat)
- S'envia al directori home de l'usuari remot

---

## 7. Autenticació amb claus SSH

### 7.1 Generació de claus a Windows
**Comanda:**
```powershell
ssh-keygen -t rsa
```

![Generació de claus SSH a Windows](/tasca05/img_t05/captura17.png)

**Procés:**
1. Demana on guardar la clau (per defecte `C:\Users\usuari\.ssh\id_rsa`)
2. Demana una passphrase (opcional, per més seguretat)
3. Es generen dos fitxers:
   - `id_rsa` → clau PRIVADA (NO compartir mai)
   - `id_rsa.pub` → clau PÚBLICA (sí que es pot compartir)

![Llistat de claus generades](/tasca05/img_t05/captura18.png)
*Veiem els dos fitxers creats: la privada (2610 bytes) i la pública (577 bytes)*

---

### 7.2 Configuració de la clau al servidor

#### 7.2.1 Crear directori .ssh
**Comanda al servidor:**
```bash
mkdir .ssh
```

![Crear directori .ssh al servidor](/tasca05/img_t05/captura20.png)

#### 7.2.2 Afegir la clau pública
**Comandes al servidor:**
```bash
touch .ssh/authorized_keys
cat id_rsa.pub >> .ssh/authorized_keys
```

![Afegir la clau a authorized_keys](/tasca05/img_t05/captura21.png)

**Explicació:**
1. Creem el fitxer `authorized_keys` si no existeix
2. Afegim el contingut de la nostra clau pública al final del fitxer
3. Això diu al servidor: "Accepta connexions d'aquesta clau"

---

### 7.3 Prova de connexió sense contrasenya
**Comanda:**
```powershell
ssh usuari@192.168.56.106
```

![Connexió SSH sense contrasenya](/tasca05/img_t05/captura22.png)

**Resultat:**
- ✅ No demana contrasenya
- ✅ Entra directament al servidor
- ✅ Mostra el missatge de benvinguda
- ✅ Confirma que l'autenticació per clau funciona

---

## 8. Resum i bones pràctiques

### ✅ **Tot el que hem après:**
1. **Configuració de xarxa** amb `netplan`
2. **Instal·lació i configuració SSH** amb seguretat
3. **Gestió d'usuaris**: root local sí, remot no
4. **Connexions bàsiques** des de Linux i Windows
5. **Túnels SOCKS** per tràfic segur
6. **Anàlisi amb Wireshark** per verificar xifrat
7. **Transferència segura** amb `scp`
8. **Autenticació amb claus** (més segura que contrasenyes)

### 🔐 **Consells de seguretat:**
- **Mai** compartiu les claus privades
- **Sempre** feu servir claus enlloc de contrasenyes quan es pugui
- **Reviseu** regularment el fitxer `authorized_keys`
- **Desactiveu** accés root remot (`PermitRootLogin no` o `prohibit-password`)
- **Limiteu** els usuaris que poden connectar (`AllowUsers`)

### 🚨 **Verificacions obligatòries abans de donar per fet:**
1. Root pot accedir localment? → `su -` (ha de funcionar)
2. Root pot accedir remotament? → `ssh root@IP` (ha de fallar)
3. Les connexions normals funcionen? → `ssh usuari@IP` (ha de funcionar)
4. El tràfic està xifrat? → Verificar amb Wireshark

---

## 📋 **Llista de comprovació final**

| Tasca | Comanda | Resultat esperat | ✔ |
|-------|---------|-----------------|---|
| Instal·lació SSH | `sudo apt install openssh-server` | Servei instal·lat | |
| Configuració sshd | `sudo nano /etc/ssh/sshd_config` | Opcions de seguretat activades | |
| Password root | `sudo passwd root` | Contrasenya configurada | |
| Accés local root | `su -` | Permís concedit | |
| Accés remot root | `ssh root@IP` | Permission denied | |
| Primera connexió | `ssh usuari@IP` | Pregunta per fingerprint | |
| Túnel SOCKS | `ssh -D 9876 usuari@IP` | Connexió establerta | |
| Transferència arxiu | `scp arxiu usuari@IP:` | Arxius transferits | |
| Generació claus | `ssh-keygen -t rsa` | Claus creades | |
| Autenticació sense password | `ssh usuari@IP` | Accés directe | |

---

## 🎓 **Per acabar**

Ara ja tens tot el necessari per:
- **Connectar-te** a qualsevol servidor de la consultora
- **Fer-ho de manera segura** (xifrat + autenticació amb claus)
- **Depurar problemes** amb Wireshark
- **Transferir arxius** de forma segura
- **Configurar nous sistemes** amb les mateixes mesures de seguretat

**Recorda:** Cada cop que configurem un servidor nou, seguim **exactament** els mateixos passos. La consistència és clau per a la seguretat.

**Benvingut/da a l'equip!** 🚀
