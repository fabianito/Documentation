## Logstash

### Mapping mit Regex und Integer
```
input {
 file {
   path => "/var/log/logstash/tcpping-output.log"
   start_position => beginning
   }
}


filter {
      grok {
        patterns_dir => ["/etc/logstash/conf.d/patterns"]
        match => [ "message", ["%{PROTOCOL:protocol} %{STATUS:status} - %{SECOND} (?<text>[a-z].*) %{DEST:Destination} [a-z].* %{PORT:Port:int}.[a-z]*\=(?<time>[0-9].*)"] ]
  }
 }



output {
  elasticsearch {
    hosts => ["localhost:9200"]
    manage_template => false
    index => "nagios"
  }
}
```

Dazu ein Verzeichnis names "patterns" und dort ein File mit den pattern:
```
PROTOCOL [A-Z]{3}
STATUS [A-Z]{2,4}
SECOND [0-9].[0-9]{3}
PORT [0-9]{3}
DEST [0-9].[0-9].[0-9].[0-9]
```
