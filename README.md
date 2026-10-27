# Clio

**[▶ Ouvrir l'application](https://yoann-cramatte.github.io/Clio/)**

L'atelier de l'écrivain : un traitement de texte pour romanciers, fondé sur
l'écriture par fragments. Vous écrivez des morceaux — une scène, une réplique,
une idée — puis vous les assemblez et les réordonnez. Le livre se reconstitue
de lui-même à la lecture et à l'export. Rien à inscrire : le lien ouvre
l'atelier et vous écrivez dans la seconde. Sur Android, Clio existe aussi sur
Google Play.

*Clio, muse de l'histoire, sœur de Calliope.*

EuropaSoft · Yoann CRAMATTE · © 2025-2026, tous droits réservés

## Vos textes restent chez vous

- **Aucun compte**, aucune inscription.
- **Aucun serveur** ne reçoit vos textes : ils ne quittent jamais votre appareil.
- **Aucun pistage**, aucune publicité, aucun cookie tiers.
- Le seul échange réseau est le chargement de l'application elle-même depuis son
  site. Ensuite, tout se passe sur votre machine, même sans connexion.

Vos livres sont enregistrés dans le navigateur, et dans les fichiers `.clio`
que vous sauvegardez vous-même. Le fichier `.clio` est la seule copie qui
survive à l'effacement des données du navigateur : enregistrez-le
régulièrement.

## Autonome et hors ligne

- **Zéro service extérieur** : polices et bibliothèque ZIP sont livrées avec
  l'application.
- **Hors ligne** : une fois ouverte ou installée, Clio démarre sans réseau.
- **Format ouvert** : le `.clio` est une archive ZIP documentée
  ([`FORMAT-CLIO.md`](FORMAT-CLIO.md)) ; vos livres ne sont prisonniers
  d'aucun logiciel.

## L'installer comme une application

Ouverte depuis le lien en ligne, Clio s'installe comme un logiciel : fenêtre
propre, icône sur le bureau ou l'écran d'accueil, fonctionnement hors ligne
ensuite.

| Appareil | Comment faire |
|---|---|
| Chrome, Edge (ordinateur) | Icône d'installation dans la barre d'adresse |
| Android | Google Play, ou menu du navigateur → *Ajouter à l'écran d'accueil* |
| iPhone, iPad | Safari → bouton *Partager* → *Sur l'écran d'accueil* |
| Mac (Safari, macOS 14 et plus) | Menu *Fichier* → *Ajouter au Dock* |

Une fois installée, l'application se met à jour d'elle-même au lancement qui
suit la publication d'une nouvelle version.

L'installation exige une adresse `https://` : c'est une règle des navigateurs,
pas un choix de Clio.

## Ce qu'on peut faire

```
LIVRE ─┬─ CHAPITRE ─┬─ SCÈNE ─┬─ BLOC ─┬─ titre (optionnel)
       │            │         │        ├─ texte
       │            │         │        ├─ note en marge
       │            │         │        ├─ couleur d'accent
       │            │         │        └─ images
       │            ├─ notes du chapitre
       │            └─ type de section (chapitre, préface, index…)
       ├─ personnages
       ├─ lieux
       └─ métadonnées (titre, auteur, genre, objectif, format)
```

- **Écrire par blocs** : le bloc est l'unité de pensée. Il se glisse dans sa
  scène, vers une autre scène, vers un autre chapitre — à la souris comme au
  doigt. Pendant le glissement, des fentes « Déposer ici » montrent où il
  s'insérera, sans rien écraser.
- **Notes en marge**, couleur d'accent et images pour chaque bloc ; une note
  peut devenir un bloc, et l'inverse.
- **Personnages et lieux** : fiches dédiées, et mention dans le texte par `@`
  (personnage) ou `#` (lieu).
- **Sections de livre** : chapitre, préface, avant-propos, prologue, épilogue,
  dédicace, exergue, remerciements, glossaire, index, bibliographie,
  mentions légales, couvertures, section libre…
- **Bibliothèque** rangée en trois rayons : Brouillon, En cours, Terminé.
- **Mode focus** : barres latérales masquées, seul le texte reste à l'écran.
- **Mode lecture** : le livre se relit d'une traite, en thème clair, sépia ou
  sombre, avec la taille de police réglable.
- **Écriture suivie** : objectif quotidien de mots, jours consécutifs,
  historique des sept derniers jours et records.
- **Recherche** dans tout le livre (Ctrl+F).
- **Export** du livre entier ou du chapitre en cours : TXT, Markdown,
  HTML / Word, EPUB, PDF — avec trois profils (Manuscrit, Brouillon, Lecture
  bêta) et le choix des titres, séparateurs et notes à inclure.
- **Sécurité** : corbeille pour les livres, chapitres, scènes, blocs,
  personnages et lieux supprimés ; instantanés automatiques à l'ouverture ou
  toutes les heures.
- **Apparence** : cinq thèmes (Parchemin, Parchemin sombre, Caramel, Sauge,
  Vibrant), choix de la police et de sa taille. Les réglages s'exportent.

## Pour les assistants IA

- **Une seule source** : `clio-tauri/src/index.html` contient toute
  l'application. Toute correction se fait là, jamais dans une copie propre à
  une plateforme.
- **Le format `.clio`** est normé par [`FORMAT-CLIO.md`](FORMAT-CLIO.md), qui
  suffit à lire ou produire un fichier sans le code de Clio.
- **La version** n'existe qu'à un endroit : `clio-tauri/src/AppInfo.json`
  (avec le journal des versions).
- **Avant toute modification** de l'empaquetage : lire
  `clio-tauri/docs/PLATEFORMES.md`. Avant tout envoi : `npm test` dans
  `clio-tauri/`.

> **Licence.** Ce dépôt est public pour consultation uniquement. Le code n'est
> pas libre : toute copie, modification, redistribution ou réutilisation est
> interdite sans autorisation écrite. Voir [`LICENSE`](LICENSE) ; composants
> tiers dans [`NOTICES`](NOTICES).

---

## Pour les développeurs

Sur l'appareil, les livres et la corbeille sont enregistrés dans la base
IndexedDB `clio-livres` du navigateur ou du WebView (depuis 1.0.2 ; avant,
dans `localStorage`, dont le quota saturait vite). Les réglages restent dans
`localStorage`.

### Une seule source, plusieurs cibles

`clio-tauri/src/index.html` est **la base commune**. Ce fichier unique contient
toute l'application — structure, style et code. Il sert à la fois la version
web et toutes les applications empaquetées.

| Cible | Comment |
|---|---|
| Web / PWA | `clio-tauri/build-web.sh` produit un dossier statique à déposer |
| Android | `clio-tauri/build-android.sh` produit un APK |
| iOS, macOS, Windows, Linux | Tauri, depuis la même source |

Le code propre à l'empaquetage passe par le drapeau `SOUS_TAURI`. Sur le web ce
drapeau vaut `false` et le comportement d'origine s'applique. **Toute correction
se fait dans la base commune**, jamais dans une copie propre à une plateforme :
une correction faite ailleurs ne profiterait qu'à une seule cible.

#### Ce que le HTML embarque

L'application ne dépend d'aucun service extérieur. JSZip et les trois familles
de polices sont livrés avec elle, dans `vendor/` et `fonts/`. Le fichier
`index.html` n'est donc **pas** autonome : il faut déployer le dossier entier.

---

### Organisation du dépôt

```
clio-tauri/
├── src/                    LA SOURCE
│   ├── index.html          l'application complète
│   ├── fonts.css, fonts/   Playfair Display, Crimson Pro, IM Fell English
│   ├── vendor/             JSZip
│   ├── AppInfo.json        identité de l'application et journal des versions,
│   │                       seule source de la version (lue par la page)
│   └── service-worker.js   démarrage hors ligne de la version web
├── src-tauri/              empaquetage Tauri (Rust, manifeste Android)
├── docs/PLATEFORMES.md     contraintes par plateforme, pièges de compilation
├── tests/                  tests automatisés (format .clio, stockage des livres)
├── sync-version.sh         aligne package.json et Cargo.toml sur AppInfo.json
├── build-web.sh            produit le dossier web
└── build-android.sh        produit l'APK

FORMAT-CLIO.md              norme du format .clio, autonome
Clio 0.0.1/                 version HTML d'origine, avant le portage
```

Les sorties de compilation ne sont pas versionnées : `web/`, `target/`,
`node_modules/` et les APK se régénèrent par les scripts.

---

### Déployer la version web

```bash
cd clio-tauri
./build-web.sh
```

Le dossier `web/Clio/` (environ 1,4 Mo) se dépose tel quel sur n'importe quel
hébergement statique. L'application répond alors sur `<votre-site>/Clio/`.

Le manifeste (`manifest.webmanifest`) n'emploie que des chemins relatifs :
posé dans `/Clio/`, il y déclare son périmètre. Rien à configurer.

**GitHub Pages.** Chaque mise à jour de `main` déclenche
`.github/workflows/pwa.yml` : tests, puis `build-web.sh`, puis mise en ligne
sur https://yoann-cramatte.github.io/Clio/. Un test en échec bloque la publication.

**Une adresse `https://` est nécessaire** à l'installation en application
(PWA). En `http://` simple, Clio fonctionne mais ne s'installe pas — c'est une
règle des navigateurs, pas un choix de Clio.

Une fois installée, l'application démarre **sans réseau** : le service worker
garde ses fichiers en cache. Pour publier une mise à jour, relancer
`build-web.sh` et redéposer le dossier ; les utilisateurs la reçoivent au
lancement suivant, l'ancien cache étant écarté automatiquement.

---

### Compiler l'application Android

```bash
cd clio-tauri
./build-android.sh            # APK de débogage, arm64
./build-android.sh --release  # APK de distribution, signature requise
```

Le script exporte lui-même les chemins du JDK, du SDK et du NDK, absents du
`PATH` par défaut. Les pièges rencontrés — et les raisons de chaque réglage —
sont consignés dans `docs/PLATEFORMES.md`. À lire avant toute modification de
la configuration d'empaquetage.

---

### Lancer les tests

```bash
cd clio-tauri
npm install   # une fois : installe aussi fake-indexeddb, seule dépendance des tests
npm test
```

Les tests utilisent le lanceur intégré de Node (`node --test`, Node 18 ou
plus). Ils ne recopient aucune logique : `tests/charger-index.mjs` découpe
`src/index.html` entre des repères (commentaires de section, débuts de
déclaration) et évalue ces extraits tels quels dans un bac à sable `node:vm`,
avec le `vendor/jszip.min.js` livré. Si un repère disparaît, le chargement
échoue au lieu de tester autre chose que l'application.

- `format-clio.test.mjs` : aller-retour d'un livre riche (tous types de
  section, scènes, blocs, notes, images, entités, caractères spéciaux) par
  `bookToClioBlob` puis `clioToBook`, conformité de l'archive à
  `FORMAT-CLIO.md`, et archives piégées (balisage dans les champs, chemins
  `../`, images qui n'en sont pas, pollution de prototype).
- `stockage-livres.test.mjs` : migration localStorage → IndexedDB, écritures
  regroupées, repli localStorage, protections contre l'écrasement de la
  bibliothèque. IndexedDB y est simulée par `fake-indexeddb` (pur JavaScript,
  aucune dépendance).

Ces tests valident la logique, pas les WebView : le comportement sur appareil
reste à vérifier à la main.

---

### Le format `.clio`

Un `.clio` contient une œuvre et une seule : le livre, ses chapitres, ses
scènes, ses blocs et ses images, dans une archive ZIP.

```
<archive>.clio
├── manifest.json
├── book.json
├── chapters/ch-001.json …
└── images/
```

[`FORMAT-CLIO.md`](FORMAT-CLIO.md) décrit la norme de façon autonome : elle
suffit à écrire un lecteur ou un producteur de `.clio` sans disposer du code de
Clio.
