# Habbo Hotel v26

Client Habbo isométrique **en un seul fichier HTML** : `habbo-v26.html`.
Aucune dépendance, aucun build — il suffit d'ouvrir le fichier dans un navigateur.

## Ce que contient le fichier

| Élément | Origine |
|---|---|
| Avatars | API d'imagerie Habbo (hôtel détecté automatiquement), secours interne |
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

**bobba.io n'expose aucun service d'images d'avatars** : son client reconstruit
les Habbos côté navigateur à partir des planches `gordon`
(`map.json`, `figuredata.json`, `partsets.json`, `draworder.json`,
`animation.json`, puis un atlas par bibliothèque). C'est pour cela que les
avatars s'y affichent bien.

En attendant ce portage, les Habbos passent par l'**API d'imagerie de Habbo**,
qui existe à l'identique sur chaque hôtel. Le client les essaie dans l'ordre au
démarrage et retient le premier qui répond — si un domaine est bloqué chez toi,
un autre peut passer :

`habbo.com` · `.fr` · `.es` · `.de` · `.it` · `.nl` · `.com.br` · `.com.tr`

Le profil (👤) affiche le **journal des essais** (✅ / ❌ avec la cause et le
délai) et permet d'imposer une adresse. Si aucun ne répond, le moteur de
secours interne dessine les Habbos et le client reste jouable.

## Déplacer les meubles

Comme dans Habbo, **le clic et le glissé ne font pas la même chose** :

| Geste | Effet |
|---|---|
| **Clic** sur un meuble | l'utilise : s'asseoir, s'allonger, allumer, commuter |
| **Glisser** un meuble | le déplace — il suit le curseur, case par case |
| **Clic droit** ou **R** | le tourne, y compris en plein déplacement |
| **Échap** | annule le déplacement en cours |
| **Maj + clic** | le ramasse dans l'inventaire |

Pendant le glissé, les cases visées s'allument en **vert** si la pose est
possible, en **rouge** sinon ; au relâchement le meuble revient à sa place si
c'était rouge. Le meuble est saisi par le point cliqué, donc un canapé de deux
cases ne saute pas sous le curseur.

L'empreinte **pivote avec le meuble** : une table 2×1 tournée d'un quart de
tour occupe bien 1×2. Et l'on peut meubler **par-dessus un tapis** : les
revêtements de sol ne bloquent pas la pose, reconnus non pas à `canstandon`
(les tables le portent aussi) mais à leur hauteur réelle, lue dans
`logic.dimensions.z`.

Tout cela marche aussi au doigt sur mobile.

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
