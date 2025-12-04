# 📡 T05: Accés Remot - Connexió via SSH

## 🎯 Descripció de l'activitat
Aquesta tasca té com a objectiu configurar i utilitzar connexions SSH (Secure Shell) per administrar servidors de manera remota i segura. Com a consultors, és essencial saber connectar-nos tant des de clients Linux com Windows per gestionar sistemes que estan en CPDs o al núvol.

## 🚀 Objectius
- Configurar el servei SSH a un servidor Linux
- Establir connexions remotes des de diferents sistemes operatius
- Implementar autenticació amb claus públiques/privades
- Analitzar el tràfic de xarxa amb Wireshark
- Crear documentació per a nous membres de l'equip

## 🛠️ Competències treballades
- **RA6**: Gestió de mètodes d'accés remot
- Instal·lació i configuració de serveis d'accés remot
- Comprovació de funcionament de connexions SSH
- Administració remota entre sistemes diferents

## 📋 Continguts de la guia

### 1. Configuració bàsica
- Configuració de xarxa amb `netplan`
- Actualització del sistema Ubuntu
- Configuració del servidor SSH (`sshd_config`)

### 2. Connexions SSH bàsiques
- Connexió des de Linux amb terminal nativa
- Connexió des de Windows amb PowerShell
- Ús de túnel SOCKS per redirigir tràfic

### 3. Seguretat avançada
- Generació de claus SSH amb `ssh-keygen`
- Configuració d'autenticació sense contrasenya
- Gestió de claus públiques i privades

### 4. Anàlisi i monitorització
- Instal·lació i ús de Wireshark
- Captura i anàlisi de tràfic SSH
- Verificació de xifrat de connexions

## 📁 Estructura de fitxers
```
tasca05/
├── README.md                    # Aquest fitxer
├── guia_configuracio_ssh.md     # Guia completa pas a pas
└── img_T05/
    ├── captura1.png             # Configuració netplan
    ├── captura2.png             # Aplicació netplan
    ├── captura3.png             # Actualització sistema
    ├── captura4.png             # Configuració sshd (part 1)
    ├── captura5.png             # Configuració sshd (part 2)
    ├── captura6.png             # Sessió SSH oberta
    ├── captura7.png             # Primera connexió SSH
    ├── captura8.png             # Descàrrega Wireshark
    ├── captura9.png             # Instal·lador Wireshark
    ├── captura10.png            # Finalització instal·lació
    ├── captura11.png            # Wireshark instal·lat
    ├── captura12.png            # Cerca Wireshark
    ├── captura13.png            # Interfície Wireshark
    ├── captura14.png            # Configuració proxy
    ├── captura15.png            # Connexió SSH Windows
    ├── captura16.png            # Anàlisi tràfic Wireshark
    ├── captura17.png            # Generació claus SSH
    ├── captura18.png            # Llistat claus generades
    ├── captura19.png            # Còpia clau al servidor
    ├── captura20.png            # Creació directori .ssh
    ├── captura21.png            # Configuració authorized_keys
    └── captura22.png            # Connexió sense contrasenya
```

## 🔧 Comandes principals utilitzades

### Configuració servidor
```bash
sudo netplan apply
sudo apt update && sudo apt upgrade -y
sudo nano /etc/ssh/sshd_config
```

### Connexions SSH
```bash
# Des de Linux
ssh usuari@192.168.56.106
ssh -D 9876 usuari@192.168.56.106  # Amb túnel SOCKS

# Des de Windows (PowerShell)
ssh usuari@192.168.56.106
```

### Gestió de claus
```powershell
# Generar claus a Windows
ssh-keygen -t rsa

# Copiar clau pública al servidor
scp .\.ssh\id_rsa.pub usuari@192.168.56.106:id_rsa.pub
```

```bash
# Configurar clau al servidor
mkdir .ssh
touch .ssh/authorized_keys
cat id_rsa.pub >> .ssh/authorized_keys
```

## 📊 Resultats d'aprenentatge
Al finalitzar aquesta activitat, seràs capaç de:
- Configurar un servidor SSH segur
- Connectar-te remotament des de qualsevol sistema operatiu
- Implementar autenticació amb claus per millorar la seguretat
- Analitzar i depurar connexions de xarxa
- Documentar processos tècnics per a l'equip

## 👥 Destinataris
Aquesta guia està pensada per:
- Nous becaris de la consultora
- Tècnics que necessitin refrescar coneixements SSH
- Qualsevol persona que vulgui aprendre administració remota segura

## 📚 Recursos addicionals
- [Documentació oficial OpenSSH](https://www.openssh.com/)
- [Guia d'ús de Wireshark](https://www.wireshark.org/docs/)
- [Moodle 0227 Serveis de Xarxa - UD4.AA2](https://moodle.escoladeltreball.org/)

## ⚡ Consells ràpids
- **Sempre** utilitza claus SSH enlloc de contrasenyes
- Configura el fitxer `sshd_config` amb les opcions de seguretat adequades
- Prova les connexions abans de donar per finalitzada la configuració
- Documenta totes les IPs i configuracions utilitzades

---

**💡 Recorda**: L'accés remot segur és una habilitat crítica per a qualsevol administrador de sistemes. Amb aquesta guia, tens tot el necessari per començar a treballar de manera professional i segura.

**🚀 Estàs preparat per connectar-te al món!**
