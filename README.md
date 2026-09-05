# Habbo Hotel v26

Client Habbo isométrique **en un seul fichier HTML** : `habbo-v26.html`.
Aucune dépendance, aucun build — il suffit d'ouvrir le fichier dans un navigateur.

## Ce que contient le fichier

| Élément | Origine |
|---|---|
| Avatars | API d'imagerie **officielle Habbo** (`habbo-imaging/avatarimage`) |
| Garde-robe | `figuredata` officiel : 13 types, 1068 jeux de vêtements |
| Meubles | **Sprites pixel art** cuits par le client (43 meubles, 4 orientations) |
| Salle et murs | Dessinés à la volée en isométrique (canvas) |
| Mascottes | Sprites du pack fourni (atlas AVIF intégré au HTML) |

## Les meubles

Les meubles sont des **sprites pixel art cuits par le client** : chaque meuble
est rastérisé une fois dans un tampon de pixels, sans anticrénelage, avec un
contour d'1 px, une palette réduite de 5 tons et des plateaux ronds obtenus par
projection isométrique d'un cercle. Ils sont ensuite affichés au plus proche
voisin, et le zoom avance par huitièmes pour que le pixel reste net.

Rien n'est téléchargé : aucun CDN, aucun CORS, aucune image manquante.
43 meubles, 10 teintes chacun, 4 orientations.

### Leurs fonctions

Cliquer un meuble déclenche sa fonction, comme dans Habbo :

| Meuble | Au clic |
|---|---|
| Chaises, canapés, tabourets, trône | on marche jusqu'à lui et on **s'assoit**, face au bon côté |
| Lits | on **s'allonge** |
| Dé Habbo | il **roule** puis affiche sa face (1 à 6) ; second clic, il se referme |
| Téléporteur | on entre, on **disparaît**, on ressort par le téléporteur jumeau |
| Barrière | elle **s'ouvre et se ferme** — fermée, elle bloque vraiment le passage |
| Lampadaire, néon, télé, borne, juke-box, boule disco | **interrupteur** on / off, la lumière suit |
| Distributeur | on s'en approche et on **se sert à boire** |
| Coffre, cadeau | le couvercle **s'ouvre** |
| Trophée | on **lit l'inscription** |
| Piste de danse, cheminée, torche, fontaine | animées en continu |

### Meubles officiels Habbo (optionnel)

Un second onglet du catalogue tente de charger le **furnidata officiel** de
Habbo et ses icônes. Ça dépend entièrement du réseau et des en-têtes CORS du
CDN de Habbo : le client essaie plusieurs motifs d'URL, retient celui qui
répond, et dit franchement ce qu'il a obtenu. En cas d'échec, rien ne casse —
les meubles pixel restent la source par défaut.

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
