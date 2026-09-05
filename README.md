# Habbo Hotel v26

Client Habbo isométrique **en un seul fichier HTML** : `habbo-v26.html`.
Aucune dépendance, aucun build — il suffit d'ouvrir le fichier dans un navigateur.

## Ce que contient le fichier

| Élément | Origine |
|---|---|
| Avatars | API d'imagerie **officielle Habbo** (`habbo-imaging/avatarimage`) |
| Garde-robe | `figuredata` officiel : 13 types, 1068 jeux de vêtements |
| Meubles | **Catalogue officiel Habbo** (`furnidata` + icônes `images.habbo.com`) |
| Salle et murs | Dessinés à la volée en isométrique (canvas) |
| Mascottes | Sprites du pack fourni (atlas AVIF intégré au HTML) |

## Les vrais meubles Habbo

Au démarrage, le client va chercher le **furnidata officiel** de Habbo. Il en
tire la liste réelle des meubles : nom, classe, révision, dimensions au sol,
catégorie, et les indicateurs « on peut s'y asseoir / s'y allonger ». Les
illustrations viennent ensuite des icônes officielles servies par le CDN de
Habbo.

Les adresses des CDN de Habbo ont changé plusieurs fois au fil des années.
Plutôt que d'en figer une, le client **essaie plusieurs motifs d'URL** et
retient celui qui répond ; il tente chacun d'abord avec CORS (nécessaire à
l'appareil photo), puis sans. Le catalogue indique en clair ce qui a été
chargé.

Trois cas, tous gérés :

1. **Tout répond** — le catalogue affiche les milliers de meubles officiels,
   avec recherche et catégories réelles.
2. **La liste passe, pas les images** — le client le signale et propose de
   basculer sur les meubles dessinés.
3. **Rien ne passe** (réseau coupé, CORS refusé) — bascule automatique sur les
   meubles dessinés, sans rien casser.

L'onglet **« Meubles dessinés »** reste disponible à tout moment : ces meubles
sont générés en volumes isométriques par le client, s'orientent sur 4 côtés et
se teintent. Les meubles officiels, eux, n'ont qu'une seule vue (l'icône) et ne
tournent donc pas.

## Fidélité au client Habbo

- Cases de **64 × 32 px**, dalle épaisse, murs avec épaisseur visible et embrasure de porte.
- Les **8 orientations** Habbo, avec la convention des émulateurs
  (2 = +x, 4 = +y, 6 = −x, 0 = −y).
- Déplacement au clic avec **cheminement A\*** qui contourne les meubles
  et ne coupe pas les angles.
- On **s'assoit** sur une chaise et on **s'allonge** sur un lit en cliquant dessus.
- Meubles **orientables sur 4 côtés** (meubles dessinés) : les volumes tournent
  réellement ; les meubles officiels utilisent leurs vraies dimensions au sol.
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
