### Auf dem Apache Server:

##### apche conf:
```
CustomLog "|/usr/bin/logger -t apache -p local6.info" combined
``` 

#### rsyslogd.conf:

Auskommentieren oder einfügen:

```
$ActionQueueFileName fwdRule1 # unique name prefix for spool files
$ActionQueueMaxDiskSpace 1g   # 1gb space limit (use as much as possible)
$ActionQueueSaveOnShutdown on # save messages to disk on shutdown
$ActionQueueType LinkedList   # run asynchronously
$ActionResumeRetryCount -1

local6.* @@10.19.60.91:514
```


### Auf dem rsyslogd Server:

Im einfachsten Fall unter "Rules":
```
local6.*                        /var/log/httpd/httpd-access.log
```

Oder:
```
# Write all apache logs to this file (please note the comma)
#$template apacheAccess,"/var/log/httpd/apache_access_log"

# If the log's tag is "apache" and matches
# the defined level, send it to a specific file
#if $syslogtag == 'apache' then {
#    local6.info ?apacheAccess
#    & ~
#}
```

