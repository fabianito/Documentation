### Documentation

```
git add *
git commit 
git push
```


#### Password 
Um das Passwort zu speichern gibt es ein git-credential helper
```
git config credential.helper store 
git push
```
Beim push wird das Password abgefragt und gleich gespeichert, somit entfällt beim nächsten push die Abfrage.
