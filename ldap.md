#### Hinweis:


- oft wird die Konfiguration nicht akzeptiert, ursache ist das meistens am Ende einer Zeile noch ein Leerzeichen ist.
  entfernen und gut (:set list)
- Nach dem deinstallieren unter Centos, müssen die Zertifikate wieder selbst erstellt werden (make-dummy.sh slapd.pem) und in der ldap.conf angegeben werden:
<ldap.conf>
````
TLS_CACERTDIR   /etc/openldap/certs/
````
Um das über ein Skript zu erledigen:
<makenewldapcert.sh>
````
#!/bin/bash

NAME="ldapserver"
CERT=/etc/openldap/certs/$NAME.crt
KEY=/etc/openldap/certs/$NAME.key

openssl req -new -x509 -nodes -out $CERT -keyout $KEY -days 1460

chown ldap:ldap $CERT $KEY

ldapmodify -Y EXTERNAL -H ldapi:/// -f <<EOF
dn: cn=config
changetype: modify
replace: olcTLSCertificateFile
olcTLSCertificateFile: $CERT

dn: cn=config
changetype: modify
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: $KEY
EOF
````
