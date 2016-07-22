# Docker

## Installation 
```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee --append /etc/apt/sources.list.d/docker.list
sudo apt-get update
sudo apt-get install docker-engine
```

## Premier test
```
sudo docker run hello-world
sudo docker run -it ubuntu bash
```

## Configuration 
```
sudo groupadd docker
sudo usermod -aG docker votreusername
```

Il est nécessaire de redémarrer votre session linux pour voir ce changement opérer.


## Container MySQL
```
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=formation --name mysql_solo mysql
```

## Maven dans un Container
Si on lance la commande suivante, maven retéléchargera à chaque fois les dépendances
```
docker run -it --rm -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven maven:3.3.9-jdk-8 mvn test
```
Désormais, on stocke les dépendances sur la machine locale (dans le répertoire ~/.m2)
```
docker run -it --rm -v "$PWD":/usr/src/mymaven -v ~/.m2:/root/.m2 -w /usr/src/mymaven maven:3.3.9-jdk-8 mvn test
```

## Lien entre container
On fait un lien vers le container nommé mysql_solo, et dans le container on aura des variables d'environnement vers ce container
```
docker run -it --rm -v "$PWD":/usr/src/mymaven -v ~/.m2:/root/.m2 -w /usr/src/mymaven --link="mysql_solo:db" maven:3.3.9-jdk-8 env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=2fd1f6768206
TERM=xterm
DB_PORT=tcp://172.17.0.2:3306
DB_PORT_3306_TCP=tcp://172.17.0.2:3306
DB_PORT_3306_TCP_ADDR=172.17.0.2
DB_PORT_3306_TCP_PORT=3306
DB_PORT_3306_TCP_PROTO=tcp
DB_NAME=/modest_hoover/db
DB_ENV_MYSQL_ROOT_PASSWORD=formation
DB_ENV_GOSU_VERSION=1.7
DB_ENV_MYSQL_MAJOR=5.7
DB_ENV_MYSQL_VERSION=5.7.13-1debian8
LANG=C.UTF-8
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
JAVA_VERSION=8u91
JAVA_DEBIAN_VERSION=8u91-b14-1~bpo8+1
CA_CERTIFICATES_JAVA_VERSION=20140324
MAVEN_VERSION=3.3.9
MAVEN_HOME=/usr/share/maven
HOME=/root
```
Il ne reste donc plus qu'a utiliser la variable d'environnement pour pouvoir se connecter au MySQL.
Exemple en java :
```
String ipMySQL = System.getenv().get("DB_PORT_3306_TCP_ADDR");
```