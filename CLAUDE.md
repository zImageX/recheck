# ReCheck ✓ — instructions pour Claude Code

Ce repo contient le **prototype de conception** de ReCheck : une app mobile qui aide un voyageur à ne rien oublier en repartant de chez quelqu'un. Le voyageur dicte sa valise, l'hôte vérifie via un lien web sans installer l'app.

## Ce qu'il y a dans le repo

- `DoubleCheck.dc.html` : le prototype complet (~30 écrans) dans un cadre iPhone. **Référence de design, pas du code de production.** Ouvre-le dans un navigateur pour voir les écrans ; le sélecteur en haut de page permet de naviguer entre eux.
- `ios-frame.jsx`, `support.js` : runtime du prototype. Ne pas modifier, ne pas réutiliser dans l'app.
- `assets/logo.png` : la mascotte (une valise qui souffle « ouf »). `assets/backpack.png` : sac à dos du mode « Aujourd'hui ».
- `github.md`, `.thumbnail`, `uploads/` : fichiers de l'outil de design, ignorer.

La tâche : **recréer ces écrans dans une vraie app mobile** (React Native / Expo recommandé, ou SwiftUI + Kotlin si natif), en respectant la charte ci-dessous. Fidélité : haute. Les couleurs, tailles, espacements, textes sont finaux.

## Périmètre v1 (à construire en premier)

Ne pas tout construire d'un coup. Ordre :

1. Splash → Bienvenue (Apple / Google / email) → Accueil (état vide, puis avec trip)
2. Nouveau trip : destination, dates, « Où dors-tu ? » (chez un proche par défaut), hôte choisi dans les contacts
3. Dictée : reconnaissance vocale → liste d'items par zone, ajout manuel, menu « ⋯ » par item
4. Lien hôte (page web mobile, sans compte) : liste, 3 états par objet, « Envoyer ma vérification »
5. Résultat voyageur : « 5/6 trouvés, 1 introuvable (le pull gris) »
6. Notification J-1
7. Réglages : compte, notifications, Ma valise, suppression des données

Tout le reste (Premium, pari entre potes, Insights, Mes affaires, Aujourd'hui, Signaler un vol, Importer mon billet, mode groupe / location, badges et classement hôtes, résumé partageable) est **v2+**. C'est dessiné dans le prototype, ne pas le coder avant que le cœur marche.

## Charte (obligatoire)

**Police** : Nunito (Google Fonts), poids 600 / 700 / 800 / 900. Rien d'autre.

**Couleurs**
- Fond app : `#F7F5EF` (crème). Fond carte : `#FFFFFF`. Fond secondaire / chip : `#F0EDE4`, `#EFEBE0`. Bordure : `#EDE8DC`.
- Marine (titres, texte principal, bouton primaire) : `#042C53`
- Corail (CTA d'action, accents, avatar hôte) : `#D85A30`
- Bleu clair (info, badge « Ajouté récemment ») : fond `#E6F1FB`, texte `#1D6BBE`, accent `#85B7EB`
- Vert « trouvé » : `#2E9E6B` (fond `#E4F4EC`)
- Orange « introuvable » : `#E07B39` (fond `#FCEEE3`)
- Gris texte secondaire : `#6B7B8C`. Gris désactivé / placeholder : `#9AA6B2`
- Ombre carte : `0 6px 24px rgba(4,44,83,0.07)`

**Formes** : cartes radius 24px, boutons radius 999px (pilule), chips radius 999px, champs radius 16px. Icônes outline (style Lucide), stroke 2 à 2.5, jamais d'emoji dans l'UI.

**Typo** : titres d'écran 26–28px / 900. Titres de carte 17–20px / 900. Corps 14–15px / 600–700. Texte informatif minimum 13px. Le 11px est réservé aux badges courts.

**Bouton primaire** : fond `#042C53` ou `#D85A30`, texte blanc 16px / 800, hauteur 54px, pilule. Jamais grisé : s'il manque quelque chose, on affiche un toast doux à la place.

## Règles produit (ne pas transgresser)

- Le nom s'écrit **« ReCheck ✓ »** avec la coche, tel quel, partout. Ne pas changer sa typographie.
- Le mot visible pour la personne qui vérifie est toujours **« Hôte »**. Le concept interne de « vérificateur » (membre du groupe qui fait le check-out en location) n'apparaît **jamais** dans l'UI. En location on écrit « Qui fait le check-out ? ».
- **3 états par objet** côté hôte : Trouvé (vert), Introuvable (orange, l'hôte a cherché), Pas encore vérifié (défaut). Jamais une simple coche binaire.
- **Jamais de garantie à 100 %** dans les textes de résultat. On dit « 5/6 trouvés », pas « Tout est là ».
- L'hôte utilise un **lien web sans compte ni mot de passe**, lié à son numéro. Le lien expire après 48 h. ReCheck ne fait **aucun tracking** de bagage (le lien « Localiser via Find My / Tile » renvoie vers l'app tierce).
- Le pari entre potes n'a **aucun paiement** dans l'app, ReCheck suit juste le résultat.
- Ton : chaleureux, posé, jamais alarmant ni culpabilisant. Tutoiement. La mascotte parle à la première personne dans les notifications (« Hehe, ne m'oublie pas... »).
- **Pas de tirets cadratins « — »** dans les textes : utiliser « : », « , » ou « ... » selon le contexte.
- Objets « privés » : invisibles pour l'hôte. Objets « de valeur » : marine `#042C53`, contour 2px, étoile pleine. Objets « communs » : vérifiés une seule fois pour le groupe, jamais dupliqués par voyageur.

## Navigation

Tab bar à 4 onglets : Accueil, Mes affaires, Insights, Profil. Bouton « + » central (nouveau trip). La carte trip de l'accueil a un seul CTA principal (dicter / compléter la valise) et un menu « ⋯ » pour les actions secondaires (Signaler un vol, Localiser).

## Données et légal (à prévoir dès la v1)

- Photos d'objets, numéros de téléphone d'hôtes non inscrits, lieux de séjour : prévoir consentement, politique de confidentialité, et une vraie suppression complète des données depuis Réglages.
- Sign in with Apple obligatoire si Google est proposé.
- Prix Premium via achats intégrés Apple / Google uniquement (2,49 €/mois ou 19,99 €/an), jamais de paiement externe.
- Dossier de vol : ajouter la mention « ce document n'a pas de valeur légale ».

## Méthode de travail

- Lire le prototype avant d'inventer : chaque écran a un `data-screen-label` dans `DoubleCheck.dc.html`, et la logique (états, textes dynamiques) est dans la classe `Component` en bas du fichier.
- Reprendre les textes tels quels (ils ont été relus).
- Commits petits et fréquents, messages en français.
- Ne jamais modifier `DoubleCheck.dc.html`, `support.js`, `ios-frame.jsx` : ils servent de référence.
