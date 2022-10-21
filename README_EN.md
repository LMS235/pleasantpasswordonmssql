# Pleasant Password Server - MSSQL Server Tuning

With larger password databases and multiple users in the Pleasant Password Server on the MSSQL Server, especially the web interface is very slow.

Unfortunately the Plesant site itself only [explains](https://pleasantpasswords.com/info/pleasant-password-server/b-server-configuration/4-changing-databases-for-pleasant-password-server#4) how to switch to the MSSQL server, but not the steps necessary for the MSSQL server to increase the performance.

Here I have collected some points for the performance optimization which bring a clear boost and provide for delay-free navigation through the passwords in the web interface.

**BACKUP (DATA and LOG backup) must of course be set up separately!** For this, it is best to refer to the [MS Help](https://learn.microsoft.com/de-de/troubleshoot/sql/admin/schedule-automate-backup-database).

## Database settings

### Data files and log files

After switching the database to the MSSQL server, set the data and log files to a suitable size to prevent constant autogrow.

![image](https://user-images.githubusercontent.com/8693550/197147107-29f9301d-b7f1-4a44-9b3d-bbf15d9064e1.png)

Via the "Disk Usage" report you can see the current utilization - there should always be enough space here.

![image](https://user-images.githubusercontent.com/8693550/197147421-79a1d929-89bc-41e1-a106-79949ed1acd4.png)

### Set asynchronous statistics

Enable Asynchronous statistics in the database options, this is one of the most important points!

![image](https://user-images.githubusercontent.com/8693550/197147974-35f57d3f-191a-4f36-8dca-e4b5afd1a71a.png)

## Missing indexes

With the script find-missing-indexes.sql ([source](https://sqlnuggets.com/sql-scripts-how-to-find-missing-indexes/)) missing indexes can be found and created (it is best to run the script after the database has been in use for a while, because the data will be reset after a restart).

## Maintenance plans

Maintenance plans should be used to schedule both INDEX reorg and STATISTICS updates.

![image](https://user-images.githubusercontent.com/8693550/197148516-0fdfa71d-133a-4aea-8cf2-ede04e588231.png)

### INDEX reorg

With the script find-index-fragmentation.sql ([source](https://www.sqlshack.com/how-to-identify-and-resolve-sql-server-index-fragmentation/)) the index fragmentation can be displayed, it should not be too bad.
Indexes can be automatically reorganized via a maintenance plan.

Schedule via Maintenance Scheduling Wizard with the following options (recommended daily).

![image](https://user-images.githubusercontent.com/8693550/197148628-c97adf33-5710-4deb-a241-848ab666b9cb.png)

![image](https://user-images.githubusercontent.com/8693550/197148921-da00ca30-3f20-4dc8-aa22-1e18a49887a6.png)

### STATISTICS updates

STATISTIK updates can be executed automatically via a maintenance plan.

Schedule via Maintenance Scheduling Wizard with the following options (recommended daily).

![image](https://user-images.githubusercontent.com/8693550/197149154-94213f08-d8e4-4602-88a1-2d1d7256a38a.png)

![image](https://user-images.githubusercontent.com/8693550/197149264-008ea48d-d43d-49d6-9cb1-4e9d85a7cb68.png)

**MACHINE TRANSLATED!**
