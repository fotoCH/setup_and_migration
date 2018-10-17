# Migration Guide

## Bildgattungen
Die Spalte "bildgattungen" bei den Beständen, Fotografen und Institutionen muss vor dem Import in der Tabelle überarbeitet werden (nur die Spalte, nicht die ganze Tabelle!):
Siehe auch Excel_Mapping/Genre_List_To_XML.xlsx

 * LOWER() über alles
 * Suchen & ersetzen:
   * Umlaute "ä" durch "ae"
   * "Unfall/Katastrophe" --> "unfall"
   * "Kunst mit Fotografie" --> "kunst_fotografie"
   * "wissenschaftliche Fotografie" --> "wissenschaft"
   * "private Fotografie" --> "private"
   
Danach werden die labels direkt auf die idno der List-Items in CollectiveAccess gematched.


## Profile Import / Export
Die Datei fotoCH_Setting_1.xml definiert die Datenstruktur und wird bei der Installation als Profil ausgewählt.

Vor der Installation wird jeweils die aktuellste Version des Profiles auf den Server kopiert (scp / pscp). Benutzer und Passwort sind im KeePass zu finden:
```bash
> $ scp .\fotoCH_Setting_1.xml [user]@46.231.207.26:/var/www/foto-ch.ch/collective_access/providence/install/profiles/xml
```

Manchmal ist es hilfreich, den aktellen Stand der Installation zu exportieren und einzelne Änderungen in die Profildatei aufzunehmen. Providence bietet dafür die "caUtils" an:
```bash
$ cd /var/www/foto-ch.ch/collective_access/providence/support
$ ./bin/caUtils export-profile > ./../../export.xml
```

### Import von Änderungen

Wie in diesem [Artikel](https://docs.collectiveaccess.org/wiki/Configuration_Sync) fast richtig beschrieben, kann die Konfiguration eines
laufenden Systems angepasst werden. Man muss also nicht neu installieren. Das ist nützlich, wenn man z.B. eine lange Liste importiern will (Beispiel Bildgattungen).

```bash
support/bin/caUtils update-installation-profile -n <name_of_your_profile>
```

wobei `<name_of_your_profile>` der Dateiname ohne `.xml` ist.

#### Validerung des Profils gegen Schema (falls import nicht funktioniert)
Falls es beim Import Validierungsfehler gibt, kann mit dem `profile.xsd` das XML validiert werden. Am besten Online. Der Importer gibt auch eine Fehlermeldung, aber keine Zeilenangabe.

## Places Export
Export aller Ortschaften aus der Datenbank für den Import in CollectiveAccess:
``` sql
SELECT ort COLLATE utf8_general_ci as ort FROM ausstellung
UNION
SELECT name COLLATE utf8_general_ci FROM arbeitsorte
UNION
SELECT dcterms_spatial COLLATE utf8_general_ci FROM fotos
UNION
SELECT name_de COLLATE utf8_general_ci FROM regiort
WHERE typ = "ort"
order by ort asc
```
Das Resultat kann in die Tabelle Excel_Mapping\Data\Orte_withIdnoFormula.ods kopiert und so die idno's generiert werden.

## Migrations-Reihenfolge
* Orte vor Ausstellungen und Bestand_Orte
* Fotografen vor Fotos
* ...

## Pawtucket Konfiguration

Alle Anpassungen an Pawtucket sind im Repository [fotoCH/pawtucket2](https://github.com/fotoCH/pawtucket2) auf GitHub zu finden. Änderungen können auf dem Server `foto-ch.ch` mit `git pull --rebase origin master`
von GitHub heruntergeladen werden.

## Providence Konfiguration

Für Providence gibt es ebenfalls ein Repository [fotoCH/providence](https://github.com/fotoCH/providence)
