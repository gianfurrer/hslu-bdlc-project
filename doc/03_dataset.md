# Dataset

Es wurde ein Formel 1 Datenset gewählt, das als CSV von Kaggle heruntergeladen wurde: https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020.

Das Datenset besteht aus mehreren CSV Dateien, die wie in einer Datenbank mit Fremdschlüsseln aufeinander verweisen.
Es wurde gewählt, da es sehr umfangreiche Daten über Formel 1 enthält über die unterschiedlichen Aspekte des Sportes. Beispielsweise gibt es nicht nur Informationen zu den Rennen, sondern auch zu den Pitstops. Die Daten sind üeber eine lange Zeitspanne (1950-2024), was nötig ist, für die gewählten Fragestellungen, da diese die Entwicklung des Sportes über Zeit untersuchen wollen.

Zusätzlich wurde auch ein Wetter Datenset verwendet. Dies besteht lediglich aus einer CSV Datei und wurde ebenfalls von Kaggle heruntergeladen: https://www.kaggle.com/datasets/quantumkaze/f1-weather-dataset-2018-2023.

Diese Datei enthält neben den Wetterdaten das Jahr und die Rennrunde, da es ein Wetterdatenset spezifisch für die Formel 1 ist. Mit der Rennrunde und dem Jahr kann es mit dem Formel 1 Dataset verknüpft werden, da dieses Datenset zu dem ersten Datenset passt wurde es gewählt. 

Die Daten wurden ins HDFS geladen mit dem folgenden Command. Die Blocksize musste nicht angepasst werden.

```bash
hdfs dfs -Ddfs.replication=1 -put -f /data/dataset_cluster/f1/*.csv /f1/raw/

```
Die Daten werden nur mit einer Kopie pro Block gespeichert. Sie werden an die Destination im HDFS kopiert und falls es bereits Dateien mit dem selben Namen gibt, werden diese überschrieben. Dies ist möglich, da die Daten statisch sind und nur einmalig extrahiert und kopiert werden.

Das Projekt ist kein Big-Data Problem, da es sich um wenig Daten handelt (~20MB). Was Big Data auszeichnet, ist, dass die normalen Algorithmen, die für kleinere Datenmengen funktionieren, nicht mehr funktionieren aufgrund der Grösse des Datensets. Dies ist hier nicht der Fall. Das Projekt wird jedoch so durchgeführt, als ob es sich um ein Big Data Problem handeln würde: Die Rechenleistung wird verteilt auf mehrere Maschinen und verteilt genutzt mit PySpark.

## F1 Dataset - Schema

Jede CSV Datei hat einen Primary Key und die Dateien referenzieren sich gegenseitig über Foreign Keys. Dies ist sind die Dateien:

- curcuits.csv: Alle Strecken, die in Formel 1 je befahren wurden, und wo sie sich befinden.
- constructor_results.csv: Punkte, die ein Team in einem Rennen erhalten hat.
- constructor_standings.csv: Pro Rennen, die Punkte, die ein Team bis zu diesem Zeitpunkt in der Saison erreicht hat.
- constructors.csv: Alle Teams, die je in Formel 1 gefahren sind.
- driver_standings.csv: Pro Rennen, die Punkte, die ein Fahrer bis zu diesem Zeitpunkt in der Saison erreicht hat.
- drivers.csv: Alle Fahrer, die je in Formel 1 gefahren sind.
- lap_times.csv: Alle Zeiten, die pro Runde gefahren worden sind und die Position, die der jeweilige Fahrer am Ende im Rennen erreicht. 
- pit_stops.csv: Alle Pitstops mit Dauer, in welchem Lap er durchgeführt wurde und der wievielte Stop des Fahrers im Rennen es war.
- qualifying.csv: Alle Resultate pro Fahrer im Qualifying, pro Q1, Q2 und Q3, falls diese gefahren wurden.
- races.csv: Alle Rennen in Formel 1, wo und wann diese waren und wann die Practices and Qualifyings dafür waren.
- results.csv: Alle Resultate pro Rennen pro Fahrer.
- seasons.csv: Alle Saisons von Formel 1, enthält lediglich das Jahr und eine URL zu der Wikipedia Seite der Saison.
- sprint_results.csv: Alle Resultate pro Sprint-Rennen pro Fahrer.
- status.csv: Status, die ein Fahrer erreichen kann in einem Rennen (e.g. Finished, Accident, Disqualified, ...).


Die nachfolgende Liste zeigt eine detaillierte Auflistung von allen einzelnen CSV Dateien, mit allen Feldern, Datentypen und falls nötig einer Beschreibung des Feldes.

_Note: Alle 'points' sind floats, da halbe Punkte verteilt werden können in seltenen Fällen._

**circuits.csv:**
- circuitId: PK
- circuitRef: String (Eindeutiger Name für Strecke)
- name: String
- location: String
- country: String
- lat: Float
- lng: Float
- alt: Integer (Altitude)
- url: String (Wikipedia Link zur Strecke)

**constructor_results:**
- constructorResultsId: PK
- raceId: FK
- constructorId: PK
- points: Float
- status: Unklar, sind alle \N

**constructor_standings:**
- constructorStandingsId: PK
- raceId: FK
- constructorId: FK
- points: Float
- position: Integer (Teamposition in Rangierung)
- positionText: String  (Teamposition in Rangierung als String)
- wins: Integer (Anzahl Wins in der Saison pro Team)


**constructors:**
- constructorId: PK
- constructorRef: String (Eindeutige Bezeichung des Teams)
- name: String
- nationality: String (Jedes Team hat eine Nationalität)
- url: String (Wikipedia Link zum Team)

**driver_standings:**
- driverStandingsId: PK
- raceId: FK
- driverId: FK
- points: Float (Punkte, die ein Fahrer erreicht hat in der Saison bis zu diesem Rennen)
- position: Integer (Position des Fahrers in Rangierung)
- positionText: String (Position des Fahrers in Rangierung als String)
- wins: Integer (Anzahl Wins eines Fahrers in dieser Saison)


**drivers:**
- driverId: PK
- driverRef: String (Eindeutige Bezeichnung des Fahrers)
- number: Integer (Jeder Fahrer hat eine zugewiesene Nummer, Nummer ist normalerweise nur besetzt, wenn momentan ein Fahrer damit fährt, also nicht einzigartig, Fahrer kann seine Nummer zu 1 wechseln, falls er die Championship gewonnen hat in der Saison zuvor)
- code: Sting (3 Buchstaben Code pro Fahrer, meist erste drei Buchstaben des Nachnamens)
- forename: String
- surname: String
- dob: Date (Geburtsdatum: **D**ate **o**f **B**irth)
- nationality: String (Nationalität des Fahrers, Land, füe das der Fahrer fährt)
- url: String (Wikipedia Link zum Fahrer)


**lap_times:**
- raceId: FK
- driverId: FK
- lap: Integer (Rennrunde)
- position: Integer (Position des Fahrers am Ende des Rennnes)
- time: String (Zeit der Runde in Format Minute:Sekunde:Hundertstel)
- milliseconds: Integer (Zeit der Runde in Millisekunden)

**pit_stops:**
- raceId: FK
- driverId: FK
- stop: Integer (Wievielter Stop in diesem Rennen)
- lap: Integer (Bei welchem Lap wurde der Stop durchgeführt)
- time: String (xx:xx:xx, wann der Pitstop durchgeführt wurde)
- duration: Float (Zeit, die in der Pitlane verbracht wurde, in Sekunden)
- milliseconds: Integer (Zeit, die in der Pitlane verbracht wurde, in Millisekunden)


**qualifying:**
- qualifyId: PK
- raceId: FK
- driverId: FK
- constructorId: FK
- number: Integer (Nummer des Fahrers)
- position: Integer (Finale Position des Fahrers im Qualifying) 
- q1: String (xx:xx:xx, beste Zeit im ersten Qualifying)
- q2: String (xx:xx:xx, beste Zeit im zweiten Qualifying)
- q3: String (xx:xx:xx, beste Zeit im dritten Qualifying)

**races:**
- raceId: PK
- year: Integer
- round: Integer (das wievielte Rennen es ist in dieser Saison)
- circuitId: FK
- name: String (Name der Strecke)
- date: Date
- time: String (Uhrzeit des Rennstarts)
- url: String (Wikipedia Link zum Rennen)
- fp1_date: Date (Datum des ersten Practices)
- fp1_time: String (Uhrzeit des ersten Practices)
- fp2_date: Date (Datum des zweiten Practices)
- fp2_time: String (Uhrzeit des zweiten Practices)
- fp3_date: Date (Datum des dritten Practices)
- fp3_time: String (Uhrzeit des dritten Practices)
- quali_date: Date (Datum des Qualifyings)
- quali_time: String (Uhrzeit des Qualifyings)
- sprint_date: Date (Datum des Sprintraces, falls es eines gab)
- sprint_time: String (Uhrzeit des Sprintraces, falls es eines gab)

**results:**
- resultId: PK
- raceId: FK
- driverId: FK
- constructorId: FK
- number: Integer (Nummer des Fahrers)
- grid: Integer (Startposition)
- position: Integer (Finale Position des Fahrers, falls das Rennen beendet wurde)
- positionText: String (Finale Position als Text)
- positionOrder: Integer (Finale Position, inklusive Fahrer, die während dem Rennen ausgeschieden sind)
- points: Float (Punkte, die der Fahrer in diesem Rennen sammelt)
- laps: Integer (Anzahl Runden des Rennens)
- time: String (Dauer des Rennens)
- milliseconds: Integer (Dauer des Rennens in Millisekunden)
- fastestLap: Integer (Runde, die am schnellsten war)
- rank: Integer (Position des eigenen schnellsten Laps in Rangierung von allen schnellsten Runden)
- fastestLapTime: String (Dauer der schnellsten Runde in Sekunden)
- fastestLapSpeed: Float (Geschwindigkeit der schnellsten Runde)
- statusId: FK

**seasons:**
- year: Integer
- url: String (Wikipedia Link zu Saison)

**sprint_results:**
- resultId: PK
- raceId: FK
- driverId: FK
- constructorId: FK
- number: Integer (Nummer des Fahrers)
- grid: Integer (Finale Position des Fahrers, falls das Rennen beendet wurde)
- position: Integer (Finale Position als Text)
- positionText: String (Finale Position als Text)
- positionOrder: Integer (Finale Position, inklusive Fahrer, die während dem Rennen ausgeschieden sind)
- points: Float (Punkte, die Fahrer in diesem Sprintrennen erhalten hat)
- laps: Integer (Anzahl Runden in diesem Sprintrennen)
- time: String (Zeit, die der Fahrer für dieses Sprintrennen gebraucht hat)
- milliseconds: Integer (Zeit, die der Fahrer für dieses Sprintrennen gebraucht hat, in Millisekunden)
- fastestLap: Integer (Runde, die am schnellsten war)
- fastestLapTime: String (Dauer der schnellsten Runde in Sekunden)
- statusId: FK

**status:**
- statusId: PK
- status: String

## Wetter Dataset - Schema

Die folgenden Kolonnen existieren im orignialen Wetter Datenset. Es gibt weder Primary Keys, noch Foreign Keys.

**weather**:
- Time: String
- AirTemp: Float (Temperatur in der Luft)
- Humidity: Float
- Pressure: Float
- Rainfall: Boolean
- TrackTemp: Float (Temperatur der Rennstrecke)
- WindDirection: Integer
- WindSpeed: Float
- Round Number: Integer (Rennrunde, in der gemessen wurde)
- Year: Integer
