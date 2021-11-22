# BigData

docker run image_name : permet de créer et démarer une nouvelle instance de l'image (conteneur)

si l'image n'existe pas, elle sera téléchargée à partir de docker hub

docker run -d image_name:tag : permet de démarrer conteneur en arrière plan

la valeur par défaut d'une image est latest

docker ps : permet de lister tous les conteneurs qui sont en cours d'exécution. Chaque conteneur dispose d'un identifiant unique

docker ps -a : permet de lister tous les onteneur aver leurs statuts (Up, Exited Created)

Exécuter une image du serveur nginx : docker run -d -p 9999:80 nginx

Pour donner un nom précis à votre conteneur, on utilise --name. Exemple : docker run --name moningx -d -p 8989:80 nginx Cette commande permet de démarrer un conteneur dont le nom est moningnx. Pour pouvoir y accéder de l'extérieur (du moteur), on utilise la notion de redirection des ports. Ici, on va utiliser le port 8989pour y accéder comme ceci : localhost:8989

Pour arrêtre un conteneur docker stop identifiant_conteneur. Pour supprimer un conteneur, on doit d'abord l'arrêter : docker rm identifiant_conteneur.

Pour lister toutes vos images docker images

Pour forcer la suppression d'une image ou d'un conteneur : docker rmi -f nom_image

La commande exec permet d'exécuter une commande dans un conteneur démarré

Exemple : notre objectif est de démarrer un conteneur MySQL et d'exécuter le client mysql

Solution : docker run --name monserveur -e MYSQL_ROOT_PASSWORD=root -d mysql

Pour lancer mysql : docker exec -ti monserveur mysql --password

Exercice : démarer un conteneur à partir de l'image ubuntu et puis installer java et exécuter un programme simple.

docker run -it ubuntu bash

Une fois à l'intérieur du conteneur, vous pouvez installer les outils dont vous avez besoin comme vous avez l'habitube de le faire en utilisant ubuntu.

Si on veut installer par exemple Java, on procède de la façon suivante : on met d'abord à jour apt-get updateet puis apt-get install java. Si on veut utiliser vim comme éditeur apt-get install vim.

Pour inspecter un contenur et avoir des détails sous forme d'un ficheir json docker inspect id_conteneur

Pour avoir les logs d'un contenur : docker logs id_conteneur

Pour redémarrer un conteneur : docker start id_conteneur

Exercice : démarrer un conteneur nginx et changer le contenu du fichier index.html.

La page index.html est par défaut dans le répertoire /usr/share/nginx/htm

Notion de volume :

docker volume

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

Pour faire "mapping volume"

docker run -dti -p 8080:80 -v C:\Users\jerem\Desktop\backupnginx:/usr/share/nginx/html/ --name myserverweb nginx

docker run -tid --name webvolumebis -p 8691:80 --mount source=mesdonnees,target=/usr/share/nginx/html/ nginx

Pour accéder aux volumes (sous mac) :

docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh

Une fois à l'intéreiur du conteneur, les volumes se trouvent dans le dossier : /var/lib/docker/volumes/

La solution ici : https://stackoverflow.com/questions/38532483/where-is-var-lib-docker-on-mac-os-x

Gestion des variables d'environnement :

docker run -tid --name variableenvironnement --env MAVARIABLE="samir1234" ubuntu:latest

Passer une variable d'environnemnt en ligne de commande : docker exec -ti variableenvironnement sh

On peut aussi passer un ensemble de variables d'environnement sauvegardé dans un fichier :

docker run -tid --name fichiervariableenvironnement --env-file mesvariables.lst ubuntu:latest

Définir sa propre image Docker :

    docker commit -m "Install nano" d7c055831f07 polytech:V1.0

Exemple : partir d'une image ubuntu, installer python et vim et ajouter queqlues variables d'environnemnetsauvegardées dans un fichier.

a. On commence par créer un fichier env.lst (on définit autant de variables souhaitées)

b. Pour ajouter ces variables d'environnement docker run --name monconteneurvariables -tdi --env-file env.lst ubuntu

c. Pour installer python et vim, on rentre à l'intérieur du conteneur docker exec -ti monconteneurvariables sh

Pour installer python et vim : apt-get update, '' apt-get install python et `àpt-get install vim

d. On quitte le conteneur exit

e. Pour créer une image à partir de ce conteneur docker commit -m "créeation d'une image ubuntu augumentée" id_conteur nom_notre_image


    Dockerfile

a. Créer un fichier Dockerfile b. On part d'une image existente (la plus petite est alpine), copier éventuellemnet un programme et l'exécuter ....

Exemple

FROM openjdk
COPY Application.class Application.class
CMD ["java", "Application"]

Exercice : Lucas et Florian avec Node et les autres avec Python

Exercice : Lucas et Florian avec Node et les autres avec Python

FROM ubuntu
COPY test.py test.py
RUN apt-get update
RUN apt-get install python -y
CMD ["python", "test.py"]

Exercice : Lucas et Florian avec Node et les autres avec Python

FROM : permet de définir l'image source

CMD : la commande par défautt lors de l'exécution du conteneur

EXPOSE : permet de définir le port d'écoute par défaut

RUN : permet d'exécuter des commandes à l'intérieur du conteneur

? ADD : permet d'ajouter des fichier dans le conteneur

VOLUME : permet de définir les volumes utilisables

FROM nginx
COPY index.html /usr/share/nginx/html
EXPOSE 80

docker run -d -p 9090:8080 syoucef/springcm19novembre

Publier une image sur DockerHub :

    Créer son image, soit josephadd
    Créer un dépôt dockerHub syoucef/joseph
    Se connecter à DockerHub via ligne de commande : docker login (facultatif si on utilise docker desktop)
    Créer un lien entre l'image josephaddet syoucef/joseph : docker tag josephadd syoucef/joseph
    Envoer l'image vers Dockerhub docker push syoucef/joseph

Pour chercher une image par ligne de commande docker serach nom_image

Exercice : publier une image de votre application Spring (utiliser une base de données H2)
