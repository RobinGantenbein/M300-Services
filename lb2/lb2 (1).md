# **Dokumentation LB2**

---

# Inhaltsverzeichnis

- [**Dokumentation LB2**](#dokumentation-lb2)
- [Inhaltsverzeichnis](#inhaltsverzeichnis)
- [Einführung](#einführung)
- [Grafische Übersicht](#grafische-übersicht)
- [Code](#code)
  - [Code-Quelle](#code-quelle)
  - [Vagrantfile](#vagrantfile)

---

# Einführung
Ich habe mich für das Projekt **Portainer Server via Ubuntu -> Docker** entschieden.
Mein Projekt umfasst die automatische Einrichtung eines portainers, welcher verwendet werden kann um verschiedene Dienste zur Verfügung zu stellen wie z.B. einen Minecraftserver. 
Der Portainer wird auf einer Docker VM installiert, welche wiederrum über Ubuntu installiert wird mit dem Vagrantfile. 


---
<a name="grafische"></a>
# Grafische Übersicht
![image](C:\Users\gantenbein\Downloads\Grafik.jpg)


---

# Code

## Code-Quelle
Ich habe das Vagrantfile von Herrn Berger aus dem Freigegebenen Ordner (C:\Git\M300\vagrant\web) als Vorlage für mein File verwendet. 
Danach habe ich den Code angepasst und Docker installiert. 
Wie das geht habe ich auf folgenden Seiten gefunden: 
https://docs.docker.com/engine/install/ubuntu/ 
https://docs.portainer.io/v/ce-2.11/start/install/server/docker/linux


## Vagrantfile

Mein Vagrantfile sieht folgendermassen aus:
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "forwarded_port", guest:9443, host:9443, auto_correct: false
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
 #Get Docker Image

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#Install Docker

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
#Install Portainer

docker volume create M300

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v M300:/data \
    portainer/portainer-ce:latest
  
SHELL
end


| Code | Beschreibung |
| -------------- | ----------------- |
| Vagrant.configure("2") do |config| | In dieser Zeile wird die API Version, in diesem Fall die Nummer 2, vom Vagrantfile beschrieben. Die Konfigurationen der Vm werden hier drin geschrieben und somit ist diese Zeile "das Herz" des Codes.  |
| config.vm.box | Hier wird die Version des OS beschrieben, welches danach erstellt wird |
| db.vm.network | Da definiere ich den Port auf welchen dann die VM zugreift. In diesem Fall wäre es für MySQL Port 9443 und für die web applikation phpmyadmin Port 9443.  |
| db.vm.provision | In diesem Schritt erlaube ich die Ausführung von einem Shell Skript nachdem das Guest OS gebootet hat. |
| config.vm.provider :virtualbox do |vb| | Hier definiere ich den Provider der VM, in diesem Fall Virtualbox. Zusätzlich habe ich noch Anpassungen gemacht, z.b mehr RAM und CPUs. |
| docker volume create M300 | Hier wird ein Volume in portainer erstellt, welches als Speicher dienen kann. |