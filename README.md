# AppImage-Sigrok-Cli
Mein AppImage mit allen Modulen für libsigrok kompiliert. Sourcen aus Github mit dem 
Stand März 2018 (commit 94cf02d5 https://github.com/jolinux/libsigrok/commit/94cf02d0c24580322fdf9238a96a563205b943d2) .
Mein System ist Debian Jessie 64bit.

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


#Meine Installationsparty
Swig
libusb
libftdi1
librevisa
libgpib
libieee1284

