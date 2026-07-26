# cristo67000.github.io

Dépôt racine du compte : sert de page d'index vers les applications hébergées
sur `github.io/<repo>/`, et surtout héberge `/.well-known/assetlinks.json`.

## Pourquoi ce dépôt existe

La vérification « Digital Asset Links » utilisée par les TWA (apps Android
empaquetant une PWA pour le Play Store) va toujours chercher
`https://<domaine>/.well-known/assetlinks.json` **à la racine du domaine**,
jamais dans un sous-chemin. Or toutes les PWA de ce compte sont hébergées en
sous-chemin (`cristo67000.github.io/<repo>/`) et partagent donc le même
domaine `cristo67000.github.io`. Une seule app ne peut donc pas publier son
propre `assetlinks.json` là où Android le cherche : il faut un fichier
racine, ici.

## Comment ça marche

Le fichier racine ne contient que des instructions `include`, une par
application publiée en TWA :

```json
[
  { "include": "https://cristo67000.github.io/sites-templiers/.well-known/assetlinks.json" }
]
```

Chaque application garde ainsi son propre `assetlinks.json` (empreinte de
signature, nom de paquet) dans son propre dépôt — seule l'ajout de la ligne
`include` ici est nécessaire pour une nouvelle app, sans jamais toucher au
fichier d'une autre.

## Ajouter une nouvelle application

1. Publier `<repo>/.well-known/assetlinks.json` dans le dépôt de l'app
   (avec `.nojekyll` à la racine de ce dépôt, sinon Jekyll ignore les
   fichiers commençant par un point).
2. Ajouter une entrée `include` ci-dessus pointant vers cette URL.
3. Vérifier : `curl https://cristo67000.github.io/.well-known/assetlinks.json`
   puis l'[outil de test Google](https://developers.google.com/digital-asset-links/tools/generator).
