# Pleasant Password Server - MSSQL Server Tuning

Bei größeren Passwort Datenbanken und mehreren Benutzern im Pleasant Passwort Server auf dem MSSQL Server ist gerade die Weboberfläche sehr träge.

Leider wird auf der Pleasant Seite selbst nur der Wechsel auf den MSSQL Server [erklärt](https://pleasantpasswords.com/info/pleasant-password-server/b-server-configuration/4-changing-databases-for-pleasant-password-server#4), jedoch nicht die für den MSSQL Server nötigen Schritte um die Performance zu erhöhen.

Hier habe ich einige Punkte für die Performanceoptimierung zusammengetragen welche einen deutlichen Schub bringen und für verzögerungsfreies Navigieren duch die Kennwörter in der Weboberfläche sorgen.

**BACKUP (DATEN und LOG Backup) müssen natürlich gesondert eingerichtet werden!** Dazu am besten die [MS Hilfe](https://learn.microsoft.com/de-de/troubleshoot/sql/admin/schedule-automate-backup-database) beachten.

## Einstellungen der Datenbank

### Daten-Dateien und Log-Dateien

Nach dem Wechsel der Datenbank auf den MSSQL Server die Daten- und Log-Dateien auf eine passende Größe einstellen um ständiges Autogrow zu verhinden.

![image](https://user-images.githubusercontent.com/8693550/197147107-29f9301d-b7f1-4a44-9b3d-bbf15d9064e1.png)

Über den Bericht "Datenträgerverwendung" sieht man die aktuelle Auslastung - hier sollte immer genug Luft sein.

![image](https://user-images.githubusercontent.com/8693550/197147421-79a1d929-89bc-41e1-a106-79949ed1acd4.png)



### Asynchrone Statistiken einstellen

Bei den Datenbankoptionen die Asynchronen Statistiken aktivieren, das ist einer der wichtigsten Punkte!

![image](https://user-images.githubusercontent.com/8693550/197147974-35f57d3f-191a-4f36-8dca-e4b5afd1a71a.png)


## Fehlende Indexe

Mit dem Script find-missing-indexes.sql ([Quelle](https://sqlnuggets.com/sql-scripts-how-to-find-missing-indexes/)) können Fehlende Indexe gefunden und angelegt werden (den Script am besten ausführen nachdem die Datenbank schon länger in Betrieb ist, da die Daten nach einem Neustart zurückgesetzt werden).

## Wartungspläne

Mit Hilfe der Wartungspläne sollten sowohl INDEX-Reorg als auch STATISTIK-Updates eingeplant werden.

![image](https://user-images.githubusercontent.com/8693550/197148516-0fdfa71d-133a-4aea-8cf2-ede04e588231.png)

### INDEX-Reorg

Mit dem Script find-index-fragmentation.sql ([Quelle](https://www.sqlshack.com/how-to-identify-and-resolve-sql-server-index-fragmentation/)) kann die Index Fragmentierung angezeigt werden, diese sollte nicht zu schlecht sein.
Über einen Wartungsplan können die Indexe automatisch reorganisiert werden.

Via Wartungsplanungs-Assistent mit folgenden Optionen einplanen (Empfehlung täglich).

![image](https://user-images.githubusercontent.com/8693550/197148628-c97adf33-5710-4deb-a241-848ab666b9cb.png)

![image](https://user-images.githubusercontent.com/8693550/197148921-da00ca30-3f20-4dc8-aa22-1e18a49887a6.png)

### STATISTIK-Updates

Über einen Wartungsplan können STATISTIK-Updates automatisch ausgeführt werden.

Via Wartungsplanungs-Assistent mit folgenden Optionen einplanen (Empfehlung täglich).

![image](https://user-images.githubusercontent.com/8693550/197149154-94213f08-d8e4-4602-88a1-2d1d7256a38a.png)

![image](https://user-images.githubusercontent.com/8693550/197149264-008ea48d-d43d-49d6-9cb1-4e9d85a7cb68.png)


