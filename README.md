# Logfils for ASL3

![Version](https://img.shields.io/badge/version-1.0-blue.svg)
![License](https://img.shields.io/badge/license-GPLv3-green.svg)
![ASL3](https://img.shields.io/badge/AllStarLink-3-orange.svg)

<img src="https://flagcdn.com/w20/us.png" width="20"/> **[English](#english)** | <img src="https://flagcdn.com/w20/fr.png" width="20"/> **[Français](#français)**

---

<a name="english"></a>
## <img src="https://flagcdn.com/w20/us.png" width="30"/> English

Connection Logger and ASTDB updater for AllStarLink 3

### 📋 Description

**Logfils-asl3** is a connection logging system for AllStarLink 3.<br>
It is compatible with the **[ConnectLogs application](https://github.com/CN8VX/ConnectLogs-for-Allmon3-and-Supermon)** as it supports Supermon formats.<br>

**Logfils-asl3** automatically manages:
- **Connection logging** for AllStar, EchoLink, IRLP
- **Automatic updates** of the node database (ASTDB)
- **Systemd service** with timer for periodic updates
- **Log rotation** via logrotate

### ✨ What's new in v2.0

- ✅ Native ASL3 detection (`asl3-update-nodelist.service`)
- ✅ Unified ASTDB path: `/var/lib/asterisk/astdb.txt`
- ✅ Smart install / update detection with confirmation prompt
- ✅ IP address displayed on connect: `(71.199.168.192)` / `()`
- ✅ Smart uninstall: only removes what was installed by logfils
- ✅ `astdb.txt` preserved if it pre-existed before installation
- ✅ Full English interface

### ✨ Features

- ✅ Automatic detection of connection type (AllStar/EchoLink/IRLP)
- ✅ Connection direction (IN/OUT)
- ✅ Automatically updated node database
- ✅ Private node support
- ✅ Supermon format compatible
- ✅ Automatic log rotation
- ✅ Simple installation via .deb package

### 📦 Installation

#### Prerequisites

- AllStarLink 3 installed and configured
- PHP CLI installed
- Root access

#### Package Installation

1. **Download the package**
```bash
cd /tmp
wget https://github.com/CN8VX/logfils-asl3/releases/download/v2.0/logfils-asl3_2.0.deb
```

2. **Install the package**
```bash
sudo dpkg -i logfils-asl3_2.0.deb
```

3. **If needed, resolve dependencies**
```bash
sudo apt-get install -f
```

#### AllStarLink 3 Configuration

Edit the `/etc/asterisk/rpt.conf` file and add the following lines to your node section:

```ini
;;;;;;;;;;;;;;;;;;; Your node settings here ;;;;;;;;;;;;;;;;;;;
[YOUR_NODE_NUMBER]  ; Example: [1999]

connpgm = /opt/logfils/smlogger 1
discpgm = /opt/logfils/smlogger 0

; ... rest of your configuration ...
```

#### Service Restart

```bash
sudo systemctl restart asterisk
```

### 🔍 Verification

### Connection and Disconnection Test

Perform a test by connecting to, then disconnecting from, one or more nodes.

#### Check connection logs

```bash
tail -f /var/log/asterisk/connectlog
```

Example output:
```
Sat Mar 28 13:12:12 +01 2026 == 1999 Connected AllStar 602810 =OUT=> (71.199.168.192)
Sat Mar 28 13:12:40 +01 2026 == 1999 Disconnected AllStar 602810 =v= ()
```

### 🗑️ Uninstallation


Uninstall (keeps configuration files)
```bash
sudo apt remove logfils-asl3
```

Complete uninstall (removes everything)
```bash
sudo apt purge logfils-asl3
```

### 🐛 Troubleshooting

#### Logger not working

1. Check configuration in `rpt.conf`:
```bash
grep -A5 "^\[YOUR_NODE\]" /etc/asterisk/rpt.conf | grep -E "connpgm|discpgm"
```

2. Check permissions:
```bash
ls -la /opt/logfils/smlogger
# Should be: -rwxr-x--- asterisk adm
```

3. Test manually:
```bash
/opt/logfils/smlogger 1 YOUR_NODE 2000
tail -1 /var/log/asterisk/connectlog
```

#### Database not updating

1. Check the timer:
```bash
systemctl status astdb.timer
systemctl list-timers | grep astdb
```

2. Check logs:
```bash
cat /opt/logfils/astdb.log
journalctl -u astdb.service
```

3. Test manually:
```bash
sudo -u asterisk /usr/bin/php /opt/logfils/astdb.php
```

#### Empty connectlog file

1. Check permissions:
```bash
ls -la /var/log/asterisk/connectlog
# Should be: -rw-rw-r-- asterisk adm
```

2. Create the file if necessary:
```bash
sudo touch /var/log/asterisk/connectlog
sudo chown asterisk:adm /var/log/asterisk/connectlog
sudo chmod 664 /var/log/asterisk/connectlog
```

### 📝 Log Format

The log format is Supermon compatible:

```
DATE == NODE STATUS TYPE REMOTE_NODE DIRECTION INFO
```

Examples:
```
Sat Mar 28 13:12:12 +01 2026 == 1999 Connected    AllStar 602810 =OUT=> (71.199.168.192)
Sat Mar 28 13:12:40 +01 2026 == 1999 Disconnected AllStar 602810 =v=   ()
Sun Jan 12 10:30:50 UTC 2025 == 1999   Connected    EchoLink 123456 =OUT=> CALLSIGN [EchoLink 123456] (Location) 
```

### 📄 License

This project is developed by [CN8VX](https://www.qrz.com/db/CN8VX) under **GNU General Public License v3.0**.

### 👤 Author

**CN8VX**
- Website: [dmr-maroc.com](https://www.dmr-maroc.com)
- GitHub: [@CN8VX](https://github.com/CN8VX)
- 📧 **Email**: [cn8vx.ma@gmail.com](mailto:cn8vx.ma@gmail.com)

#### 🤝 Support and Suggestions

All questions, issues or suggestions are welcome! Feel free to:
- Report bugs
- Suggest improvements
- Share your feedback

#### 📞 Support

For any questions or issues:
- Check the [Allmon3 documentation](https://github.com/AllStarLink/allmon3)
- 📧 **Email**: [cn8vx.ma@gmail.com](mailto:cn8vx.ma@gmail.com)

---

**73 from [CN8VX](https://www.qrz.com/db/CN8VX)** 📻

*If you like this project, don't hesitate to give it a ⭐ on the repository!*

---

<a name="français"></a>
## <img src="https://flagcdn.com/w20/fr.png" width="30"/> Français

# Logfils pour ASL3

Système de journalisation des connexions et mise à jour ASTDB pour AllStarLink 3

### 📋 Description

**Logfils-asl3** est un système de journalisation des connexions pour AllStarLink 3.<br> 
Il est compatible avec **[l'application ConnectLogs](https://github.com/CN8VX/ConnectLogs-for-Allmon3-and-Supermon?tab=readme-ov-file#français)**, car elle prend en charge les formats Supermon.<br>

**Logfils-asl3** gère automatiquement :
- **Journalisation des connexions** AllStar, EchoLink, IRLP
- **Mise à jour automatique** de la base de données des nœuds (ASTDB)
- **Service systemd** avec timer pour les mises à jour périodiques
- **Rotation des logs** via logrotate

### ✨ Nouveautés v2.0

- ✅ Détection du service ASL3 natif (`asl3-update-nodelist.service`)
- ✅ Chemin ASTDB unifié : `/var/lib/asterisk/astdb.txt`
- ✅ Détection intelligente installation / mise à jour avec confirmation
- ✅ Affichage IP à la connexion : `(71.199.168.192)` / `()`
- ✅ Désinstallation intelligente : supprime uniquement ce qui a été installé
- ✅ `astdb.txt` conservé s'il existait avant l'installation
- ✅ Interface entièrement en anglais

### ✨ Fonctionnalités

- ✅ Détection automatique du type de connexion (AllStar/EchoLink/IRLP)
- ✅ Sens de connexion (IN/OUT)
- ✅ Base de données des nœuds mise à jour automatiquement
- ✅ Support des nœuds privés
- ✅ Compatible format Supermon
- ✅ Rotation automatique des logs
- ✅ Installation simple via paquet .deb

### 📦 Installation

#### Prérequis

- AllStarLink 3 installé et configuré
- PHP CLI installé
- Accès root

#### Installation du paquet

1. **Télécharger le paquet**
```bash
cd /tmp
wget https://github.com/CN8VX/logfils-asl3/releases/download/v2.0/logfils-asl3_2.0.deb
```

2. **Installer le paquet**
```bash
sudo dpkg -i logfils-asl3_2.0.deb
```

3. **Si besoin, résoudre les dépendances**
```bash
sudo apt-get install -f
```

#### Configuration AllStarLink 3

Éditez le fichier `/etc/asterisk/rpt.conf` et ajoutez les lignes suivantes dans la section de votre nœud :

```ini
;;;;;;;;;;;;;;;;;;; Your node settings here ;;;;;;;;;;;;;;;;;;;
[VOTRE_NUMERO_NODE]  ; Exemple: [1999]

connpgm = /opt/logfils/smlogger 1
discpgm = /opt/logfils/smlogger 0

; ... reste de votre configuration ...
```

#### Redémarrage du service

```bash
sudo systemctl restart asterisk
```

### 🔍 Vérification

### Test de connexion et de déconnexion

Effectuez un test en vous connectant, puis en vous déconnectant d’un ou de plusieurs nœuds.

#### Vérifier les logs de connexion

```bash
tail -f /var/log/asterisk/connectlog
```

Exemple de sortie :
```
Sat Mar 28 13:12:12 +01 2026 == 1999 Connected AllStar 602810 =OUT=> (71.199.168.192)
Sat Mar 28 13:12:40 +01 2026 == 1999 Disconnected AllStar 602810 =v= ()
```

### 🗑️ Désinstallation


Désinstaller (garde les fichiers de configuration)
```bash
sudo apt remove logfils-asl3
```

Désinstaller complètement (supprime tout)
```bash
sudo apt purge logfils-asl3
```

### 🐛 Dépannage

#### Le logger ne fonctionne pas

1. Vérifiez la configuration dans `rpt.conf` :
```bash
grep -A5 "^\[VOTRE_NODE\]" /etc/asterisk/rpt.conf | grep -E "connpgm|discpgm"
```

2. Vérifiez les permissions :
```bash
ls -la /opt/logfils/smlogger
# Doit être : -rwxr-x--- asterisk adm
```

3. Testez manuellement :
```bash
/opt/logfils/smlogger 1 VOTRE_NODE 2000
tail -1 /var/log/asterisk/connectlog
```

#### La base de données ne se met pas à jour

1. Vérifiez le timer :
```bash
systemctl status astdb.timer
systemctl list-timers | grep astdb
```

2. Vérifiez les logs :
```bash
cat /opt/logfils/astdb.log
journalctl -u astdb.service
```

3. Testez manuellement :
```bash
sudo -u asterisk /usr/bin/php /opt/logfils/astdb.php
```

#### Fichier connectlog vide

1. Vérifiez les permissions :
```bash
ls -la /var/log/asterisk/connectlog
# Doit être : -rw-rw-r-- asterisk adm
```

2. Créez le fichier si nécessaire :
```bash
sudo touch /var/log/asterisk/connectlog
sudo chown asterisk:adm /var/log/asterisk/connectlog
sudo chmod 664 /var/log/asterisk/connectlog
```

### 📝 Format des logs

Le format de log est compatible Supermon :

```
DATE == NODE STATUS TYPE REMOTE_NODE DIRECTION (IP)
```

Exemples :
```
Sat Mar 28 13:12:12 +01 2026 == 1999 Connected    AllStar 602810 =OUT=> (71.199.168.192)
Sat Mar 28 13:12:40 +01 2026 == 1999 Disconnected AllStar 602810 =v=   ()
Sun Jan 12 10:30:50 UTC 2025 == 1999   Connected    EchoLink 123456 =OUT=> CALLSIGN [EchoLink 123456] (Location)
```

### 📄 Licence

Ce projet est développé par [CN8VX](https://www.qrz.com/db/CN8VX) sous licence **GNU General Public License v3.0**.

### 👤 Auteur

**CN8VX**
- Website: [dmr-maroc.com](https://www.dmr-maroc.com)
- GitHub: [@CN8VX](https://github.com/CN8VX)
- 📧 **Email** : [cn8vx.ma@gmail.com](mailto:cn8vx.ma@gmail.com)

#### 🤝 Support et Suggestions

Toutes questions, problèmes ou suggestions sont les bienvenus ! N'hésitez pas à :
- Signaler des bugs
- Proposer des améliorations
- Partager vos suggestions

#### 📞 Support

Pour toute question ou problème :
- Consultez la [documentation Allmon3](https://github.com/AllStarLink/allmon3)
- 📧 **Email** : [cn8vx.ma@gmail.com](mailto:cn8vx.ma@gmail.com)

---

**73 de [CN8VX](https://www.qrz.com/db/CN8VX)** 📻

*Si vous aimez ce projet, n'hésitez pas à mettre une ⭐ sur le dépôt !*
