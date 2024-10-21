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
(Beispiel für htaccess Datei im FeBe)
Diese Datein erstellen in "Tag1":<br>
![image](https://github.com/user-attachments/assets/992eef20-b960-4cfc-8e7b-eaeb9f55ee03)<br>

Jetzt erstellen wir in beiden Ordnern eine .htaccess Daatei.
Die im Tag1 soll alles zu index.php Umleiten und die in data soll den Zugriff zu den Datein im Ordner blocken.
#### Datein Umleiten
Hauptdatei:
```html
RewriteEngine on
RewriteRule . index.php [L]
```
[L] steht für Last und bedeute das keine rewriteRule's mehr angenommen werden.
#### Datein sperren
Unterordner:
```html
<Files *>
Order allow,deny
Deny from all
</Files>
```
Egal was mann eingibt (fast) kommt Man auf die index.php Seite (wo ich Haiy hineingeschrieben habe):<br>
![image](https://github.com/user-attachments/assets/41c73585-108e-4110-95ce-8f327a7bdb66)<br>
Und wenn man versucht auf data zu gehen kommt das:<br>
![image](https://github.com/user-attachments/assets/6a7f9235-2029-47b4-ab17-0e273b67267b)<br>
#### Regeln
Das problem mit der htaccess Datei im Tag1 ist, es leitet auch von Orten ausserhalb Tag1 um.
Um dass zu umgehen müssen wir das hinzufügen:
```html
RewriteEngine on
RewriteCond %{REQUEST_URI} ^/Tag1/
RewriteRule . index.php [L]
```
Alternative Lösung:
```html
RewriteEngine on
RewriteRule ^(.*) index.php [L]
```
## Git
Ein Terminal öffnen.
Auf Tag1 switchen.
Dass auswählen:<br>
![image](https://github.com/user-attachments/assets/b5e4161c-1c16-4319-95ce-39b56b37494a)<br>
Und dann die cmd wählen:<br>
![image](https://github.com/user-attachments/assets/81a8067e-0d7b-4814-879d-1830a1d0f97c)<br>
Jetzt das Terminal schliessen und neu wählen, jetzt hat Man ein cmd Zeichen.
Dort machen wir dann ein Git Repository:<br>
![image](https://github.com/user-attachments/assets/ace070ff-25fb-4406-831a-4986479a60a7)<br>
Die Sachen jetzt commiten.
## PHP
### Variablen
#### definieren
Werden immer mit einem $ am Anfang dargestellt.
Dürfen diretk nach dem $ keine Zahl haben.
Sonst _, Buchstaben & Zahlen erlaubt.
#### Ausgaben
Mit dem ```<pre>```-Tag kann man Code schöner ausgeben lasse, z.B. Array's werden damit schöner formatiert.
#### Arrays
"count(array)" um die Anzahl Elemente im Array auszugeben.
Mit json_encode kann man ein Array zu einem JSON Element umwandeln und mit json-decode zurück zu einem array.
### POST & GET
#### Per Code
Code:<br>
![image](https://github.com/user-attachments/assets/efa2f045-5e2b-4d47-9328-80b1658311ae)<br>
Resultat:<br>
![image](https://github.com/user-attachments/assets/3203110a-b3f5-4fa6-8afd-d21388098628)<br>
#### Postman
