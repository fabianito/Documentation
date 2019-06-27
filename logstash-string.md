Feld verändern:

Das muss am Anfang stehen - vor dem Grok


```
filter {

  mutate {
   add_field => {"rechner" => "%{[host][name]}" }
   add_field => { "rechnerA.name" => "" }
   add_field => { "Servername" => "%{[host][name]}" }
  }


  mutate { 
   copy => {"[host][name]" => "rechnerA.name" }
   }


  mutate {
    gsub => [ "rechner", ".*", "Geheim" ]
    gsub => [ "Servername", ".*", "Geheim" ]
    gsub => [ "rechnerA.name", ".*", "Testgeheim" ]
   }
```


Älter:
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
