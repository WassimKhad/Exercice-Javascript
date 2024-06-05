Architecture d'un projet Frontend - React :
Comment choisir une architecture ?
Il ne s'agit pas de faire l'architecture la plus complexe mais plutôt l'architecture la plus simple à :

maintenir.
étendre.
modifier.
et notamment à expliquer.
La règle numéro 1 pour choisir une architecture : elle doit être KISS (Keep It Simple Stupid, don't let me think).

Une architecture KISS sera :

simple à expliquer.
simple à démontrer.
simple à comprendre.
simple à modifier.
La règle numéro 2 : abstraction et modules :

L'architecture doit aussi présenter un niveau progressif d'abstraction : couches, modules, fichiers, classes, fonctions.
Elle ne doit pas tout présenter depuis le début (exemple: l'architecture flate). Une telle architecture devient illisible en terme d’arborescence.
Elle doit permettre de se focaliser à chaque fois à un niveau : UI, service, data, flow de données, actions, …
Elle doit créer des frontières et des périmètres pour isoler les changements : un changement au niveau des services ne doit jamais toucher l’UI, un changement au niveau de la logique ne doit jamais toucher l’UI.
Elle doit être SOLID et robuste aux changements.
Les contraintes :
Les contraintes sous lesquelles une application frontend est soumise :

le changement de la réponse du backend.
le changement des termes.
le changement de la mise en forme ou du style.
le changement de la structure du site.
L'application doit être flexible pour absorber la variation de ses contraintes.

Comment avoir une architecture flexible ?
Réponse : en augmentant le nombre d’atomes.

Plus on a d’atomes plus l’architecture est flexible, plus on a des modules réutilisables.

La diversité des molécules est due à la diversité des atomes : HO^2, CO^2, NaCl, …

En contrepartie, les molécules sont difficiles à réutiliser.

Les atomes peuvent être :

des atomes UI : les composants réutilisables.
des atomes services : un service REST = un atome service.
des atomes logiques.
Ces atomes peuvent être rassemblés différemment dans des pages pour former les vues.

Les règles de base :
Pour avoir une architecture simple et modulaire, les principes de base à respecter :

separation Of Concern : SOC.
single Responsibility Principle : SRP.
le concept boîte noire.
Diviser pour régner
Divide and conquer (en anglais) est une technique algorithmique consistant à :

Diviser : découper un problème initial en sous-problèmes.
Régner : résoudre les sous-problèmes (récursivement ou directement s'ils sont assez petits).
Combiner : calculer une solution au problème initial à partir des solutions des sous-problèmes.
Découpage niveau 1 : découpage fonctionnel
Single Responsibility Principle : Gather together the things that changes for the same reasons. Separate those things that change for different reasons. – Robet C. Martin –

En appliquant ce principe, on aura une première décomposition par Feature.

Par exemple :

Feature : comptes. Ce feature groupe les fonctionnalités permettant de gérer les comptes utilisateurs (ajout, suppression, liste, modification, recherche, détails, …).
Feature : produits. Ce feature groupe les fonctionnalités relatives à la gestion des produits (recherche, filtre, liste produits, détails produits, panier, …).
Avec une telle décomposition, on limite à chaque fois la modification à un périmètre : du problème global vers des sous problèmes.

Découpage niveau 2 : découpage technique
When designing an application or system, the goal of a software architect is to minimize the complexity by separating the design into different areas of concern. For example, the user interface (UI), business processing, and access data all represent different area of concern. Within each area, the components you design should focus on that specific area and should not mix code from other areas of concern. UI processing components should not include code that directly access data source, but instead should use either business components or data access components to retrieve data. Do not mix different types of components in the same logical layer.- Microsoft Software Architecture And Design -

En appliquant le principe de Separation Of Concern, on aura un deuxième découpage technique ou par rôle technique ou logique.

Chaque feature est décomposé en :

une couche Services : qui gère la communication avec le backend.
une couche Components : la couche contenant des composants purs, affichent uniquement, ne faisant aucune logique, ne dépendent que de l’entrée et ne créant aucun side effects (stateless, dumb).
une couche Pages : Presentation Logic Components (container, statefull, couplé à un service ou une logique).
Les components doivent être des boîtes noires : ils exposent des entrées (props) et des sorties (actions).

Pour avoir un système prédictible et un fonctionnement correcte, les components ne doivent jamais modifier les entrées et ne créent jamais des side-effects (pures).

Pour que les components soient réutilisables, les tâches suivantes sont déléguées à la page :

Comment les entrées sont récupérées : service, données statiques, …
Que va-t-on faire si les actions sont déclenchées.
Ainsi les composants peuvent être assemblés dans des différentes pages.

Exemple :

Affichage de la liste de tous les produits et de tous les produits dans le panier.
On crée un composant générique qui prend en paramètres une liste des produits et en sortie l’action sera le click sur un produit.
Dans la première page on affiche la liste des tous les produits et au click on ouvre la page Détails Produit.
Dans la deuxième page on affiche la liste des produits dans le panier (par id user) et au click on affiche une popup Détails Produit.
Dans les deux pages j’ai exploité le même composant Liste des produits différemment :
Entrée : soit tous les produits soit les produits par panier mais l’essentiel une entrée de type Liste Produit.
Sortie : chaque page a fait son traitement spécifique au choix d’un produit.
ARCH_FRONT_1.png ARCH_FRONT_2.png

Découpage niveau 3 : créer les atomes
On se concentre sur les couches : components et services.
A chaque service REST correspond un fichier : un atome service.

Les components sont des unités UI :

autonomes.
complètes.
pures.
ne font aucune logique, uniquement affichage.
facilement testable.
Pour les termes, créer des atomes Dico :

ARCH_FRONT_3.png

Si vous utilisez Redux-Saga, créer un Reducer par service Rest : atomes data.

Créer les molécules
La couche Pages va rassembler les atomes (services, dico, data et UI) pour former la vue finale (la molécule) :

Une page = une route.

La page (la molécule) est difficilement réutilisable.

Elle forme l'environnement adéquat et commun pour assembler les différents atomes et assurer le partage et l’échange.

Synthèse :
Problème globale => décomposition features => décomposition en couches techniques => créer les atomes => former les molécules

A chaque phase on descend dans le niveau d’abstraction et on se concentre sur une partie du problème globale :

Le problème globale est découpé en des features.
Les features sont découpés en des couches techniques.
On se concentre sur les couches techniques : components et services.
On crée les atomes.
On rassemble ces atomes dans les molécules Pages.
Profondeur des modules :
Pour la couche UI, essayez d’avoir un maximum de profondeur = 4 :

Page => Liste => Row.
Page => Formulaire => Liste => Row.
Cette stratégie permet d’éviter les passages des props dans les profondeurs et aide à centraliser la logique dans la page.

N'utilisez pas le Redux pour combler les défauts de conception.

Redux-Saga ou HOCs pour la couche service :
HOC :
si le nombre de services à appeler dans une page est au maximum 1 service (1-1).
vous avez un seul state à gérer par page.
single source of truth. Les HOCs avec les Hooks sont recommandés dans ces cas.
Redux-Saga :
si le nombre de services dans une page >1 (1-n).
vous avez plusieurs states à gérer par page.
single source of truth.
Vous pouvez toujours commencer par des HOCs et Hooks jusqu’à ce que la gestion du state (1-1) devienne difficile.

Le state React doit être toujours prédictible, claire et l’architecture doit donner clairement qui est en train de gérer le state.

Architecture UI-Redux-Saga :
UI-REDUX-SAGA.png
