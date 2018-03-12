# AppImage-Sigrok-Cli
* Mein AppImage mit allen Modulen für libsigrok kompiliert.
* Sourcen von Github mit dem Stand März 2018 
* (commit 94cf02d5 https://github.com/jolinux/libsigrok/commit/94cf02d0c24580322fdf9238a96a563205b943d2) .

Mein System ist ein Debian Jessie 64bit.

Download unter 
https://github.com/jolinux/AppImage-Sigrok-Cli/releases

# Hardware
Für die Ansteuerung der jeweiligen Hardware sind Firmware und udev-rules in die entsprechenden Verzeichnisse zu kopieren!

sudo cp "/entpackverzeichnis/"libsigrok/contrib/60-libsigrok.rules /etc/udev/rules.d/

sudo cp "/entpackverzeichnis/"libsigrok/contrib/61*.rules /etc/udev/rules.d/

Bitte auch die 61*.rules kopieren, da nur dort die Rechteverwaltung beinhaltet ist. Damit die Ausführung vom AppImage 
für den Zugriff auf die Hardware ohne Root-Rechte (sudo) erlaubt wird.

Firmware wird von Sigrok, bis auf ihre eigene Opensource-Firmware "fx2lafw" nicht gestellt.
Das Suchverzeichnis ist hier erklärt:
https://sigrok.org/wiki/Firmware
(bei mir /usr/local/share/sigrok-firmware)

Es kann aber auch jedes andere Verzeichnis sein, wenn der Aufruf entsprechend diesem Befehls-Beispiel folgt:

SIGROK_FIRMWARE_DIR=/usr/local/share/sigrok-firmware/dslogic-firmware ./sigrok-cli

Damit lassen sich z.B. auch lokale Firmwareverzeichnisse zu Testzwecken nutzen. 


# Meine Installationsparty
Um das AppImage erstellen zu können sind einige Überlegungen und Vorbereitungen notwendig.
Ein AppImage ist ein Readonly-System und eine nachträgliche Erweiterung ist nicht möglich ohne es neu zu erstellen.
D.h. die Funktionalität ist beschränkt auf die einkompilierten Sourcen. Meine Erwartung ging dahin sigrok-cli bzw. 
libsigrok mit dem größten zur Verfügung stehenden Funktionsumfang (Stand heute) in ein geschloßenes System eines 
AppImages zu bannen, damit der höhste Grad an Nutzung möglich ist.
Dies erwartet eigentlich ein aktuelles Linuxsystem, egal welcher Distribution, um die neusten Libs von Haus aus einsetzen zu können. (Hier waren es die Zusätze Swig und libftdi die in der Version von meinem System nicht ausreichend vorlagen)
Aus diesem Grunde die Verwendung eines aktuellen Linuxsystem steht die Forderung der AppImage-Entwicklungsgruppe ältere
Linuxsysteme zum Bau von AppImages zu verwenden. Warum ?, vielleicht: damit nicht neuere Libs im AppImage auf ältere 
Systemvoraussetzungen des Gastsystem stoßen und somit nicht lauffähig sind.

## Was war nun im Einzelnen zu tun ?
Der Grundgedanke war sigrok lokal zu installieren. Dies vereinfacht das Zusammensammeln aller Komponenten auf ein Maximum.
Mit einem Prefix läßt sich ein Verzeichnis bestimmen, das später auch als AppDir verwendet werden kann!!!
Das Verfahren ist sehr gut dokumentiert im Sigrok-Wiki.

https://sigrok.org/wiki/Building#Installing_to_a_non-standard_directory_using_LD_RUN_PATH

(Eine kleine Hürde ist zu nehmen beim Installieren von libsigrok, wenn das (Prefix)Ziel-Verzeichnis neu/leer ist. 
Der Prozeß meldet einen Fehler weil der Zielpfad "/lib/python2.7/site-packages" nicht existiert)
Bei einer Standard-Linux-Installation fehlen einige LIBs für den vollständigen sigrok-Funktionsumfang. Dies meldet der
libsigrok-configure-Prozeß mit einer seitenlangen Ausgabe im Terminal.

_Swig_ mußte auf einen neueren Versionstand gehoben werden => hier hilft ein DEB-File aus den Backports

_libftdi1_ mußte aus den Sourcen generiert und ins System installiert werden (kann ich vieleicht noch allgemein brauchen) 

_libieee1284_ war einfach, konnte ich per Systemverwaltung installieren 

_librevisa_ und _libgpib_ waren da schon komplizierter. Obwohl librevisa auf Debian-Webseiten bekannt ist so ist es in der DEB-Systemverwaltung nicht zu finden. Von Debian ist librevisa kurzerhand umbennant worden in libvisa, was man bei genauerem Hinsehen auf den Debian-Webseiten auch sehen kann. Ein Installieren führt auch nicht zum Erfolg, da es von dem 
configure-Prozeß nicht gefunden werden kann. 

Nur ein Forschen in den configure-Sourcen (libsigrok) kam der Ursache näher. Es fehlte eine entsprechende librevisa.pc-Datei im selbstdefinierten lokalen PKG_CONFIG_PATH, welche bei einer händisch lokalen Installation mit Prefix aus den Sourcen mit 'configure', 'make' und 'make install' angelegt wird.

_libgpib_ ist für sigrok eigentlich nur eine Lib. Im Internet findet man dazu nicht viel. Schlußendlich fand ich dann doch ein umfangreiches Kernel-Source-Packet zum selber kompilieren. Da ich mein Hostsystem mit Kernelerweiterungen nicht belasten wollte und nur an der Lib interessiert war, entschied ich mich auch hier für eine lokale Installation mit dem schon angewanten PREFIX. Ein 'make install' ohne Rootrechte ließ nur das lokale Kopieren zu.

### Quellen:
* https://packages.debian.org/search?suite=jessie-backports&searchon=names&keywords=swig
* https://www.intra2net.com/en/developer/libftdi/download.php
* http://www.librevisa.org/downloads/
* https://sourceforge.net/projects/linux-gpib/files/


### Dateien:
* swig3.0_3.0.10-1.1~bpo8+1_amd64.deb
* libftdi1-1.4.tar.bz2
* librevisa-0.0.20130412.tar.gz
* linux-gpib-4.1.0.tar.gz
