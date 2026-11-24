# Activité Pratique N°2 — ORM, JPA, Hibernate et Spring Data
 
## Objectif
 
Mettre en place la couche de persistance d'une application Spring Boot avec JPA et Hibernate, puis la manipuler avec Spring Data. L'application gère des produits et expose en plus une API REST pour tester les opérations.
 
## Technologies
 
- Java et Spring Boot
- Spring Data JPA (Hibernate comme implémentation JPA)
- Base de données H2 en mémoire, console H2 activée
- Spring Web (API REST)
- Lombok
- springdoc-openapi (documentation Swagger)
## Structure
 
```
src/main/java/abdospringdata
├── AbdoSpringDataApplication.java   Point d'entrée + CommandLineRunner de test
├── entities/Product.java            Entité JPA
├── repository/ProductRepository.java Interface Spring Data
└── web/ProductRestController.java   API REST
src/main/resources/application.properties
```
 
## Entité `Product`
 
| Attribut | Rôle |
|---|---|
| `id` | Clé primaire, générée automatiquement |
| `name` | Nom du produit |
| `price` | Prix |
| `quantity` | Quantité en stock |
 
La table générée s'appelle `Products`. Lombok génère getters, setters, constructeurs, `toString` et le builder.
 
## Repository
 
`ProductRepository` étend `JpaRepository<Product, Long>` et ajoute une méthode dérivée du nom, `findByNameContains(String kw)`, qui recherche les produits dont le nom contient un mot-clé.
 
## Opérations testées
 
Au démarrage, un `CommandLineRunner` enregistre trois produits (par setters, par constructeur et par builder) puis les affiche. L'API REST permet de tester :
 
| Méthode | URL | Action |
|---|---|---|
| GET | `/products` | Lister tous les produits |
| GET | `/products/{id}` | Consulter un produit |
| GET | `/find/products?kw=...` | Rechercher par mot-clé |
| POST | `/products` | Ajouter un produit (ou mettre à jour si l'`id` existe) |
| DELETE | `/products/{id}` | Supprimer un produit |
 
## Configuration
 
Dans `application.properties` : nom de l'application, URL de la base H2 en mémoire (`jdbc:h2:mem:products-db`), console H2 activée, port `8080`.
 
## Exécution
 
```bash
mvn spring-boot:run
```
 
- API : `http://localhost:8080/products`
- Console H2 : `http://localhost:8080/h2-console` (JDBC URL : `jdbc:h2:mem:products-db`)
- Swagger UI : `http://localhost:8080/swagger-ui.html`
La base étant en mémoire, les données sont perdues à chaque arrêt de l'application.
 
## Concepts illustrés
 
- Mapping objet-relationnel (ORM) et JPA
- Entité, clé primaire et génération d'identifiant
- Repository Spring Data et méthodes dérivées
- Opérations CRUD
- Exposition d'une API REST
