# ConnectLogs ASL3

![Version](https://img.shields.io/badge/version-1.0-blue.svg)
![License](https://img.shields.io/badge/license-GPLv3-green.svg)
![ASL3](https://img.shields.io/badge/AllStarLink-3-orange.svg)

<img src="https://flagcdn.com/w20/us.png" width="20"/> **[English](#english)** | <img src="https://flagcdn.com/w20/fr.png" width="20"/> **[Français](#français)**

---

<a name="english"></a>
## <img src="https://flagcdn.com/w20/us.png" width="30"/> English

Connection Logger and ASTDB updater for AllStarLink 3

### 📋 Description

ConnectLogs-asl3 is a connection logging system for AllStarLink 3.<br>
It is compatible with the **ConnectLogs application** as it supports Supermon formats.<br>

ConnectLogs-asl3 automatically manages:
- **Connection logging** for AllStar, EchoLink, IRLP
- **Automatic updates** of the node database (ASTDB)
- **Systemd service** with timer for periodic updates
- **Log rotation** via logrotate

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
wget https://github.com/CN8VX/connectlogs-asl3/releases/download/v1.0/connectlogs-asl3_1.0.deb
```

2. **Install the package**
```bash
sudo dpkg -i connectlogs-asl3_1.0.deb
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

#### Check connection logs

```bash
tail -f /var/log/asterisk/connectlog
```

Example output:
```
Sun Jan 12 10:30:45 UTC 2025 == 1999 Connected AllStar 2000 <=IN== (:)
Sun Jan 12 10:35:12 UTC 2025 == 1999 Disconnected AllStar 2000 <=IN== (:)
```

#### Check ASTDB service

```bash
# Timer status
systemctl status astdb.timer

# Update logs
cat /opt/logfils/astdb.log

# Manually test the update
sudo -u asterisk /usr/bin/php /opt/logfils/astdb.php
```

### ⚙️ Advanced Configuration

#### Add private nodes

Create the `/opt/logfils/privatenodes.txt` file:

```bash
sudo nano /opt/logfils/privatenodes.txt
```

Format (one node per line):
```
123456|CALLSIGN|Location|Frequency
123457|MYCALL|My City|145.500
```

Then restart the timer:
```bash
sudo systemctl restart astdb.timer
```

#### Change update frequency

Edit `/etc/systemd/system/astdb.timer`:

```bash
sudo nano /etc/systemd/system/astdb.timer
```

Modify the `OnUnitActiveSec` line (default: 6h):
```ini
OnUnitActiveSec=3h  # Update every 3 hours
```

Then reload:
```bash
sudo systemctl daemon-reload
sudo systemctl restart astdb.timer
```

### 🗑️ Uninstallation


Uninstall (keeps configuration files)
```bash
sudo apt remove connectlogs-asl3
```

Complete uninstall (removes everything)
```bash
sudo apt purge connectlogs-asl3
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
Sun Jan 12 10:30:45 UTC 2025 == 1999 Connected AllStar 2000 <=IN== (:)
Sun Jan 12 10:30:50 UTC 2025 == 1999 Connected EchoLink 123456 =OUT=> CALLSIGN [EchoLink 123456] (Location)
Sun Jan 12 10:31:00 UTC 2025 == 1999 Disconnected IRLP 8500 <=IN== 
```

### 🤝 Contribution

Contributions are welcome! Feel free to:

- 🐛 Report bugs via [Issues](https://github.com/CN8VX/connectlogs-asl3/issues)
- 💡 Suggest improvements
- 🔧 Submit Pull Requests

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

Système de journalisation des connexions et mise à jour ASTDB pour AllStarLink 3

### 📋 Description

ConnectLogs-asl3 est un système de journalisation des connexions pour AllStarLink 3.<br> 
Il est compatible avec **l'application ConnectLogs**, car elle prend en charge les formats Supermon.<br>

ConnectLogs-asl3 gère automatiquement :
- **Journalisation des connexions** AllStar, EchoLink, IRLP
- **Mise à jour automatique** de la base de données des nœuds (ASTDB)
- **Service systemd** avec timer pour les mises à jour périodiques
- **Rotation des logs** via logrotate

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
wget https://github.com/CN8VX/connectlogs-asl3/releases/download/v1.0/connectlogs-asl3_1.0.deb
```

2. **Installer le paquet**
```bash
sudo dpkg -i connectlogs-asl3_1.0.deb
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

#### Vérifier les logs de connexion

```bash
tail -f /var/log/asterisk/connectlog
```

Exemple de sortie :
```
Sun Jan 12 10:30:45 UTC 2025 == 1999 Connected AllStar 2000 <=IN== (:)
Sun Jan 12 10:35:12 UTC 2025 == 1999 Disconnected AllStar 2000 <=IN== (:)
```

#### Vérifier le service ASTDB

```bash
# Statut du timer
systemctl status astdb.timer

# Logs de mise à jour
cat /opt/logfils/astdb.log

# Tester manuellement la mise à jour
sudo -u asterisk /usr/bin/php /opt/logfils/astdb.php
```

### ⚙️ Configuration avancée

#### Ajouter des nœuds privés

Créez le fichier `/opt/logfils/privatenodes.txt` :

```bash
sudo nano /opt/logfils/privatenodes.txt
```

Format (un nœud par ligne) :
```
123456|CALLSIGN|Localisation|Fréquence
123457|MYCALL|Ma Ville|145.500
```

Puis redémarrez le timer :
```bash
sudo systemctl restart astdb.timer
```

#### Modifier la fréquence de mise à jour

Éditez `/etc/systemd/system/astdb.timer` :

```bash
sudo nano /etc/systemd/system/astdb.timer
```

Modifiez la ligne `OnUnitActiveSec` (par défaut: 6h) :
```ini
OnUnitActiveSec=3h  # Mise à jour toutes les 3 heures
```

Puis rechargez :
```bash
sudo systemctl daemon-reload
sudo systemctl restart astdb.timer
```

### 🗑️ Désinstallation


Désinstaller (garde les fichiers de configuration)
```bash
sudo apt remove connectlogs-asl3
```

Désinstaller complètement (supprime tout)
```bash
sudo apt purge connectlogs-asl3
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
DATE == NODE STATUS TYPE REMOTE_NODE DIRECTION INFO
```

Exemples :
```
Sun Jan 12 10:30:45 UTC 2025 == 1999 Connected AllStar 2000 <=IN== (:)
Sun Jan 12 10:30:50 UTC 2025 == 1999 Connected EchoLink 123456 =OUT=> CALLSIGN [EchoLink 123456] (Location)
Sun Jan 12 10:31:00 UTC 2025 == 1999 Disconnected IRLP 8500 <=IN== 
```

### 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :

- 🐛 Signaler des bugs via les [Issues](https://github.com/CN8VX/connectlogs-asl3/issues)
- 💡 Proposer des améliorations
- 🔧 Soumettre des Pull Requests

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
