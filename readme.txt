Dies ist eine kurze Erklärung zu den beiden Playbooks.

Vorbereitung:

Die Gruppe acer besteht nur aus Laptop den ich als "Remoteserver" benutze.
Soll das Playbook sich auf weitere Server verbinden so kann man weitere IP Adressen oder Hostnamen in folgende Datei eintragen:
/etc/ansible/hosts

Die IP-Adressen die hinter den Hostnamen stehen können in folgender Datei angesehen und bearbeitet werden:
/etc/hosts

Auf dem Remotesystem muss SSH installiert werden mit folgendem Befehl:
sudo apt-get install openssh-server

Vor dem aufrufen des Playbooks müssen SSH Schlüssel erstellt werden. Dies geschieht durch folgenden Befehl:
ssh-keygen


Dann muss der öffentliche Schlüssel auf das Remotesystem übertragen werden mit folgendem Befehl:
ssh-copy-id -i ~/.ssh/id_rsa.pub nutzername@Hostname

Wenn man als Nutzername root wählt so muss man in folgender Datei:
/etc/ssh/sshd_config

folgende Zeile suchen:
#PermitRootLogin prohibit-password

und durch folgnde Zeile ersetzen:
PermitRootLogin=yes


Erklärung zu apache.yaml:
das Playbook verbindet sich zur Gruppe Acer die nur aus dem Remoteserver Laptop besteht.
um Pakete zu installieren benötigt man Rootrechte, daher ist become auf yes gesetzt.
Das PB installiert apache2 mit apt auf ubuntu/Debian basierenden Systemen.
Auf Rhel,CentOS oder Fedora wird httpd mit dem yum Packetmanager installiert (Dies ist ein apache2 nur mit anderem Namen).
Damit es das richtige Paket auf dem richtigen Server installiert wird eine Abfrage mit ansible_pkg_mgr durchgeführt.
Der Port und der Servername wird mit einer Lineinfilefunktion bearbeitet, die in den jeweiligen Configdateien nach einem regulären Ausdruck sucht und diesen ersetzt.
Damit die Änderungen wirksam werden wird ein Händler ausgeführt der den Server neustartet.
Das Playbook kann mit folgendem Befehl gestartet werden:
ansible-playbook apache.yaml

Das Playbook apacheitem.yaml arbeitet wie das apache.yaml Playbook, jedoch wird der Webserver als Item installiert.
Sollten weitere Anwendung z.b eine Datenbank benötigt werden, Können diese mit installiert werden indem man ein weiteres Item hinzufügt.




