## Structure du projet:
Le projet est structuré en 2 dossiers : MongoDB et Hyperledger. Chaque dossier contient:
- Résultats des 3 tests sur chaque combinaison de charge de travail (.txt)
- Fichier de configuration YAML
- Étapes à suivre pour recréer l'environnement de tests (README)

Un sommaire des résultats de tests (Excel) se trouve dans la source du projet. 


## Setup pour l'environnement:
Voici le setup idéal pour l'environnement de tests:
- 8Go de RAM
- 2+ vCPUs
- Distribution Ubuntu : préférablement Ubuntu 20 (MongoDB) et Ubuntu 22 (Hyperledger)

Alternative : Utiliser une instance AWS de type t2.large ou supérieur. Une machine avec moins de capacité peut causer problème en raison de la charge.  


## Pour plus d'informations:
- Outil Hyperledger Caliper pour évaluer les performances d'Hyperledger Fabric : https://github.com/hyperledger/caliper
- Outil Yahoo! Cloud Service Benchmark pour comparer les performances de la base de données NoSQL (MongoDB) : https://github.com/brianfrankcooper/YCSB