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

# Tag 2
## OOP
### Klassen
Klasse erstellen:
```php
class ClassName  {
	public string $var1;
	public int $var2;
}
```
Konstruktor erstellen:
```php
public function __construct(string $var1, int $var2) {
	$this->var1 = $var1;
	$this->var2 = $var2;
}
```
#### Abstrakte Klassen
Abstrakte Klasse erstellen:
```php
    abstract class defaultCar {
        public function drive() {
            return "VROOOOOOOOOOOOM!!";
        }
    }
```
FUnktionen von einer abstrakten Klasse kann man von überall abrufen.
Abstrakte Klassen haben normalerweise keinen Konstruktor.
#### Interface
Ein Interface erstellen:
```php
interface Template{}
```
Interfaces haben mehrere Funktionen, die man dann direkt bei Klassen einbauen kann.
So macht man eine Klasse mit einem INterface:
```php
class ClassName implements Template{}
```
#### Namespace
Namespace wird verwendet um Namenskonflikte zu vermeiden.
Vorallem beim verwenden bei 3rd party Applikationen serh gut.
Wird normallerweise nur für Klassen verwendet.

Namespace erstellen:
```php
namespace Tag2\cars;
```
Alle Kalssen in der Datei gehören jetzt zu der Namespace "Tag2\cars".

Wenn wir jetzt dort drin eine Klasse "car" haben und dann die File mit der Namespace in einer anderen includieren, können wir so darauf zugreifen:
```php
namespace Tag2;
include("cars.php");
$car = new cars\car();
```
### Laden von Files/Klassen
#### Require
Anstatt mit include, kann man mit rquire arbeiten.

#### Autoloader
Mit dem Befehl ```spl_autoload_register``` kann man einen Autoloader erstellen.
Mit so einem Code, kann man jetzt mit dem Link angeben, welche Klassen man braucht:
```php
spl_autoload_register(function($class) {
    include 'lib/' . $class . '.php';
});
```

Wir machen eine Abfrage um den Link zu überprüfen:
```php
if(isset($_GET['class'])) {
    $class = $_GET['class'];
}else{
    $class = 'cars';
}
echo $class;
```
Falls keine Klasse angegeben wird, wird die Klasse "cars" standardmässig verwendet.
Wenn wir die Seite mit dem Link "index.php?class=books" öffnen, wird nur die Klasse books geladen.
##### Mit Namespace
Wenn wir jetzt bei jeder Klasse die Namespace "lib" hinzufügen, müssen wir denn Code ein wenig abändern.
Zuerst nehmen wir vom Autoloader den Folderpfad weg:
```php
spl_autoload_register(function($class) {
    include $class . '.php';
});
```
Jetzt passen wir noch die Abfrage an:
```php
if (isset($_GET['class'])) {
    $class = $defaultNamespace . $_GET['class'];
} else {
    $class = $defaultNamespace . 'cars';
}
```
Wir können das noch kürzen:
```php
$check = isset($_GET['class']) ? $_GET['class'] : 'cars';
$class = $defaultNamespace . $check;
```
Das Problem mit dieser Methode, ist das dies nicht mehr funktioniert, wenn wir andere Ordner und Namespaces haben.

### MVC
#### Logik
Es gibt in der MVC Architectur drei zusammenarbeitende Objekte, das Model, die View und der Controller.
Das Model ist die Datenbank und hält die Datenobjekte.
Die View ist das was der Nutzer sieht (templates).
Der Controller ist die Verbindung zum Server und sendet/bekommt Daten.
#### Verzeichnisstruktur
(Auf testing Ordner von FeBe)
### Respones Code
Um einen Error Code zu senden:
```php
http_response_code(404);
```

# Tag 3
## Composer
### Installation
Composer ist ein Paketmanager, der PHP-Packete verwaltet.
Auch ein Abhängigkeitsverwalter (Dependency Manager).
Zum Composer installieren:
```cmd
npm install --save composer
```
(Globaler Download / Ordner spielt keine Rolle)
Zum testen welche Version man hat:
```cmd
composer -v
```
Ansonsten per Installer holen.
### Wichtige Commands
Bei einem Composer Projekt kann man die PACKETE mit diesem Befehl updaten
```cmd
composer update
```
Mann kann automatische Updates (für Composer selbst) so einschalten:
```cmd
composer auto-update
```
So kann man die Packete herunterladen:
```cmd
composer download
```
### Struktur erstellen
#### public
Ein Public Ordner erstellen.
Dort drin eine index.html Datei erstellen.
In der Apache-Konfig den Pfad von dem Tag3 (c.local) am Ende "/public" hinschreiben.
Jetzt sollte die Seite (c.local) direkt zu diesem index.html führen.
Dass macht man, weil der Browser nur auf die public Dateien Zugriff haben soll und nicht die anderen.
#### Initialisieren
Terminal/Konsole öffnen und zu dem Tag3 gehen.
Den Befehl ausführen:
```cmd
composer init
```
Jetzt kommen einige Fragen, bei der ersten tut man normeallerweise GitHubName/RepositoryName. (interuniversalbuddy/amuriqi_m295)
Als nächstes die Beschreibung. (M295-Tag3)
Beim rest einfach Enter drücken.

Jetzt sollten zwei neue Ordner und eine Datei auf der gleichen Ebene wie der public Ordner gemacht.<br>
![image](https://github.com/user-attachments/assets/1f98e005-8b02-4616-b1f1-389d6dd004cc)<br>

Beim weitergeben des Projektes, muss die Venodor Datei nicht mitgegeben werden, aber die composer,json Datei sschon.
### Routing
#### Installieren
Auf Packagist finde, diesen Command: (gleichen Ordner wie vorher)
```cmd
composer require steampixel/simple-php-router
```
Jetzt sollte im composer.jsion eine "require" Section erschienen sein:<br>
![image](https://github.com/user-attachments/assets/dc211094-d2dd-40f4-b35b-de9f5f73a141)<br>
Wir schreiben noch hinzu das die PHP-Version mind. 8.0 sein soll:<br>
![image](https://github.com/user-attachments/assets/9535f210-e2be-4fc4-918d-c7a2db7d8649)<br>
#### Verbinden
index.html zu index.php ändern.
Mit diesem Befehl sagen wir, dass wir ALLE Variablen zuerst deklarieren und danach verwemnden:
```php
declare(strict_types= 1);
```
Jetzt wollen wir den composer verwenden (alle Klassen im Vendor werden geladen):
```php
require __DIR__ ."/../vendor/autoload.php";
```

**Anleitungen & Besipiele auf Steampixel Packagist!!**
Wir kopieren das "Simple Example" ohne den ersten Teil.
```php
// Use this namespace
use Steampixel\Route;

// Add the first route
Route::add('/user/([0-9]*)/edit', function($id) {
  echo 'Edit user with id '.$id.'<br>';
}, 'get');

// Run the router
Route::run('/');
```

Wegen dem ```Route::run('/');``` wenn wir in den Link z.B. /hallo schreiben (c.local/hallo) versucht er eine hallo-Datei zu öffnen.
Was heisst wir müssen eine htaccess Datei verwenden.

#### Seiten einstellen
Die zweitletzten Abschnitt so abändern:
```php
Route::add('/info', function() {
    phpinfo();
}, 'get');
```
Wenn wir jetzt auf c.local/ifno gehen, werden die PHP Daten ausgegeben.
Wir erstellen noch eine .htaccess Datei, mit den standard Einstellungen im public.

So können wir jetzt mehrere Seiten hinzufügen:
```php
Route::add('/([a-zA-Z0-9]*)', function($class) {
    echo "Hello $class!";
}, 'get');
```
Bei diesem wird jetzt "Hallo" und dann das Eingegebene ausgegeben, solange es aus Buchstaben & Zahlen besteht.
