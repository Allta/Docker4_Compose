# TP Docker_3_Images - Ynov DevOps B3


## **Rendu :** Un fichier MD : DEVOPS_DOCKER3_[NOM]\_[PRENOM].md

Vous avez un template de rendu dans le repo. 
Pour chaque étape, documenter vos actions : 

        Screenshot commande + output
        Explication d'une ou 2 ligne sur ce que fait la commande
        
A chaque exercice rajouter un titre et le nom de l'exercice. La syntaxe ainsi que l'upload de l'image sont décrit dans des liens en haut du template.

:bangbang::bangbang: Pensez à renommer le fichier avec votre **Nom** et **Prénom**

:sparkles: Une fois le TP et le rendu terminé commitez et pushez le dans le repo. :sparkles:
  
### Tips   
Si vous avez des problèmes sur une command utilisez `docker [command] --help`.

:raising_hand: Si vous avez des soucis n'hésitez pas à m'appeler. 

## Avant Exercice : 
Pensez à installer `docker-compose` : 

Pour Linux : 
```bash 
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Pour les autres : https://docs.docker.com/compose/install/ 

## Exercice 1 : Wordsmith

- Avec les Dockerfile présents dans chaque dossier il faut lancer la stack wordsmith avec un `docker-compose`.
- Le service `web` doit tourner sur le port 80.
- Checker les status de notre stack uniquement (Pas de tout les containers présent sur notre hôte. Utiliser les commandes docker-compose.)
  - `docker-compose --help`
- Stopper la stack avec une commande docker-compose 

## Exerice 2 : Dockercoins

- A l'aide du fichier `ports.yml` lancer la stack docercoin via docker-compose.
- Lancer le service `worker` en dépendance de : (Chercher sur google la dépendance entre 2 containers dans Compose)
  - rng
  - hasher
  - redis
- Lancer le service `webui` en dépendance de `redis`. 

## Exerce 3 : ID Dock 

ID Dock est une application Flask qui utilise Redis comme base de données:
`uWSGI` est un serveur python de production. 

- Construire l'image `id_dock` et vérifier qu'elle tourne bien.
- Rentrer dans le container et vérifier l'utilisateur qui lance l'application `id && whoami`
- Vérifier que le serveur uwsgi est bien lancé
- A la racine du project créer un fichier `docker-compose.yml`. Avec les services : 
  - id_dock : 
    - Port 9090
    - Il doit builder l'image présent dans le projet
  - dnmonster :
    - image: amouat/dnmonster:1.0
  - redis : 
    - image: redis
- Créer un réseau appelé `id_net` et ajouter tout les containers de la stack à ce réseau
- Vérifier que l'application tourne       


## Exercice 4 : Stack ELKT

La stack Elastic permet de centraliser tout les logs de nos containers  au même endroit, nous allons la coupler à Trafiek pour faciliter la gestion de nos containers.

- Télécharger la configuration Filebeat : `curl -L -O https://raw.githubusercontent.com/elastic/beats/7.10/deploy/docker/filebeat.docker.yml`
- ```mv filebeat.docker.yml filebeat.yml
sudo chown root filebeat.yml
```
- Rajouter au docker-compose existant le service trafiek
  - image: traefik:v2.3
  - command: --api.insecure=true --providers.docker
  - ports: 80 et 8080
  - volumes: /var/run/docker.sock:/var/run/docker.sock
- Expliquer en quelques mots le fichier `docker.sock`
- BONUS : Expliquez pourquoi le partage de ce fichier impacte la sécurité du serveur. 


