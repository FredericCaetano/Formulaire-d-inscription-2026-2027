# Formulaire d'inscription — 19PokerClub saison 2026-2027

Formulaire d'inscription en ligne pour le 19PokerClub. Envoie les données vers un
Google Sheet et déclenche des e-mails automatiques (bureau du club + joueur).

## Contenu du dépôt

- `Index` — page web du formulaire (HTML/CSS/JS, aucune dépendance externe)
- `Code` — script Google Apps Script à coller dans le Google Sheet
- `README.md` — ce fichier

## ⚠️ Sécurité — dépôt public

Ce dépôt est **public**. Le fichier `Code` ne doit **jamais** contenir de valeurs
sensibles en clair (lien d'affiliation Winamax, code d'entrée club BlindValet).
Ces valeurs sont stockées côté Google, dans les **Propriétés du script**
(Apps Script > Paramètres du projet), qui ne sont jamais exposées dans le code
source ni synchronisées avec Git.

> Un ancien code d'entrée BlindValet a été committé en clair dans une version
> précédente de ce fichier. Si ce n'est pas déjà fait : **change ce code côté
> BlindValet**, puisqu'il est visible dans l'historique public du dépôt.

## Déploiement

### 1. Google Sheet + Apps Script

1. Crée un Google Sheet (ex. "Inscriptions 19PokerClub 2026-2027").
2. Menu **Extensions > Apps Script**.
3. Colle le contenu du fichier `Code` de ce dépôt (remplace tout le code par défaut).
4. Dans l'éditeur Apps Script, va dans **Paramètres du projet** (icône ⚙️) >
   **Propriétés du script** > **Ajouter une propriété de script**, et ajoute :

   | Propriété | Valeur |
   |---|---|
   | `WINAMAX_LIEN_AFFILIATION` | ton lien d'affiliation Winamax |
   | `BLINDVALET_LIEN_SITE` | l'URL du site BlindValet |
   | `BLINDVALET_CODE_CLUB` | le code d'entrée du club |

   *(Alternative : renseigne temporairement ces valeurs dans la fonction
   `configurerProprietes_()` du script, exécute-la une fois via le bouton
   ▶️ Exécuter, puis efface ces valeurs du fichier avant de committer.)*

5. **Déployer > Nouveau déploiement** → type "Application web" → exécuter en
   tant que **Moi**, accès **Tout le monde**. Autorise les permissions demandées.
6. Copie l'URL du déploiement (se termine par `/exec`).

### 2. Page web (`Index`)

1. Ouvre `Index` et remplace la ligne :
   ```js
   const SCRIPT_URL = "COLLER_ICI_URL_DU_WEB_APP_APPS_SCRIPT";
   ```
   par l'URL copiée à l'étape précédente.
2. Héberge `Index` en le renommant `index.html` sur GitHub Pages, Vercel, etc.
   (renomme le fichier avec l'extension `.html` pour que l'hébergeur le serve
   correctement).

## Fonctionnement

- Chaque soumission ajoute une ligne dans l'onglet **Inscriptions** du Sheet et
  envoie un e-mail récapitulatif à `19pokerclub@gmail.com`.
- Si la case **« Je n'ai pas de compte Winamax »** est cochée → le joueur reçoit
  un e-mail l'incitant à créer un compte via le lien d'affiliation du club.
- Si la case **« Je n'ai pas de compte BlindValet »** est cochée → le joueur
  reçoit un e-mail avec le lien du site et le code d'entrée du club.
- Si les deux cases sont cochées, le joueur reçoit les deux e-mails.

## Mise à jour du script après modification

Après toute modification de `Code`, il faut redéployer : dans l'éditeur Apps
Script, **Déployer > Gérer les déploiements > ✏️ (modifier) > Nouvelle version
> Déployer**. L'URL `/exec` reste la même, pas besoin de la changer côté `Index`.
