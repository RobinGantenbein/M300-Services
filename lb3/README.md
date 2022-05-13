# **Dokumentation LB3**

---

# Inhaltsverzeichnis

- [**Dokumentation LB3**](#dokumentation-lb3)
- [Inhaltsverzeichnis](#inhaltsverzeichnis)
- [Einführung](#einführung)
- [Installationsanleitung](#Installationsanleitung)
- [Codebeschreibung](#code-beschreibung)

---

# Einführung
Im laufe der LB03 haben wir einige verschiedene Projekte wie z.B. Kali/Metasploitable in Kombination mit VNC oder auch Pihole mit HTTPS DNS versucht zu kreieren. Leider sind diese Versuche gescheitert (Technische Fehler) und nun haben wir uns mit Herrn Berger dafür entschieden eine Installationsanleitung für Rancher zu schreiben.

---  

# Installationsanleitung
<h2>Vorbereitungen</h2>

Für die Installation von Rancher auf Ubuntu sind nur eine Ubuntu VM mit der Standardinstallation notwendig. Dabei ist es nicht wichtig ob es sich um die Desktop oder Server version handelt.

<b>Standartinstallation für VM: </b>

2GB RAM, 
16GB Speicher, 2 Prozessor Cores

---
<h2>Installation</h2>
<h3>Docker installieren</h3>

<em>`sudo apt-get remove docker docker-engine docker.io containerd runc `

`sudo apt-get update`

` sudo apt-get install \`

  `  ca-certificates \`
  `  curl \
    gnupg \
    lsb-release`
	
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`


`echo \
  deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin`
 </em>
 
 

- <h3>Rancher installieren</h3>

https://rancher.com/docs/rancher/v2.6/en/installation/other-installation-methods/single-node-docker/

Note: Es gibt viele andere Methoden um Rancher zu installieren, bei unserer handelt es sich um eine K3s single Node installation.

<em>`docker run -d --restart=unless-stopped \
-p 80:80 -p 443:443 \
--privileged \
rancher/rancher:stable`
</em>


- <h3>Konfiguration Rancher</h3>

<em>`docker ps`</em> 

<em>`docker logs ("vorherige ausgelesene ID") 2>&1 | grep "Bootstrap Password:"`

Dieses Passwort kopieren wir ins Webgui, dort können wir dan das PW ändern.
</em> 

<h4>Nun kann man die eigene Adresse für Rancher auswählen</h4>

![image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/add-host.png)

<h4>Das Dashboard von Rancher kann nun über die vorherig definierte Adresse geöffnet werden und Cluster hinzufügen</h4>


![image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/clusterview.png)


<h4>Um sich nun bei Rancher anzumelden werden die Anmeldedaten, welche man zuvor festgelegt haben verwendet</h4>

Wir müssen bevor wir ein Volume mappen es zuerst erstellen. Dies ist einfach der Standartweg um ein Directory zu erstellen. Mit pwd können wir uns diesen Pfad angeben lassen um ihn zu kopieren.

![image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/rancherhost.png)

- <h3>Volume erstellen</h3>
<h4>Um ein neues Volume zu erstellen kann man direkt auf "PersistentVolume" gehen und dort ein neues erstellen, dort unter HostPath geben wir nun unser kopierter Pfad an, damit Rancher dies Mappen kann.</h4>

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/volume.png)

- <h3>Installation Apps</h3>
<h4>Um eine App aus den Charts zu installieren muss man folgendes vornehmen:</h4>

Zuerst ein Repository erstellt werden: 

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/repository.png)

Danach kann man unter den Charts die jeweilige App auswählen und diese installieren:
![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/charts.png)

<h4>Wenn man ein File aus einem Yaml möchte verwenden kann man unter "Deployments" oben rechts dieses kreieren</h4>

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/deployments.png)
![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/deployment%20create.png)


# Code Beschreibung

| Code| Beschreibung|
| --------------| -----------------|
|docker run -d --restart=unless-stopped|Dieser Command started rancher durchgehen, bis der User ihm sagt, dass er stoppen soll. Somit erreicht man eine 100% Verfügbarkeit|
|--privileged \ | Viele Features von Rancher benötigen Adminrechte, weshalb wir mit diesem Parameter Rancher alle benötigten Rechte geben.|
|rancher/rancher:stable|Die stabile version von Rancher wird hiermit heruntergeladen, da wir mit den alten Beta releases keine positiven Erfahrungen gesammelt haben.|

Codebeschreibung für Dockerinstallation findet man bei folgendem Link: 
https://github.com/Samtheboogieman/M300-Services/blob/master/LB2/lb2.md#code-beschreibung


---