# DNS in Freifunk Lübeck
Freifunk Lübeck ist eine lose Gemeinschaft, die sich mit dem Aufbau eines stadtweiten dezentralen Netzwerks beschäftigt.
Nähere Informationen gibt es unter [[http://luebeck.freifunk.net]].

## Idee
In diesem Repo werden unsere Zonefiles der ffhl-TLD verwaltet.

## Anleitungen
Da oft Fehler bei den Eintragungen gemacht werden und Kleinigkeiten vergessen werden, kommen hier nun einige Anleitungen.

### IPv6-Adresse berechnen
Da wir uns entschieden haben, dass zu jeder Domain auch eine IPv6-Adresse gelinkt sein soll, muss diese nach folgendem Schema berechnet werden.
Nur in Ausnahmefällen (wenn z.B. der Zielhost nicht IPv6-fähig ist) sollte dies unterbleiben.

Aus der IPv4-Adresse 10.130.a.b leitet sich die IPv6-Adresse durch folgendem Schritte ab:
* XX = [[Hexadezimale Darstellung|https://de.wikipedia.org/wiki/Hexadezimalsystem#Umwandlung_von_Dezimalzahlen_in_Hexadezimalzahlen]] von a
* YY = Hexadezimale Darstellung von b
* Zusammensetzen zu fdef:ffc0:3dd7::XXYY/64

Muehlentor als Beispiel:
10.130.10.1 wird zu fdef:ffc0:3dd7::a01/64

### Neue Domain
Für eine neue Domain muss diese in die Datei ffhl.zone nach folgendem Muster (Mit Leerzeichen statt Tabs!) gemacht werden:
```
hostname      A     10.130.10.1
              AAAA  fdef:ffc0:3dd7::a01
```
Danach muss für die rückwärtige Zuordnung noch die IPv6- und IPv4-Zone angefasst werden. Dabei ist die umgedrehte Adresse zu beachten.

In der 10.130.zone muss dies für 10.130.a.b und hostname.ffhl so aussehen:
```
b.a         PTR   hostname.ffhl.
```

In der fdef:ffc0:3dd7.zone muss der Eintrag für fdef:ffc0:3dd7::ABCD/64 so aussehen (Nullen bis zur 16. Stelle auffüllen!):
```
D.C.B.A.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0  PTR   hostname.ffhl.
```
