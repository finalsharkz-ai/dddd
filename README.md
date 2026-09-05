# Habbo Hotel v26

Client Habbo isométrique **en un seul fichier HTML** : `habbo-v26.html`.
Aucune dépendance, aucun build — il suffit d'ouvrir le fichier dans un navigateur.

## Ce que contient le fichier

| Élément | Origine |
|---|---|
| Avatars | API d'imagerie **officielle Habbo** (`habbo-imaging/avatarimage`) |
| Garde-robe | `figuredata` officiel : 13 types, 1068 jeux de vêtements |
| Salle, murs, mobilier | Dessinés à la volée en isométrique (canvas) |
| Mascottes | Sprites du pack fourni (atlas AVIF intégré au HTML) |

## Fidélité au client Habbo

- Cases de **64 × 32 px**, dalle épaisse, murs avec épaisseur visible et embrasure de porte.
- Les **8 orientations** Habbo, avec la convention des émulateurs
  (2 = +x, 4 = +y, 6 = −x, 0 = −y).
- Déplacement au clic avec **cheminement A\*** qui contourne les meubles
  et ne coupe pas les angles.
- On **s'assoit** sur une chaise et on **s'allonge** sur un lit en cliquant dessus.
- Meubles **orientables sur 4 côtés** : les volumes tournent réellement.
- **Bulles de chat** ancrées au-dessus de la tête, avec la vignette du visage,
  qui remontent à chaque nouveau message.
- Les Habbos **tournent la tête** vers celui qui parle.
- Interface Habbo : pièce en plein écran, **fenêtres flottantes déplaçables**,
  barre d'outils en bas, boutons verts, catalogue, inventaire, profil.

## Commandes de chat

`:assis` `:debout` `:danse` `:salut` `:rire` `:dodo` `:boire` `:coucher`
`:sourire` `:triste` `:colere` `:surpris` `:clin` `:crier <texte>` `:pousser` `:aide`

## Souris et clavier

- **Clic** : marcher, ou s'asseoir sur un siège
- **Clic droit** : tourner un meuble
- **Maj + clic** : ramasser un meuble
- **Molette** : zoom · **Flèches** : tourner sur place · **Entrée** : parler · **Échap** : fermer

## Hors ligne

Si l'API d'avatars est injoignable, le client bascule automatiquement sur un
moteur de rendu interne : la salle, les meubles et les déplacements continuent
de fonctionner, avec des Habbos simplifiés dessinés à partir des couleurs de
leur figure.

La progression (tenue, crédits, meubles, mascotte, décoration) est enregistrée
dans le `localStorage` du navigateur.
