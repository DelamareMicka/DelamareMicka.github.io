---
layout: post
title: "Former à et par l'IA : concevoir un dispositif pédagogique avec le projet CAIRE"
title_en: "Training with and about AI: designing a teaching module for the CAIRE project"
date: 2026-08-18 08:00:00+0200
description: >
  <span class="lang-fr-i">Comment apprendre à 466 étudiants de première année à douter d'une IA, plutôt qu'à lui faire confiance aveuglément : la genèse d'un module d'acculturation à l'IA construit avec le jeu de Nim, la Moral Machine et une IA générative défaillante.</span><span class="lang-en-i">How do you teach 466 first-year students to question an AI rather than trust it blindly? The story of an AI awareness module built around the game of Nim, the Moral Machine, and a deliberately unreliable generative AI.</span>
tags: ia pedagogie enseignement caire
categories: recherche
related_posts: false
---

<div class="lang-fr" markdown="1">

Avec Maud Rousseau, ingénieure pédagogique à CESI, nous venons de rédiger un article sur un dispositif de formation que nous avons conçu dans le cadre du projet **CAIRE** (France 2030) : un module d'« acculturation » à l'intelligence artificielle, déployé auprès de 466 étudiants de première année d'école d'ingénieurs. L'article est en cours de publication aux Presses universitaires de la Méditerranée (PULM) ; je le lierai ici dès qu'il sera en ligne côté éditeur. En attendant, voici de quoi il parle.

*Lien vers l'article : [À COMPLÉTER une fois publié par PULM].*

## Le problème de départ

On pourrait croire qu'apprendre l'IA à des étudiants de première année consiste à leur enseigner comment ça marche techniquement. Ce n'était pas notre objectif. Un questionnaire préalable nous a montré une réalité plus inquiétante : une partie des étudiants savait déjà utiliser des outils d'IA (traduction, génération de texte), mais sans comprendre ce qui se passe « sous le capot », et avec une confiance excessive dans la fiabilité des réponses obtenues.

Le vrai enjeu n'était donc pas technique, mais critique : comment faire en sorte que ces futurs ingénieurs ne prennent jamais l'assurance affichée par une IA pour une preuve de vérité ? Nous avons cherché à former ce que le chercheur Charles Hadji (2025) appelle des « anthropolescents avisés » : des personnes capables de reconnaître, d'assumer et de questionner leur rapport aux technologies, plutôt que de le subir.

## Faire toucher du doigt ce qui reste abstrait

Plutôt qu'un cours magistral sur les réseaux de neurones, nous avons construit quatre activités concrètes, déployées lors du séminaire de rentrée des étudiants de première année (Cycle Préparatoire Intégré).

**Le jeu de Nim.** C'est un jeu de retrait d'allumettes (ou de perles) vieux de plus d'un siècle (Bouton, 1901), mais il se prête remarquablement bien à faire toucher du doigt l'apprentissage par renforcement. Les étudiants jouent d'abord humain contre humain, pour ressentir intuitivement la stratégie. Puis ils construisent une « machine » toute simple : des gobelets représentant les états du jeu, des billes représentant les actions possibles. Quand la machine perd, on retire une bille de la partie jouée (punition) ; quand elle gagne, on en ajoute (récompense). Après quelques dizaines de parties, la machine « apprend » à bien jouer, sans qu'on lui ait jamais expliqué la théorie du jeu. L'idée qui s'ancre là, très concrètement : une IA n'apprend pas par compréhension, mais par ajustement statistique répété.

**La Moral Machine.** Cet outil, basé sur les travaux d'Awad et al. (2018), confronte les étudiants aux dilemmes éthiques d'une voiture autonome en situation d'accident inévitable : qui privilégier ? L'activité rend concret un fait souvent oublié : une IA ne fait pas de choix moraux « neutres », elle applique des choix humains, codifiés à l'avance, avec toutes leurs zones grises.

**L'atelier « Grand-Mère ».** Ici, les étudiants sont plongés dans un scénario où une IA générative, baptisée « Grand-Mère », doit les aider à résoudre une situation d'urgence. Le twist : elle se trompe, avec la même assurance que lorsqu'elle a raison. L'objectif est de provoquer une vraie dissonance, pour ancrer une règle simple : une réponse d'IA générative est une hypothèse à vérifier, jamais une certitude. (Vous pouvez essayer une [petite démo inspirée de cet atelier](/grand-mere/) directement sur ce site.)

**Les débats structurés.** Pour clore le parcours, des débats en petits groupes relient l'expérience vécue aux grandes questions de société (transparence, biais, confiance), et rappellent une chose que ni le jeu de Nim ni aucun algorithme ne peut remplacer : la responsabilité finale d'une décision reste humaine.

## La mécanique derrière le dispositif

Concevoir ce parcours n'a rien eu d'improvisé. Nous avons suivi le modèle ADDIE (Analyse, Design, Développement, Implémentation, Évaluation), utilisé moins comme une suite d'étapes figées que comme un cadre de pilotage permettant des allers-retours constants entre les phases, avec l'alignement pédagogique comme fil conducteur à chaque itération.

Sur le plan théorique, le dispositif s'appuie sur les pédagogies actives (l'étudiant construit son savoir par l'expérience plutôt que de la recevoir passivement) et sur la ludopédagogie, qui utilise le jeu comme espace sécurisé pour expérimenter et se tromper sans enjeu. Ces deux piliers sont eux-mêmes mis au service d'un objectif plus large : articuler la maîtrise technique de l'IA (*AI literacy*), une posture critique face à ses usages (*critical AI education*) et une véritable citoyenneté numérique.

## Est-ce que ça a marché ?

Nous avons évalué le dispositif avec le modèle de Kirkpatrick (1975), qui distingue quatre niveaux : réaction, apprentissage, comportement, résultats. Les données proviennent d'un questionnaire administré à froid auprès des 466 étudiants.

Côté réaction (niveau 1), l'accueil a été très positif : le jeu de Nim en particulier a été qualifié de « super ludique » par les étudiants. Mais le résultat le plus intéressant se situe au niveau 2, celui des apprentissages réels. Avant le dispositif, une partie des étudiants surestimait la fiabilité des IA. Après, le discours change nettement : *« Avant, je pensais que les IA étaient bien plus fiables. Aujourd'hui je comprends mieux l'enjeu des IA »*, ou encore, à propos du fonctionnement probabiliste plutôt que sémantique d'un modèle de langage : *« L'IA pouvait comprendre et expliquer ce que l'on écrivait, alors qu'en réalité elle complète juste avec des probabilités. »*

Des signaux plus ténus, mais encourageants, apparaissent aussi au niveau 3 (changement de comportement déclaré) : les étudiants décrivent l'IA comme une « épée à double tranchant » et plusieurs disent vouloir approfondir le sujet de leur propre initiative, ce qui est exactement le genre de motivation intrinsèque qu'un dispositif ponctuel de 8 heures ne peut pas garantir, mais peut chercher à amorcer.

## Ce qui reste à améliorer

Tout n'a pas aussi bien fonctionné. L'atelier « Grand-Mère » a reçu des avis plus mitigés que le jeu de Nim : la liberté d'expérimentation avec une IA générative semble demander un accompagnement plus serré que le cadre très structuré du jeu de Nim, sous peine de désorienter certains étudiants. Nous avons aussi identifié la taille des groupes de débat comme un point à retravailler pour une participation plus active.

La suite logique de ce travail est une évaluation plus longitudinale : mesurer, plusieurs mois après la formation, si la vigilance critique observée dans les questionnaires se traduit vraiment dans les pratiques (niveau 4 de Kirkpatrick). Des entretiens semi-directifs sont prévus pour ça.

## Le lien avec mes recherches, et ce que ça ouvre comme perspectives

Ce dispositif s'inscrit dans le prolongement direct de mes travaux sur l'alignement pédagogique des IA génératives. L'atelier « Grand-Mère » en particulier touche à quelque chose que j'étudie par ailleurs sous l'angle de la **sycophantie pédagogique** : une IA qui affiche la même assurance qu'elle ait raison ou tort n'est pas seulement un problème de fiabilité factuelle, c'est un problème d'alignement, elle optimise pour paraître utile et confiante dans l'instant, pas pour la compréhension réelle de qui l'utilise. Faire vivre cette dissonance aux étudiants, très concrètement, est une manière de les acculturer à ce risque avant même qu'ils ne l'affrontent comme futurs professionnels utilisant l'IA dans leur travail.

Le contraste observé entre l'accueil très positif du jeu de Nim et l'accueil plus mitigé de l'atelier « Grand-Mère » me semble aussi révélateur d'un phénomène que je retrouve dans mes travaux sur les agents pédagogiques fondés sur des LLM : un cadre très structuré (le jeu de Nim, avec ses règles fermées) facilite l'engagement immédiat, tandis qu'un espace plus ouvert (dialoguer librement avec une IA générative) demande un étayage (*scaffolding*) beaucoup plus soigné pour rester productif plutôt que déstabilisant. C'est exactement le type d'arbitrage, entre liberté d'exploration et guidage, que l'on retrouve au cœur de l'alignement constructif de Biggs, et donc au cœur du **Biggs Alignment Index (BAI)** que je développe pour évaluer ce genre de dispositif.

Une perspective concrète qu'ouvre ce travail : appliquer la métrique **APed**, conçue pour quantifier l'alignement pédagogique des interactions de tutorat générées par IA, à l'atelier « Grand-Mère » lui-même, pour objectiver ce qui, dans son déroulé, fonctionne réellement comme déclencheur de réflexion critique et ce qui relève simplement de l'anecdote marquante. Ce serait une manière de faire dialoguer directement l'ingénierie pédagogique de CAIRE avec mes travaux méthodologiques sur l'évaluation de l'alignement.

## Pour aller plus loin

Delamare, M., & Rousseau, M. (à paraître). *Former à et par l'IA : un exemple de conception de ressources pédagogiques dans le cadre du projet CAIRE.* Presses universitaires de la Méditerranée (PULM).

*Lien vers l'article : [À COMPLÉTER une fois publié par PULM].*

</div>

<div class="lang-en" markdown="1">

Together with Maud Rousseau, a learning designer at CESI, I have just written a paper about a training module we designed as part of the **CAIRE** project (France 2030): an AI "acculturation" module deployed with 466 first-year engineering students. The paper is being published by Presses universitaires de la Méditerranée (PULM, a French university press); I will link it here as soon as it is live on the publisher's side. In the meantime, here is what it is about.

*Link to the paper: [TO BE COMPLETED once published by PULM].*

## The starting problem

You might assume that teaching AI to first-year students means teaching them how it works technically. That was not our goal. A preliminary survey revealed a more worrying reality: some students already used AI tools (translation, text generation) but had no idea what was happening "under the hood," and placed excessive trust in the reliability of the answers they got.

The real challenge, then, wasn't technical, but critical: how do you make sure these future engineers never mistake the confidence an AI displays for proof that it's right? We aimed to train what researcher Charles Hadji (2025) calls "anthropolescents avisés" ("informed anthropolescents"): people able to recognise, own, and question their relationship with technology, rather than simply undergo it.

## Making the abstract tangible

Rather than a lecture on neural networks, we built four hands-on activities, deployed during the first-year students' integration seminar (the *Cycle Préparatoire Intégré*).

**The game of Nim.** This is a century-old counter-removal game (Bouton, 1901), but it turns out to be remarkably well suited to making reinforcement learning tangible. Students first play human against human, to get an intuitive feel for the strategy. They then build a very simple "machine": cups representing the states of the game, marbles representing the possible actions. When the machine loses, a marble is removed from the play that led there (punishment); when it wins, one is added (reward). After a few dozen games, the machine "learns" to play well, without anyone ever having explained game theory to it. The idea that sticks, very concretely: an AI doesn't learn by understanding, it learns through repeated statistical adjustment.

**The Moral Machine.** This tool, based on the work of Awad et al. (2018), confronts students with the ethical dilemmas of a self-driving car facing an unavoidable accident: who should it prioritise? The activity makes a often-forgotten fact concrete: an AI doesn't make "neutral" moral choices, it applies human choices, coded in advance, with all their grey areas.

**The "Grandma" workshop.** Here, students are placed in a scenario where a generative AI, named "Grandma" ("Grand-Mère"), is supposed to help them resolve an emergency. The twist: it gets things wrong, with the same confidence it shows when it's right. The goal is to trigger genuine cognitive dissonance, to anchor a simple rule: a generative AI's answer is a hypothesis to verify, never a certainty. (You can try a [small demo inspired by this workshop](/grand-mere/) directly on this site.)

**Structured debates.** To close the sequence, small-group debates connect the hands-on experience to broader societal questions (transparency, bias, trust), and drive home something that neither the game of Nim nor any algorithm can replace: final responsibility for a decision remains human.

## The mechanics behind the module

Designing this sequence was not improvised. We followed the ADDIE model (Analysis, Design, Development, Implementation, Evaluation), used less as a fixed sequence of steps than as a steering framework allowing constant back-and-forth between phases, with pedagogical alignment as the guiding thread at every iteration.

On the theoretical side, the module draws on active pedagogies (students build their own knowledge through experience rather than receiving it passively) and on game-based learning, which uses play as a safe space to experiment and fail without real stakes. Both of these pillars serve a broader goal: bringing together technical mastery of AI (*AI literacy*), a critical stance towards its uses (*critical AI education*), and genuine digital citizenship.

## Did it work?

We evaluated the module using Kirkpatrick's model (1975), which distinguishes four levels: reaction, learning, behaviour, results. The data came from a delayed ("cold") questionnaire administered to the 466 students.

On the reaction side (level 1), the response was very positive: the game of Nim in particular was described by students as "super fun." But the most interesting result sits at level 2, actual learning. Before the module, some students overestimated how reliable AI systems are. Afterwards, the discourse shifts noticeably: *"Before, I thought AI was much more reliable. Now I understand the stakes of AI better,"* or, about the probabilistic rather than semantic nature of a language model: *"The AI seemed to understand and explain what we wrote, when in reality it just completes with probabilities."*

Fainter but encouraging signals also appear at level 3 (self-reported behaviour change): students describe AI as a "double-edged sword," and several say they want to dig deeper into the topic on their own initiative, which is exactly the kind of intrinsic motivation a single 8-hour module cannot guarantee, but can try to spark.

## What still needs work

Not everything worked equally well. The "Grandma" workshop received more mixed reviews than the game of Nim: the open-ended freedom of experimenting with a generative AI seems to require tighter scaffolding than the game of Nim's very structured format, or it risks disorienting some students. We also identified debate group size as something to rework for more active participation.

The logical next step is a more longitudinal evaluation: measuring, several months after the training, whether the critical vigilance observed in the questionnaires actually translates into practice (Kirkpatrick's level 4). Semi-structured interviews are planned for that.

## The link with my research, and the perspectives it opens

This module is a direct extension of my work on the pedagogical alignment of generative AI. The "Grandma" workshop in particular touches on something I study elsewhere through the lens of **pedagogical sycophancy**: an AI that displays the same confidence whether it's right or wrong isn't just a factual-reliability problem, it's an alignment problem, it optimises for appearing helpful and confident in the moment, not for the real understanding of whoever is using it. Making students live through that dissonance, very concretely, is one way to build resistance to that risk before they face it as future professionals using AI in their own work.

The contrast between the very positive reception of the game of Nim and the more mixed reception of the "Grandma" workshop also strikes me as revealing something I keep running into in my work on LLM-based pedagogical agents: a tightly structured setting (the game of Nim, with its closed rules) makes immediate engagement easy, while a more open space (freely conversing with a generative AI) needs much more careful scaffolding to stay productive rather than disorienting. That is exactly the kind of trade-off, between freedom to explore and guidance, that sits at the heart of Biggs' constructive alignment, and therefore at the heart of the **Biggs Alignment Index (BAI)** I am developing to evaluate this kind of module.

One concrete perspective this work opens up: applying the **APed** metric, designed to quantify the pedagogical alignment of AI-generated tutoring interactions, to the "Grandma" workshop itself, to make explicit what, in the workshop's flow, genuinely functions as a trigger for critical reflection versus what is simply a memorable anecdote. That would be a way to put CAIRE's instructional-design work directly in dialogue with my own methodological work on evaluating alignment.

## Further reading

Delamare, M., & Rousseau, M. (forthcoming). *Former à et par l'IA : un exemple de conception de ressources pédagogiques dans le cadre du projet CAIRE.* Presses universitaires de la Méditerranée (PULM).

*Link to the paper: [TO BE COMPLETED once published by PULM].*

</div>
