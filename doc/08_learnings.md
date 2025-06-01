# Learnings

Im Verlauf von diesem Projekt gab es mehrere Herausforderung. Die erste Herausforderung waren die Pyspark Sessions.
Es konnte jeweils nur eine Pyspark Session initiiert werden, da diese alle Cores beanspruchte.
Dies konnten wir über eine Änderung bei der Spark Konfiguration anpassen (siehe Kapitel [Cluster](./02_cluster.md)). 

Ebenfalls war es eine Herausforderung die Pitstop Daten zu entschlüsseln. Es brauchte Domain Knowledge, um zu sehen, dass die Werte sehr unrealistisch sind. Es war noch schwieriger das Problem zu finden, da die gesamte Zeit, die das Auto in der Pitlane verbringt, gemessen wird. Die meisten anderen Ressourcen, die verwendet hätten werden können, um die Werte zu prüfen, speichern nur die Zeit, die das Auto effektiv bei den Mechanikern verbringt.
Die Daten konnten schlussendlich aufgeräumt werden, jedoch dauerte dies recht lange aus den oben genannten Gründen.

Wir denken, dass das Projekt grundsätzlich gut gelaufen ist und sind auch zufrieden mit dem Resultat. Was wir dennoch anders machen würden, ist die Planung vor Beginn des Projekts. 
Wir haben die Fragenstellungen definiert, das Data Cleansing gemacht und dann direkt mit den Analysen begonnen.
Es gab keine wirkliche Planungsphase, in der besprochen wurde, wie die Analysen am besten umgesetzt werden könnten oder welche Schritte pro Analyse durchgeführt werden sollten.
Deshalb enthielten die verschiedenen Analysen unterschiedliche Ansätze und Strukturen.
Um dies zu beheben wurde zusätzliche Zeit nach der eigentlichen Analyse aufgewendet, um eine einheitliche struktur zu erlangen. 
In einem nächsten Projekt, würden wir uns zuerst gemeinsam Gedanken machen, wie die einzelnen Schritte der Analyse aufgebaut sein sollten und nicht erst danach.

Das wichtigste Learning für uns war, wie wichtig die Überprüfung der Daten Qualität ist. Unsere Daten waren allgemein in einem sehr guten Zustand und die Informationen stammen ursprünglich von einer sehr grossen Organisation, der FIA. Dennoch wurden auch in diesen Daten Inkonsistenzen gefunden. Diese inkonsitenzen könnten wir im Data Cleasing beheben, was uns deutlich machte, wie wichtig diese Vorbereitung der Daten ist.

Im Rahmen dieses Projektes konnten wir unsere Fähigkeiten mit Daten umzugehen ausbauen. Wir haben mit neuen Technologien gearbeitet und sind nun besser dafür ausgrüstet mit grossen Datenmengen zu arbeiten.