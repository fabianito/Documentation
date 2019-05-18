Create Statement extrahieren:

```
cat minisql.sql |tr -s "\n" "Z"|grep -o "create table.*;"|tr -s "Z" "\n"
```
