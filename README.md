# Widget Grist — Timeline hiérarchique à 2 niveaux

Ce projet est un **widget pour Grist** qui affiche un planning type **Gantt** avec une hiérarchie **Parent → Enfant**.

L’interface permet de naviguer dans le temps, regrouper/plier les tâches, changer la coloration des barres et modifier les dates directement depuis le widget (drag & drop), avec synchronisation vers Grist.

## Fonctionnalités principales

- Affichage Gantt hiérarchique (parents + tâches enfants).
- Zoom temporel : `jour`, `semaine`, `mois`, `année`, `tout`.
- Navigation temporelle : précédent / suivant / aujourd’hui.
- Repli / dépli global et par parent.
- Coloration configurable selon différents champs (priorité, statut, responsable, etc.).
- Infobulles riches au survol (dates, statut, priorité, responsables).
- Édition des dates par glisser-déposer sur les barres.
- Persistance locale de l’état UI (zoom, champ couleur, affichage labels, etc.) via `localStorage`.

## Structure du projet

- `index.html` : UI du widget (layout + styles + chargement API Grist).
- `widget.js` : logique métier (données, rendu timeline, interactions, persistance, sync Grist).
- `README.md` : documentation du projet.

## Prérequis

- Un document **Grist**.
- La possibilité d’ajouter un widget par **URL personnalisée** dans une vue Grist.
- Une table contenant au minimum des colonnes compatibles avec :
  - niveau parent,
  - niveau enfant,
  - date de début,
  - date de fin.

> Le code prévoit aussi des champs optionnels (ex: priorité, statut, responsables) utilisés pour l’affichage, les tooltips et la coloration.

## Installation / utilisation dans Grist

1. Héberger `index.html` et `widget.js` sur une URL accessible (GitHub Pages, serveur interne, etc.).
2. Dans Grist, ajouter un widget via une **URL personnalisée**.
3. Renseigner l’URL de `index.html`.
4. Mapper les colonnes Grist attendues par le widget.
5. Interagir avec le Gantt (zoom, navigation, drag & drop, filtres visuels).


## Écriture vers une table source (recommandé avec colonnes calculées)

Si votre vue Gantt est une table consolidée avec des `lookup` / formules (donc non éditables), configurez deux colonnes supplémentaires dans le mapping du widget :

- `sourceRowId` : ID de la ligne métier réelle (ex: `$Sous_Projet.id`)
- `sourceTableId` : identifiant de la table métier (ex: `Sous_Projets`)

Dans ce mode, le widget continue à lire la vue consolidée mais envoie les modifications de dates directement vers la table source via `docApi.applyUserActions`.

Sans ces colonnes, le widget conserve le comportement standard et tente d’écrire dans la table sélectionnée.

## Détails techniques

- API Grist chargée depuis : `https://docs.getgrist.com/grist-plugin-api.js`.
- Le widget utilise un modèle de dates normalisées au jour.
- Les modes de zoom sont définis dans `ZOOMS`.
- Une palette de couleurs par défaut est définie dans `PALETTE`.
- L’état utilisateur est stocké sous la clé `grist_gantt_state_v11`.

## Personnalisation

Vous pouvez adapter rapidement :

- la palette (`PALETTE`),
- les niveaux de zoom (`ZOOMS`),
- les champs proposés pour la coloration (`availableColorFields`),
- les textes FR/UX et les styles CSS.

## Limites connues

- Le widget dépend de la qualité du mapping colonnes dans Grist.
- Les performances peuvent baisser sur de très gros volumes de lignes.
- La persistance d’état étant locale au navigateur, elle n’est pas partagée entre utilisateurs.

## Licence

Ce projet est distribué sous licence **MIT**.
