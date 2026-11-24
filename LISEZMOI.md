# Surligneur d'entités nommées — extension Chrome

Repère les entités nommées de la page ouverte, les surligne par couleur, ajoute une coche après
chacune et exporte la sélection (ou tout) en CSV. **Analyse 100 % locale** : aucun texte n'est
envoyé sur Internet.

| Couleur | Type |
|---|---|
| Jaune | Personnes |
| Fuchsia | Œuvres (livres, films, tableaux, albums…) |
| Vert | Institutions / organisations |
| Bleu | Lieux |
| Orange | Événements |
| Lavande | Dates |
| Gris | Autres noms propres (désactivé par défaut) |

## Langue de l'interface

L'interface (fenêtre de l'extension, panneau, messages, en-têtes et types du CSV, nom du fichier) suit
la langue du navigateur : **français** si Chrome est en français, **anglais** dans tous les autres cas.
Les textes sont dans `_locales/en/messages.json` et `_locales/fr/messages.json`. Pour ajouter une langue,
copiez le dossier `_locales/en` sous le code de la langue (ex. `_locales/es`) et traduisez les valeurs
`message`. La détection des entités, elle, fonctionne quelle que soit la langue de l'interface.

## Installation (mode développeur)

1. Décompressez le fichier `.zip` dans un dossier permanent.
2. Ouvrez `chrome://extensions` et activez le **Mode développeur** (en haut à droite).
3. Cliquez **Charger l'extension non empaquetée** et choisissez le dossier `ner-surligneur`.
4. Épinglez l'icône de l'extension dans la barre d'outils (icône casse-tête → épingle).

## Utilisation

- **Paramétrer** : dans la fenêtre de l'extension (clic sur l'icône), cochez les types à repérer,
  le séparateur CSV (point-virgule recommandé pour Excel en français) et l'affichage des coches.
- **Analyser** : bouton *Analyser cette page*, ou raccourci **Alt+Maj+E**.
- **Dans la page** : chaque entité surlignée est suivie d'une coche. Cocher une occurrence coche
  toutes les occurrences de la même entité.
- **Panneau flottant** (déplaçable par son en-tête) :
  - **Tout exporter** (en haut à droite) : toutes les entités des types cochés ;
  - **Exporter la sélection** (en bas) : seulement les entités cochées ;
  - cases des types pour afficher/masquer chaque couleur, avec le nombre d'entités ;
  - liste filtrable : clic sur un nom = défiler jusqu'à l'occurrence suivante ; menu déroulant
    pour **corriger le type** ou **ignorer** une entité (mémorisé jusqu'au rechargement de la page) ;
  - **Relancer** : nouvelle analyse (pages dynamiques), les coches sont conservées.

Le CSV (UTF-8 avec BOM) contient : `Entité, Type, Occurrences, Page` (en anglais : `Entity, Type, Occurrences, Page`).

## Comment la détection fonctionne

Moteur heuristique sans IA : mots à majuscule et particules (de, van, von, of, d'…), mots-indices
multilingues (Université, Museum, Festival, rue, lac…), titres (Dr, Mme, président…), dictionnaires de
prénoms et de lieux, initiales, sigles, guillemets « » “ ” „ “ 《 》 『 』, balises `<cite>`, `<i>`, `<em>`,
contexte (« le roman X », « X (1802-1885) »), cohérence à l'échelle de la page (un nom de famille seul
hérite du type « personne », un sigle entre parenthèses hérite du type de l'entité qui le précède),
segmentation du chinois/japonais/coréen et données schema.org de la page.

**Limites** : précision variable (≈ bonne sur des textes rédigés en français, anglais, espagnol,
italien, portugais, russe ; plus faible en allemand, où tous les noms prennent une majuscule, et dans
les écritures sans majuscules comme l'arabe, l'hébreu ou l'hindi, où seuls les dictionnaires et les
guillemets servent). Les titres d'œuvres en minuscules sans italique ni guillemets ni mot-indice sont
souvent manqués. Utilisez le menu de correction du panneau avant l'export.

**Personnaliser** : toutes les listes (prénoms, lieux, mots-indices, institutions connues…) sont dans
`src/data.js`. Après modification, cliquez ⟳ sur la carte de l'extension dans `chrome://extensions`.

## Fichiers

- `manifest.json` — Manifest V3 ; permissions `activeTab`, `scripting`, `storage` uniquement
  (l'extension n'agit que sur l'onglet où vous la déclenchez).
- `_locales/` — traductions (anglais par défaut, français).
- `popup.html/.css/.js` — paramètres ; `background.js` — raccourci clavier et pastille de compteur.
- `src/data.js` — dictionnaires ; `src/ner-engine.js` — moteur ; `src/content.js` / `content.css` —
  surlignage, coches, panneau et export.

Pour les fichiers locaux (`file://`), activez « Autoriser l'accès aux URL de fichier » dans les
détails de l'extension. Chrome interdit l'action des extensions sur les pages `chrome://` et le Web Store.
