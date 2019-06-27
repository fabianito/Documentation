Feld verändern:

Das muss am Anfang stehen - vor dem Grok

```
mutate { add_field => { "Servername" => "%{[host][name]}" } }
mutate {
     gsub => [ "Servername", ".*", "Geheim" ]
```     
     
oder:
```
mutate { add_field => { "rechnerA.name" => "" } }
mutate { 
     copy => {"[host][name]" => "rechnerA.name" }
     }
mutate { gsub => [ "rechnerA.name", ".*", "Testgeheim" ] }
```
