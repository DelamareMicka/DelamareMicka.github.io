# Migration al-folio — ce qu'il reste à faire

Ce fichier liste ce qui a été mis en place automatiquement et, surtout, ce qu'il
reste à compléter manuellement. Cherchez `[À COMPLÉTER]` dans le dépôt pour
retrouver tous les emplacements exacts (`grep -rn "À COMPLÉTER" .`).

## 1. À faire en priorité

- **Photo de profil** — `assets/img/prof_pic.jpg` est encore l'image
  générique du thème. La remplacer par une vraie photo (même nom de fichier,
  ou mettre à jour `profile.image` dans `_pages/about.md`).
- **E-mail et liens** — `_data/socials.yml` : `email`, `orcid_id`,
  `scholar_userid`, `hal_id`, `linkedin_username` sont commentés/absents.
  Décommenter et renseigner chaque ligne connue.
- **Biographie** — `_pages/about.md` a un paragraphe factuel (poste,
  labo, thématiques) mais le texte de biographie détaillée et le bureau/
  adresse sont des placeholders `[À COMPLÉTER]`.
- **Contenu MkDocs Material à migrer** — l'ancien site (MkDocs) mentionné
  n'a pas été fourni dans ce dépôt, donc rien n'a été porté automatiquement.
  Il faudra retrouver ce contenu et migrer manuellement les pages/articles
  pertinents vers les pages `_pages/*.md` ou de nouveaux billets `_posts/`.
- **Nom de domaine personnalisé** — le site est configuré pour
  `https://delamaremicka.github.io` (`url`/`baseurl` dans `_config.yml`).
  Pour un domaine personnalisé : ajouter un fichier `CNAME` à la racine
  (contenant le domaine), configurer les DNS chez le registrar, puis activer
  "Enforce HTTPS" dans Settings → Pages une fois le DNS propagé.

## 2. Publications (`_bibliography/papers.bib`)

Les 3 entrées actuelles (`delamareXXXXjournal`, `delamareXXXXconference`,
`delamareXXXXpreprint`) sont des **gabarits d'exemple**, pas de vraies
publications — titre, auteur·rice·s, revue/conférence, DOI et ID arXiv sont
des placeholders. La convention pour en ajouter (type BibTeX, champs qui
activent les badges DOI/PDF/arXiv/Abstract/Selected) est expliquée dans le
commentaire en tête du fichier. Une fois les vraies publications ajoutées,
supprimer les 3 entrées d'exemple.

Pour afficher le nombre de citations Google Scholar (`_data/citations.yml`,
actuellement vide), renseigner `scholar_userid` dans `_data/socials.yml`,
puis lancer `bin/update_scholar_citations.py` (nécessite le package Python
`scholarly`, déjà dans `requirements.txt`) — ce script n'est plus appelé par
un workflow GitHub Actions automatique (celui d'origine a été retiré avec
le reste de la CI de maintenance du thème), à relancer manuellement ou à
ré-automatiser si besoin.

## 3. Projets (`_projects/`)

- `caire.md` (ANR-23-CMAS-0031) et `paloma-dana.md` : compléter tous les
  `[À COMPLÉTER]` (objectifs, partenaires, financement, durée, résultats).
- `template-project.md` : gabarit à dupliquer pour chaque nouveau projet
  (`published: false` l'exclut du site tant qu'il n'est pas complété — retirer
  cette ligne une fois la fiche prête).

## 4. Encadrement doctoral (`_pages/supervision.md`)

Page créée avec deux sections vides (doctorants, étudiants de master) et un
exemple de structure en commentaire à dupliquer.

## 5. CV (`_pages/cv.md`, `assets/json/resume.json`)

Le format `jsonresume` a été choisi plutôt que `rendercv` (plus simple à
maintenir à la main, pas de pipeline Python séparé à exécuter). Les sections
`work` et `education` ont été remplies avec les informations réellement
publiées sur l'ancien site (parcours ESIGELEC/Rouen, CESI, Normandy Web
School, SIAtech, Renault Trucks/Volvo). Sections encore vides à compléter
si pertinent : `awards`, `certificates`, `skills`, `languages`, `references`,
`projects`. Le bouton de téléchargement PDF (`cv_pdf` dans `_pages/cv.md` et
`_data/socials.yml`) pointe vers le PDF d'exemple du thème
(`assets/pdf/example_pdf.pdf`) — à remplacer par un vrai PDF de CV.

## 6. Blog (`_posts/`)

Tous les billets de démonstration du thème ont été supprimés. Un seul billet
d'exemple reste : `_posts/2026-07-23-exemple-maths-citation.md`, qui montre
la syntaxe des maths et d'une citation bibliographique — à réécrire ou
dupliquer comme point de départ pour de vrais billets, puis à supprimer.

**Note sur les maths** : la demande initiale mentionnait KaTeX, mais
al-folio v1.x utilise **MathJax** pour le rendu mathématique
(`enable_math: true` dans `_config.yml`). La syntaxe d'entrée (LaTeX,
`$$...$$`) est la même dans les deux cas ; seul le moteur de rendu diffère.
Passer à KaTeX demanderait de remplacer ce moteur dans le thème, ce qui n'a
pas été fait ici.

## 7. Langue / bilinguisme

Le site est configuré en français par défaut (`lang: fr`). al-folio v1.x n'a
pas de support i18n natif robuste (pas de bascule de langue intégrée). Pour
un site bilingue, l'approche la plus simple sans plugin supplémentaire est :
dupliquer chaque page importante avec un suffixe (ex. `_pages/about.md` /
`_pages/about-en.md`), et ajouter un petit lien manuel "EN / FR" en haut de
chaque page. Cette convention n'a pas été appliquée à toutes les pages pour
éviter de dupliquer des placeholders — à mettre en place page par page une
fois le contenu français stabilisé.

## 8. Couleur d'accent et favicon

- Couleur d'accent : `_sass/_themes.scss` (variables `$accent-color` /
  `$accent-color-dark` en tête de fichier) — actuellement un bleu
  générique, pas une couleur de marque CESI/HESAM confirmée. Ce fichier est
  une copie locale de celui du thème avec seulement ces deux couleurs
  modifiées ; le reste doit rester synchronisé avec le thème.
- Favicon : `icon: 🎓` dans `_config.yml` (emoji placeholder). Remplacer par
  une image (ex. `assets/img/favicon.png`) si un logo existe.

## 9. Déploiement GitHub Pages

Le workflow `.github/workflows/deploy.yml` build le site avec Jekyll et
déploie le dossier `_site` sur la branche `gh-pages` via
`JamesIves/github-pages-deploy-action`. Après le premier push sur `main` (ou
déclenchement manuel via l'onglet Actions → "Deploy site" →
"Run workflow") :

1. Aller dans **Settings → Pages** du dépôt GitHub.
2. **Source** : "Deploy from a branch".
3. **Branch** : `gh-pages`, dossier `/ (root)`.
4. Enregistrer. Le site sera servi sur `https://delamaremicka.github.io/`
   (propagation possible de quelques minutes après le premier déploiement).

**Ce dépôt local n'a pas encore été poussé (`git push`) vers GitHub.**
L'historique git a été entièrement réinitialisé (voir commit
"Scaffold site from al-folio theme"), donc le premier `git push` vers
`origin main` nécessitera un `--force` (l'historique distant de l'ancien
site TemplateMo est incompatible). C'est une action destructrice sur
l'historique distant : à faire explicitement quand vous êtes prêt, pas
automatiquement.

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
