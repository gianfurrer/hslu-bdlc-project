# Learnings

Im Verlauf von diesem Projekt gab es mehrere Herausforderung. Die erste Herausforderung waren die Pyspark Sessions.
Es konnte jeweils nur eine Pyspark Session initiiert werden, da diese alle Cores beanspruchte.
Dies konnten wir über eine Änderung bei der Spark Konfiguration anpassen (siehe Kapitel [Cluster](./02_cluster.md)). 

Ebenfalls war es eine Herausforderung die Pitstop Daten zu entschlüsseln. Es brauchte Domain Knowledge, um zu sehen, dass die Werte sehr unrealistisch sind. Es war noch schwieriger das Problem zu finden, da die gesamte Zeit, die das Auto in der Pitlane verbringt, gemessen wird. Die meisten anderen Ressourcen, die verwendet hätten werden können, um die Werte zu prüfen, speichern nur die Zeit, die das Auto effektiv bei den Mechanikern verbringt.
Die Daten konnten schlussendlich aufgeräumt werden, jedoch dauerte dies recht lange aus den oben genannten Gründen.

Wir denken, dass das Projekt grundsaetzlich gut gelaufen ist und sind auch zufrieden mit dem Resultat. Was wir trotzdem anders machen würden, ist die Planung, bevor das Projekt begonnen wurde. Wir haben die Fragenstellungen definiert, das Data Cleansing gemacht und dann bei den Analysen haben wir direkt begonnen.
Es gab keine wirkliche Planungsphase, wie die Analysen am besten umgesetzt werden könnten oder inwiefern dieselben Schritte durchgeführt werden sollten pro Analyse.
Das Resultat war zwar gut aber recht durcheinander.
Deswegen musste noch zusätzliche Zeit nach der eigentlichen Analyse aufgewendet werden, um diese besser zu strukturieren. In einem nächsten Projekt, würden wir uns zuerst gemeinsam Gedanken machen, wie die einzelnene Schritte der Analyse aufgebaut sein sollten und nicht erst danach.

Das für uns wichtigste Takeaway war, wie wichtig es ist die Daten immer zu prüfen. Unsere Daten waren in einem sehr guten Zustand und die Informationen stammen ursprünglich von einer sehr grossen Organisation, der FIA. Trotzdem haben wir noch Fehler gefunden, da dies immer passieren kann. Dies kann jedoch behoben werden, wenn genug Zeit in das Data Cleansing gesteckt werden kann. Diese Schritt darf nicht übersprungen oder verkürzt werden.

Im Rahmen dieses Projektes konnten wir unsere Fähigkeiten mit Daten umzugehen ausbauen. Wir haben mit neuen Technologien gearbeitet und sind nun besser dafür ausgrüstet mit grossen Datenmengen zu arbeiten.