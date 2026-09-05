# Habbo Hotel v26

Client Habbo isométrique **en un seul fichier HTML** : `habbo-v26.html`.
Aucune dépendance, aucun build — il suffit d'ouvrir le fichier dans un navigateur.

## Ce que contient le fichier

| Élément | Origine |
|---|---|
| Avatars | Service d'images d'avatars, détecté parmi bobba.io et les hôtels Habbo |
| Garde-robe | `figuredata` officiel : 13 types, 1068 jeux de vêtements |
| Meubles | **Vrais meubles Habbo** via `images.bobba.io` (pipeline bobba_client) |
| Salle et murs | Dessinés à la volée en isométrique (canvas) |
| Mascottes | Sprites du pack fourni (atlas AVIF intégré au HTML) |

## Les meubles

**Tous les meubles viennent du catalogue Habbo.** Il n'y a plus aucun meuble
dessiné par le client. Le pipeline est porté de
[bobba_client](https://github.com/Josedn/bobba_client) (Josedn, GPL) et lit les
assets miroir de `images.bobba.io` :

```
<base>furnidata.json      la liste : nom, dimensions, cansiton / canlayon
<base><nom>/furni.json    calques, offsets, atlas, animations, couleurs
<base><nom>/atlas.png     la planche de sprites
```

Un meuble Habbo n'est pas une image : le client découpe les assets dans
l'atlas, empile les calques (ombre portée, puis `a`, `b`, `c`… avec `ink ADD`,
alpha et miroir par calque), applique la teinte de la variante de couleur
(`chair_basic*2`) et choisit l'image d'animation. L'ancre tombe sur le centre
de la case.

On s'assoit et on s'allonge d'après `cansiton` / `canlayon`, les meubles à
plusieurs états se commutent au clic, et les téléporteurs fonctionnent par
paire.

## Les salles publiques

Les salles ne citent aucun nom de meuble en dur. Chaque emplacement décrit une
**intention**, résolue à l'exécution contre le vrai furnidata :

```js
S(['chair_norja','chair_basic','chair'], 6,5, 2, {need:'sit'})
```

Le résolveur note les candidats (nom exact, puis préfixe, puis simple
présence), préfère la variante sans couleur et la bonne taille, et surtout
**filtre par capacité** : un emplacement `need:'sit'` ne peut tomber que sur un
meuble où l'on s'assoit réellement. Si aucun mot-clé ne correspond, il prend
quand même un meuble capable plutôt que de laisser le trou.

Conséquence : les cinq salles se meublent seules avec ce qui existe réellement
dans le catalogue, sans que j'aie à deviner un seul nom de classe.

## Les avatars

Le service d'images d'avatars n'est pas figé. bobba, Habbo et ses hôtels
nationaux exposent tous la même signature (`figure`, `direction`,
`head_direction`, `action`, `gesture`, `size`, `headonly`). Au démarrage le
client **essaie la liste dans l'ordre et retient le premier qui répond** :

1. `images.bobba.io/avatarimage.php`
2. `habbo.com`, puis `habbo.fr`, `habbo.es`, `habbo.com.br`, `habbo.de`

Tu peux aussi imposer une adresse depuis le profil (👤). Si aucun service ne
répond, les Habbos sont dessinés par le moteur de secours interne.

## Fidélité au client Habbo

- Cases de **64 × 32 px**, dalle épaisse, murs avec épaisseur visible et embrasure de porte.
- Les **8 orientations** Habbo, avec la convention des émulateurs
  (2 = +x, 4 = +y, 6 = −x, 0 = −y).
- Déplacement au clic avec **cheminement A\*** qui contourne les meubles
  et ne coupe pas les angles.
- On **s'assoit** sur une chaise et on **s'allonge** sur un lit en cliquant dessus.
- Meubles **orientables** : le clic droit parcourt les orientations du meuble.
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
