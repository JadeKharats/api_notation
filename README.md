Construire une API micro service de notation avec sinatra et mongo en TDD
==

C'est quoi, un microservice?
-
http://www.touilleur-express.fr/2015/02/25/micro-services-ou-peon-architecture/
http://martinfowler.com/articles/microservices.html

Identifions les besoins
-

On doit savoir :
* ce qui est noté
* par qui
* suivant quel critère
* la note

En SQL, le modèle de donnée pourrait donné quelque chose de ce genre là.

NOTATION

| Champs              | Type     | Exemple                          |
| ------------------- | -------- | -------------------------------- |
| id                  | integer  |                                  |
| Product             | string   | http://amazon.com/products/23325 |
| Author              | string   | jade.kharats@gmail.com           |
| Criteria            | string   | Fruité                           |
| note                | integer  | 3                                |

Mais là, on va réflechir à l'usage final pour adapter le modèle.
Le but final de notre api est de permettre à un site de rajouter un composant de notation sur une page produit. Notre point d'entrée est donc le produit

PRODUIT

| Champs | Type   |
| ------ | ----   |
| id     | hash   |
| url    | string |

Pour un produit, on va vouloir permettre à nos utilisateurs de rajouter une note quivant une liste de critère.

NOTATION

| Champs              | Type     |
| ------------------- | -------- |
| id                  | hash     |
| Author              | string   |
| Criteria            | string   |
| note                | integer  |

Charge à l'application appelante de gérer ce qui identifie un Author et la liste des critères. Comme on va utiliser mongo, NOTATION sera un EmbedDocument de Produit.

PRODUIT

| Champs     | Type     |
| ---------- | -------- |
| id         | hash     |
| url        | string   |
| notations  | notation |

On part sur ce modèle pour l'instant. On ajoutera plus tard un traitement asychrone pour le calcul des moyennes par critère.

Identifions les routes et le dialogue
-

* Le client appel l'api pour remonter les notations pour l'url courante.
* L'api renvoi le document produit correspondant.
  * si aucun élément trouvé pour l'url produit alors le produit est créé.
* Le client affiche à ses users les notations et un formulaire de saisie de notation.
* Un user saisie une notation
* Le client envoi cette notation à l'api
* L'api l'ajoute au produit et renvoi un code retour pour success or fail

on peut en déduire les urls suivantes :

| Verbe  | Urls                     | Données                           | Conditions       | Retour                                           |
| ------ | ------------------------ | ----------------------------------| ---------------- | ------------------------------------------------ |
| POST   | /products                | url du produit                    |                  | renvoi la liste des notations ou crée le produit |
| POST   | /products/:id/notations  | id_produit et fields notations    |                  | succes or fail                                   |
