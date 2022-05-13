# **Dokumentation LB3**

---

# Inhaltsverzeichnis

- [**Dokumentation LB3**](#dokumentation-lb3)
- [Inhaltsverzeichnis](#inhaltsverzeichnis)
- [Einführung](#einführung)
- [Installationsanleitung](#Installationsanleitung)

---

# Einführung
Im laufe der LB03 haben wir einige verschiedene Projekte wie z.B. Kali/Metasploitable oder auch Docker mit Phihole versucht zu kreieren. Leider sind diese Versuche gescheitert und nun haben wir uns mit Herrn berger dafür entschieden eine Installationsanleitung für Rancher zu schreiben.

---  

# Installationsanleitung
<h2>Vorbereitungen</h2>

Für die Installation von Rancher auf Ubuntu sind nur eine Ubuntu VM mit der Standardinstallation notwendig.

<b>Standartinstallation: </b>

2GB RAM, 
16GB Speicher, 1 Prozessor

---
<h2>Installation</h2>
<h3>Docker installieren</h3>

<em>
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
	
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
 </em>

- <h3>Rancher installieren</h3>

<em>docker run -d --restart=unless-stopped \
-p 80:80 -p 443:443 \
--privileged \
rancher/rancher:stable
</em>

- <h3>Konfiguration Rancher</h3>

<em>docker ps</em> 

<em>docker logs ("vorherige ausgelesene ID") 2>&1 | grep "Bootstrap Password:"

</em> 

<h4>Nun kann man die eigene Adresse für Rancher auswählen</h4>

![image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/add-host.png)

<h4>Das Dashboard von Rancher kann nun über die vorherig definierte Adresse geöffnet werden und Cluster hinzufügen</h4>

![image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/clusterview.png)

- <h3>Installation Apps</h3>
<h4>Um eine App aus den Charts zu installieren muss man folgendes vornehmen:</h4>

Zuerst ein Repository erstellt werden: 

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/repository.png)

Danach kann man unter den Charts die jeweilige App auswählen und diese installieren:
![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/charts.png)

<h4>Wenn man ein File aus einem Yaml möchte verwenden kann man unter "Deployments" oben rechts dieses kreieren</h4>

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/deployments.png)
![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/deployment%20create.png)

- <h3>Volume erstellen</h3>
<h4>Um ein neues Volume zu erstellen kann man direkt auf "PersistentVolume" gehen und dort ein neues erstellen</h4>

![Image](https://github.com/RobinGantenbein/M300-Services/blob/main/lb3/images/volume.png)