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
