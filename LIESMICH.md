CSV Exporter
============

Eine Sammlung von Skripten, die �ber einen Cronjob Shop-Produkte in eine CSV-Datei exportiert.

Installieren und Konfigurieren
------------------------------
1.  Legen Sie alle Dateien des Moduls in Ihrem Shop in das folgende Verzeichnis:

          modules/marm/csvexporter

2.  Konfigurieren, erweitern oder schreiben Sie neue Exporter. 
Einige Exporter sind bereits enthalten. An diesen k�nnen Sie sich orentieren.
Die Exporter liegen im Verzeichnis `exporter`.

3.  Nun k�nnen Sie Ihre Exportscripte aufrufen und die CSV-Datei erstellen. 
Dies k�nnen Sie entweder manuell durchf�hren oder �ber einen Cronjob automatisieren.
Der Aufruf lautet: 

        shopurl.de/modules/marm/csvexporter/exporter/IHR-EXPORTER/IHR-EXPORTER.php

4.  Schon fertig. Sie k�nnen nun Ihre CSV-Datei an die Preisportale melden.


Hinweise
--------

Getestet auf Oxid CE 4.6.5, 4.7.5, 4.8.1

Version
-------
Aktuelle Version 1.0


Exporter konfigurieren
======================

Grundkonfiguration
------------------

Im oberen Bereich Ihres Exporterscripts finden Sie die Konfiguration. 
Diese ist in der Variablen `$_config` als Array hinterlegt.
Folgende Optionen stehen Ihnen zur Verf�gung.

        'export_parents'                => Anzeigen von Eltern Produkten in der CSV-Datei
        'filename'                      => Pfad und Dateiname
        'limit'                         => Limit f�r Export
        'debug'                         => debug Option An/Aus, f�r Entwickler
        'silent'                        => debug Ausgaben An/Aus, f�r Entwickler
        'header'                        => Kopfzeile An/Aus
        'langid'                        => Sprachid, f�r welche Sprache Exportiert werden soll
        'shippingcost'                  => Versand Optionen      
        'productLinkPrefix'             => Standard Produkt URL Pr�fix
        'geizhalsProductLinkParameters' => Exporter spezifischer Produkt Parameter    
        'imageurl'                      => Pfad der zu Exportierenden Produkt Bilder
        'inStock'                       => Ausgabe, wenn Produkt Lageberstand hat
        'outOfStock'                    => Ausgabe, wenn Produkt kein Lagerbestand hat      
        'cutFirstPosArticlenumber'      => Die ersten x Zeichen der Artikelnummer abschneiden
        'generalVat'                    => MwSt f�r die Nettopreise
        'netPrices'                     => Nettopreise An/Aus
        'categoryPathSeparator'         => Trennzeichen f�r die Kategoriepfade

CSV-Konfiguration
-----------------

Nun geht es darum, die Ausgabe der Daten zu steuern.
In der Variablen `$_entry` werden die Felder, die Sie exportieren m�chten, angegeben.

Das Array enth�lt folgende Optionen.

        'header'    => Spalten Namen f�r die CSV-Datei.
        'fields'    => Spalten Inhalte f�r die CSV-Datei.
        'separator' => Trennzeichen der CSV-Datei.

Hier noch einige Tipps:

- Die Spalten Namen werden im `header` nacheinander geschrieben und mit `;` getrennt.

- Um eine Leerzeile zwischen der Kopfzeile und den Inhalten in der CSV-Datei zu erzeugen, schreiben Sie ein `\n` an den letzen Spaltennamen.

- Die Spalteninhalte werden in Markern `#IHRER MARKER#` geschrieben und mit `|` getrennt.

- Man kann Marker miteinander verkn�pft in einem Datensatz ausgeben, die werden mit `+` geschrieben.
Das sieht dann so aus: `#Marker 1#+#Marker 2#`.

- Oder man m�chte einen Fallback haben, dann werden die Marker mit ein `/` geschrieben.
Das bedeutet wenn `#Marker 1#` leer ist wird `#Marker 2#` ausgegeben.
Das sieht dann so aus: `#Marker 1#/#Marker 2#`.

- Die Operatoren k�nnen gemischt werden und einen Spalteninhalt erstellen der aus Fallback und Verkn�pfung besteht.
zB. `#Marker 1#/#Marker 2#+#Marker 3#`, hier wird entweder Marker 1 oder Marker 2 mit Marker 3 ausgegeben.

Eigene Konfigurationen
----------------------

Im unteren Bereich Ihres Exporterscripts k�nnen Sie nun eigene Funktionen schreiben.
Alle Funktionen aus der **marmCsvExporter.php** k�nnen �berschrieben und erweitert werden.

Eine nicht vorhandene Spalte hinzuf�gen, aber wie?

Bsp.: Wir wollen das Attribut **Farbe** der einzelnen Variantenprodukte in die CSV-Datei exportiert haben.

Dazu schreiben wir im `header` den neuen Spaltennamen `Farbe`. In das Feld `fields` kommt ein neuer Marker namens `#color#`.
Diesen Marker m�ssen wir nun dynamisch bef�llen. Marker werden in der Funktion `getDataByMarker($marker)` definiert, diese m�ssen wir erweitern.
Hier bekommt der Marker einen eigenen Funktionsaufruf. Danach k�nnen wir die eigentliche Funktion in unserem Exporterscript schreiben,
die die Farbe der Produkte ausliest. Als Beispiel sehen Sie sich Die Funktion `getSeoUrl()` in den mitgelieferten Exportern,
von geizhals oder google.