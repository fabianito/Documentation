1. android tools installieren (gpg, fastboot) (dnf install android-tools.x86_64)
2. Telefon in den Fastboot Modus versetzten (Komplett ausschalten - wieder einschalten mit "Volume down + Powerbutton" bis das "Reparatur Männchen" zu sehen ist
3. MIUI herunterladen (http://en.miui.com/a-234.html)
4. fastboot devices (sollte ein Gerät anzeigen):
5. 
[root@localhost  fastboot devices

4ea807e7d530	fastboot

5.  Dann in das Verzeichnis des Images wechsel
6. ./fastboot-exept-data_storage.sh ausführen und beten 😃
7. Bei Version 7.9.x heißt es "flash_all_except_data_storage.sh" 
