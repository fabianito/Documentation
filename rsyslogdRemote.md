## Client
### rsyslogd
#### /etc/rsyslog.d/apacheLexcom.conf
```
$ModLoad imfile

# Default Apache Error Log
$InputFileName /var/log/httpsd/dev.partslink24.com/error/error.log
$InputFileTag httpd-error-default:
$InputFileStateFile stat-httpd-error
$InputFileSeverity notice
$InputFileFacility local3
$InputRunFileMonitor

# Default Apache Access Log
$InputFileName /var/log/httpsd/dev.partslink24.com/access/access.log
$InputFileTag httpd-access-default:
$InputFileStateFile stat-httpd-access
$InputFileSeverity notice
$InputFileFacility local3
$InputRunFileMonitor

$InputFileName /var/log/m2.log
$InputFileTag testlog:
$InputFileStateFile testlog
$InputFileSeverity notice
$InputFileFacility local3
$InputRunFileMonitor


$InputFilePollInterval 2
```
#### /etc/rsyslog.conf
```
local3.* /var/log/apache-access.log
local3.* @@10.19.60.91:514
```

## Server
### rsyslogd
#### /etc/rsyslog.conf
```
#### RULES ####

$template RemoteHost,"/var/log/remote/%HOSTNAME%/%$YEAR%/%$MONTH%-%$DAY%.log"

if $hostname contains 'tortuga-en-01' then {
*.* -?RemoteHost
}

local6.*                        /var/log/httpd/httpd-access.log
```




