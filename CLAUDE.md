# La Constellation — Atlas des Âmes Libres

Site personnel pour cartographier les figures admirées, dresser des liens entre elles et gagner en vision globale.

- **Publié sur** : https://sriracha-star.github.io/laconstellation/
- **Repo** : https://github.com/sriracha-star/laconstellation (branche `main`, GitHub Pages auto-déploie)
- **Dossier de travail** : `/Users/rachariache/Desktop/La Constellation/`

## Architecture

Tout le site tient dans **un seul fichier** : `index.html` (~1100 lignes, ~570 ko).

- HTML + CSS + JS inline — aucun build, aucune dépendance npm
- Polices Google Fonts chargées via `<link>` (Playfair Display, Cormorant Garamond, Josefin Sans)
- **Portraits encodés en base64** directement dans le HTML (21 `data:image/...`) — le site est totalement autonome
- Pas de framework — vanilla JS

Les fichiers `.jpg`/`.png`/`.webp`/`.avif` du dossier sont la **matière brute** (sources des portraits avant encodage). Ignorés par git via `.gitignore`. Quand on ajoute une figure, on encode son portrait en base64 et on l'inline dans index.html.

## Données

Structure principale au début du JS (~ligne 478) :

```js
const FIGURES = [
  { key, cat, era, eraColor, eraLabel, name, dates, nation, tagline, desc,
    quotes: [{text, source, link}],
    works: [{title, type, done}],
    connections: ["autreKey", ...],
    connectionDesc: {autreKey: "pourquoi ils sont reliés"},
    recos: [{title, type}],
    yt_link, videos, threads, mapPos: {x, y} },
  ...
]

const MAP_CONNS = [
  {from: "key1", to: "key2", desc: "...", theme: "desire|aesthetic|flame"},
  ...
]
```

**Catégories (`cat`)** : poète, écrivain, peintre, musicien, danse, cinéaste
**Threads (rubans thématiques)** : femmes, flamme, transgression, magie, paris, corps
**Thèmes de connexion (`theme` dans MAP_CONNS)** : desire, aesthetic, flame

### Figures déjà présentes (20)

Sappho · Natalie Barney · Colette · Renée Vivien · Oscar Wilde · Arthur Rimbaud · Charles Baudelaire · William Blake · Leonora Carrington · Remedios Varo · Patti Smith · David Bowie · Frida Kahlo · Nijinski · Robert Mapplethorpe · Virginia Woolf · Derek Jarman · Sylvia Plath · Hilma af Klint · Pier Paolo Pasolini

## Esthétique

Palette (variables CSS en haut du `<style>`) :
- Ivoire/papier : `--ivory #f5f0e8`, `--ivory2 #ece4d0`, `--paper #e8dfc8`
- Encre : `--ink #1a1208`, `--ink2 #2e2416`, `--ink3 #3d3020`
- Or : `--gold #c4922a`, `--gold2 #d4ab4e`, `--gold3 #e8cc80`
- Accents : `--carmine #8b1a2a`, `--plum #4a2060`, `--teal #1a4a5a`
- Brumes : `--mist #5a4a38`, `--fog #8a7a68`, `--ghost #b0a090`

Typos :
- **Playfair Display** : titres (display 900, italique 400)
- **Cormorant Garamond** : corps de texte (serif italique)
- **Josefin Sans** : labels, navigation, métadonnées (sans-serif fin, letter-spacing large, UPPERCASE)

Texture : bruit SVG fractalNoise en overlay `mix-blend-mode: multiply` à 2.5% opacité (fixed, z-index 9999).

## Composants

- **Hero** : titre central, sunburst en rotation lente, palette sombre
- **Pinboard** : panneau fixe top-right pour épingler des citations
- **Barre de recherche** : filtre les figures par nom/contenu
- **Navbar sticky** : navigation par catégorie
- **Filtres** : par catégorie + thread
- **Cartes flip** : recto portrait + dates, verso citation + œuvres + connexions
- **Carte des connexions** : SVG avec `mapPos` de chaque figure et lignes `MAP_CONNS`

## Workflow

```sh
cd "/Users/rachariache/Desktop/La Constellation"
# éditer index.html
git add index.html
git commit -m "Ajout de [figure] / mise à jour de [section]"
git push     # GitHub Pages redéploie automatiquement
```

Pour prévisualiser localement : ouvrir `index.html` dans un navigateur, ou lancer un serveur statique (`python3 -m http.server 8000`).

## Conventions

- Français partout (textes, commits, commentaires)
- Citations : toujours avec `source` précise (œuvre, éditeur, année, page si possible) et `link` quand un lien stable existe (Wikisource, Gutenberg, Wikipédia)
- Connexions : bidirectionnelles dans `connections`, avec une `connectionDesc` côté figure source qui explique le lien personnel/historique
- `mapPos` : coordonnées dans un SVG ~1200×600 — placer les figures par époque et affinité visuelle
