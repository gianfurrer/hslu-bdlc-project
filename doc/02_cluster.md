# Cluster

- Zeigen Sie die Cluster-Topologie als Bild. Wie heissen die Server, welche Services laufen auf welcher Maschine.
- Zeigen Sie, wie Sie die Ressourcen (CPU / Memory) aufgeteilt haben.


Jeder Spark Applikation werden 4 Cores zugewisen:

```bash
su - cluster
echo "spark.cores.max 4" >> spark/conf/spark-defaults.conf
```

- Erklären Sie, warum Sie sich für diese Topologie entschieden haben und warum Sie die eingesetzten Frameworks und Tools benutzen.
- Wo gab es Probleme, was haben Sie neu dazugelernt?
- Wie haben Sie getestet, dass die Services funktionieren?

---
