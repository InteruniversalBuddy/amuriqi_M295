# Tag 1
## Installation
VM erstellen
Git, VSC, XAMPP, Node, DBeaver installieren auf VM

XAMPP in Admin öffnen und Den hacken links bei Apache & MySQL drücken und dan starten. (Jetzt wird es beim neustart der VM automatisch gestartet.)

## Struktur erstellen
### Neuer link für Datei erstellen
Neuen Ordner "Tag1" in htdocs erstellen.
Bei Apache unter "Konfig" die erste Datei öffnen, dann Apache stoppen und diesen Code reinkopieren:
```html
Listen 55080
<VirtualHost *:55080>
    Servername Page1.local
    DocumentRoot "C:\xampp\htdocs\Tag1"
    <Directory "C:\xampp\htdocs\Tag1">
        AllowOverride All
	Allow from All
    </Directory>
</VirtualHost>
```
Speichern und Apache wieder starten.
Mit dem link "http://localhost:55080" kommt man jetzt auf die Tag1 Datei.

### Link kürzen
auf "C:\Windows\System32\drivers\etc" die Datei hosts als Admin (VSC) öffnen und unten nue hinzuschreiben:
```html
127.0.0.1  a.local
::1        a.local
```
Mann kann soviele erstellen wie Man will und der name ist egal (.local muss sein).
In der Konfig Datei jetzt bei "Servername" die neue Seite hineinschreiben (a.local) und den Listen wegmacehn (auskommentieren) und die IP bei VirtualHost entfernen:
```html
# Listen 55080
<VirtualHost *>
    Servername a.local
    DocumentRoot "C:\xampp\htdocs\Tag1"
    <Directory "C:\xampp\htdocs\Tag1">
        AllowOverride All
	Allow from All
    </Directory>
</VirtualHost>
```
Wir machen jetzt für jeden Tag einen Ordner und Seite (den prozess nochmals).

## Konfigurationen
### Regeln
#### php.ini
Unter "Konfig" bei Apache öffnen.<br>
![image](https://github.com/user-attachments/assets/ec4024e0-fb3c-4bca-8470-d673fb6c4ee5)<br>
Diese Linie finden und sichergehen das ; nicht vorne dran steht.
Jetzt ist diese Extension active.

#### htaccess
Eine Konfigurationsdatei für Apache-Webserver.
Man kann so umleiten, dass es nicht zu index.html geht, sondern es instanziert.
Diese können auf der oberste Ebene oder in jedem Ordner gemacht werden. Es zählt dann für diesen Ordner.
Dass macht Man, dass Leute keinen Zugriff über den Browser/Links auf die ini Dateî und wichtige Dateien kommen.

### Umsetzen
#### Datein Umleiten
(Beispiel für htaccess Datei im FeBe)
Diese Datein erstellen in "Tag1":<br>
![image](https://github.com/user-attachments/assets/992eef20-b960-4cfc-8e7b-eaeb9f55ee03)<br>

Jetzt erstellen wir in beiden Ordnern eine .htaccess Daatei.
Die im Tag1 soll alles zu index.php Umleiten und die in data soll den Zugriff zu den Datein im Ordner blocken.
Hauptdatei:
```html
DirectoryIndex index.php

# enable apache rewrite engine
RewriteEngine on

# set your rewrite base
# Edit this in your init method too if you script lives in a subfolder
RewriteBase /

# Deliver the folder or file directly if it exists on the server
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-l

RewriteCond %{REQUEST_URI} !(\.css|\.js|\.png|\.jpg|\.jpeg|\.gif|\.xlsx|\.xls)$ [NC]
 
# Push every request to index.php
RewriteRule ^(.*)$ index.php [QSA]
```
#### Datein sperren
Unterordner:
```html
<Files *>
Order allow,deny
Deny from all
</Files>
```
