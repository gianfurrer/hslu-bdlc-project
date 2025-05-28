# Dataflow

Erklären Sie, wie die Daten End-to-End durch das System laufen.
Wie speichern Sie die Rohdaten?
Haben Sie die Daten partitioniert?
In welchem Format haben Sie die prozessierten Daten gespeichert. Wieso?


- csvs downloaden
- zu parquet umwandeln (overwrite, da nur einmal, falls laufendes projekt: nicht overwite sondern versionieren)
- eins nach dem anderen laden, cleanup, zu parquet speichern wieder total in folgendem Format: (auch overwrite deswegen)
   nur die noetigen Attribute, dei fuer Analyse benoetigt sind, als parquet weil schnell laden

TODO: Schema der cleaned Parquets

- geladen in jeweiligen Analysis parts

- Note: keine Paritionierung, weil ganz andere noetig sind fuer die unterschiedlichen analysen, werden aber paritioniert wenn noetig in der Analyse