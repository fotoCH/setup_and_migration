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

## Places Export
Export aller Ortschaften aus der Datenbank für den Import in CollectiveAccess:
``` sql
SELECT ort FROM ausstellung
UNION
SELECT name FROM arbeitsorte
UNION
SELECT dcterms_spatial FROM fotos
UNION
SELECT name_de FROM regiort
WHERE typ = "ort"
```
Das Resultat kann in die Tabelle Excel_Mapping\Data\Orte_withIdnoFormula.ods kopiert und so die idno's generiert werden.