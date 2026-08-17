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

Le site est configuré en français par défaut (`lang: fr`). al-folio v1.x n'a
pas de support i18n natif robuste (pas de bascule de langue intégrée). Pour
un site bilingue, l'approche la plus simple sans plugin supplémentaire est :
dupliquer chaque page importante avec un suffixe (ex. `_pages/about.md` /
`_pages/about-en.md`), et ajouter un petit lien manuel "EN / FR" en haut de
chaque page. Cette convention n'a pas été appliquée à toutes les pages — à
mettre en place page par page une fois le contenu français stabilisé (le CV
fourni est en anglais et pourrait servir de base pour une version `-en`).

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
