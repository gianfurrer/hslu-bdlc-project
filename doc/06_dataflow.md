# Dataflow

Erklären Sie, wie die Daten End-to-End durch das System laufen.
Wie speichern Sie die Rohdaten?
Haben Sie die Daten partitioniert?
In welchem Format haben Sie die prozessierten Daten gespeichert. Wieso?

Auf folgender Grafik ist der Datenfluss dargestellt.

![Ressourcen Autfeilung](assets/dataflow.png)

Die Dateien werden geladen in dem Collect Data Skript. Die Formel 1 Daten werden als Zip Datei geladen und entpackt. Die CSV Dateien darin werden gespeichert.
Die Wetter Daten werden direkt als CSV Datei geladen.

Alls CSV Daten werden zu einem Parquet transformiert im Overwrite Modus, da die Daten statisch sind.

Diese Parquet Dateien werden in dem EDA Skript geladen, einzeln untersucht und gegebenfalls angepasst.
Falls die Daten fuer die Analyse benoetigt werden, werden sie als neue Parquet Files gespeichert mit dem 'cleaned' Suffix.
Dies geschieht ebenfalls wieder in dem Overwrite Modus. In einem real life Projekt, wuerden diese Dateien versioniert werden und nicht einfach uebrschrieben.
Das pre-processing hat als Ziel, dass nur die benoetigten Daten gespeichert werden muessen und die Daten werden als Parquet gespeichert, um Speicher zu sparen und das Laden zu beschleunigen.

Die Daten sind nun in der Form gespeichert, wie auf der folgenden Grafik dargestellt. 

_Note: Die Daten wurden nicht partitioniert gespeichert, weil pro Analyse nicht die selben Partitionen noetig sind. Die Daten werden jedoch in Memory partitioniert in den einzelnen Analysen, falls dies hilfreich ist._

![Ressourcen Autfeilung](assets/cleaned-data-schema.png)

Die 'cleaned' Parquet Dateien werden in der Analyse geladen, wo sie gebraucht werden.
