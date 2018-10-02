# Migration Guide

## Bildgattungen
Die Spalte "bildgattungen" bei den Beständen und Fotografen muss vor dem Import in der Tabelle überarbeitet werden (nur die Spalte, nicht die ganze Tabelle!):
Siehe auch Excel_Mapping/Genre_List_To_XML.xlsx

 * LOWER() über alles
 * Suchen & ersetzen:
   * Umlaute "ä" durch "ae"
   * "Unfall/Katastrophe" --> "unfall"
   * "Kunst mit Fotografie" --> "kunst_fotografie"
   * "wissenschaftliche Fotografie" --> "wissenschaft"
   * "private Fotografie" --> "private"
   
Danach werden die labels direkt auf die idno der List-Items in CollectiveAccess gematched.




