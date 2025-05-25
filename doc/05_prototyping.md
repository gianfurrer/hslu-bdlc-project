# Prototyping

Das Prototyping ist in diesem Kapitel Zusammengefasst.
Die effektiv unternommenen Schritte sind in der Datei [02_EDA_data_cleansing.ipynb](./../02_EDA_data_cleansing.ipynb) 
ersichtlich und dokumentiert.


## Erster Kontakt mit den Daten

- als erstes von CSV zu parquet (siehe dataloading oder so file), hier overwrite, da dies einmal gemacht, ansonsten nicht overwrite sondern versionier


Für dieses Projekt wurden verschiedene Datensets analysiert:
- Circuits (Rennstrecken)
- Constructor Results
- Constructor Standings
- Constructors
- Driver Standings
- Drivers
- Lap Times
- Pit Stops
- Qualifying
- Races
- Results
- Season
- Sprint Result
- Status
- Weather

Jedes Dataset wurde in diesen Schritten untersucht:
- Schema ausgeben
- Anzahl Reihen ausgeben
- Ersten 5 Reihen ausgeben
- Bestimmte Werte mit 'describe' untersuchen
- Auf NULL Werte prüfen
- Auf Duplikate prüfen (ganze Reihen oder Werte von bestimmten Attributen)
- ID auf uniqueness prüfen TODO
- Je nach Beobachtung kann es noch weitere Prüfungen geben


## Pre-processing Schritte

- nicht benoetigte Spalten loeschen
- ggf Spalten umbennen
- dann alsneues parquet speichern, hier overwrite, da dies einmal gemacht, ansonsten nicht overwrite sondern versionier

Pit Stops: 
- 2024 Monaco: Alle Pitstops nach derdem ersten Stop hatten lap und stop value vertauscht, alle wo stop > 1, wurden diese Werte getauscht. Alls mit offizieller FIA seite doublechecked
- Pitstop outliers  pro Rennen wurden entfernt, um nur diese Stops zu haben, die lediglcih Reifen oder andere Teile wechseln (keine wo Penaltie abgessen wird oder sonsitige grosse Probleme waren.)
- Auch nach Outliers, sehr grosse Differenz von minimum zu maximum, sehr unrealistische Werte als Maximum. Mit Boxplot passender Cutoff gefunden und Zeilen mit diesen Werten entfernen. Diese wurden nicht gefunden bei Outlier schritt, da diese pro Rennen gemacht wurden und diese Probleme waren nie nur einmal in einem Rennen, immer mehrmals.

Wetter: TODO GIAN


### Überraschungen und Erkenntnisse

- sehr guter Zustand der Daten, vor allem Verlinkung
- Redundanz: z.B position als mehrere datentypen: z.B results: position (string), positionText (string), posiitonOrder (integer)
- Problem mit pit times

#### Spezifische Überraschungen

Wie viele Fahrer in fruehen Seasons gefahren sind: max Position in driver Standings = 108

## Learnings aus dem Prototyping

- wie wichtig es ist, da Daten, vor allem in 'unregulierten' Dateien wie CSV
- wie sehr es auch hilfti n weiteren schritten, weil mandie Daten bereits kennt und weiss, wo man was findet