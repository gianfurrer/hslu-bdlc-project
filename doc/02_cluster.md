# Cluster

Die folgende Grafik zeigt die Cluster-Topologie als Bild. Es istersichtlich wie die Server heisst, welche Servieces auf welche Maschine laufen und wie die Ressourcne aufgeteilt sind.

![Ressourcen Autfeilung](assets/aufteilung-ressouren.png)


Jeder Spark Applikation werden 4 Cores zugewisen:

```bash
su - cluster
echo "spark.cores.max 4" >> spark/conf/spark-defaults.conf
```

TODO GIAN:

- Erklären Sie, warum Sie sich für diese Topologie entschieden haben und warum Sie die eingesetzten Frameworks und Tools benutzen.
- Wo gab es Probleme, was haben Sie neu dazugelernt?
- Wie haben Sie getestet, dass die Services funktionieren?



---
