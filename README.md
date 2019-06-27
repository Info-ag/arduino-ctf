## Installation

Der Arduino Nano V3 benutzt den *CH340-Chip* (weißer Druckknopf auf der Vorderseite und *Az-Delivery* Beschriftung) bzw. *FTDI-Chip* (schwarzer Druckknopf auf der Vorderseite) für die Verbindung mit dem Computer.
Die Treiber dafür sind auf den meisten Betriebssystemen bereits vorinstalliert. Sollte der *CH340* Treiber nicht vorhanden sein, gibt es [hier eine Anleitung für Windows, Linux und macOS](https://www.makershop.de/ch340-341-usb-installieren/).


Die **Arduino IDE** schadet nie. Ihr könnt sie euch [auf der offiziellen Arduino-Website herunterladen](https://www.arduino.cc/en/Main/Software).
Damit ihr die Arduino IDE und ihren *Seriellen Monitor* verwenden könnt, müsst ihr unter `Werkzeuge > Board` das Board `Arduino Nano` auswählen. Außerdem muss der richtige Port unter `Port` ausgewählt werden. Da muss man erst etwas rumprobieren, bis man den richtigen Port findet. Zum Testen könnt ihr den *Seriellen Monitor* (oben rechts im Fenster, das Symbol mit der Lupe) öffnen. Die Baud Rate sollte auf **19200** und das Zeilenende auf **Sowohl NL als auch CR** eingestellt sein. Ist der Port richtig gewählt, solltet ihr eine Nachricht im *Seriellen Monitor* sehen. Um selbst Skripts auf den Arduino hochzuladen (dabei überschreibt ihr die Challenge!), müsst ihr für die *CH340-Version* erst den Prozessor unter `Werkzeuge > Prozessor` zu `ATmega328 (old Bootloader)` ändern. Für die *FTDI-Version* funktioniert `ATmega328`.

## CTF – Capture The Flag

CTFs (Capture The Flag) sind Information Security Wettbewerbe, bei denen man `Flags` aus speziell geschriebenen Programmen erobern muss. Dabei gibt es viele verschiedene Ansätze. Meistens muss man Sicherheitslücken der Programme ausnutzen. Wer mehr darüber erfahren will, kann sich [*CTFtime*](https://ctftime.org/) ansehen.


Das Ziel dieser Aufgabe ist es, den `secret key` (oder auch `Flag`) zu erhalten. Über den *Seriellen Monitor* (oder `screen` unter Linux/macOS) könnt ihr mit dem Programm interagieren. Die Beschreibung ist relativ selbsterklärend.


Die Aufgabe war Teil der [Riscure Rhme](https://rhme.riscure.com) Challenge aus dem Jahr 2016. Den Code dazu [könnt ihr hier finden](https://github.com/Riscure/Rhme-2016).

### Tipps

Bei dieser Aufgabe handelt es sich um ein **Format String Exploit**. Ihr müsst also eine Stelle im Programm finden, bei der ihr einen String (Text) eingeben könnt und dieser dann auch wieder ausgegeben wird. Mit Strings wie `%x` oder `%s` kann man dann die Rückgabe manipulieren und schließlich mit der Position des `Flags` im Speicher des Programms den `Flag` erhält.


Tipp: Die Wahrscheinlichkeit, durch `Spin the wheel` den `secret key` zu gewinnen ist so gering, dass diese Brute-Force-Methode mehrere Woche dauern wird. Jedoch kann man damit Coupons gewinnen, die ihr an der Bar einlösen könnt. Das wird euch nachher weiter bringen.


Es empfiehlt sich, ein Programm zu schreiben, dass automatisch verschiedene *Memory Locations* ausprobiert, so dass ihr das nicht per Hand machen müsst. 
Mit **Python** könnte das folgendermaßen aussehen:
```python
from serial import Serial

# Ändere '/dev/tty.usbserial-1410' zu dem Port aus der Arduino IDE
arduino = Serial('/dev/tty.usbserial1410', 19200)

# Warte bis die Begrüßung vom Arduino gesendet wurde
read_until(arduino, '[5] Restart')

# Starte ein Spiel
# \r entspricht der Enter-Taste
# Unter Python 2 muss das b vor dem String entfernt werden!
# Python 3 ist natürlich immer empfehlenswert. Die Unterstützung für
# das ältere Python 2 wird bald eingestellt!
arduino.write(b'1\r')

def read_unitl(s, until):
    res = ''
    while until not in out:
        res = res + s.read(1)

    return res
```
**Wichtig**: Ihr benötigt `pyserial` um eine serielle Verbindung herzustellen. Dieses Paket könnt ihr etwa mit `pip` installieren:
```bash
pip install pyserial
```
Unter Windows ist das etwas kompliziert. Mithilfe von *PyCharm* geht das aber etwas einfacher. Oder ihr verwendet einfach Linux, dann ist es ganz unkompliziert.


Kommt ihr gar nicht weiter, hilft [dieses Video](https://www.youtube.com/watch?v=fRgNtGXDMlY) vielleicht weiter.