1. Ubuntu update

sudo apt update
sudo apt full-upgrade

2. Zeitzone einstellen


sudo dpkg-reconfigure tzdata


3. Neuer Benutzer


adduser <yournewuser>

rechte geben:

usermod -aG sudo <yournewuser>

prüfen (Soll Ergebnis: root):

su - yournewuser
sudo su
whoami

4. RSA Paar erstellen

puttygen.exe öffnen

Einstellung:
Type of key to generate= RSA
number of bits in a generated key= 2048

Kommentar und Passwort setzten. Generate dann abspeichern.


5. RSA in ubuntu einstellen

Nicht als sudo su ausführen!

im home verzeichnis ordner erstellen:
mkdir .ssh  

Datei erstellen 

sudo nano authorized_keys

und schlüssel rein kopieren
ssh-rsa …..




Hier RSA einstellen:
sudo nano /etc/ssh/sshd_config 


Erst Testen dann Passwort login deaktivieren:
sudo su

grep -q "^PasswordAuthentication" /etc/ssh/sshd_config && sed -i 's/^PasswordAuthentication.*/PasswordAuthentication no/g' /etc/ssh/sshd_config || echo "PasswordAuthentication no" >> /etc/ssh/sshd_config

systemctl restart sshd


6. Root deaktivieren

sed -i 's/^PermitRootLogin.*/PermitRootLogin no/g' /etc/ssh/sshd_config

sudo


7. Firewall
Installieren:
sudo apt-get install ufw

Status:
sudo ufw status

Ipv6 auf yes setzen:
sudo nano /etc/default/ufw


Einstellen:

sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow ssh
sudo ufw allow 22/tcp

sudo ufw disable
sudo ufw enable
sudo ufw status


8. SSH Port ändern

In Firewall freigeben:
sudo ufw allow 5522/tcp

und testen:
sudo ufw status

SSH Port in Datei auf 5522 ändern:
sudo nano /etc/ssh/sshd_config

Neustart:
sudo systemctl restart ssh

Check:
ss -an | grep 5522

Check:
lsof -Pni|grep sshd

Fenster offen lassen und mit neuem Fenster testen
Wenn geht:

sudo ufw deny 22/tcp


