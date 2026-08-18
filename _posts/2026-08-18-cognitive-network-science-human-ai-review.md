---
layout: post
title: "Cartographier l'esprit en réseaux : la Cognitive Network Science face à l'IA"
title_en: "Mapping the mind as a network: Cognitive Network Science meets AI"
date: 2026-08-18 06:00:00+0200
description: >
  <span class="lang-fr-i">Une revue systématique PRISMA 2020 de 36 études (sur plus de 53 000 candidates) montre que les grands modèles de langage reproduisent, mesurablement, les mêmes biais cognitifs que les humains, comme l'anxiété face aux mathématiques, quand on les représente sous forme de réseaux de concepts.</span><span class="lang-en-i">A PRISMA 2020 systematic review of 36 studies (out of more than 53,000 candidates) shows that large language models measurably reproduce the same cognitive biases as humans, such as maths anxiety, once represented as networks of concepts.</span>
tags: cognitive-network-science ia recherche
categories: recherche
related_posts: false
---

<div class="lang-fr" markdown="1">

Avec Christophe Cruz, Hussam Ghanem, Samir Jabbar, Sarah Theroine, Laurent Gautier, Maria Alice Bertolim et Hocine Cherifi, nous venons de terminer une revue systématique sur la **Cognitive Network Science (CNS)** appliquée aux systèmes homme-IA, soumise à la conférence KES 2026 et destinée à *Procedia Computer Science* (Elsevier). Le code, les figures et les données bibliographiques sont déjà publics sur [GitHub](https://github.com/ChristopheCruz/cns-human-ai-review/).

*Lien vers l'article : [À COMPLÉTER une fois le DOI final attribué par l'éditeur].*

## L'idée de départ : penser en réseau plutôt qu'en liste

La Cognitive Network Science, c'est l'un des deux axes de recherche que j'indique sur ce site, à côté de l'IA en éducation. L'idée de base est simple : au lieu de représenter ce que quelqu'un sait ou ressent comme une liste de faits isolés, on le représente comme un réseau. Chaque mot, concept ou émotion devient un nœud ; chaque lien entre deux nœuds (parce qu'ils se ressemblent, se prononcent pareil, ou apparaissent souvent ensemble) devient une arête. Une fois ce réseau dessiné, on peut lui appliquer les outils classiques de la théorie des graphes : quels concepts sont les plus centraux ? Y a-t-il des zones densément connectées (des « communautés » de sens) ? Quelle est la distance, en nombre de liens, entre deux idées apparemment sans rapport ?

Ce type de réseau, construit à partir des associations libres d'une personne sur un sujet donné, s'appelle un **forma mentis network** : littéralement, la structure de son état d'esprit sur ce sujet. C'est un outil puissant pour rendre visibles des biais qui, autrement, resteraient de simples impressions.

La question que pose cette revue est directe : maintenant que les IA génératives absorbent des quantités massives de texte humain, héritent-elles aussi de la structure en réseau de la cognition humaine, biais compris ? Et peut-on utiliser les mêmes outils pour auditer les IA, comprendre les équipes mixtes humains-IA, ou concevoir de meilleurs outils pédagogiques ?

## Une revue systématique, au sens strict du terme

Pour y répondre sérieusement, nous avons suivi la méthodologie PRISMA 2020, la référence pour ce type de travail. Concrètement : recherche systématique sur huit bases de données majeures (arXiv, PubMed, IEEE Xplore, ScienceDirect, Springer Nature, ACM Digital Library, MDPI, Semantic Scholar), ce qui a remonté environ **53 374 références**. Après déduplication, il en restait environ 2 800 à examiner titre par titre et résumé par résumé. Au final, **36 études** ont été retenues pour la synthèse, publiées entre 2010 et 2026, dont 92 % depuis 2019, ce qui confirme qu'il s'agit d'un champ de recherche en pleine émergence.

Détail qui nous plaisait bien : puisqu'on écrit une revue sur la science des réseaux, autant utiliser des réseaux pour analyser la revue elle-même. Nous avons construit un réseau de co-citations entre les 36 études (quelles études sont citées ensemble dans la même section) et un réseau de co-auteurs, puis appliqué un algorithme de détection de communautés (Louvain) pour voir si la structure du champ, telle que dessinée par nos citations, correspondait à notre classement thématique fait à la main. Elle correspond assez bien, ce qui est plutôt rassurant sur la cohérence du travail.

## Cinq grandes familles de travaux

L'analyse a fait émerger cinq groupes thématiques :

- **A. Fondations théoriques** (13 études) : la boîte à outils elle-même, des réseaux cérébraux à petit monde aux réseaux lexicaux multiplex, où le sens, le son et l'orthographe d'un mot forment trois couches distinctes mais reliées.
- **B. Audit des IA/LLM par la CNS** (6 études) : le résultat le plus frappant de toute la revue. En construisant des forma mentis networks à partir des associations libres produites par GPT-3, GPT-3.5 Turbo et GPT-4 sur le thème des mathématiques, Abramski et al. (2023) montrent que les trois modèles reproduisent quantitativement les mêmes schémas d'anxiété mathématique que des lycéens humains. Autrement dit : ce n'est pas juste que l'IA « parle comme nous », sa structure de pensée mesurable ressemble statistiquement à la nôtre, angoisses comprises. D'autres travaux étendent cet audit aux stéréotypes de genre, à l'identité raciale et aux opinions politiques.
- **C. Intelligence collective homme-IA** (3 études) : comment la forme du réseau social d'une équipe (l'équilibre entre petits groupes très soudés et ponts entre groupes) détermine sa performance collective, et comment intégrer une IA dans cette équipe peut aider ou nuire, selon que ses représentations sont bien alignées avec celles des humains ou non.
- **D. IA sociale, émotionnelle et de santé** (10 études, le groupe le plus fourni) : détection de la dépression et de l'anxiété dans des textes, analyse des lettres d'adieu de personnes suicidaires (où l'anxiété apparaît comme un marqueur plus central que ne le suggérait la simple analyse de mots-clés), suivi des sentiments publics pendant la pandémie de Covid-19.
- **E. Éducation, créativité et augmentation cognitive** (4 études) : comment une pédagogie fondée sur l'investigation produit des réseaux sémantiques plus riches et plus flexibles que l'apprentissage par transmission, et comment l'expertise dans un domaine se traduit par un réseau de concepts plus dense et mieux connecté.

## Ce qu'il reste à faire

Cinq limites structurelles ressortent de l'ensemble du corpus. La plupart des études utilisent des réseaux statiques, une photographie à un instant donné, alors que suivre en temps réel comment le réseau conceptuel d'un utilisateur évolue pendant une interaction avec une IA serait bien plus informatif. La grande majorité des données provient de populations anglophones et occidentales, ce qui pose un vrai problème d'équité pour des IA déployées mondialement. Les résultats restent surtout corrélationnels, pas causaux. Le cadre éthique pour l'usage de données aussi intimes que la structure cognitive d'une personne reste à construire. Et enfin, intégrer techniquement ces outils dans l'architecture des grands modèles de langage actuels reste un défi ouvert.

## Le lien avec mes recherches

Deux résultats de cette revue résonnent directement avec ce que j'étudie sous l'angle de l'alignement pédagogique et de la sycophantie pédagogique.

Le premier, c'est le constat du Cluster B : les LLM ne se contentent pas d'imiter le ton humain, ils reproduisent structurellement nos biais cognitifs, mesurablement, réseau par réseau. C'est une preuve tangible, indépendante de mes propres travaux, d'un phénomène que je soupçonne au cœur de la sycophantie pédagogique : un tuteur fondé sur un LLM ne se contente pas d'être trop accommodant dans sa formulation, il peut aussi hériter et renforcer les conceptions erronées ou les angoisses déjà présentes chez l'apprenant, plutôt que de les corriger, simplement parce qu'il a appris sur des données humaines porteuses de ces mêmes biais.

Le second, c'est l'observation du Cluster E selon laquelle un enseignement optimisé pour la transmission efficace de connaissances produit des réseaux sémantiques plus pauvres qu'un enseignement fondé sur l'investigation. Transposé à une IA tutrice, ce résultat est presque un avertissement direct : une IA qui optimise pour donner la réponse la plus rapide et la plus satisfaisante risque d'homogénéiser la structure conceptuelle de l'apprenant, exactement l'inverse de ce que visent les difficultés désirables ou l'échec productif de Kapur, déjà au cœur du cadre que j'utilise pour le Biggs Alignment Index (BAI).

Une perspective concrète s'en dégage : coupler mes métriques d'alignement pédagogique (BAI, APed) à une analyse en forma mentis networks, pour ne plus seulement mesurer si une interaction de tutorat IA est pédagogiquement alignée, mais visualiser directement comment elle fait, ou ne fait pas, évoluer la structure conceptuelle de l'apprenant. Ce serait une manière très concrète de faire dialoguer ces deux champs que je place, depuis le début, côte à côte sur ce site.

## Pour aller plus loin

Cruz, C., Ghanem, H., Jabbar, S., Theroine, S., Delamare, M., Gautier, L., Bertolim, M. A., & Cherifi, H. (2026, à paraître). *Cognitive Network Science for Human-AI Systems: A Systematic Review.* Procedia Computer Science, actes de la 30e conférence KES (Knowledge-Based and Intelligent Information & Engineering Systems), Elsevier.

Code, figures et données bibliographiques : [github.com/ChristopheCruz/cns-human-ai-review](https://github.com/ChristopheCruz/cns-human-ai-review/)

*Lien vers l'article : [À COMPLÉTER une fois le DOI final attribué par l'éditeur].*

</div>

<div class="lang-en" markdown="1">

Together with Christophe Cruz, Hussam Ghanem, Samir Jabbar, Sarah Theroine, Laurent Gautier, Maria Alice Bertolim and Hocine Cherifi, we have just completed a systematic review of **Cognitive Network Science (CNS)** applied to human-AI systems, submitted to the KES 2026 conference and destined for *Procedia Computer Science* (Elsevier). Code, figures and bibliographic data are already public on [GitHub](https://github.com/ChristopheCruz/cns-human-ai-review/).

*Link to the paper: [TO BE COMPLETED once the final DOI is assigned by the publisher].*

## The starting idea: thinking in networks rather than in lists

Cognitive Network Science is one of the two research directions I list on this site, alongside AI in education. The basic idea is simple: instead of representing what someone knows or feels as a list of isolated facts, you represent it as a network. Every word, concept or emotion becomes a node; every link between two nodes (because they resemble each other, sound alike, or often appear together) becomes an edge. Once that network is drawn, you can apply the classic tools of graph theory to it: which concepts are the most central? Are there densely connected areas (communities of meaning)? How many links separate two seemingly unrelated ideas?

This kind of network, built from a person's free associations on a given topic, is called a **forma mentis network**: literally, the structure of their state of mind on that topic. It's a powerful tool for making visible biases that would otherwise remain mere impressions.

The question this review asks is direct: now that generative AI absorbs massive amounts of human text, does it also inherit the network structure of human cognition, biases included? And can the same tools be used to audit AI, understand mixed human-AI teams, or design better learning tools?

## A systematic review, in the strict sense of the term

To answer this seriously, we followed the PRISMA 2020 methodology, the reference standard for this kind of work. Concretely: a systematic search across eight major databases (arXiv, PubMed, IEEE Xplore, ScienceDirect, Springer Nature, ACM Digital Library, MDPI, Semantic Scholar), which returned about **53,374 records**. After deduplication, around 2,800 remained to be screened title by title and abstract by abstract. In the end, **36 studies** were retained for synthesis, published between 2010 and 2026, 92% of them since 2019, confirming that this is a rapidly emerging research field.

A detail we rather liked: since we were writing a review about network science, we might as well use networks to analyse the review itself. We built a co-citation network across the 36 studies (which studies get cited together in the same section) and a co-authorship network, then applied a community-detection algorithm (Louvain) to see whether the structure of the field, as drawn by our citations, matched the thematic classification we had done by hand. It matched fairly well, which is reassuring about the coherence of the work.

## Five broad families of work

The analysis revealed five thematic clusters:

- **A. Theoretical foundations** (13 studies): the toolkit itself, from small-world brain networks to multiplex lexical networks, in which a word's meaning, sound and spelling form three distinct but interconnected layers.
- **B. Auditing AI/LLMs with CNS** (6 studies): the most striking result in the whole review. By building forma mentis networks from the free associations produced by GPT-3, GPT-3.5 Turbo and GPT-4 on the topic of mathematics, Abramski et al. (2023) show that all three models quantitatively reproduce the same maths-anxiety patterns as human high-school students. In other words: it's not just that the AI "talks like us," its measurable thought structure statistically resembles ours, anxieties included. Other work extends this auditing approach to gender stereotypes, racial identity and political attitudes.
- **C. Human-AI collective intelligence** (3 studies): how a team's social network shape (the balance between tight local clusters and bridges between groups) determines its collective performance, and how bringing an AI into that team can help or hurt, depending on whether its representations are well aligned with the humans' or not.
- **D. Social, emotional and health AI** (10 studies, the largest cluster): detecting depression and anxiety in text, analysing suicide notes (where anxiety turns out to be a more central marker than simple keyword analysis suggested), and tracking public sentiment during the COVID-19 pandemic.
- **E. Education, creativity and cognitive augmentation** (4 studies): how inquiry-based pedagogy produces richer, more flexible semantic networks than transmission-based teaching, and how domain expertise translates into a denser, better-connected concept network.

## What's still missing

Five structural limitations stand out across the corpus. Most studies use static networks, a single snapshot in time, whereas tracking in real time how a user's conceptual network evolves during an interaction with an AI would be far more informative. The vast majority of the data comes from English-speaking, Western populations, which is a genuine equity problem for AI systems deployed globally. Results remain mostly correlational, not causal. The ethical framework for using data as intimate as a person's cognitive structure still needs to be built. And finally, technically integrating these tools into today's large language model architectures remains an open challenge.

## The link with my research

Two findings from this review resonate directly with what I study through the lens of pedagogical alignment and pedagogical sycophancy.

The first is Cluster B's finding: LLMs don't just imitate a human tone, they structurally reproduce our cognitive biases, measurably, network by network. That's tangible evidence, independent of my own work, for something I've suspected sits at the heart of pedagogical sycophancy: an LLM-based tutor doesn't just risk being overly agreeable in how it phrases things, it can also inherit and reinforce a learner's existing misconceptions or anxieties rather than correct them, simply because it learned from human data that carries those same biases.

The second is Cluster E's observation that teaching optimised for efficient knowledge transmission produces poorer semantic networks than inquiry-based teaching. Transposed to an AI tutor, that finding reads almost like a direct warning: an AI that optimises for giving the fastest, most satisfying answer risks homogenising the learner's conceptual structure, exactly the opposite of what desirable difficulties or Kapur's productive failure aim for, both already at the heart of the framework I use for the Biggs Alignment Index (BAI).

A concrete perspective follows from this: pairing my pedagogical-alignment metrics (BAI, APed) with forma mentis network analysis, so as to not only measure whether an AI tutoring interaction is pedagogically aligned, but to directly visualise how it does, or doesn't, reshape the learner's conceptual structure. That would be a very concrete way of putting these two fields, which I've placed side by side on this site from the start, into dialogue with each other.

## Further reading

Cruz, C., Ghanem, H., Jabbar, S., Theroine, S., Delamare, M., Gautier, L., Bertolim, M. A., & Cherifi, H. (2026, forthcoming). *Cognitive Network Science for Human-AI Systems: A Systematic Review.* Procedia Computer Science, proceedings of the 30th KES (Knowledge-Based and Intelligent Information & Engineering Systems) conference, Elsevier.

Code, figures and bibliographic data: [github.com/ChristopheCruz/cns-human-ai-review](https://github.com/ChristopheCruz/cns-human-ai-review/)

*Link to the paper: [TO BE COMPLETED once the final DOI is assigned by the publisher].*

</div>
