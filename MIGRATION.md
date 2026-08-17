# Migration al-folio — ce qu'il reste à faire

Ce fichier liste ce qui a été mis en place automatiquement et, surtout, ce qu'il
reste à compléter manuellement. Cherchez `[À COMPLÉTER]` dans le dépôt pour
retrouver tous les emplacements exacts (`grep -rn "À COMPLÉTER" .`).

**Mise à jour** : le CV réel (`CV_MD__English_.pdf`) a été fourni et intégré —
biographie, poste, parcours (formation + expérience détaillées), 5 vraies
publications et la fiche CAIRE sont maintenant du vrai contenu, plus des
placeholders. Voir le détail par section ci-dessous.

## Confidentialité : PDF du CV

Le PDF fourni contenait votre **adresse personnelle** et votre **numéro de
mobile personnel** sur la page de garde. `assets/pdf/CV_Delamare_Mickael.pdf`
est une version où cette page de garde a été régénérée (script Python,
pypdf + reportlab) en ne gardant que nom/titre/labo/e-mails ; les pages 2 à 9
(contenu du CV) sont copiées telles quelles depuis l'original, non modifiées.
Vérifié visuellement (rendu PNG des pages 1 et 3) avant intégration.

## 1. À faire en priorité

- **Liens restants** — `_data/socials.yml` : `email` et `github_username`
  sont renseignés (réels). `orcid_id`, `scholar_userid`, `hal_id`,
  `linkedin_username` restent commentés — à décommenter et renseigner si
  ces profils existent.
- **Bureau** — `_pages/about.md` : le numéro de bureau CESI est encore
  `[À COMPLÉTER]` (l'adresse institutionnelle, elle, est renseignée).
- **Contenu MkDocs Material à migrer** — l'ancien site (MkDocs) mentionné
  en tout début de conversation n'a pas été fourni dans ce dépôt ; seul le
  CV PDF a été intégré. S'il reste du contenu MkDocs (articles, pages) non
  couvert par le CV, il faudra le retrouver et le migrer manuellement.
- **Nom de domaine personnalisé** — le site est configuré pour
  `https://delamaremicka.github.io` (`url`/`baseurl` dans `_config.yml`).
  Pour un domaine personnalisé : ajouter un fichier `CNAME` à la racine
  (contenant le domaine), configurer les DNS chez le registrar, puis activer
  "Enforce HTTPS" dans Settings → Pages une fois le DNS propagé.

## 2. Publications (`_bibliography/papers.bib`)

Contient maintenant les 5 vraies publications du CV :

- 2 articles publiés (*Sci*, MDPI 2020 ; *Sensors* 2020), avec DOI réels.
- 3 articles soumis/en cours d'évaluation (type BibTeX `unpublished`, sans
  DOI/volume/pages puisque non encore publiés ; leur statut de soumission
  est dans le champ `note`) : le papier fondateur BAI (IJAIED), la métrique
  APed (IJAIED), et le dispositif « Grand-Mère Ylla » (Éducation permanente).

Quand un article `unpublished` est accepté : passer son type en `article`,
ajouter `journal`/`volume`/`pages`/`doi`, retirer `note`, en gardant la même
clé de citation. La convention complète pour ajouter de nouvelles entrées est
expliquée dans le commentaire en tête du fichier.

Pour afficher le nombre de citations Google Scholar (`_data/citations.yml`,
actuellement vide), renseigner `scholar_userid` dans `_data/socials.yml`,
puis lancer `bin/update_scholar_citations.py` (nécessite le package Python
`scholarly`, déjà dans `requirements.txt`) — ce script n'est plus appelé par
un workflow GitHub Actions automatique (celui d'origine a été retiré avec
le reste de la CI de maintenance du thème), à relancer manuellement ou à
ré-automatiser si besoin.

## 3. Projets (`_projects/`)

- `caire.md` (ANR-23-CMAS-0031) : rôle et statut lauréat renseignés depuis le
  CV. Reste `[À COMPLÉTER]` : objectifs détaillés, partenaires, montant du
  financement, durée, résultats/livrables.
- `alignement-agents-llm.md` (ex-« PALOMA/DANA » — nom de code retiré du
  site : le papier correspondant est en review en double-aveugle, "Anonymous
  Author(s)" / "Affiliation withheld" ; publier son titre ou son contenu
  détaillé sous votre nom risquerait de casser l'anonymat côté relecteurs).
  Fiche volontairement succincte tant que la décision n'est pas connue —
  à détailler une fois la review terminée.
- `template-project.md` : gabarit à dupliquer pour chaque nouveau projet
  (`published: false` l'exclut du site tant qu'il n'est pas complété — retirer
  cette ligne une fois la fiche prête).

## 4. Encadrement doctoral (`_pages/supervision.md`)

Les sections Doctorants/Étudiants de master restent vides avec un exemple de
structure en commentaire à dupliquer — à remplir une fois des encadrements
effectifs. (La mention de l'HDR en préparation a été retirée à la demande de
Mickaël — préférence à ne pas afficher un diplôme encore en cours.)

## 5. CV (`_pages/cv.md`, `assets/json/resume.json`)

Format `jsonresume` (plus simple à maintenir à la main que `rendercv`, pas de
pipeline Python séparé à exécuter). Les sections `work` et `education` ont été
remplies avec les données précises du CV fourni (dates exactes, directeurs de
thèse, jury, encadrants de stage, etc.). Le bouton de téléchargement PDF
pointe maintenant vers le vrai CV (`assets/pdf/CV_Delamare_Mickael.pdf` —
voir l'avertissement en haut de ce fichier sur son contenu).

Sections encore vides à compléter si pertinent : `awards`, `certificates`,
`skills`, `languages`, `references`, `projects` (le champ `publications` du
JSON reste volontairement vide : la bibliographie de référence est
`_bibliography/papers.bib`, pas ce fichier, pour éviter la duplication).

Note : l'ancienne entrée d'enseignement "Normandy Web School" (présente sur
l'ancien site TemplateMo) n'apparaît pas dans le CV fourni et a été retirée
de `_teachings/` en conséquence — à réintégrer si ce poste est toujours
d'actualité et volontairement omis du CV.

## 6. Blog (`_posts/`)

Le billet d'exemple générique a été retiré maintenant qu'un premier vrai
billet existe : `_posts/2026-07-27-introduction-pomdp-bebe-qui-pleure.md`,
des notes en français sur les POMDP (à partir d'une vidéo Julia Academy),
avec maths et un exemple de code Julia.

**Note sur les maths** : la demande initiale mentionnait KaTeX, mais
al-folio v1.x utilise **MathJax** pour le rendu mathématique
(`enable_math: true` dans `_config.yml`). La syntaxe d'entrée (LaTeX,
`$$...$$`) est la même dans les deux cas ; seul le moteur de rendu diffère.
Passer à KaTeX demanderait de remplacer ce moteur dans le thème, ce qui n'a
pas été fait ici.

## 7. Langue / bilinguisme

Le site dispose maintenant d'une bascule FR/EN entièrement côté client
(bouton dans la barre de navigation, à côté du bouton clair/sombre). Aucun
plugin i18n : le contenu des deux langues est présent simultanément dans le
HTML généré, et seul l'affichage est basculé.

**Mécanisme** (implémenté dans `_layouts/default.liquid`,
`_layouts/page.liquid`, `_layouts/post.liquid`, `_includes/header.liquid` et
`_includes/metadata.liquid`, tous des copies locales qui surchargent le
thème al-folio) :
- Classes CSS `.lang-fr` / `.lang-en` (blocs) et `.lang-fr-i` / `.lang-en-i`
  (inline), affichées/masquées selon l'attribut `data-lang="en"` sur
  `<html>`.
- Préférence persistée dans `localStorage` (`lang-pref`), appliquée par un
  `<script>` placé tout en haut de `<head>` pour éviter un flash de la
  mauvaise langue au chargement.
- Convention de contenu : blocs de plusieurs paragraphes →
  `<div class="lang-fr" markdown="1">...</div>` suivi d'un
  `<div class="lang-en" markdown="1">...</div>` ; texte court dans un seul
  élément (titre de nav, cellule de tableau, front matter YAML) →
  `<span class="lang-fr-i">...</span><span class="lang-en-i">...</span>`
  côte à côte.
- Front matter des pages : `title` (FR) + `title_en` (EN, optionnel, repris
  par la nav et par les fiches de projet/enseignement avec fallback sur
  `title` si absent).

**Couverture** : l'intégralité du contenu rédactionnel est bilingue —
`about.md`, `cv.md` (voir remarque ci-dessous), `publications.md` (+ notes
`_bibliography/papers.bib`), `projects.md` et les 3 fiches projet,
`teaching.md` et les 3 fiches d'enseignement, `supervision.md`, les 2
billets de blog, la carte de cursus (`curriculum-ia.md` +
`_data/curriculum_ia.yml`, 40 UE / 97 ECUE), `404.md`, et les chaînes
globales de `_config.yml` (`description`, `footer_text`, `blog_name`,
`blog_description`, `keywords`).

**Bug corrigé en cours de route** : le gem original (`metadata.liquid`)
injecte `page.title`/`page.description` bruts dans `<title>…</title>` (texte
seul — les balises `<span>` s'affichaient littéralement) et dans des
attributs `content="…"` (les guillemets doubles de `class="lang-fr-i"`
fermaient l'attribut en avance, ce qui faisait fuiter le reste de la balise
comme texte visible en haut de `<body>`, juste après `</head>`). Idem pour
les titres `<h1>` des pages en `layout: page` / `layout: post`
(`page.liquid`/`post.liquid` du gem, qui n'affichaient que `page.title` sans
jamais lire `page.title_en`). Les trois fichiers ont été surchargés
localement : `metadata.liquid` passe `page.title`/`page.description` par
`strip_html` (avec un espace inséré avant chaque `<span>` pour ne pas
recoller les deux langues) avant de les mettre dans `<title>`/`content="…"`/
JSON-LD ; `page.liquid`/`post.liquid` affichent désormais
`<span class="lang-fr-i">{{ page.title }}</span><span class="lang-en-i">{{ page.title_en | default: page.title }}</span>`
au lieu de `{{ page.title }}` seul. Vérifié par test Playwright automatisé
sur les 16 pages du site (bascule + persistance + absence de texte
"fuité") avant commit.

**Limites connues, volontairement non traitées** (chrome du thème
al-folio, dans les gems, non surchargé) :
- L'attribut `lang` de `<html>` n'est pas mis à jour par la bascule (reste
  `fr`, valeur de `site.lang`) — impact mineur sur l'accessibilité/lecteurs
  d'écran quand la page est affichée en anglais.
- D'éventuels textes d'interface générés par le thème lui-même (ex. état
  vide "no news", libellés de pagination) restent en anglais par défaut
  (langue du thème) et n'ont pas été cherchés/surchargés individuellement.

**Page CV** : `cv.md` est passée de `layout: cv` / `cv_format: jsonresume`
(rendu server-side par le plugin `al_folio_cv` depuis
`assets/json/resume.json`, incompatible avec la bascule côté client) à
`layout: page` avec le contenu écrit directement en Markdown bilingue.
`assets/json/resume.json` n'est donc plus utilisé pour l'affichage de la
page (peut être retiré si non utilisé ailleurs).

## 8. Couleur d'accent et favicon

- Couleur d'accent : `_sass/_themes.scss` (variables `$accent-color` /
  `$accent-color-dark` en tête de fichier) — actuellement un bleu
  générique, pas une couleur de marque CESI confirmée. Ce fichier est
  une copie locale de celui du thème avec seulement ces deux couleurs
  modifiées ; le reste doit rester synchronisé avec le thème.
- Favicon : `icon: 🎓` dans `_config.yml` (emoji placeholder). Remplacer par
  une image (ex. `assets/img/favicon.png`) si un logo existe.

## 9. Déploiement GitHub Pages

Le workflow `.github/workflows/deploy.yml` build le site avec Jekyll et
déploie le dossier `_site` sur la branche `gh-pages` via
`JamesIves/github-pages-deploy-action`.

**État actuel : le dépôt a été poussé (`git push --force`) vers
`origin main`** — l'ancien historique TemplateMo a été remplacé. Reste à
faire côté GitHub (pas automatisable sans `gh` CLI depuis cet environnement) :

1. Vérifier que le workflow "Deploy site" s'est bien exécuté dans l'onglet
   **Actions** du dépôt (déclenché automatiquement par le push).
2. Aller dans **Settings → Pages**.
3. **Source** : "Deploy from a branch".
4. **Branch** : `gh-pages`, dossier `/ (root)`.
5. Enregistrer. Le site sera servi sur `https://delamaremicka.github.io/`
   (propagation possible de quelques minutes après la première activation).

## 10. Divers

- Les commits ont été créés avec l'identité git auto-détectée
  `DELAMARE Mickael <mdelamare@cesi.fr>`. Si ce n'est pas la bonne identité,
  configurer `git config user.name` / `user.email` (localement ou
  globalement) avant de futurs commits.
- `requirements.txt` conserve `rendercv` et `scholarly` (utilisés
  respectivement par l'ancien pipeline CV et par la mise à jour des
  citations Google Scholar) même si le CV utilise maintenant le format
  `jsonresume` — sans impact sur le site, juste des dépendances Python
  installées mais non utilisées à ce stade.
