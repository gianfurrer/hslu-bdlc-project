# Prototyping

Das Prototyping ist in diesem Kapitel zusammengefasst.
Die effektiv unternommenen Schritte sind in den Dateien [01_collect_data.ipynb](./../01_collect_data.ipynb) und [02_EDA_data_cleansing.ipynb](./../02_EDA_data_cleansing.ipynb) 
ersichtlich und dokumentiert.

Das Prototyping wird von Anfang an mit allen Daten durchgeführt aufgrund der geringen Datenmenge. Handelte es sich um ein richtiges Big Data Projekt, würde zu Beginn erst ein Teil der Daten untersucht werden und das Prototyping würde erst nach einem initialen Prototyping auf alle Daten ausgeweitet werden.

## Erster Kontakt mit den Daten

Als erstes wurden die CSV Dateien zu Parquet transformiert. Dies geschieht im Overwrite Modus, da diese ursprünglichen CSV Dateien immer die gleichen sind.
Falls die Daten dynamisch wären, würden die alten Parquet Files nicht ueberschrieben werden. Die Transformation würde versionierte Dateien produzieren, damit alte Versionen noch verfügbar wären.

Alle einzelnen CSV Dateien sind im Kapitel Dataset beschrieben.
Jedes Subdataset wurde in diesen Schritten untersucht:

- Schema ausgeben
- Anzahl Reihen ausgeben
- Ersten 5 Reihen ausgeben
- Bestimmte Werte mit 'describe' untersuchen
- Auf NULL Werte prüfen
- Auf Duplikate prüfen (ganze Reihen oder Werte von bestimmten Attributen)
- ID auf Uniqueness prüfen
- Je nach Beobachtung kann es noch weitere Prüfungen geben

Die folgenden Datensets wurden nicht weiter untersucht, da sie nicht benötigt werden für die Analysen:

- Constructor Results
- Seasons
- Sprint Results


## Pre-processing Schritte

Die meisten Dateien waren bereits in einem sehr guten Zustand. Es wurden die Spalten gelöscht, die nicht benötigt werden für die Analyse und gegebenfalls wurden auch welche umbenannt.
Die aufgeräumten Datein wurden dann als neues Parquet File mit dem Prefix 'cleaned' gespeichert. Falls weitere Schritte durchgefürht wurden, ist dies in der Datei 02 ersichtlich.
Dies wurde wieder mit der Overwrite Funktion gemacht, da die Daten nicht dynamisch sind und auch nur relativ wenige pre-processing Schritte nötig waren. In einem 'real life' Projekt, wären sie nicht overwritten, sondern versioniert gespeichert worden.

Für die Datei der Pitstops mussten noch mehr pre-processing Schritte unternommen werden, da es hier einige Fehler gab:
- Im Monaco Rennen in 2024 waren bei allen Pitstops nach dem ersten Lap die Werte für 'Lap' und 'Stop' vertauscht. Diese Vermutung konnte mit der offiziellen FIA Seite (https://www.formula1.com/en/results) bestätigt werden. Alle Pitstopeinträge für dieses Rennen, wo der Wert für Stop > 1 war, wurden die Werte getauscht, um sie zu korrigieren.
- Die Outlier pro Rennen wurden entfernt, damit nur die 'normalen' Pitstops übrig bleiben, für die Analyse sollen keine Pitstops, wo beispielsweise ein Penalty abgessesen wurde, berücksichtigt werden. Trotzdem hatte es noch eine grosse Spannweite an Werten. Es gab sehr unrealistische Werte als Maximum. Mit einem Boxplot wurde ein passender Cutoff gefunden, wobei die Werte über diesem Cutoff unrealistisch sind. Reihen mit diesen Werten wurden entfernnt.

Auch fuer die Wetterdaten mussten noch mehr pre-processing Schritte unternommen werden:
- TODO GIAN: WETTER what did you group there


## Überraschungen und Erkenntnisse

Es war eine Ueberraschung, dass die CSV F1 Dateien in einem sehr guten Zustand waren. 
Das CSV Format wurde überall korrekt eingehalten, es gab keine Duplikate in den Ids, es gab keine NULL Values, wo es keine geben soll.
Jede Datei hatte eine Id und die Verlinkung unter den Datensets ist ebenfalls korrekt mit den Fremdschlüsseln.

Ebenfalls war es überraschend, dass es recht viele Redundanzen gab. Zum einen kam die Kolonne 'number' immer wieder vor um den Fahrer zu beschreiben, obwohl auch die 'driverId' als Kolonne existierte. Ebenfalls was es überraschend, dass die 'position' immer mehrmals vorkam in verschiedenen Datentypen oder als leicht anderer Wert: position (string), positionText (string), positionOrder (integer).

Es war interessant zu sehen, dass es Probleme gab mit den Pitstops. Das eine Problem, war das Vertauschen von zwei Kolonnen. Das andere Problem mit den starken Outliern, war auch auf der offiziellen FIA Seite falsch. Es ist interessant zu sehen, dass auch eine riesige Organisation wie die FIA noch Fehler in ihren Daten hat.

### Domainspezifische Überraschungen

Es war sehr interessant, wie sich Formel 1 über die Jahre verndert hat. Vor allem wie viele Fahrer in früheren Saisons gefahren sind, während es heutzutage selten 25 oder mehr Fahrer gibt pro Saison.

## Learnings aus dem Prototyping

Was wir aus dem Prototyping gelernt haben, ist, wie wichtig es ist, sich Daten anzuschauen bevor man sie benutzt, vor allem in 'unregulierten' Dateiformaten wie CSV Dateien. Es können immer Fehler passieren bei der Bereitstellung von Daten.

Ebenfalls ist dieser Schritt auch sonst sehr hilfreich bei weiteren Analysen. Bei einem ersten Protoyping wird ein erster Kontakt mit den Daten hergestellt und die Daten werden kennengelernt. Das Schema wird vertrauter und auch die potentiellen Mängel sind bekannt und können bei weiteren Analysen berücksichtigt werden.
