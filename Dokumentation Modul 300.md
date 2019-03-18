# Dokumentation Modul 300 - Cédric Droz

Dies ist eine Dokumentation zur Arbeit im Modul 300 von Cédric Droz.



## Initielle Idee:

Ich werde mit Hilfe von Vagrant ein LAMP-Stack erstellen, welcher nur mit einem Befehl soweit alles fertig aufsetzt. Firewallrules, eine angepasste Apache2 Standard-Seite und MySQL mit phpmyadmin werden auf dem System vorkonfiguriert installiert. Netzwerktechnisch werden entsprechende Firewallrules generiert und die Ports so weitergeleitet, dass auch von der Host-Maschine auf den Webserver zugegriffen werden kann.



## Vorwissen/Vorgehen:

Ich konnte in meinem Geschäft bereits viel Erfahrung mit Apache2, Linux und Git/Github machen. In diesem Modul wird für mich vorallem die Automatisierung eine neue Erfahrung werden. Anstatt mit irrgendwelchen Startup-Scripts, werde ich ein Vagrantfile erstellen, welches die gewünschten Einstellungen bereits beinhaltet und nur noch durch Vagrant gestartet werden muss.



## Verwendete Tools:

- Visual Studio Code - für die bearbeitung der verschiedenen Files wie z.B. Vagrantfile, index.html, etc..
- Typora - für die Bearbeitung von diesem Mark Down File
- Git client - für den Sync zwischen der lokalen Maschine und dem eigenen Git-Repo



## Sicherheit:

Zur verbesserung der Sicherheit innerhalb der VM verwende ich ufw als Firewall und SSH-Key authorization bei "vagrant ssh"-Befehl. Die Benutzer- und Rechteverwaltung habe ich einmal ausgelassen, da jedes Tool welches ich ausgewählt habe, eigene Benutzer hat, mit welchen Sie die Services verwenden, ich brauche keine zusätzlichen Benutzer.



## Meine VM:

Bei der VM, welche ich mit dem Vagrantfile nach meinen Wünschen angepasst habe, handelt es sich um einen Ubuntu 18.04 LTS Headless-Server, welcher mit apache2, mysql (Server und Client) und phpmyadmin (zur Administration der MySQL Datenbanken) vorinstalliert wird. Dieses Setup wird oft als LAMP Stack bezeichnet. 



## Was wird beim Setup mit "vagrant up" alles gemacht?

Wenn mein eigenes Vagrantfile auf einem lokalen Computer ausgeführt wird, wird als erstes ein "sauberes" Ubuntu 18.04 erstellt, dort werden danach die entsprechenden Netzwerkeinstellungen vorgenommen. Dies bedeutet Port-Freigaben und Firewall-Regeln. Nachdem die Netzwerkeinstellungen gemacht wurden, wird noch ein Sync erstellt, damit im gleichen Verzeichnis indem das Vagrantfile vorhanden ist, ein angepasstes "index.html"-File abgelegt werden, kann, welches durch den Webserver gleich benutzt wird. Danach wird das apt-Repo aktualisiert und die oben genannten Tools installiert. Speziell ist da, dass bei der installation von "phpmyadmin" eigentlich noch eine Prompt kommt, welche auch mit dem Zusatz "-y" nicht übersprungen wird. Dies musste ich auch noch passend machen, damit die Installation vollkommen inputlos von Statten laufen kann. Ganz am Schluss wird dann noch die Firewall mit den entsprechenden Regeln aktiviert



Hier noch das Vagrantfile:

```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.synced_folder ".", "/var/www/html"

  config.vm.provider "virtualbox" do |vb| 

​    vb.memory = "2048"

  end

  config.vm.provision "shell", inline: <<-SHELL

​    ufw allow 80/tcp

​    ufw allow from 127.0.0.1 to any port 22

​    ufw allow from 10.0.2.15 to any port 3306

  

​    apt-get update

​    APP_PASS="m300apppw"

​    ROOT_PASS="m300rootpw"

​    APP_DB_PASS="m300dbpw"

​    

​    echo "phpmyadmin phpmyadmin/dbconfig-install boolean true" | debconf-set-selections

​    echo "phpmyadmin phpmyadmin/app-password-confirm password $APP_PASS" | debconf-set-selections

​    echo "phpmyadmin phpmyadmin/mysql/admin-pass password $ROOT_PASS" | debconf-set-selections

​    echo "phpmyadmin phpmyadmin/mysql/app-pass password $APP_DB_PASS" | debconf-set-selections

​    echo "phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2" | debconf-set-selections

​    apt-get -y install apache2 mysql-server phpmyadmin

​    ufw enable

  SHELL

end
```



## Reflexion/Wissenszuwachs:

Ich fand persönlich diese Arbeit ziemlich spannend, da ich noch nie richtig mit Vagrant in Kontakt gekommen bin, jedoch viele Begriffe diesbezüglich oft gehört habe. Nun einmal ein kleines Projekt auf die Beine gestellt zu haben, hat mir persönlich geholfen, näher das Thema Container zu betrachten und zu verstehen.

Es gab natürlich viele Situationen während dieser Arbeit, bei denen ich nicht selber weiter Wusste und dem Internet ein paar Fragen stellen musste oder ein Tutorial als Verständnisstütze anschauen musste. Ich bin jedoch zufrieden mit dem Endergebnis.