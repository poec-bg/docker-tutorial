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
```
docker run -it --rm -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven maven:3.3.9-jdk-8 mvn test

mkdir ~/.m2

docker run -it --rm -v "$PWD":/usr/src/mymaven -v ~/.m2:/root/.m2 -w /usr/src/mymaven maven:3.3.9-jdk-8 mvn test
```
