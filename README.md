# Image de mon projet SpringBoot

https://hub.docker.com/repository/docker/gennesonj/image_mongo_bigdata


# Pour tester mon projet

1. Recupérer le docker-compose.yml sur GitHub.

      https://github.com/GennesonJ/BigData

2. Ouvrir un cmd à l'emplacement du docker-compose.yml.

3. Lancer la commande
      ```bash
      docker-compose up
      ```
      Elle permet de télécharger les images du projet (si elles ne sont pas présentes), créer les conteneurs et les lancer.


# Réalisation de l'exercice

## Création du projet SpringBoot

Tout d'abord, j'ai initialisé un projet SringBoot sans oublier d'ajouter les dépendances "Spring Web" et "Spring Data MongoDB" pour créer une API Rest simple qui gère des produits stockés dans une base de donnée MongoDB.

Pour ce faire j'ai créée, dans un premier temps une entité produit comme ci-dessous, afin de mapper les attributs de mon produit avec ceux de la base MongoDB:

```java
@Document(collection="produit")
public class Produit {
    @Id
    private String id;
    private String name;
    private double prix;

    public Produit() {
    }

    public Produit(String id, String name, double prix) {
        this.id = id;
        this.name = name;
        this.prix = prix;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrix() {
        return prix;
    }

    public void setPrix(double prix) {
        this.prix = prix;
    }
}
```

J'ai ensuite créé un repository pour requêter la base de donnée: 

```java
@Repository
public interface ProduitRepository extends MongoRepository<Produit,String> {

    Produit findByName(String name);
    
}
```

Pour finir j'ai implémenté mon controller avec des méthodes afin d'ajouter et de récupérer des produits:

```java
@RestController
@RequestMapping("/produit")
public class ProduitService {

    private final ProduitRepository produitRepository;

    public ProduitService(ProduitRepository produitRepository) {
        this.produitRepository = produitRepository;
    }
    @GetMapping("/{name}/{prix}")
    public Produit add(@PathVariable String name, @PathVariable double prix){
        if(!name.isEmpty())
            return produitRepository.insert(new Produit(null, name, prix));
        else
            return null;
    }
    @GetMapping("/{name}")
    public Produit add(@PathVariable String name){
        return produitRepository.findByName(name);
    }
}

```

## Création de l'image de mon projet SpringBoot

J'ai crée un Dockerfile qui va me permettre de build l'image. Le voici:

```dockerfile
FROM openjdk:14
ADD mongo-bigdata.jar mongo-bigdata.jar
CMD ["java", "-jar", "mongo-bigdata.jar"]
expose 8080
```
 
Ensuite je build l'image:

```bash
docker build -t gennesonj/image_mongo_bigdata .
```

Et enfin je créé le conteneur et le lance:

```bash
docker run -p 9091:8080 gennesonj/image_mongo_bigdata
```

On peut constater que l'image ainsi que le projet fonctionne à l'aide d'un navigateur à l'URL **localhost:9091**

## Push de l'image de mon projet SpringBoot sur Docker Hub

J'ai créé un repository sur DockerHub.

Puis pour push l'image vers Dockerhub, j'ai utilisé la commande:

```bash
docker push gennesonj/image_mongo_bigdata
```

## Création du docker-compose.yml pour lier le projet Springboot à MongoDB

Pour finir, afin de lier notre projet Spring Boot à notre base MongoDB, on crée un fichier docker-compose.yml:

```yml
version: "3.8"
services:
  api-database:
    image: mongo
    container_name: "api-database"
    ports:
      - 27017:27017
  api:
    image: gennesonj/image_mongo_bigdata
    ports:
      - 9091:8080
    links:
      - api-database
```

Ce fichier est composé de:
1. la version de docker-compose que l'on choisi
2. deux services nécessaires au projet (mon projet Springboot et MongoDB)

Ces services sont composé eux-même de:
1. l'image utilisée
2. si on souhaite donner un nom au conteneur
3. le port utilisé
4. si on souhaite faire un lien
