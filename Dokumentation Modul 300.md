# Dokumentation Modul 300

Dies ist eine Dokumentation zur Arbeit im Modul 300 von Cédric Droz.

## Initielle Idee:

Ich werde einen LAMP-Stack vollautomatisiert ausetzen lassen. Ich werde dementsprechende Firewallrules setzen und eine angepasste Standardseite des Apache2 Webservers hinterlegen, welche automatisch auf Port 80 Freigegeben wird.



## Vorwissen/Vorgehen:

Ich konnte in meinem Geschäft bereits viel Erfahrung mit Apache2, Linux und Git/Github machen. In diesem Modul wird mich vorallem die Automatisierung eine neue Erfahrung für mich werden. Anstatt mit irrgendwelchen Startup-Scripts, werde ich ein Vagrantfile erstellen, welche die gewünschten Einstellungen bereits haben und nur noch gestartet werden müssen durch Vagrant.



## Verwendete Tools:

- Visual Studio Code - für die bearbeitung der verschiedenen Files wie z.B. Vagrantfile, index.html, etc..
- Typora - für die Bearbeitung von diesem Mark Down File
- Git client - für den Sync zwischen der lokalen Maschine und dem eigenen Git-Repo



## Sicherheit:

Zur verbesserung der Sicherheit innerhalb der VM verwende ich ufw als Firewall und SSH-Key authorization bei "vagrant ssh"-Befehl. Die Benutzer- und Rechteverwaltung habe ich einmal ausgelassen, da jedes Tool welches ich ausgewählt habe, eigene Benutzer hat, mit welchen Sie die Services verwenden, ich brauche keine zusätzlichen Benutzer.



## Meine VM:

Bei der VM, welche ich mit dem Vagrantfile nach meinen Wünschen angepasst habe, handelt es sich um einen Ubuntu 18.04 LTS Headless-Server, welcher mit apache2, mysql (Server und Client) und phpmyadmin (zur Administration der MySQL Datenbanken) vorinstalliert wird. Dieses Setup wird oft als LAMP Stack bezeichnet. 



## Was wird beim Setup mit "vagrant up" alles gemacht?

Wenn mein eigenes Vagrantfile auf einem lokalen Computer ausgeführt wird, wird als erstes ein "sauberes" Ubuntu 18.04 erstellt, dort werden danach die entsprechenden Netzwerkeinstellungen vorgenommen. Dies bedeutet Port-Freigaben und Firewall-Regeln. Nachdem die Netzwerkeinstellungen gemacht wurden, wird noch ein Sync erstellt, damit im gleichen Verzeichnis indem das Vagrantfile vorhanden ist, ein angepasstes "index.html"-File abgelegt werden, kann, welches durch den Webserver gleich benutzt wird. Danach wird das apt-Repo aktualisiert und die oben genannten Tools installiert. Speziell ist da, dass bei der installation von "phpmyadmin" eigentlich noch eine Prompt kommt, welche auch mit dem Zusatz "-y" nicht übersprungen wird. Dies musste ich auch noch passend machen, damit die Installation vollkommen inputlos von Statten laufen kann. 

