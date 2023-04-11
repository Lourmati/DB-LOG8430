## Commandes à exécuter (environnement MongoDB & YCSB):
1. Installation de Python.
```shell
sudo apt-get install build-essential
wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
tar -xvf Python-2.7.18.tgz
./configure
make
sudo make install
which python2.7
sudo ln -sf /usr/local/bin/python2.7 /usr/bin/python
python --version
```

2. Installation des dépendances.
```shell
sudo apt-get update
sudo apt install default-jre
sudo apt install default-jdk
sudo apt install maven
mvn -version
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

3. Installation de Docker et ajouter au groups.
```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
sudo usermod -aG docker ${USER}
su - ${USER}
```
- Si mot de passe oublié.
```shell
sudo su -
passwd ubuntu
exit
```

4. Quitter et ouvrir un nouveau terminal pour vérifier si Docker est dans les groups.
```shell
groups
```

5. Installation de Docker Compose et de YCSB.
```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz
tar xfvz ycsb-0.17.0.tar.gz
```

6. Configurer le fichier YAML (copier docker-compose.yml du dossier GitHub)
```shell
cd ycsb-0.17.0/
mkdir database
nano docker-compose-mongodb.yml
```

7. Lier et initialiser les clusters.
```shell
docker exec -it primary mongosh --eval "rs.initiate({
 _id: \"myReplicaSet\",
 members: [
   {_id: 0, host: \"primary\"},
   {_id: 1, host: \"secondary1\"},
   {_id: 2, host: \"secondary2\"},
   {_id: 3, host: \"secondary3\"}
 ]
})"
docker exec -it primary mongosh --eval "rs.status()"
docker-compose -f docker-compose-mongodb.yml
```

8. Ouvrir un autre terminal et modifier etc/hosts.
```shell
nano etc/hosts
```

9. Ajouter au début de etc/hosts (après localhost) ce qui suit. Utilisez l'adresse IP de votre instance/machine.
```shell
34.230.30.142   primary
32.230.30.143   secondary1
34.230.30.144   secondary2
34.230.30.145   secondary3
```

9. Exécuter les commandes pour tester Hyperledger Fabric.
```shell
cd
cd ycsb-0.17.0/
./bin/ycsb load mongodb -s -P workloads/workloada -p recordcount=1000 -p mongodb.upsert=true -p mongodb.url=mongodb://primary:27017,secondary1:27017,secondary2:27017,secondary3:27017/?replicaSet=myReplicaSet > outputLoad.txt
./bin/ycsb run mongodb -s -P workloads/workloada -p recordcount=1000 -p mongodb.upsert=true -p mongodb.url=mongodb://primary:27017,secondary1:27017,secondary2:27017,secondary3:27017/?replicaSet=myReplicaSet > outputRun.txt
```

## Remarques:
- Remplacez ```workloada``` dans les commandes de test par ```workloadb``` ou ```workloadc``` pour tester les combinaisons de charge de travail.
- ```workloada``` (50/50 lecture/écriture), ```workloadb``` (10/90 lecture/écriture), ```workloadc``` (100/0 lecture/écriture)
- Si vous voulez modifier les combinaisons : simplement modifier les fichiers workloada, workloab, ... avec la commande ```nano``` dans /workloads

