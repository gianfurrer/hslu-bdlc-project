# Prototyping

Das Prototyping ist in diesem Kapitel zusammengefasst.
Die effektiv unternommenen Schritte sind in den Dateien [01_collect_data.ipynb](./../01_collect_data.ipynb) und [02_EDA_data_cleansing.ipynb](./../02_EDA_data_cleansing.ipynb) 
ersichtlich und dokumentiert.


## Erster Kontakt mit den Daten

Als erstes wurden die CSV Daten zu Parquet transformiert. Dies geschieht im Overwrite Modus, da dies urspruenglichen CSV Dateien immer die gleichen sind.
Falls die Daten dynamisch waeren, wuerden die alten Parquet Files nicht ueberschrieben werden. Die Transformation wuerde versionierte Dateien produzieren, damit alte Versionen noch verfuegbar waeren.

Für dieses Projekt wurden diese verschiedenen Datensets verwendet:

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
- ID auf Uniqueness prüfen
- Je nach Beobachtung kann es noch weitere Prüfungen geben

Die folgenden Datensets wurden nicht aufgeraeumt, da sie nicht benoetigt werden fuer die Analysen:

- Constructor Results
- Season
- Sprint Result


## Pre-processing Schritte

Die meisten Dateien waren bereits in einem sehr guten Zustand. Es wurden die Spalten geloescht, die nicht benoetigt werden fuer die Analyse und gegebenfalls wurden auch welche umbenannt.
Die aufgeraeumten Datein wurden dann als neues Parquet File mit Suffix 'cleaned' gespeichert. Falls weitere Schritte durchgefuerht wurden, ist die in der Datei 02 ersichtlich.
Dies wurde wieder mit der Overwrite Funktion gemacht, da die Daten nicht dynamisch sind und auch nur relativ wenige pre-processing Schritte noetig waren. In einem richten Projekt, waere die definiv nicht nur overwritten worden, sondern versioniert gespeichert.

Fuer die Datei der Pitstops mussten noch mehr pre-processing Schritte unternommen werden, da es hier einige Fehler gab:
- Im Monaco Rennen in 2024 waren bei allen Pitstops nach dem ersten Lap die Werte fuer 'Lap' und 'Stop' vertauscht. Diese Vermuting konnte mit der offiziellen FIA Seite bestaetigt werden. Alle Pitstopeintraege fuer dieses Rennen, wo der Wert fuer Stop > 1 war, wurden die Werte getauscht, um sie zu korrigieren.
- Die Outlier pro Rennen wurden entfernt, damit nur die 'normalen' Pitstops uebrig bleiben, fuer die Analyse sollen keine Pitstops, wo beispielsweise ein Penaltie abgessesen wurde, beruecksichtigt werden. Trotzdem hatte es noch eine grosse Spannweite an Werten. Es gab sehr unrealistische Werte als Maximum. Mit einem Boxplot wurde ein passender Cutoff gefunden, wobei die Werte ueber diesem Cutoff unrealistisch sind. Reihen mit diesen Werten wurden entfernnt.

Auch fuer die Wetterdaten mussten noch mehr pre-processing Schritte unternommen werden:
- TODO GIAN: WETTER what did you group there


### Überraschungen und Erkenntnisse

Es war eine Ueberraschung, dass die CSV F1 Dateien in einem sehr guten Zustand waren. 
Das CSV Format wurde ueberall eingehalten, es gab keine Duplikate in den Ids, es gab keine Null Values, wo es keine geben soll.
Jede Datei hatte eine Id und die Verlinkung unter den Datensets ist ebenfalls korrekt mit den Fremdschluesseln.

Ebenfalls war es ueberraschend, dass es recht viele Redundanzen gab. Zum einen kam die Kolonne 'number' immer wieder vor um den Fahrer zu beschreiben, obwohl auch die 'driverId' als Kolonne existierte. Ebenfalls was es ueberraschend, dass die 'position' immer mehrmals vorkam in verschiedenen Datentypen oder als leicht anderer Wert: position (string), positionText (string), positionOrder (integer)

Es war interessant zu sehen, dass es Probleme gab mit den Pitstops. Das eine Problem, war das Vertauschen von zwei Kolonnen. Das andere Problem mit den starken Outliern, war auch auf der offiziellen FIA Seite falsch. Es ist interessant zu sehen, dass auch eine riesige Organisation wie die FIA noch Fehlere in ihren Daten hat.

#### Domainspezifische Überraschungen

Es war sehr interessant, wie sich Formel 1 ueber die Jahre veraendert hat. Vor allem wie viele Fahrer in frueheren Saisons gefahren sind, waehrend es heutzutage selten 25 oder mehr Fahrer gibt pro Saison.

## Learnings aus dem Prototyping

Was wir aus dem Prototyping gelernt haben, ist wie wichtig es ist sich Daten anzuschauen, bevor man sie benutzt, vor allem in 'unregulierten' Dateiformaten wie CSV Dateien. Es koennen immer Fehler auftreten bei der Bereitstellung von Daten.

Ebenfalls ist dieser Schritt auch sonst sehr hilfreich bei weiteren Analysen. Bei einem ersten Protoyping wird ein erster Kontakt mit den Daten hergestellt und die Daten werden kennengelernt. Das Schema wird vertrauter und auch die potentiellen Mängel sind bekannt und koennen bei weiteren Analysen beruecksichtigt werden.
