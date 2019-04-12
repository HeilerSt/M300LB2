# Serverdienste automatisieren mit Vagrant

## Inhaltsverzeichnis
1. [Kurzbeschrieb](#Kurzbeschrieb)
2. [Dockerbefehle](#Dockerbefehle)
3. [Wissensstand vor der LB2](#Wissensstand-vor-der-LB2)
4. [Implementierung](#Implementierung)
    * [Sicherheit](#Sicherheit)
    * [Netzwerkplan](#Netzwerkplan)
    * [Dockerfile](#Dockerfile)
    * [Dockercomposefile](#Dockercomposefile)
5. [Testing](#Testing)
6. [troubleshooting](#Troubleshooting)
7. [Jetziger Wissensstand](#Jetziger-Wissensstand)
8. [Reflexion](#Reflexion)

## Kurzbeschrieb
Das Ziel für mich war es einen Container mit einer MySQL-Datenbannk, sowie ein Webserver mit phpMyAdmin laufen.

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

~~~~

~~~~


### Dockercomposefiles

Die einzelnen Code Abschnitte des Dockercomposefiles.

~~~~

~~~~

## Testing

|Testfall | Erwartet | Resultat |
|:--:|:--:|:--:|
|Webserver|Webserver kann im Browser abgerufen werden|Der Webserver konnte im Browser abgerufen werden|
|phpMyAdmin|Das phpMyAdmin Tool wird angezeigt|Das phpMyAdmin Tool wurde auf dem Webserver angezeigt|
|mySQL-Server|phpMyAdmin sollte mit dem mySQL-Server kommunizieren|Ich konnte über phpMyAdmin auf die MySQL-Datenbank zugreifen|

*phpMyAdminWebsite* </br>
![Image](/phpmyadminwebsite.PNG)

##Troubleshooting


## Jetziger Wissensstand
*Docker* </br>
Mein Wissen über Docker ist deutlich gewachsen. Da ich weiss, dass ich in meinem Arbeitsumfeld dies nochmals anschauen werde, lässt hoffen, dass da dieselben Probleme nicht mehr auftreten wie hier.</br>

## Reflexion
Ich hatte mir Docker deutlich einfacher vorgestellt als es dann wirklich war.
Die kürzeren Code-Längen (verglichen mit Vagrant) haben mich da ziemlich reingelegt. Es war deutlich anspruchsvoller.
Besonders fand man deutlich weniger im Internet dazu, was natürlich das Lösen von Fehlern schwieriger machte.
