# Big Data


docker run image_name : permet de créer et démarer une nouvelle instance de l'image (conteneur)

si l'image n'existe pas, elle sera téléchargée à partir de docker hub

docker run -d image_name:tag : permet de démarrer conteneur en arrière plan 

la valeur par défaut d'une image est latest

docker ps : permet de lister tous les conteneurs qui sont en cours d'exécution. Chaque conteneur dispose d'un identifiant unique 

docker ps -a : permet de lister tous les onteneur aver leurs statuts (Up, Exited Created) 

Exécuter une image du serveur nginx : ``docker run -d -p 9999:80 nginx``

Pour donner un nom précis à votre conteneur, on utilise --name. Exemple : ``docker run --name moningx -d -p 8989:80 nginx`` Cette commande permet de démarrer un conteneur dont le nom est ``moningnx``. Pour pouvoir y accéder de l'extérieur (du moteur), on utilise la notion de redirection des ports. Ici, on va utiliser le port  ``8989``pour y accéder comme ceci : ``localhost:8989`` 


Pour arrêtre un conteneur ``docker stop identifiant_conteneur``. Pour supprimer un conteneur, on doit d'abord l'arrêter : ``docker rm identifiant_conteneur``. 

Pour lister toutes vos images ``docker images``


Pour forcer la suppression d'une image ou d'un conteneur : ``docker rmi -f nom_image``


La commande ``exec`` permet d'exécuter une commande dans un conteneur démarré

Exemple : notre objectif est de démarrer un conteneur MySQL et d'exécuter le client mysql

Solution : ``docker run --name monserveur -e MYSQL_ROOT_PASSWORD=root -d mysql``

Pour lancer mysql : ``docker exec -ti monserveur mysql --password``


Exercice : démarer un conteneur à partir de l'image ubuntu et puis installer java et exécuter un programme simple. 
 
 
 ``docker run -it ubuntu bash``  
 
 Une fois à l'intérieur du conteneur, vous pouvez  installer les outils dont vous avez besoin comme vous avez l'habitube de le faire en utilisant ubuntu. 

Si on veut installer par exemple Java, on procède de la façon suivante : on met d'abord à jour ``apt-get update``et puis ``apt-get install java``. Si on veut utiliser vim comme éditeur `` apt-get install vim``.  
  
  
Pour inspecter un contenur et avoir des détails sous forme d'un ficheir json ``docker inspect id_conteneur`` 

Pour avoir les logs d'un contenur : ``docker logs id_conteneur`` 

Pour redémarrer un conteneur : ``docker start id_conteneur`` 

Exercice : démarrer un conteneur nginx et changer le contenu du fichier index.html. 

La page index.html est par défaut dans le répertoire ``/usr/share/nginx/htm ``  

Notion de volume :


``docker volume``

```java 
Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes
```



Pour faire "mapping volume" 


``docker run -dti -p 8080:80 -v C:\Users\jerem\Desktop\backupnginx:/usr/share/nginx/html/ --name myserverweb nginx``

``docker run -tid --name webvolumebis -p 8691:80 --mount source=mesdonnees,target=/usr/share/nginx/html/ nginx`` 


Pour accéder aux volumes (sous mac) : 

`` docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh ``

Une fois à l'intéreiur du conteneur, les volumes se trouvent dans le dossier :  ``/var/lib/docker/volumes/``


La solution ici : https://stackoverflow.com/questions/38532483/where-is-var-lib-docker-on-mac-os-x


Gestion des variables d'environnement : 

``docker run -tid --name variableenvironnement --env MAVARIABLE="genneson1234" ubuntu:latest ``

 Passer une variable d'environnemnt en ligne de commande : ``docker exec -ti variableenvironnement sh``
 
 
 On peut aussi passer un ensemble de variables d'environnement sauvegardé dans un fichier : 
 
 
 ``docker run -tid --name fichiervariableenvironnement --env-file mesvariables.lst  ubuntu:latest ``
 
 
 
 Définir sa propre image Docker :
 
 1. docker commit 
 
 Exemple : partir d'une image ubuntu, installer python et vim et ajouter queqlues variables d'environnemnetsauvegardées dans un fichier. 
 
 a. On commence par créer un fichier env.lst (on définit autant de variables souhaitées)
 
 b. Pour ajouter ces variables d'environnement `` docker run --name monconteneurvariables -tdi --env-file env.lst ubuntu``
 
 c. Pour installer python et vim, on rentre à l'intérieur du conteneur `` docker exec -ti monconteneurvariables sh``
 
 Pour installer python et vim : ``apt-get update``, '' apt-get install python`` et  `àpt-get install vim``
 
 d. On quite le conteneur ``exit``
 
 e. Pour créer une image à partir de ce conteneur `` docker commit -m "créeation d'une image ubuntu augumentée" id_conteur nom_notre_image``



 2. Dockerfile 

 a. Créer un fichier ``Dockerfile``
 b. On part d'une image existante (la plus petite est alpine), copier éventuellement un programme et l'exécuter ....
  
  Exemple
  
  
  ```java 
  FROM openjdk
  COPY Application.class Application.class
  CMD ["java", "Application"]
  ```
 
 Exercice : Lucas et Florian avec Node et les autres avec Python 
 

 
 Exercice : Lucas et Florian avec Node et les autres avec Python 
 
 
 ```dockerfile
 FROM ubuntu
COPY test.py test.py
RUN apt-get update
RUN apt-get install python -y
CMD ["python", "test.py"]
 ```
 
 Exercice : Lucas et Florian avec Node et les autres avec Python 
 
 
 FROM : permet de définir l'image source 
 
 CMD : la commande par défaut lors de l'exécution du conteneur 
 
 EXPOSE : permet de définir le port d'écoute par défaut 
 
 RUN : permet d'exécuter des commandes à l'intérieur du conteneur 
 
 ? ADD : permet d'ajouter des fichier dans le conteneur 
 
 VOLUME : permet de définir les volumes utilisables



```dockerfile 
FROM nginx
COPY index.html /usr/share/nginx/html
EXPOSE 80
```


``docker run -d -p 9090:8080 jerem/springcm19novembre``
 
 
 Publier une image sur DockerHub :
 
 1. Créer son image, soit ``josephadd``
 2. Créer un dépôt dockerHub ``jerem/joseph``
 3. Se connecter à DockerHub via ligne de commande : ``docker login`` (facultatif si on utilise docker desktop)
 4. Créer un lien entre l'image ``josephadd``et ``jerem/joseph`` : ``docker tag josephadd jerem/joseph``
 5. Envoyer l'image vers Dockerhub ``docker push jerem/joseph``

 
Pour chercher une image par ligne de commande ``docker serach nom_image``


**Exercice** : publier une image de votre application Spring-boot (utiliser une base de données MySQL pour la persistance des données)

**Solution** : L'objectif de cette exercice est de vous montrer comment lier deux conteneur. Autrement dit, si un conteneur c1 a besoin d'un conteneur c2, comment peut-on l'exprimer ? 

On commence d'abord par développer une application Spring-Boot (ce n'est pas le code en soit qui est important). Partant par exemple d'une API Rest simple, en utilisant Spring Data Repository, JPA, MySQL et Spring Web.  L'API permet de gérer un ensemble de produit (elle est constitué d'une unique classe appelée ``Produit``). On définit l'interface ``Poruitinterface`` qui hérita de la classe ``JpaRepository``. Le code cette interface est donnée par le code suivant : 


**Remarque** Pour développer une API Rest, vous avez trois possibilités : (i) utiliser les annotations JAX-RS, (ii) utiliser les annotations de RestController ou (iii) utiliser Spring Data Rest (c'est la solution la plus aboutie, dans le sens où vous n'auriez pas à réimplémenter les méthodes génértiques de toute API Rest telles que GetById, GetAll, etc.). 

```java 
@Repository
public interface Poruitinterface extends JpaRepository<Produit, Integer> {}
```

La classe ``Produit``est définie comme suit :

```java

@Entity
public class Produit {
    @Id
    private int identifiant;
    private String designation;
    private double prix;
    
    // n'oubliez pas de générer un constructeur sans paramètre, des getters et setters ou utilisez Loombok
    
```

Le fichier de configuration (``application.properties``) est donné ci-après : 

```java
#spring.datasource.url=jdbc:mysql://localhost:8889/produitsdocker?serverTimezone=UTC
# pour docker ....
spring.datasource.url=jdbc:mysql://nomconteneurmysql:3306/produitsdocker
spring.datasource.username=root
spring.datasource.password=root
#spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=create
```

Comme vous pouvez le remarque la valeur de ``spring.datasource.url``est ``jdbc:mysql://nomconteneurmysql:3306/produitsdocker``, où ``nomconteneurmysql``fait référence tout simplement au conteneur (``hostname``) qui empaquette ``MySQL``, sans oublier les variables d'environnements  ``username=root`` et ``password=root`` (vous pouvez bien sûr les changer).  Notre application aura donc besoin du conteneur ``nomconteneurmysql``que nous pouvons lancer comme suit : 

``docker run --name nomconteneurmysql -d -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=produitsdocker  -e MYSQL_PASSWORD=root  mysql``

Nous avons à présent un conteneur qui peut être utilisé notre API Rest. Il nous reste qu'à l'empaqueter. Pour cela (les étapes sont décrites précédemment). Le contenu du fichier Dockerfile est : 


```Dockerfile  
FROM openjdk:8
ADD target/test.jar test.jar
CMD ["java", "-jar", "test"]
EXPOSE 8080
```

Pour créer une image dont le nom est ``spring29novembre``: `` docker build -t spring29novembre``. A ce stade, on l'image  ``spring29novembre``. 


```Dockerfile  
#spring.datasource.url=jdbc:mysql://localhost:8889/produitsdocker?serverTimezone=UTC
# pour docker ....
spring.datasource.url=jdbc:mysql://nomconteneurmysql:3306/produitsdocker
spring.datasource.username=root
spring.datasource.password=root
#spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=create
```

Pour le lancer : ``docker run -p 8181:8080 --link nomconteneurmysql: nomconteneurmysql  -d webspring``

---
---


# Interrogation d'une base de données MongoDB
L'objectif dans ce qui suit est d'installer et d'importer des données dans le SGBD MongoDB, un système NoSQL orienté documents. On commence par télécharger la version community qui se trouve à l'adresse suivante : [https://www.mongodb.com/try/download/community]() (version 3.2.22)

On lance les deux commandes suivantes ``mongod`` pour le serveur et ``mongo``pour le client. On commence par appréhender MongoDB, en utilisant le client ``mongo`` et puis utiliser un client graphique tel que ``Robo 3T``. 

Remarque  : le dossier de sauvegarde des bases de données par défaut de MongoDB est ``/data/db``.  Si vous ne voullez pas utiliser ce répertoire mais un autre ``mongod --dpath $repetoiredesauvegarde``. Le port d'écoute par défaut de MongoDB est ``27017``.  Pour le lancer sur un autre port, on peut utiliser le tag ``--port numero_port``. 


Pour créer une base de donnée `use contacts`; (on utilise un intérpreteur Javascript). 

Pour créer une collection `db.createCollection("amis");` 

Pour créer une deuxième collection `db.cretaeCollection("profesionels")`;

Ajouter un document dans une collection `db.amis.insert({"prenom":"samir"})`; 

Il n'existe pas de schéma pour une base de données NoSQL en général. On peut donc insérer des éléments qui n'ont rien à voir les uns avec les autres. ``db.amis.insert({"numero":1234, "nom":"Alfred", "Tel":0665434343})``;

Le langage d'interoggartion d'une base de données MongoDB est propre à ce système et ne peut être utiliser pour interogger un autre de système de gestion de bases de données (contrairement à SQL). 

Pour afficher toutes les collections ``show collections;``

Pour afficher tous les documents ``db.amis.find();``  

``{ "_id" : ObjectId("619e30bbf168d42f513793ae"), "nom" : "Youcef", "telephone" : 14112994 }``MongoDB attribué un identifiant unique pour chaque document. On peut nous-mêmes donner un identifiant que l'on veut insérer ``db.amis.insert({"_id":1, "nom":"samuel", "Tel":065454321});``.  


Pour parcourir toute une collection : ``db.films.find();``

Pour compter le nombre d'éléments d'une collection ``db.films.find();``

Pour afficher les 15 éléments d'une collection à partir du 11 ème élément ``db.amis.find().skip(1).limit(12)`` 

Pour trier une collection ``db.films.find().sort({"nom":1})`` La valeur 1 est pour le tri décroissant et -1 pour un tri croissant. 

Pour chercher un élément sachant son identifiant ``db.amis.find({"_id":1})``

On peut faire une recherche en utilisant des motifs plus complexe ``db.amis.find({"nom":"Youcef", "telephone":14112994})``




Télécharger le document json des restaurants de New-York. 

Pour importer des données dans MongoDB : ``$MONGO/bin/mongoimport --db mabase --collection restaurants $chemin/restaurants.json``. ``mabase``est le nom de votre base de données, et ``restaurants``est le nom de votre collection. 

Télécharger le fichier json des restaurants de New York que vous trouverez à l(adresse suivante 
[restaurants.json](https://filesender.renater.fr/?s=download&token=443d4a3b-0fa3-4d6b-95f4-ab35055db15b)
 
``mongoimport --db restaurants --collection restaurants ../restaurants.json``

Une fois les restaurants importés dans une base de données ``restaurants``, vous pouvez voir la structure d'un document de la base ``db.restaurats.findOne();``.


```json
{
	"_id" : ObjectId("61a398fe9eb5a64b2978ca52"),
	"address" : {
		"building" : "2300",
		"coord" : {
			"type" : "Point",
			"coordinates" : [
				-73.8786113,
				40.8502883
			]
		},
		"street" : "Southern Boulevard",
		"zipcode" : "10460"
	},
	"borough" : "Bronx",
	"cuisine" : "American ",
	"grades" : [
		{
			"date" : ISODate("2014-05-28T00:00:00Z"),
			"grade" : "A",
			"score" : 11
		},
		{
			"date" : ISODate("2013-06-19T00:00:00Z"),
			"grade" : "A",
			"score" : 4
		},
		{
			"date" : ISODate("2012-06-15T00:00:00Z"),
			"grade" : "A",
			"score" : 3
		}
	],
	"name" : "Wild Asia",
	"restaurant_id" : "40357217"
}
```



#### Filtrage et projection avec un motif JSON
Commençons par parcourir toute une collection. On utilise la fonction ``find()``sans argument. 

``db.restaurants.find()``

Pour compter le nombre de document, on utilise la fonction ``count()``. 

``db.resraurants.find().count()`` 

Comme pour SQL, on peut utiliser la pagination. L'exemple suivant affiche  15 documents en ignorant les 10 premiers. 

``db.restaurants.find().skip(10).limit(115)``

Pour trier les documents on utilise la fonction ``sort``. Pour afficher les restaurant par nom croissant. 

``db.restaurants.find().sort({"name":1})``. Valeur 1 pour un ordre croissant et -1 pour un ordre décroissant. 

Si on veut afficher tous les restaurants du quartier par exemple ``manathan`` : ``db.restaurants.find({"borough": "Manhattan"})``.


``db.getCollection('restaurants').find({"borough" : "Manhattan",  "cuisine" : "Italian", "address.street" : "3 Avenue", "name" : /capri/i }).count()``

db.getCollection('restaurants').find({"borough" : "Manhattan",  "cuisine" : "Italian", "address.street" : "3 Avenue"}, {"name":1, "grades.score":1,  "_id":0}).sort({"name":-1})`` 

#### Projection 


#### Création d'une séquence d'opérations

#### Mise à jour des données


# Map-reduce 


# Protéger vos données grâce au ReplicaSet



La réplication des données sert essentiellement à atteindre deux objectifs :

1. Tolérance aux pannes : si un serveur, un disque tombe en panne, une données reste toujours disponible ;
2. Scalabilité 
	
	
	
	2.1. Distribution des lectures : les lectures sont réparties sur plusieurs serveurs (scalabilité) 
	
	2.2. Distribution des écritures 	si un serveur est très chargé en cas d'écriture, on utilise un autre serveur. Remarque : la plus part des systèmes ne distribuent des écriture (il faut réconcilier des données). 
	
	
	Méthode générale de réplication : soit un client (X) et un serveur S1. Le client soumet une requête d'écriture au serveur. Le serveur écrit le document sur le disque et en cas de réplication transmet la demande aux autres serveurs s2, ..., sn qui feront de même et uniquement une fois que tous les serveurs ont terminé l'écriture sur disque  que le serveur s1 (serveur principal) envoie un acquittement au client.  C'est qu'on appelle le mode de réplication **Synchrone**.  C'est un mode qui favorise **la cohérence des données**. 
	
	Il existe un autre type (ou mode) de réplication dit **asynchrone**. Dans ce cas, le serveur s1 écrit sur son propre disque et rend la main au client X et transmet la demande d'écriture aux autres serveurs s2, ..., sn. C'est un mode qui favorise **les performances (temps de réponse)**.


Trois copies pour une sécurité totale et deux au minimum.


#### Mise en oeuvre d'une architecture tolérante aux pannes avec MongoDB 



![](replicasetmongodb.png)


Pour instancier une Replicaset qui permet de gérer la tolérance aux pannes, par exemple composé de trois serveurs (un ``Primary`` et deux ``Secondary``), il faut définir. 

**Bref rappel**  Une application (client) se connecte directement se connecte directemnent au serveur principal (``primary``) pour toutes opérations de lecture et d'écriture. On associe à ce serveur ``primary``un ensemble de serveurs ``secondary``. Ces derniers ont pour rôle de répliquer les données provenant du serveur ``primary`` . 

1. Donner un nom au Replicatset ``test``
2. Associer un port d'écoute pour chaque serveur
3. Définir un répertoire de stockage pour chaque serveur 
4. Lancer les serveurs
5. A ce stade le ``replicaset``n'existe pas, il faut donc connecter les serveurs


Soit data1, data2, data3 les répertoire de stockage des serveurs S1, S2 et S3 qui écoutent sur les port 27020, 27021, 27022 respectivemnent. 


Pour connecter les serveurs, on procède de la façon suivante : 

On lance les trois serveurs : 


``mongod --replSet rscm --port 27021 --dbpath data1``, ``mongod --replSet rscm --port 27022 --dbpath data2`` et ``mongod --replSet rscm --port 27023 --dbpath data3``


On se connecte au serveur principal S1, par exemple, ``mongo --port 27021`` et on initialise le replicaset ``rs.initiate(configuration)``. 

Le contenu du fichier configuration est : 



```json 
rs.initiate(
   {
      _id: "test",
      version: 1,
      members: [
         { _id: 0, host : "localhost:27021" },
         { _id: 1, host : "localhost:27022" },
         { _id: 2, host : "localhost:27023" }
      ]
   }
)

```

Pour voir la configuration d'un replicaset ``rs.conf();``.   La méthode ``rs.status()``nous donne des information sur qui est le serveur ``primary``et qui sont les serveurs ``secondary``. 


En cas de panne du serveur ``primary``, il peut y avoir un très grand délais de latence (dégradation donc des performance). Pour y remédier, on peut utiliser un serveur qui joue le rôle d'arbitre. 

Pour définir un arbitre, comme pour le autre serveurs qui font partie de cette architecture tolérante aux pannes, on définit le répertoire de stockage ``dataar`` et le lancer : ``rs.addArb("localhost:20724");``. Pour pouvoir lancer un serveur qui jouera le rôle d'un arbitre, il faut absolument le faire à partir du serveur ``primary``. 



**Remarque** pour limiter la taille du fichier log créer au moment du lancement du serveur, on peut utiliser l'option  ``--oplogSize 500``. 


# Distribuer vos données avec MongoDB


# Travail à rendre : 


#### Travail à rendre 

Développer un micro-service qui utilise pour la persistance des données MongoDB. Créer une image Docker de votre service et renseigner le lien au début (vraiment au début) de votre redme.md pour la télécharger.
