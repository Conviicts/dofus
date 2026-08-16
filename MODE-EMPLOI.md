# Mode d'emploi

Le tracker est en ligne : **https://conviicts.github.io/dofus/**

Tout est déjà configuré. Rien à installer, aucun compte à créer, sur aucun appareil.

---

## Rejoindre l'équipe

Sur n'importe quel navigateur, téléphone compris :

1. Ouvre l'URL.
2. Clique **Rejoindre l'équipe** en haut.
3. Tape le **code d'équipe** et clique **Connecter**.
4. La page demande un pseudo — c'est uniquement pour afficher qui a modifié en dernier.

Le point à gauche passe au **vert** : vous voyez les mêmes données en direct.

C'est tout. Le code d'équipe est la seule chose à connaître, et la seule chose à ne pas laisser traîner : c'est lui qui donne accès aux données.

> Le code est insensible à la casse (`Natsu` = `natsu`) et les espaces autour sont ignorés, **mais pas les fautes de frappe**. Un caractère en trop mène à une équipe vide et séparée. Si ça arrive, la page te le dit maintenant explicitement au lieu d'afficher « Synchronisé ».

---

## Ce qui se synchronise

**Partagé entre vous deux** : les cases cochées du plan, les niveaux de métier, la prospection, les noms des personnages, le choix du récolteur principal.

**Local à chaque navigateur** : la **checklist quotidienne** de l'onglet Routine. C'est voulu — vous ne faites pas la même journée. Elle se remet à zéro toute seule au changement de jour.

---

## Bon à savoir

**Sans connexion à l'équipe, la page marche quand même.** Elle enregistre la progression dans le navigateur. La synchro est un bonus, pas une dépendance.

**Le SDK Firebase n'est téléchargé qu'au moment de se connecter.** Si le réseau tombe, la page reste utilisable en mode local.

**Un bloqueur de pub agressif peut bloquer `gstatic.com`.** Si le statut reste rouge, mets l'URL en liste blanche.

**La configuration Firebase est intégrée à la page, et ce n'est pas une fuite.** L'`apiKey` d'une application web Firebase est un identifiant public, pas un secret — elle ne donne aucun droit à elle seule. Ce qui protège vos données, ce sont les règles de la base et le code d'équipe, qui lui n'est écrit nulle part dans le fichier publié.

**Quotas gratuits Firebase** (plan Spark) : 1 Go stocké, 10 Go de transfert par mois. Le tracker pèse quelques kilo-octets. Le plan gratuit ne bascule jamais en payant tout seul.

**Le bouton « Sauvegarder / charger »** reste la roue de secours : il sort un fichier JSON avec toute la progression, à recharger ailleurs ou à garder de côté avant une grosse manip.

---

## Mettre la page à jour

Sur GitHub, ouvre `index.html` → icône crayon → colle la nouvelle version → *Commit*. Le site se met à jour en une minute. Vos données ne sont pas dans le fichier, elles sont dans Firebase : elles survivent à la mise à jour.

---

## Annexe — refaire la base à zéro

À ne lire que si vous voulez héberger les données sur un autre projet Firebase (celui d'origine étant `dofus-team-59f4e1b7-cd96b`).

1. [console.firebase.google.com](https://console.firebase.google.com) → **Créer un projet**, sans Google Analytics.
2. Menu de gauche → **Créer** → **Realtime Database** (⚠️ pas *Firestore*, juste au-dessus) → région **europe-west1** → **Démarrer en mode verrouillé**.
3. Onglet **Règles** → remplacer par le contenu de `regles-firebase.json` → **Publier**. Ces règles n'ouvrent que `teams/<code>` pour un code d'au moins 8 caractères : impossible de lire la racine ni d'énumérer les équipes existantes.
4. Roue crantée → *Paramètres du projet* → **Vos applications** → icône `</>` (Web) → sans Firebase Hosting → copier le bloc `const firebaseConfig = { … };`.
5. Dans la page : **Rejoindre l'équipe** → déplier *« Utiliser une autre base Firebase que celle intégrée »* → coller le bloc → saisir le code → **Connecter**.
