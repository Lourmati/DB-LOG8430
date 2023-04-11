## Commandes à exécuter (environnement Hyperledger):
1. Installation des dépendances et des librairies.  
```shell
sudo apt-get update
sudo apt install nodejs
sudo apt-get install build-essential
sudo apt install npm
```
2. Installation de Python (utilisez la version 2.7).
```shell
wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
tar -xvf Python-2.7.18.tgz
cd Python-2.7.18/
./configure
make
sudo make install
sudo ln -sf /usr/local/bin/python2.7 /usr/bin/python
python --version
```
3. Clonage et installation du projet Hyperledger Caliper (outil de benchmark).
```shell
git clone https://github.com/hyperledger/caliper-benchmarks.git
cd caliper-benchmarks/
git checkout d02cc8bbc17afda13a0d3af1122d43bfbfc45b0a
npm init -y
npm install --only=prod @hyperledger/caliper-cli@0.4
cd networks/fabric/config_solo_raft/
./generate.sh
``` 
4. Utilisation de Docker pour obtenir la base de données Hyperledger Fabric.
```shell
cd
cd caliper-benchmarks/
sudo snap install docker
sudo docker pull hyperledger/fabric-ccenv:1.4.4
sudo docker tag hyperledger/fabric-ccenv:1.4.4 hyperledger/fabric-ccenv:latest
npm install --save fabric-client fabric-ca-client
curl https://raw.githubusercontent.com/creationix/nvm/v0.25.0/install.sh | bash
source ~/.profile
nvm install 12
```
5. Modification des paramètres txNumber, tps et txDuration du fichier config.yaml afin de tester les combinaisons de charge de travail. Un exemple config.yaml (paramètres ajustés) se trouve dans chaque dossier (Workload A, Workload B, Workload C). 
```shell
cd caliper-benchmarks/benchmarks/samples/fabric/marbles/config.yaml
nano config.yaml
```

6. Exécution de la commande de test.
```shell
cd
cd caliper-benchmarks/
sudo npx caliper launch manager --caliper-workspace . --caliper-benchconfig benchmarks/samples/fabric/marbles/config.yaml --caliper-networkconfig networks/fabric/v1/v1.4.4/2org1peercouchdb_raft/fabric-go-tls-solo.yaml
```
## Remarques:
- Utilisez la version 6.14.16 de npm, et 12.22.12 de node (commande ```nvm install 12```)
- Exécutez ```./generate.sh``` dans le dossier config_solo_raft
- En cas d'erreur gRPC : ```npm rebuild grpc --force```
- En cas de problèmes de permission : ```sudo chmod 777 /var/run/docker.sock``` 
