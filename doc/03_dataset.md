# Dataset

Es wurde ein Formel 1 Datenset gewaehlt, dass als CSV von Kaggle heruntergeladen wurde: https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020 

Das Datenset besteht aus mehreren CSV Dateien, die wie in einer Datenbank mit Fremdschluesseln aufeinander verweisen.
Es wurde gewaehlt, da es sehr umfangreiche Daten ueber Formel 1 enthaelt ueber die unterschiedlichen Teile des Sportes. Beispielsweise gibt es nicht nur Informationen zu den Rennen, sondern auch zu den Pitstops. Ebenfalls gibt es Daten ueber eine lange Zeitspanne (1950-2024), was noetig ist, fuer die gewaehlten Fragestellungen, da diese alle eine Entwicklung ueber Zeit untersuchen wollen.

TODO GIAN _Wie haben Sie die Daten ins HDFS geladen? Musste die Blocksize von HDF angepasst werden?_

Das Projekt ist kein Big-Data Problem, da es sich um wenig Daten handelt (~20MB). Was Big Data auszeichnet, ist, dass die normalen Algorithmen, die fuer kleinere Datenmengen funktionieren, nicht mehr funktionieren aufgrund der Groesse des Datensets. Dies ist hier nicht der Fall. Das Projekt wird jedoch so durchgefuehrt, als ob es sich um ein Big Data Problem handeln wuerde: Die Rechenleistung wird verteilt auf mehrere Maschinen und verteilt genutzt mit PySpark.

## Dataset Dateien

Jede CSV Datei hat einen Primary Key und die Dateien referenzieren sich gegenseitig ueber Foreign Keys. Dies ist ein Ueberblick ueber die Dateien:
- curcuits.csv: Alle Strecken, die in Formel 1 je befahren wurden, und wo sie sich befinden.
- constructor_results.csv: Punkte, die ein Team in einem Rennen erhalten hat.
- constructor_standings.csv: Pro Rennen, die Punkte, die ein Team bis zu diesem Zeitpunkt in der Saison erreicht hat.
- constructors.csv: Alle Teams, die je in Formel 1 gefahren sind.
- driver_standings.csv: Pro Rennen, die Punkte, die ein Fahrer bis zu diesem Zeitpunkt in der Saison erreicht hat.
- drivers.csv: Alle Fahrer, die je in Formel 1 gefahren sind.
- lap_times.csv: Alle Zeiten, die pro Runde gefahren wurden sind und die Position, die der jeweilige Fahrer im Rennen erreicht. 
- pit_stops.csv: Alle Pitstops mit Dauer, in welchem Lap er durchgefuehrt wurde und der wievielte Stop des Rennens es war.
- qualifying.csv: Alle Resultate pro Fahrer im Qualifying, pro Q1, Q2 und Q3, falls diese gefahren wurden.
- races.csv: Alle Rennen in Formel 1, wo und wann diese waren und wann die Practices and Qualifyings dafuer waren.
- results.csv: Alle Resultate pro Rennen pro Fahrer.
- seasons.csv: Alle Saisons von Formel 1, enthaelt lediglich das Jahr und eine URL zu der Wikipedia Seite der Saison.
- sprint_results.csv: Alle Resultate pro Sprint-Rennen pro Fahrer.
- status.csv: Status, die ein Fahrer erreichen kann in einem Rennen (e.g. Finished, Accident, Disqualified, ...)


Die nachfolgende Liste zeigt eine detaillierte Auslistung von allen einzelnen CSV Dateien, mit allen Feldern, Datentypen und falls noetig einer Beschreibung des Feldes.

_Note: Alle 'points' sind floats, da halbe Punkte verteilt werden koennen in seltenen Faellen._

**circuits.csv:**
- circuitId: PK
- circuitRef: String (eindeutiger Name fuer Strecke)
- name: String
- location: String
- country: String
- lat: Float
- lng: Float
- alt: Integer (Altidue)
- url: String (Wikipedia Link zu Strecke)

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
- constructorRef: String (eindeutige Bezeichung des Teams)
- name: String
- nationality: String (jedes Team hat eine Nationalitaet)
- url: String (Wikipedia Link zu Team)

**driver_standings:**
- driverStandingsId: PK
- raceId: FK
- driverId: FK
- points: Float (Punkte, die Fahrer erreicht hat in Saison bis zu diesem Rennen)
- position: Integer (Position des Fahrers in Rangierung)
- positionText: String (Position des Fahrers in Rangierung als String)
- wins: Integer (Anzahl Wins eines Fahrers in dieser Saison)


**drivers:**
- driverId: PK
- driverRef: String (eindeutige Bezeichnung des Fahrers)
- number: Integer (jeder Fahrer hat eine zugewiesene Nummer, Nummer ist normalerweise nur besetzt, wenn momentan ein Fahrer damit faehrt (gibt 'retired' Nummern, von z.B. Fahrern, die bei einem Rennen gestorben sind), Fahrer kann seine Nummer zu 1 wechseln, falls er die Championship gewonnen hat in der Saison zuvor)
- code: Sting (3 Buchstaben Code pro Fahrer, meist erste drei Buchstaben des Nachnamens)
- forename: Stirng
- surname: String
- dob: Date (Geburtsdatum: **D**ate **o**f **B**irth)
- nationality: String (Nationalitaet des Fhrers, Land, fuer das der Fahrer faehrt)
- url: String (Wikipedia Link zu Fahrer)


**lap_times:**
- raceId: FK
- driverId: FK
- lap: Integer (Rennrunde, fuer die die Zeit gilt)
- position: Integer (Position des Fahrers am Ende des Rennnes)
- time: String (Zeit der Runde in Format minute:sekunde:hunderstel)
- milliseconds: Integer (Zeit der Runde in Millisekunden)

**pit_stops:**
- raceId: FK
- driverId: FK
- stop: Integer (wievielter Stop in diesem Rennen)
- lap: Integer (bei welchem Lap wurde der Stop durchgefuehrt)
- time: String (xx:xx:xx, wann der Pitstop durchgefuehrt wurde)
- duration: Float (Zeit, die in der Pitlane verbracht wurde in Sekunden)
- milliseconds: Integer (Zeit, die in der Pitlane verbracht wurde in Millisekunden)


**qualifying:**
- qualifyId: PK
- raceId: FK
- driverId: FK
- constructorId: FK
- number: Integer (Nummer des Fahrers)
- position: Integer (Finale Poistion des Fahrers im Qualifying) 
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
- grid: Integer (Starposition)
- position: Integer (Finale Position des Fahrers, falls das Rennen beendet wurde)
- positionText: String (Finale Position als Text)
- positionOrder: Integer (Finale Position, inklusive Fahrer, die waehrend dem Rennen ausgeschieden sind)
- points: Float (Punkte, die der Fahre in diesem Rennen sammelt)
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
- positionOrder: Integer (Finale Position, inklusive Fahrer, die waehrend dem Rennen ausgeschieden sind)
- points: Float (Punkte, die Fahrer in diesem Sprintrennen erhalten hat)
- laps: Integer (Anzahl Runden in diesem Sprintrennen)
- time: String (Zeit, die der Fahrer fuer dieses Sprintrennen gebraucht hat)
- milliseconds: Integer (Zeit, die der Fahrer fuer dieses Sprintrennen gebraucht hat, in Millisekunden)
- fastestLap: Integer (Runde, die am schnellsten war)
- fastestLapTime: String (Dauer der schnellsten Runde in Sekunden)
- statusId: FK

**status:**
- statusId: PK
- status: String