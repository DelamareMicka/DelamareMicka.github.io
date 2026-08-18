---
layout: post
title: "Un tuteur IA qui devine juste peut quand même mal enseigner"
title_en: "An AI tutor that guesses right can still teach badly"
date: 2026-09-03 08:00:00+0200
description: >
  <span class="lang-fr-i">Un tuteur IA n'agit jamais sur l'état réel de l'élève, seulement sur une estimation. Un article que nous venons de terminer, avec Florian et Benoît, montre que la qualité de cette estimation ne prédit presque rien de la qualité des décisions qui en découlent, et que ça se mesure.</span><span class="lang-en-i">An AI tutor never acts on a student's real state, only on an estimate of it. A paper we've just finished, with Florian and Benoît, shows that how good that estimate is barely predicts how good the resulting decisions are, and that this is measurable.</span>
tags: llm-agents pomdp tutorat recherche
categories: recherche
related_posts: false
_styles: >
  .aipaa-loop-wrap { margin: 1.25rem 0 0.5rem; }
  .aipaa-loop { display: flex; flex-wrap: wrap; align-items: stretch; gap: 0.35rem; }
  .aipaa-loop-stage {
    flex: 1 1 120px;
    min-width: 100px;
    border: 1px solid var(--global-divider-color);
    border-radius: 0.4rem;
    background: var(--global-card-bg-color);
    padding: 0.5rem 0.6rem;
    cursor: pointer;
    font-size: 0.8rem;
    text-align: center;
    transition: border-color 0.15s, background 0.15s;
  }
  .aipaa-loop-stage:hover, .aipaa-loop-stage.active {
    border-color: var(--global-theme-color);
    background: color-mix(in srgb, var(--global-theme-color) 10%, transparent);
  }
  .aipaa-loop-stage .num { display: block; font-weight: 700; font-size: 0.68rem; opacity: 0.6; margin-bottom: 0.15rem; }
  .aipaa-loop-arrow { display: flex; align-items: center; justify-content: center; opacity: 0.4; flex: 0 0 auto; width: 1.2rem; }
  .aipaa-loop-arrow::before { content: '\2192'; }
  @media (max-width: 640px) {
    .aipaa-loop { flex-direction: column; }
    .aipaa-loop-arrow { width: auto; height: 1rem; }
    .aipaa-loop-arrow::before { content: '\2193'; }
  }
  .aipaa-loop-detail {
    margin-top: 0.75rem;
    padding: 0.7rem 1rem;
    border-left: 3px solid var(--global-theme-color);
    background: color-mix(in srgb, var(--global-theme-color) 6%, transparent);
    border-radius: 0.25rem;
    font-size: 0.86rem;
    min-height: 3em;
  }
  .aipaa-loop-note { margin-top: 0.6rem; font-size: 0.8rem; color: var(--global-text-color-light); font-style: italic; }

  .aipaa-verdicts-wrap { margin: 1.25rem 0; }
  .aipaa-verdicts { display: flex; flex-wrap: wrap; gap: 0.75rem; }
  .aipaa-verdict-panel {
    flex: 1 1 180px;
    border: 1px solid var(--global-divider-color);
    border-radius: 0.4rem;
    padding: 0.75rem 0.85rem;
    cursor: pointer;
    background: var(--global-card-bg-color);
    transition: border-color 0.15s;
  }
  .aipaa-verdict-panel.active { border-color: var(--global-theme-color); }
  .aipaa-verdict-panel h4 { margin: 0 0 0.6rem; font-size: 0.82rem; }
  .aipaa-bar-row { display: flex; align-items: center; gap: 0.5rem; margin-bottom: 0.4rem; font-size: 0.74rem; }
  .aipaa-bar-label { flex: 0 0 3.4rem; opacity: 0.75; }
  .aipaa-bar-track { flex: 1; background: var(--global-divider-color); border-radius: 999px; height: 0.55rem; overflow: hidden; }
  .aipaa-bar-fill { height: 100%; border-radius: 999px; width: 0%; background: var(--global-theme-color); transition: width 0.6s ease; }
  .aipaa-bar-fill.alt { background: #d9822b; }
  .aipaa-bar-value { flex: 0 0 2.6rem; text-align: right; font-variant-numeric: tabular-nums; opacity: 0.75; }
  .aipaa-verdict-summary {
    margin-top: 0.9rem;
    padding: 0.6rem 0.85rem;
    border-radius: 0.3rem;
    background: color-mix(in srgb, var(--global-theme-color) 8%, transparent);
    font-size: 0.85rem;
  }
---

<div class="lang-fr" markdown="1">

Avec Florian et Benoît, nous venons de terminer un article qui mesure quelque chose que, jusqu'ici, personne ne pouvait vraiment mesurer : à quel point une IA tutrice se trompe *à cause de* son estimation du niveau de l'élève, plutôt qu'à cause de sa pédagogie. L'article est en cours de relecture en double aveugle ; nous le lierons ici une fois la décision connue, en espérant début septembre.

*Lien vers l'article : [À COMPLÉTER une fois la décision de relecture connue].*

## Le problème du bébé qui pleure, version salle de classe

Si vous avez lu mon billet sur les [POMDP et le bébé qui pleure](/blog/2026/introduction-pomdp-bebe-qui-pleure/), vous connaissez déjà l'idée centrale : un agent qui décide dans l'incertitude n'a jamais accès à l'état réel du monde, seulement à une observation, et doit construire une croyance à partir de ça. Un tuteur IA est exactement dans cette situation. Il ne voit jamais ce que l'élève sait vraiment ; il ne voit que ses réponses, et doit en déduire une estimation de son niveau pour décider quoi enseigner ensuite : monter en difficulté, redescendre, ou rester.

Le problème, c'est que personne ne peut vérifier si cette estimation est bonne. Un système déployé n'a jamais accès à la vérité sur l'état de l'élève, seulement à ce que son propre capteur lui rapporte. Alors comment savoir si une meilleure estimation produit vraiment de meilleures décisions ? Jusqu'ici, la plupart des travaux se contentent de vérifier que l'élève réussit mieux à la fin, ce qui ne dit rien sur si c'est grâce à une meilleure estimation ou grâce à une tâche plus indulgente.

## L'astuce : fabriquer un monde où on connaît la vérité

L'idée de l'article est simple à énoncer, plus délicate à construire proprement : simuler toute la boucle (élève, capteur, estimateur, règle de décision, tuteur) dans un environnement où on impose nous-mêmes la trajectoire réelle de maîtrise de l'élève, invisible à tous les composants du système, mais connue de nous en tant qu'expérimentateurs. On peut alors comparer deux choses : les décisions que le tuteur prend réellement, à partir de son estimation, et les décisions qu'un tuteur parfait, ayant accès à la vérité, aurait prises exactement au même moment. On appelle ce second tuteur l'« oracle ». La différence entre les deux flux de décisions, c'est exactement l'erreur causée par une mauvaise estimation, rien d'autre.

<div class="aipaa-loop-wrap">
<div id="aipaa-loop-fr" class="aipaa-loop" aria-label="Schéma interactif de la boucle de mesure"></div>
<div id="aipaa-loop-fr-detail" class="aipaa-loop-detail">Cliquez sur une étape pour voir ce qui s'y passe.</div>
<p class="aipaa-loop-note">En parallèle, un « oracle » invisible applique exactement la même règle, mais directement à la vérité cachée B* plutôt qu'à l'estimation. La différence entre les deux flux de décisions, comparée aux étapes 3, 5 et 6, donne les trois mesures E, G et S présentées plus bas.</p>
</div>

## Cinq façons de deviner le niveau de l'élève

Cinq « observateurs » sont mis à l'épreuve dans cette boucle : trois méthodes statistiques classiques (une simple moyenne mobile, un filtre de Kalman, le même genre d'outil utilisé pour suivre la position d'une fusée ou d'un GPS, et le Bayesian Knowledge Tracing, une méthode spécifique à l'éducation), et deux grands modèles de langage utilisés comme observateurs récursifs, à qui on redonne à chaque tour leurs propres estimations précédentes pour qu'ils les mettent à jour. Aucun n'est entraîné spécifiquement pour la tâche : ce sont cinq façons différentes de lire la même série de réponses et d'en tirer un niveau estimé.

## Trois endroits où compter l'erreur, trois verdicts différents

C'est le résultat le plus surprenant de l'article. On peut compter l'erreur à trois endroits de la même boucle : à la lecture (l'estimation du niveau est-elle fausse ?), à la décision (la règle appliquée à l'estimation choisit-elle une action différente de celle de l'oracle ?), et à l'état laissé à l'élève (combien de temps l'élève passe-t-il, au final, à un niveau de difficulté que l'oracle n'aurait pas choisi ?). Le cas le plus net dans l'article oppose deux des cinq méthodes : sur 60 tours de dialogue, l'une des deux se trompe sur le niveau 13 fois de plus que l'autre, un écart net. Pourtant, une fois qu'on regarde les décisions réellement prises, les deux méthodes se retrouvent à moins d'un tour d'écart l'une de l'autre, quasiment à égalité. Puis, en regardant l'état où l'élève est laissé au fil du temps, l'écart réapparaît, cette fois dans l'autre sens et à hauteur de 17 tours. Trois classements différents pour les deux mêmes méthodes, selon l'endroit où on regarde. Aucun des trois n'est faux ; ils répondent juste à trois questions différentes.

<div class="aipaa-verdicts-wrap">
<div id="aipaa-verdicts-fr" class="aipaa-verdicts" aria-label="Comparaison interactive BKT contre Kalman aux trois endroits de mesure"></div>
<div id="aipaa-verdicts-fr-summary" class="aipaa-verdict-summary"></div>
</div>

Autre moment révélateur : une politique absurde qui ne fait jamais rien, qui laisse toujours l'élève au même niveau, obtient un score d'accord brut avec l'oracle meilleur que trois des cinq vraies méthodes testées, tout simplement parce que l'oracle lui-même ne bouge que sur 10 tours sur 60. Une méthode qui ne fait rien a donc l'air compétente sur cette mesure brute. L'article corrige ce biais avec le kappa de Cohen (une mesure d'accord qui retire ce qu'on obtiendrait par hasard), et le classement change : deux méthodes que la mesure brute plaçait mal se révèlent en fait clairement meilleures que le hasard.

## Ce que ça remet en cause

Deux enseignements se dégagent, au-delà des chiffres précis. D'abord, la frontière entre méthodes statistiques classiques et IA génératives n'est pas celle qu'on attendrait : sur la vitesse à laquelle une erreur de lecture se transforme en mauvaise décision, la plus simple des trois méthodes classiques (la moyenne mobile) se comporte comme les deux modèles de langage, pas comme les deux autres méthodes classiques. La distinction utile n'est pas « statistique contre IA », mais plutôt « une méthode dont les erreurs survivent au filtre de la règle de décision » contre « une méthode dont les erreurs s'y dissolvent ».

Ensuite, ce travail reste prudent sur la portée de ses propres résultats : nous le présentons comme la preuve qu'une mesure de ce type est possible et informative, pas comme un classement définitif des méthodes. Nous documentons nous-mêmes une limite importante : l'élève simulé (un grand modèle de langage jouant le rôle de l'apprenant) a tendance à répondre légèrement au-dessus du niveau qu'on lui a demandé de jouer, un biais documenté ailleurs sous le nom de « paradoxe de compétence ». Ce biais touche l'élève simulé, pas les méthodes d'estimation elles-mêmes, mais il limite ce qu'on peut affirmer sur la boucle complète, ce qui nous pousse à nous appuyer surtout sur une version simplifiée de l'expérience (où l'estimateur reçoit directement le niveau réel, sans bruit de lecture) pour tirer nos conclusions les plus solides.

## Le lien avec mes recherches

Ce travail est, d'une certaine façon, la version instrumentée de ce que j'explore depuis le début sur ce site sous l'angle de la sycophantie pédagogique et de l'alignement pédagogique. La sycophantie, telle que je la conçois, porte surtout sur le *ton* d'un tuteur IA : une IA trop conciliante, qui valide plutôt qu'elle ne corrige. Cet article éclaire un mécanisme complémentaire, silencieux celui-là : même un tuteur au ton irréprochable peut dériver simplement parce que son estimation de l'élève glisse hors de la réalité, sans que rien, dans le comportement observable du système, ne le signale. C'est un désalignement structurel, pas un désalignement de style, et c'est exactement le genre de désalignement que le Biggs Alignment Index (BAI) et la métrique APed cherchent à rendre mesurable de mon côté, à un autre endroit de la chaîne.

La perspective la plus directe que j'en tire : les trois comptages de cet article (à la lecture, à la décision, à l'état laissé à l'élève) et mes propres métriques d'alignement pourraient converger vers un même diagnostic à plusieurs étages, capable de distinguer un tuteur IA qui se trompe sur l'élève d'un tuteur IA qui, même avec une estimation correcte, choisit la mauvaise action pédagogique. Ce sont deux défaillances très différentes, et pour l'instant nos outils respectifs n'en isolent chacun qu'une partie.

## Pour aller plus loin

*Measuring Estimation-to-Policy Drift in LLM Agents.* Article actuellement en relecture en double aveugle. Référence complète et lien ajoutés ici une fois la décision connue.

*Lien vers l'article : [À COMPLÉTER une fois la décision de relecture connue].*

</div>

<div class="lang-en" markdown="1">

Together with Florian and Benoît, we've just finished a paper that measures something nobody could really measure before: how much an AI tutor goes wrong *because of* its estimate of the student's level, as opposed to because of its teaching itself. The paper is currently under double-blind review; we'll link it here once the decision is known, hopefully in early September.

*Link to the paper: [TO BE COMPLETED once the review decision is known].*

## The crying baby problem, classroom edition

If you've read my post on [POMDPs and the crying baby](/blog/2026/introduction-pomdp-bebe-qui-pleure/), you already know the core idea: an agent deciding under uncertainty never has access to the true state of the world, only to an observation, and has to build a belief out of that. An AI tutor is in exactly that position. It never sees what a student actually knows; it only sees their answers, and has to infer an estimate of their level from that in order to decide what to teach next: raise the difficulty, lower it, or hold steady.

The trouble is that nobody can check whether that estimate is any good. A deployed system never has access to the truth about the student's state, only to whatever its own sensor reports. So how do you know whether a better estimate actually produces better decisions? Most existing work simply checks whether the student ends up performing better, which says nothing about whether that's thanks to a better estimate or to a more forgiving task.

## The trick: building a world where the truth is known

The idea behind the paper is easy to state, trickier to build properly: simulate the whole loop (student, sensor, estimator, decision rule, tutor) in an environment where we ourselves impose the student's real mastery trajectory, invisible to every component of the system, but known to us as experimenters. We can then compare two things: the decisions the tutor actually makes, based on its estimate, and the decisions a perfect tutor, with access to the truth, would have made at the exact same moment. We call that second tutor the "oracle." The difference between the two decision streams is exactly the error caused by a bad estimate, and nothing else.

<div class="aipaa-loop-wrap">
<div id="aipaa-loop-en" class="aipaa-loop" aria-label="Interactive diagram of the measurement loop"></div>
<div id="aipaa-loop-en-detail" class="aipaa-loop-detail">Click a stage to see what happens there.</div>
<p class="aipaa-loop-note">In parallel, an invisible "oracle" applies the exact same rule, but directly to the hidden truth B* instead of to the estimate. The difference between the two decision streams, compared at stages 3, 5, and 6, gives the three measures E, G, and S presented below.</p>
</div>

## Five ways of guessing the student's level

Five "observers" are put through this loop: three classic statistical methods (a simple moving average, a Kalman filter, the same kind of tool used to track a rocket's or a GPS's position, and Bayesian Knowledge Tracing, a method specific to education), and two large language models used as recursive observers, fed their own previous estimates at every turn so they can update them. None is trained specifically for the task: these are five different ways of reading the same sequence of answers and turning it into an estimated level.

## Three places to count the error, three different verdicts

This is the paper's most surprising result. Error can be counted at three points of the same loop: at the reading (is the level estimate wrong?), at the decision (does the rule applied to the estimate choose a different action than the oracle's?), and at the state the student is left in (how much time does the student end up spending at a difficulty level the oracle would not have chosen?). The clearest case in the paper pits two of the five methods against each other: over 60 turns of dialogue, one of them misreads the level 13 more times than the other, a clear gap. Yet once you look at the decisions actually taken, the two methods end up within one turn of each other, essentially tied. Then, looking at the state the student is left in over time, the gap reappears, this time in the other direction and to the tune of 17 turns. Three different rankings for the same two methods, depending on where you look. None of the three is wrong; they're just answering three different questions.

<div class="aipaa-verdicts-wrap">
<div id="aipaa-verdicts-en" class="aipaa-verdicts" aria-label="Interactive comparison of BKT versus Kalman at the three measurement points"></div>
<div id="aipaa-verdicts-en-summary" class="aipaa-verdict-summary"></div>
</div>

Another telling moment: a policy that does literally nothing, always leaving the student at the same level, scores a higher raw agreement rate with the oracle than three of the five real methods tested, simply because the oracle itself only moves on 10 of the 60 turns. Doing nothing therefore looks competent on that raw measure. The paper corrects for this with Cohen's kappa (an agreement measure that removes what you'd get by chance), and the ranking changes: two methods that the raw measure ranked poorly turn out to be clearly better than chance after all.

## What this calls into question

Two lessons emerge beyond the specific numbers. First, the line between classic statistical methods and generative AI isn't where you'd expect: on how fast a reading error turns into a bad decision, the simplest of the three classic methods (the moving average) behaves like the two language models, not like the other two classic methods. The useful distinction isn't "statistics versus AI," but rather "a method whose errors survive the decision rule's filter" versus "a method whose errors dissolve in it."

Second, this work stays careful about the scope of its own results: we present it as proof that this kind of measurement is possible and informative, not as a definitive ranking of methods. We document an important limitation ourselves: the simulated student (a large language model playing the role of the learner) tends to answer slightly above the level it was instructed to play, a bias documented elsewhere as a "competence paradox." That bias sits in the simulated student, not in the estimation methods themselves, but it limits what can be claimed about the full loop, which is why we lean mostly on a simplified version of the experiment (where the estimator is fed the true level directly, with no reading noise) to draw our strongest conclusions.

## The link with my research

This work is, in a way, the instrumented version of what I've been exploring on this site from the start under the lens of pedagogical sycophancy and pedagogical alignment. Sycophancy, as I frame it, is mostly about an AI tutor's *tone*: an AI that is too accommodating, that validates rather than corrects. This paper illuminates a complementary, quieter mechanism: even a tutor with an impeccable tone can drift simply because its estimate of the student slips away from reality, with nothing in the system's observable behaviour signalling it. That's a structural misalignment, not a stylistic one, and it's exactly the kind of misalignment the Biggs Alignment Index (BAI) and the APed metric are meant to make measurable on my side, at a different point in the chain.

The most direct perspective I take from this: the paper's three counts (at the reading, at the decision, at the state the student is left in) and my own alignment metrics could converge toward a single multi-stage diagnostic, able to tell apart an AI tutor that misreads the student from an AI tutor that, even with a correct estimate, picks the wrong pedagogical action. Those are two very different failures, and right now our respective tools each isolate only one of them.

## Further reading

*Measuring Estimation-to-Policy Drift in LLM Agents.* Currently under double-blind review. Full reference and link added here once the decision is known.

*Link to the paper: [TO BE COMPLETED once the review decision is known].*

</div>

<script>
(function () {
  function initLoop(containerId, detailId, cfg) {
    var container = document.getElementById(containerId);
    var detail = document.getElementById(detailId);
    if (!container || !detail) return;

    cfg.stages.forEach(function (stage, i) {
      if (i > 0) {
        var arrow = document.createElement('div');
        arrow.className = 'aipaa-loop-arrow';
        container.appendChild(arrow);
      }
      var box = document.createElement('div');
      box.className = 'aipaa-loop-stage';
      var num = document.createElement('span');
      num.className = 'num';
      num.textContent = cfg.stepLabel + ' ' + stage.num;
      box.appendChild(num);
      box.appendChild(document.createTextNode(stage.label));
      box.addEventListener('click', function () {
        Array.prototype.forEach.call(container.querySelectorAll('.aipaa-loop-stage'), function (b) {
          b.classList.remove('active');
        });
        box.classList.add('active');
        detail.textContent = stage.detail;
      });
      container.appendChild(box);
    });
  }

  initLoop('aipaa-loop-fr', 'aipaa-loop-fr-detail', {
    stepLabel: 'Étape',
    stages: [
      { num: 1, label: 'Vérité cachée (B*)', detail: "Le niveau réel de l'élève à cet instant. Personne dans le système ne le voit jamais : il n'est connu que de nous, les expérimentateurs, pour pouvoir mesurer l'erreur." },
      { num: 2, label: "L'élève répond", detail: "Un élève simulé répond à la question, avec une part d'aléatoire : sa réponse ne reflète jamais son niveau réel de façon parfaite." },
      { num: 3, label: 'Le capteur lit', detail: "Un premier composant lit uniquement la réponse texte, jamais la vérité, et en tire un niveau brut. C'est la seule information dont dispose la suite de la chaîne." },
      { num: 4, label: "L'observateur estime", detail: "Celui qu'on fait varier dans l'expérience : il intègre l'historique des lectures pour produire une estimation plus stable du niveau, sa tendance et sa confiance." },
      { num: 5, label: 'La règle décide', detail: "Une règle fixe, identique pour tous les observateurs, transforme l'estimation en une décision : monter le niveau, le baisser, ou rester." },
      { num: 6, label: 'Le tuteur écrit la suite', detail: "L'action choisie détermine la difficulté de la prochaine question posée à l'élève, et la boucle recommence." }
    ]
  });

  initLoop('aipaa-loop-en', 'aipaa-loop-en-detail', {
    stepLabel: 'Step',
    stages: [
      { num: 1, label: 'Hidden truth (B*)', detail: "The student's real level at this instant. No component in the system ever sees it: it's known only to us, the experimenters, so we can measure the error." },
      { num: 2, label: 'The student answers', detail: "A simulated student answers the question, with some randomness: their answer never perfectly reflects their real level." },
      { num: 3, label: 'The sensor reads', detail: "A first component reads only the text answer, never the truth, and extracts a raw level from it. That's the only information the rest of the chain has to work with." },
      { num: 4, label: 'The observer estimates', detail: "The component the experiment varies: it integrates the history of readings to produce a more stable estimate of the level, its trend, and its confidence." },
      { num: 5, label: 'The rule decides', detail: "A fixed rule, identical for every observer, turns the estimate into a decision: raise the level, lower it, or hold steady." },
      { num: 6, label: 'The tutor writes the next item', detail: "The chosen action sets the difficulty of the next question given to the student, and the loop starts again." }
    ]
  });

  function initVerdicts(containerId, summaryId, cfg) {
    var container = document.getElementById(containerId);
    var summary = document.getElementById(summaryId);
    if (!container || !summary) return;

    var panels = [];
    cfg.panels.forEach(function (panel, idx) {
      var el = document.createElement('div');
      el.className = 'aipaa-verdict-panel';

      var h4 = document.createElement('h4');
      h4.textContent = panel.title;
      el.appendChild(h4);

      [['kalman', cfg.labels.kalman, panel.kalman, ''], ['bkt', cfg.labels.bkt, panel.bkt, 'alt']].forEach(function (row) {
        var rowEl = document.createElement('div');
        rowEl.className = 'aipaa-bar-row';
        var label = document.createElement('span');
        label.className = 'aipaa-bar-label';
        label.textContent = row[1];
        var track = document.createElement('span');
        track.className = 'aipaa-bar-track';
        var fill = document.createElement('span');
        fill.className = 'aipaa-bar-fill' + (row[3] ? ' ' + row[3] : '');
        fill.style.width = '0%';
        fill.dataset.target = Math.min(100, (row[2] / cfg.scaleMax) * 100).toFixed(1);
        track.appendChild(fill);
        var value = document.createElement('span');
        value.className = 'aipaa-bar-value';
        value.textContent = row[2].toFixed(3);
        rowEl.appendChild(label);
        rowEl.appendChild(track);
        rowEl.appendChild(value);
        el.appendChild(rowEl);
      });

      el.addEventListener('click', function () {
        panels.forEach(function (p) { p.classList.remove('active'); });
        el.classList.add('active');
        summary.textContent = panel.summary;
      });

      container.appendChild(el);
      panels.push(el);
    });

    // reveal bar widths shortly after mount, and select the first panel by default
    setTimeout(function () {
      Array.prototype.forEach.call(container.querySelectorAll('.aipaa-bar-fill'), function (f) {
        f.style.width = f.dataset.target + '%';
      });
    }, 50);
    if (panels.length) panels[0].click();
  }

  initVerdicts('aipaa-verdicts-fr', 'aipaa-verdicts-fr-summary', {
    labels: { kalman: 'Kalman', bkt: 'BKT' },
    scaleMax: 0.45,
    panels: [
      {
        title: 'À la lecture (E)', kalman: 0.083, bkt: 0.300,
        summary: "À la lecture, l'écart est net : BKT se trompe sur le niveau 0,300 du temps contre 0,083 pour Kalman, soit environ 13 tours de plus sur 60."
      },
      {
        title: 'À la décision (G)', kalman: 0.150, bkt: 0.133,
        summary: "À la décision, l'écart s'efface presque entièrement : 0,133 contre 0,150, les deux méthodes sont à moins d'un tour d'écart."
      },
      {
        title: "À l'état laissé (S)", kalman: 0.133, bkt: 0.417,
        summary: "À l'état laissé à l'élève, l'écart réapparaît, dans l'autre sens : 0,417 contre 0,133, soit environ 17 tours de plus passés au mauvais niveau."
      }
    ]
  });

  initVerdicts('aipaa-verdicts-en', 'aipaa-verdicts-en-summary', {
    labels: { kalman: 'Kalman', bkt: 'BKT' },
    scaleMax: 0.45,
    panels: [
      {
        title: 'At the reading (E)', kalman: 0.083, bkt: 0.300,
        summary: 'At the reading stage, the gap is clear: BKT misreads the level 0.300 of the time versus 0.083 for Kalman, about 13 more turns out of 60.'
      },
      {
        title: 'At the decision (G)', kalman: 0.150, bkt: 0.133,
        summary: 'At the decision stage, the gap almost vanishes: 0.133 versus 0.150, the two methods are within one turn of each other.'
      },
      {
        title: 'At the state left (S)', kalman: 0.133, bkt: 0.417,
        summary: 'At the state the student is left in, the gap reappears, in the other direction: 0.417 versus 0.133, about 17 more turns spent at the wrong level.'
      }
    ]
  });
})();
</script>
