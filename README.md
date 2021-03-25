# M300 Dokumentation (Markdown)

## Einleitung

In meinem Markdown halte ich fest, was ich neues im Modul 300 gelernt habe und wie ich die vorgegebenen Aufgaben gelöst habe. Dies bedeutet aber auch, dass dies auch Fehlerbehebungen oder Umwege beinhaltet um zur optimalen Lösung zu kommen.



## Inhaltsverzeichnis

* [10 Toolumgebung](#10-toolumgebung) 
* [20 Infrastruktur-Automatisierung](#20---infrastruktur-automatisierung)
* [25 Sicherheit](#25-Sicherheit)
* [30 Container](#30-Container)
* [35 Sicherheit](#35-Sicherheit)
* [40 Kubernetes (k8s)](#40-Kubernetes-(k8s))
* [80 Ergänzungen zu den Unterlagen](#80-ergänzungen-zu-den-unterlagen)


## 10 Toolumgebung
## 01 - Github Account

Als erstes wird ein Github Account benötigt den man ganz einfach im www.github.com erstellen kann. 



### Account erstellen
Folgendes Schritte müssen beachtet werdenn:

1. Auf www.github.com ein Account erstellen.
2. Anschliessend muss die Email bestätigt werden und anschliessend kann man sich wieder anmelden. 

### Repository erstellen

Damit man die Dokumentation festhalen kann muss man im Repository eine ReadMe Datei erstellen.. In diese Datei gehört dann die Dokumentation.

1. Anmelden unter www.github.com
2. Auf der Startseite dann start a project anwählen
3. Unter Repositoryname einen Namen definieren wie zum Beispiel M300-Services
4. Nun kann festgelegt werden wer alles auf dieses Repository schauen / Dinge bearbeiten kann. Wie in der Anleitung beschrieben wird diese Einstellung auf Public belassen.
5. Anschliessend muss nur noch das Häkchen bei create README File gesetzt werden und schon kann das Repository erstellt werden.

### SSH-Key erstellen

1. Terminal Bash öffnen
2. Den Befehl eingeben mit der Emailadresse des Github Accounts:

```
 $ ssh-keygen -t rsa -b 4096 -C 'DEINE EMAILADRESSE' 
```

3. Danach wird ein Key erstellt und man muss mit Enter bestätigen.

4. Nun erstellt man ein Passwort.

### SSH-Key hinzufügen

1. Unter www.github.com anmelden
2. Auf Benutzerkonto klicken und die Settings aufrufen
3. Unter den Menübereichen auf der linken Seite zum Abschnitt SSH und GPG keys wechseln
4. Auf New SSH Key klicken
5. Im Formular unter Title eine Bezeichnung vergeben (z.B. MB SSH-Key)
6. Den zuvor kopierten Key mit CTRL + V einfügen und auf Add SSH key klicken
7. Der Schlüssel (SSH-Key) sollte nun in der übergeordneten Liste auftauchen


## 02 - Git Client

Jetzt muss der Git Client installiert werden. Dieser ermöglicht uns, Cloud-Repositories zu klonen, zu pullen (herunteraden) oder ein lokales Repository zu pushen (hochladen).

Hierzu müssen folgende Schritte durchgeführt werden:

1. Git Client installieren
2. Konfigurieren des Git Clients

```
$ git config --global user.name <username>
```
```
$ git config --global user.email <e-mail>
```

3. Konfiguration ist abgeschlossen

### Repository klonen

Mit dem folgenden Befehl kann das Repository geklont werden:

```
$ git clone https://github.com/kerimaltintas/M300-Services/
```

Mit diesem Befehl kann der Status angezeigt werdenk, dies ist nützlich zum anzeigen ob das Repository aktuell ist, oder wenn Probleme gibt beim Changes pushen:
```
$ git pull
```

Mit dem folgenden Befehl werden Geänderte Datei(en) werden rot aufgelistet:

```
 $ git status
```


### Repository herunteraden

Dieser Befehl ist wichtig, damit die Änderungen lokal gemacht werden können und anschliessen einfach auf Github hochgeladen werden kömmem. Dazu hier die Schritte: 

1. Terminal Bash öffnen
2. Ordner im gewünschtenn Verzeichnis erstellen
3. Repository mit SSH klonen:

```
 $ git clone git@github.com:kerimaltintas/M300-Services.git
```

### Repository pushen (hochladen)

1. Terminal Bash öffnen
2. Zum Verzeichnis gehen des repository
3. Dateien dem Upload hinzufügen:

```
$ git add -a
```
Upload wird "commited" > Kommentar zu Dokumentationszwecken ist dafür notwendig

4. Upload commiten:

Dieser Schritt soll zukünftig mit dem Markdown Editor gemacht werden, aber für den Moment wäre dies der Schritt um ein Update zu commiten:
```
 $ git commit -m "Mein Kommentar"
```


1. Zum Schluss noch pushen, mit diesem Befehl werden die Changes endgültig in das Github Repository hochgeladen:
```
$ git push
```



## 03 - VirtualBox
Hier geht es darum das meine eine lauffähige Linuxmaschine mit einem Hyperviser Typ zwei in mienem Fall Virtualbox erstellt. Dies habe ich mit meinem IT-Kenntnissen aus den vergangenen Jahren hinbekommen. 





## 04 - Vagrant

Wenn man schon einmal selber die VM's erstellt hat, dann weiss man für mehrere braucht man lange. Für eine schnellere Variante gibt es Vagrant. Mit dem können VM's automatisch erstellt werden mit nur einem kurzen Code.

#### Erste VM mit Vagrant aufsetzen

Im gewünschten Verzeichnis kann man mit einer Zeile, die VM erzeugen:

Mit diesem Befehl wird das Vagrant File erstellt, welches notwendig ist um danach die VMs von Vagrant auf zu erstellen:
```
$ vagrant init ubuntu/xenial64
```

Nach einiger Wartezeit sollte der Befehl erfolgreich durchgelaufen sein, danach kann die VM mit dem folgendem Befehl gestartet werden:
```
$ vagrant up --provider virtualbox
````
### Netzwerkplan





#### Automatisierten Webserver aufsetzen

Folgendes Szenario, man ist ein Hosting Unternehemn und man möchte, konstant VMs laufen haben, auf den das gleiche läuft. Natürlich könnte man dies per Hand machen, viel schneller geht dies jedoch wenn man ein Code hat, welches jeweils immer eine VM erstellt wenn man dieses ausführt.

In meinem Fall konnte ich automatisiert einen Webserver installieren, ausserdem habe ich darauf geachtet dass ich das Vagrantfile so anpasse, dass ich auch einen schönen VM Namen habe, natürlich müsste man dies dann auch im Linux anpassen, aber das ist dann eine reinse Config Datei Erweitertungs Sache.

Dafür wurde zuerst ein neuer Ordner erstellt, in dem ich die Apache Tests durchführen werde.

Danach wird das der folgende Code als Vagrantfile erstellt:

```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"
config.vm.provider "virtualbox" do |vb|
   vb.name = "M300WEB01"
  vb.memory = "512"
end
config.vm.provision "shell", inline: <<-SHELL
  # Packages vom lokalen Server holen
  # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list
  sudo apt-get update
  sudo apt-get -y install apache2
SHELL
end
```

Mit diesem Code konnte ich die Linux Maschine M300WEB01 erstellen, ein test über den Browser über localhost:8080 hat gezeigt, dass der Webserver funktioniert:

## 05 - Visual Studio Code

Einige der Klasse haben sich dazu entschieden nicht mit Visual Studio Code zu arbeiten, ich hingegen mag es mit Visual Studio Code zu arbeiten, demanch wurde alles gemäss der Anleitung installiert das beinhaltet folgende Schritte:

Zuerst wurden folgenden Extensions installiert:

* Markdown All in One (von Yu Zhang)
* Vagrant Extension (von Marco Stanzi)
* vscode-pdf Extension (von tomiko1207)

Nun muss die folgende Datei bearbeitet werden:  `File` > `Preferences` > `Settings` (`Ctrl` + `,`) auf `Open setting.kerimaltintas`, nun sollte theoretisch ein Fenster sich öffnen. Dieses noch leere Fenster wurde mit dem folgendem Code erweitert:

```
       // Konfiguriert die Globmuster zum Ausschließen von Dateien und Ordnern.
      "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/.vagrant": true,
        "**/.DS_Store": true
      },
```
## 20 - Infrastruktur-Automatisierung



## 25 Sicherheit

## 01 - Firewall

Eine Firewall ist ein Sicherungssystem, das ein Rechnernetz oder einen einzelnen Computer vor unerwünschten Netzwerkzugriffen schützt und ist weiter gefasst auch ein Teilaspekt eines Sicherheitskonzepts.

Mit einem vagrantfile wurde eine virtuelle Maschine erstellt, die Firewall-Regeln hat.

````
Vagrant.configure("2") do |config|
config.vm.box = "debian/buster64"
config.vm.provider "virtualbox" do |vb|
vb.name="M300SEC01"
end
config.vm.provision "shell", inline: <<-SHELL
sudo apt-get install ufw
sudo ufw --force enable
sudo ufw allow 80/tcp
sudo ufw allow from any to any port 22
sudo ufw allow from any to any port 3306
SHELL
end

````


## 02 - Reverse Proxy

Ein Reverse Proxy ("umgekehrter Proxy") ist eine zusätzliche Schutzmaßnahme, die vor einen oder mehreren Webservern geschaltet werden kann.
Im Gegensatz zu einem Proxy wird die Adressumsetzung in der entgegengesetzten Richtung durchgeführt.
Die Aufgabe des Reverse Proxys ist es Anfragen von Servern stellvertretend anzunehmen und an den entsprechenden Client weiterzuleiten.
Dabei gewährt der Reverse Proxy einem oder mehreren Clients eines externen Netzes den Zugriff auf ein internes Netz.

````
Vagrant.configure("2") do |config|
config.vm.box = "debian/buster64"
config.vm.provider "virtualbox" do |vb|
vb.name="M300ReverseProxy01"
end
config.vm.provision "shell", inline: <<-SHELL
sudo apt-get install apache2 -y
sudo apt-get install libapache2-mod-proxy-html -y
sudo apt-get install libxml2-dev -y
sudo a2enmod proxy
sudo a2enmod proxy_html
sudo a2enmod proxy_http
sudo systemctl restart apache2
sudo echo "ServerName localhost" >> /etc/apache2/apache2.conf
sudo systemctl restart apache2
sudo touch /etc/apache2/sites-enabled/001-reverseproxy.conf
sudo echo "ProxyRequests Off
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>
    ProxyPass /master http://master
    ProxyPassReverse /master http://master" >> /etc/apache2/sites-enabled/001-reverseproxy.conf
SHELL
end

````

## 03 - Benutzer und Rechte

Mit einem Script wurde ein Benutzer, eine Gruppe und bestimmte Berechtigungen auf ein Textfile gesetzt.

````
Vagrant.configure("2") do |config|
config.vm.box = "debian/buster64"
config.vm.provider "virtualbox" do |vb|
vb.name="M300BenutzerRechte01"
end
config.vm.provision "shell", inline: <<-SHELL
sudo useradd Kerim
sudo groupadd M300
sudo usermod -aG M300 Kerim
sudo touch test.txt
sudo chmod 774 test.txt
sudo chown Kerim test.txt
sudo chgrp M300 test.txt
SHELL
end

````

## 04 - SSH
Diese drei wichtigen Eigenschaften führten zum Erfolg von ssh:

* Authentifizierung der Gegenstelle, kein Ansprechen falscher Ziele
* Verschlüsselung der Datenübertragung, kein Mithören durch Unbefugte
* Datenintegrität, keine Manipulation der übertragenen Daten

Wem die Authentifizierung über Passwörter trotz der Verschlüsselung zu unsicher ist, der benutzt das Public-Key-Verfahren. Hierbei wird asymmetrische Verschlüsselung genutzt, um den Benutzer zu authentifizieren.


## Vergleich Vorwissen - Wissenszuwachs

Vorher wusste ich so ziemlich gar nichts über diese Themen ausser Virtualbox. Ich wusste wie man ein Ubuntu Server aufsetzt. Ich wusste am Anfang nicht was ein Atom ist. Jetzt weiss ich, dass ist ein Editor, nur besser und übersichtlicher. Wie man ein Vagrantfile gestaltet wusste ich auch nicht. Jetzt weiss ich auch wie man diese erweitern kann.

## Reflexion

Ich habe einiges lernen können, über diese Themen. Vor allem wusste ich nicht was Vagrant ist und jetzt weiss ich was es macht und wie ich ein solches File erzeugen kann. Was ich auch noch gelernt habe ist, was Github überhaupt ist und was ein Markdown ist. Mit dem kann man in Atom eine Dokumentation schreiben und gleichzeitig auf Github hochladen mit dem pushen. Anfangs wusste ich nicht genau wie das geht, nach einer Erklärung wusste ich das dann. Bilder konnte ich mit der Zeit dann auch schon hochladen. Die bestimmten Befehle wie man Tabellen oder Schriften gestalten möchte, habe ich kennengelernt.


## Glosar


## 30 Container

## 35 Sicherheit

## 40 Kubernetes (k8s)

## 80 Ergänzungen zu den Unterlagen

