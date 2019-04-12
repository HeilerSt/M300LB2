# Serverdienste automatisieren mit Vagrant

## Vorwort
Ich habe das Projekt alleine angefangen. Da jedoch (wahrscheinlich wegen Storage-Problemen) meine VM nicht mehr starten wollte, wo ich alle meine Files drauf hatte, habe ich nun dieselben Files wie mein Tandempartner David Milovanovic.

## Inhaltsverzeichnis
1. [Kurzbeschrieb](#Kurzbeschrieb)
2. [Dockerbefehle](#Dockerbefehle)
3. [Wissensstand vor der LB2](#Wissensstand-vor-der-LB2)
4. [Implementierung](#Implementierung)
    * [Netzwerkplan](#Netzwerkplan)
    * [Dockerfile](#Dockerfile)
5. [Testing](#Testing)
6. [Troubleshooting](#Troubleshooting)
7. [Jetziger Wissensstand](#Jetziger-Wissensstand)
8. [Reflexion](#Reflexion)

## Kurzbeschrieb
Das Ziel für mich war es einen Container mit einer MySQL-Datenbank, sowie ein Webserver mit phpMyAdmin laufen.

## Dockerbefehle

|Befehl | Effekt|
|:--:|:--:|
|docker build|Aus einem Dockerfile heraus ein Image bauen|
|docker run|Container starten|
|docker start|Einen gestoppten Container starten|
|docker stop|Einen Container stoppen|
|docker kill|Einen Container gewaltsam stoppen|
|docker ps|Alle laufenden Container auflisten|
|docker compose|Multi-container Umgebung starten|

## Wissensstand vor der LB2
*Docker* </br>
Ich hatte bereits eine kurze Berührung mit Docker bei meinem Arbeitgeber. Dies war jedoch nur eine kurze oberflächliche Einführung. Es ware mehr zum zeigen wie es eingesetzt werden kann.</br>

## Implementierung

### Containerplan
![Image](/Containerplan.PNG)

### Dockerfile

Die einzelnen Code Abschnitte des Dockerfiles.

Docker installieren
~~~~
sudo apt-get install docker.io
~~~~
Um Container 1 zu starten geht man in den Ordner webserver rein. Dort befindet sich ein Dockerfile. In diesem Ordner gibt man dann diese Befehle ein:
~~~~
docker build -t webserver .
docker run --rm -d -p 8080:80 webserver
~~~~
So sieht das Dockerfile aus:
~~~~
FROM ubuntu:14.04

MAINTAINER David Milovanovic



RUN apt-get update

RUN apt-get -q -y install apache2 


# Konfiguration Apache

ENV APACHE_RUN_USER www-data

ENV APACHE_RUN_GROUP www-data

ENV APACHE_LOG_DIR /var/log/apache2


RUN mkdir -p /var/lock/apache2 /var/run/apache2


#Für phpMyAdmin prompt-Config, das kommentierte sind die alten Befehle, welche ich umschreiben musste

#RUN debconf-set-selections <<< "phpmyadmin phpmyadmin/dbconfig-install boolean true"

#RUN debconf-set-selections <<< "phpmyadmin phpmyadmin/app-password-confirm password asdf123"

#RUN debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/admin-pass password asdf123"

#RUN debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/app-pass password asdf123"

#RUN debconf-set-selections <<< "phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2"

RUN echo 'phpmyadmin phpmyadmin/dbconfig-install boolean true' | debconf-set-selections

RUN echo 'phpmyadmin phpmyadmin/app-password-confirm password asdf123' | debconf-set-selections

RUN echo 'phpmyadmin phpmyadmin/mysql/admin-pass password asdf123' | debconf-set-selections

RUN echo 'phpmyadmin phpmyadmin/mysql/app-pass password asdf123' | debconf-set-selections

RUN echo 'phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2' | debconf-set-selections


#Installation phpMyAdmin

RUN sudo apt-get -y install phpmyadmin


EXPOSE 80



VOLUME /var/www/html


CMD /bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"

~~~~
Als nächstes geht man in den Ordner mysql rein. Dort gibt man dann diese Befehle ein:
~~~~
docker build -t mysql .
docker run --rm -d -p 3306:3306 mysql
~~~~
So sieht dieses Dockerfile aus:
~~~~
FROM ubuntu:14.04

MAINTAINER David Milovanovic



#Prompt befehle für mysq-server

RUN echo 'mysql-server mysql-server/root_spassword password asdf123' | debconf-set-selections 

RUN echo 'mysql-server mysql-server/root_password_again password asdf123' | debconf-set-selections 



# Installation

RUN apt-get update

RUN apt-get install -y mysql-server


#Port für alle Hosts öffnen

RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

EXPOSE 3306


VOLUME /var/lib/mysql

CMD ["mysqld"]
~~~~

## Testing

|Testfall | Erwartet | Resultat |
|:--:|:--:|:--:|
|Webserver|Webserver kann im Browser abgerufen werden|Der Webserver konnte im Browser abgerufen werden|
|phpMyAdmin|Das phpMyAdmin Tool wird angezeigt|Das phpMyAdmin Tool wurde auf dem Webserver angezeigt|
|mySQL-Server|phpMyAdmin sollte mit dem mySQL-Server kommunizieren|Ich konnte über phpMyAdmin auf die MySQL-Datenbank zugreifen|

*phpMyAdminWebsite* </br>
![Image](/phpmyadminwebsite.PNG)

## Troubleshooting
### Allgemeines Troubleshooting
* Sicherstellen, dass man Root-Rechte hat.
* Schreib- und Leserechte sollten richtig verteilt sein.
* Das Mounten von Volumes braucht Shared Drives für Linux-Container.
* Auf die Versionen achten. Manche Versionen sind nicht miteinander kompatibel.

### Ressourcen-Begrenzung
|Suffix | Effekt|
|:--:|:--:|
|-m|RAM begrenzen. (-m 4m ist z.B. 4MB)|
|--kernel-memory|Kernel-Memory begrenzen|
|--cpus= value |Stellt ein wie viel CPU-Ressourcen der Container benutzen darf (z.B bei 2 CPUs kann man 1.5 eingeben um nicht alles zu benutzen|

## Jetziger Wissensstand
*Docker* </br>
Mein Wissen über Docker ist deutlich gewachsen. Da ich weiss, dass ich in meinem Arbeitsumfeld dies nochmals anschauen werde, lässt hoffen, dass da dieselben Probleme nicht mehr auftreten wie hier.</br>

## Reflexion
Ich hatte mir Docker deutlich einfacher vorgestellt als es dann wirklich war.
Die kürzeren Code-Längen (verglichen mit Vagrant) haben mich da ziemlich reingelegt. Es war deutlich anspruchsvoller.
Besonders fand man deutlich weniger im Internet dazu, was natürlich das Lösen von Fehlern schwieriger machte.
