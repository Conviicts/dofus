# Mettre le plan en ligne — gratuitement

Deux parties, ~10 minutes en tout, une seule fois. Tout est gratuit et **sans carte bancaire**.

- **Partie A — Firebase** : la base qui synchronise ta progression et celle de ton pote.
- **Partie B — GitHub Pages** : l'hébergement de la page, avec une URL permanente.

Fais la partie A en premier : tu auras besoin de son résultat à la fin.

> Une seule personne fait les parties A et B. L'autre reçoit juste l'URL et le code d'équipe.

---

## Partie A — Créer la base Firebase

### 1. Créer le projet

Va sur **console.firebase.google.com** et connecte-toi avec un compte Google.

Clique **Créer un projet**, donne-lui un nom (`dofus-team` par exemple). À l'étape Google Analytics, **décoche-le** : inutile ici, et ça t'évite une page de configuration.

### 2. Créer la base de données

Dans le menu de gauche : **Créer** (ou *Build*) → **Realtime Database** → **Créer une base de données**.

- Région : **europe-west1** (ou celle qui t'est proposée par défaut).
- Mode de sécurité : choisis **Démarrer en mode verrouillé**. On ouvre juste ce qu'il faut à l'étape suivante.

⚠️ Attention à ne pas confondre avec **Firestore Database**, qui est juste au-dessus dans le menu. Il te faut bien **Realtime Database**.

En haut de la page tu vois maintenant une URL du type
`https://dofus-team-default-rtdb.europe-west1.firebasedatabase.app`
👉 **copie-la quelque part**, elle peut servir.

### 3. Ouvrir les règles d'accès

Toujours dans Realtime Database, onglet **Règles**. Remplace tout le contenu par ceci, puis clique **Publier** :

```json
{
  "rules": {
    "teams": {
      "$code": {
        ".read": "$code.length >= 8",
        ".write": "$code.length >= 8"
      }
    }
  }
}
```

Ce que ça veut dire : votre **code d'équipe** sert de clé d'accès à votre espace, et personne ne peut toucher au reste de la base. Prends un code un peu long — genre `convict-natsu-k7x9p2m4` plutôt que `dofus` — histoire que personne ne tombe dessus par hasard et n'aille cocher vos cases.

### 4. Récupérer la configuration

Clique sur la **roue crantée** en haut à gauche → **Paramètres du projet**. Descends tout en bas, section **Vos applications**, et clique sur l'icône **`</>`** (Web).

- Pseudo de l'application : ce que tu veux.
- **Ne coche PAS** « Configurer aussi Firebase Hosting » — on utilise GitHub Pages.
- Clique **Enregistrer l'application**.

Firebase affiche un bloc de code. Copie **tout le bloc `const firebaseConfig = { … };`**, accolades comprises. C'est ce que tu colleras dans la page.

> Si `databaseURL` n'apparaît pas dans ce bloc, pas de panique : c'est l'URL que tu as copiée à l'étape 2, il y a un champ prévu pour elle dans la page.

---

## Partie B — Publier la page sur GitHub Pages

### 1. Compte et dépôt

Crée un compte gratuit sur **github.com** si tu n'en as pas.

Clique **+** en haut à droite → **New repository** :

- Nom : `dofus`
- Visibilité : **Public** — obligatoire, GitHub Pages sur dépôt privé est payant.
- Clique **Create repository**.

### 2. Envoyer le fichier

Sur la page du dépôt, clique **uploading an existing file** (ou **Add file → Upload files**).

Glisse le fichier **`index.html`** dedans, puis clique **Commit changes**.

Le nom `index.html` est important : c'est lui qui rend l'URL propre.

### 3. Activer Pages

**Settings** (onglet en haut du dépôt) → **Pages** dans le menu de gauche.

- Source : **Deploy from a branch**
- Branch : **main**, dossier **/ (root)**
- **Save**

Attends 1 à 2 minutes, puis recharge la page : l'URL apparaît en haut.

```
https://TON-PSEUDO.github.io/dofus/
```

---

## Partie C — Mise en route

1. Ouvre ton URL, clique **Configurer l'équipe** en haut.
2. Colle le bloc `firebaseConfig` dans le champ 1.
3. Si `databaseURL` n'y était pas, colle l'URL de la base dans le champ 2.
4. Choisis ton **code d'équipe** (champ 3) et clique **Connecter**.
5. La page demande ton pseudo — c'est uniquement pour afficher qui a modifié en dernier.

Le point à gauche passe au **vert** : c'est synchronisé.

Envoie ensuite à ton pote **l'URL, le bloc `firebaseConfig` et le code d'équipe**. Il fait la même manip de son côté (il n'a besoin d'aucun compte). À partir de là vous voyez les mêmes données en direct : une case cochée chez toi apparaît chez lui.

---

## Bon à savoir

**Ce qui se synchronise, et ce qui ne se synchronise pas.** Sont partagés : les cases cochées du plan, les niveaux de métier, la prospection, les noms des personnages et le choix du récolteur principal. Reste local à chaque navigateur : la **checklist quotidienne** de l'onglet Routine — c'est voulu, vous ne faites pas la même journée. Elle se remet à zéro toute seule au changement de jour.

**Votre configuration Firebase n'est pas dans le fichier publié.** Le dépôt GitHub est public, mais `index.html` ne contient ni votre `firebaseConfig` ni votre code d'équipe : ils restent dans le navigateur de chacun. Quelqu'un qui tombe sur votre dépôt récupère une page vide, pas vos données.

**L'`apiKey` Firebase n'est pas un secret.** Elle identifie le projet, elle ne donne aucun droit à elle seule — c'est prévu pour être visible dans une page web. Tu peux l'envoyer à ton pote sans y penser. Ce qui protège vos données, c'est le code d'équipe et les règles de l'étape A3.

**Sans connexion à l'équipe, la page marche quand même.** Elle enregistre ta progression dans ton navigateur. La synchro est un bonus, pas une dépendance.

**Le SDK Firebase n'est téléchargé qu'au moment de se connecter.** Si le réseau tombe ou qu'un bloqueur de pub s'en mêle, tu verras un message d'erreur mais la page restera utilisable.

**Un bloqueur de pub agressif peut bloquer `gstatic.com`.** Si le statut reste en erreur, mets ton URL GitHub Pages en liste blanche.

**Quotas gratuits Firebase** (plan Spark) : 1 Go stocké et 10 Go de transfert par mois. Le tracker pèse quelques kilo-octets — vous êtes à environ 0,001 % de la limite. Aucun risque de facturation, le plan gratuit ne bascule jamais en payant tout seul.

**Le bouton « Sauvegarder / charger » reste disponible** : c'est ta roue de secours si tu veux repartir d'un état précis, ou déplacer ta progression vers un autre navigateur.

**Pour mettre à jour la page plus tard** : sur GitHub, ouvre `index.html` → icône crayon → colle la nouvelle version → *Commit*. Le site se met à jour tout seul en une minute. Vos données ne sont pas dans le fichier, elles sont dans Firebase : elles survivent à la mise à jour.
