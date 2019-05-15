#### Übersicht

Server
  Informix
Client
  dbaccess
  
  
### Befehle

- dbaccess <datenbank> file.sql
- echo 'select * from ...' | dbaccess <datenbank>
- onmode -kuy   | *Datenbank stoppen - ohne Nachfrage*
- oninit        | *Daten starten*
- oninit -i     | *Datenbank neu initialisieren*
- onstat -g his | *trace log anzeigen*
- dbschema -d <databank> | *show create" 


### Remote
 - Dazu muss die Remote Datenbank in der sqlhosts.??? eingetragen sein
 - Kein Neustart nach der Änderung der Datei notwendig
 - Der connect to kann entweder über ein file oder per echo übergeben werden
    - echo "connect to @<datenbank im sqlhosts file> user 'xxxx'" | dbaccess <datenbank>
 - Das Passwort geht nur über ein File, nicht per echo



### Dateien
```
/opt/Informix.../etc/onconfig.<datenbankinstanzname>
/opt/Informix.../etc/sqlhosts.<datenbankinstanzname>
``` 

#### Beispiel:
```
sqlhosts.<name>
Datenbankname protokoll host port
ol_informix1210 onsoctcp 127.0.0.1 1526
...
``` 

protokoll:
on
soc|pipe|..
tcp|pipe


### Logging
Das logging kann man per set für die Session einstellen oder per ondblog bzw. ontabe für die Datenbank allgemein
```` 
ontape -s -L 0 -U security
````
Bei mir hat nur ontape funktioniert - obwohl IBM schreibt das es auch nur ondblog verwendet.


### Tracing
#### lokales Tracing
Um das Tracing zu aktivieren gibt es zwei Möglichkeiten:
- per sql
  - sysadmin Datenbank muss vorhanden sein, nur diese hat die Prozedur "task"
  - *EXECUTE FUNCTION task("set sql tracing on", 1000, 1,"high","global"); *
- per Datei
  - In der Konfigurationsdatei "onconfig.<datenbankinstanzname>" gibt es ein Teil für das Tracing
  - Der Vorschlag kann übernohmmen werden:
    - SQLTRACE level=high,ntraces=1000,size=2,mode=global
  
#### Netzwerk Tracing
  Das Netzwerktracing kann mit onmode eingeschalten werden.
  Vorgang laut IBM:
  
  - Instanz starten
  - onmode -p +1 sql_dbg    # Debug einschalten
  - Anwendung starten
  - onmode -p -1 sql_dbg    # Debug ausschalten
  - sqliprim -o ausgabedatei.txt sqli.<process-id>   # In ein lesbares Format umwandeln
  
  Achtung: Es wird erst eine null-byte Datei angelegt und wenn der Process beendet ist wird die Datei aus dem buffer gefüllt.
  Deshalb war es notwenig den Websphere Server zu stoppen!
  
#### Zugriff
- per *onstat -g his* kann auf die Traces zugegriffen werden

