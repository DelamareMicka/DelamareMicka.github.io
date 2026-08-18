---
layout: post
title: "Introduction aux POMDP : décider sous incertitude partielle avec le « bébé qui pleure »"
title_en: "Introduction to POMDPs: deciding under partial observability with the “crying baby” problem"
date: 2026-07-27 10:00:00+0200
description: >
  <span class="lang-fr-i">Notes sur les processus de décision markoviens partiellement observables (POMDP) : le 7-uplet formel, la mise à jour de croyance bayésienne et les méthodes de résolution (QMDP, FIB, PBVI, POMCP), à partir de l'exemple pédagogique du « bébé qui pleure ».</span><span class="lang-en-i">Notes on partially observable Markov decision processes (POMDPs): the formal 7-tuple, Bayesian belief updating and solution methods (QMDP, FIB, PBVI, POMCP), built around the classic "crying baby" teaching example.</span>
tags: pomdp ia decision
categories: notes-de-lecture
related_posts: false
---

<div class="lang-fr" markdown="1">

Ce billet est un ensemble de notes prises en regardant [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs), un cours de la série *Decision Making Under Uncertainty* de Julia Academy (Robert Moss, Stanford University), qui s'appuie sur l'écosystème [`POMDPs.jl`](https://github.com/JuliaPOMDP/POMDPs.jl). L'exemple pédagogique utilisé (le « bébé qui pleure », *crying baby problem*) et les notebooks associés sont disponibles sur le dépôt [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty).

*Une version sans formules ni jargon est disponible ici : [Décider sans tout savoir : ce que le « bébé qui pleure » nous apprend sur l'IA](/blog/2026/decider-sans-tout-savoir-bebe-qui-pleure/).*

## Des MDP aux POMDP

Un processus de décision markovien (MDP) suppose que l'agent connaît exactement l'état $s$ du système à chaque instant. C'est rarement vrai : la plupart des systèmes réels (un robot avec des capteurs bruités, un patient dont on ne connaît pas l'état de santé exact, ou un tuteur intelligent qui ne peut qu'*observer* les réponses d'un élève sans connaître son état de connaissance réel) ne donnent accès qu'à des **observations** partielles de l'état sous-jacent.

Un POMDP (*Partially Observable MDP*) formalise cela comme un 7-uplet :

$$\langle \mathcal{S}, \mathcal{A}, \mathcal{O}, T, R, O, \gamma \rangle$$

| Symbole | Description | Rôle |
|:---|:---|:---|
| $\mathcal{S}$ | Espace des états | états réels, non observés directement |
| $\mathcal{A}$ | Espace des actions | actions disponibles pour l'agent |
| $\mathcal{O}$ | Espace des observations | ce que l'agent perçoit réellement |
| $T$ | Fonction de transition | $T(s' \mid s, a)$ |
| $R$ | Fonction de récompense | $R(s, a)$ |
| $O$ | Fonction d'observation | $O(o \mid s', a)$ |
| $\gamma \in [0,1]$ | Facteur d'actualisation | pondère les récompenses futures |

La différence avec un MDP tient aux deux éléments en bleu dans la formulation d'origine : l'espace d'observations $\mathcal{O}$ et la fonction d'observation $O$. L'agent ne reçoit jamais l'état vrai (seulement une observation) et doit donc maintenir une **croyance** (*belief*) sur l'état réel : une distribution de probabilité sur $\mathcal{S}$.

## L'exemple du bébé qui pleure

Le problème jouet classique pour illustrer un POMDP tient en deux états, deux actions et deux observations :

$$
\begin{align}
\mathcal{S} &= \{\text{affamé}, \text{rassasié}\}\\
\mathcal{A} &= \{\text{nourrir}, \text{ignorer}\}\\
\mathcal{O} &= \{\text{pleure}, \text{silencieux}\}
\end{align}
$$

On ne connaît jamais directement si le bébé est affamé : on ne fait qu'*entendre* s'il pleure ou non, et on doit décider de le nourrir ou de l'ignorer sur la base de cette seule observation (et de l'historique).

**Transition** $T(s' \mid s, a)$ : nourrir rassasie toujours le bébé ; l'ignorer le laisse devenir affamé avec une probabilité de 10 % s'il était rassasié, et il reste affamé s'il l'était déjà :

$$
\begin{align}
T(\text{rassasié} \mid s, \text{nourrir}) &= 100\% \quad \text{quel que soit } s\\
T(\text{affamé} \mid \text{affamé}, \text{ignorer}) &= 100\%\\
T(\text{affamé} \mid \text{rassasié}, \text{ignorer}) &= 10\%
\end{align}
$$

**Observation** $O(o \mid s')$ : un bébé affamé pleure 80 % du temps, un bébé rassasié ne pleure que 10 % du temps (les faux signaux sont donc possibles dans les deux sens) :

$$
\begin{align}
O(\text{pleure} \mid \text{affamé}) &= 80\%\\
O(\text{pleure} \mid \text{rassasié}) &= 10\%
\end{align}
$$

**Récompense** $R(s, a)$, additive : coût de $-10$ à chaque pas de temps où le bébé est affamé, plus un coût de $-5$ à chaque fois qu'on le nourrit (nourrir n'est jamais gratuit) :

$$R(s, a) = \underbrace{(-10 \text{ si } s=\text{affamé, sinon } 0)}_{\text{coût de laisser affamé}} + \underbrace{(-5 \text{ si } a=\text{nourrir, sinon } 0)}_{\text{coût de nourrir}}$$

Avec un facteur d'actualisation $\gamma = 0.9$ pour un horizon infini.

## La croyance et sa mise à jour

Puisque l'état réel est caché, la politique $\pi$ ne prend plus l'état en entrée mais la **croyance** $b$ :

$$\pi(s) = a \quad \text{(MDP)} \qquad\qquad \pi(b) = a \quad \text{(POMDP)}$$

Pour le bébé, $\mathbf{b} = [\,p(\text{affamé}),\; p(\text{rassasié})\,]$, un vecteur de probabilités non négatif qui somme à 1.

La mise à jour de croyance est un filtre bayésien classique : on **prédit** le nouvel état avec le modèle de transition, puis on **corrige** avec la vraisemblance de l'observation reçue, avant de renormaliser :

$$b'(s') \;\propto\; O(o \mid s', a) \sum_{s} T(s' \mid s, a)\, b(s)$$

Voici le déroulé numérique du notebook, en partant d'une croyance uniforme $b_0 = [0.5,\ 0.5]$ (recalculé à partir des probabilités ci-dessus) :

| Étape | Action | Observation | Croyance résultante $[p(\text{affamé}), p(\text{rassasié})]$ |
|:---:|:---|:---|:---|
| $b_0$ | / | / | $[0.500,\ 0.500]$ |
| $b_1$ | ignorer | pleure | $[0.907,\ 0.093]$ |
| $b_2$ | nourrir | silencieux | $[0.000,\ 1.000]$ |
| $b_3$ | ignorer | silencieux | $[0.024,\ 0.976]$ |
| $b_4$ | ignorer | silencieux | $[0.030,\ 0.970]$ |
| $b_5$ | ignorer | pleure | $[0.538,\ 0.462]$ |

Deux points intéressants : nourrir ($b_2$) fait retomber la croyance sur `rassasié` de façon *déterministe*, puisque le modèle de transition dit que nourrir rassasie toujours le bébé, indépendamment de l'observation reçue ensuite. Et à l'étape $b_5$, un seul signal de pleurs après plusieurs silences suffit à faire remonter la croyance vers `affamé`, mais seulement légèrement au-dessus de l'incertitude uniforme (0.538 contre 0.5) : la croyance accumulée pèse plus qu'une observation isolée.

## Résoudre un POMDP : vecteurs alpha

Comme l'état n'est plus connu exactement, l'utilité d'une croyance $b$ se calcule comme :

$$U(b) = \sum_s b(s)\, U(s) = \boldsymbol{\alpha}^\top \mathbf{b}$$

où $\boldsymbol{\alpha}$ est un **vecteur alpha** : l'utilité espérée pour chaque état sous-jacent, pour une action donnée. La politique optimale (ou approchée) devient alors un ensemble de vecteurs alpha, et choisir une action revient à trouver le vecteur qui maximise $\boldsymbol{\alpha}^\top \mathbf{b}$ pour la croyance courante.

Trois méthodes *hors-ligne* classiques, du plus simple au plus informé :

- **QMDP** : traite chaque état de croyance comme s'il était l'état vrai (ramenant le problème à un MDP), puis applique l'itération de la valeur :
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_{s'} T(s' \mid s, a) \max_{a'} \alpha_{a'}^{(k)}(s')$$
  Limite connue : QMDP ne « comprend » pas qu'une action puisse servir à *réduire l'incertitude* (il n'y a pas de terme d'information dans sa mise à jour).

- **FIB** (*Fast Informed Bound*) : utilise en plus le modèle d'observation, ce qui le rend plus informé que QMDP :
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_o \max_{a'} \sum_{s'} O(o \mid a,s')\, T(s' \mid s, a)\, \alpha_{a'}^{(k)}(s')$$

- **PBVI** (*Point-Based Value Iteration*) : au lieu de couvrir tout l'espace des croyances, opère sur un ensemble fini de $m$ croyances échantillonnées, avec un vecteur alpha associé à chacune ; c'est une borne inférieure de la fonction de valeur optimale, généralement plus précise que QMDP/FIB pour un coût de calcul raisonnable.

Pour la résolution *en ligne*, `POMDPs.jl` fournit **POMCP** (*Partially Observable Monte Carlo Planning*), qui construit un arbre de recherche guidé par UCT à partir de la croyance courante plutôt que de précalculer une politique complète, utile quand l'espace d'états est trop grand pour une résolution hors-ligne.

## Définition concise en Julia

Le notebook résume l'ensemble du problème en une définition `QuickPOMDP` compacte :

```julia
using POMDPs, POMDPModelTools, QuickPOMDPs

@enum State hungry full
@enum Action feed ignore
@enum Observation crying quiet

pomdp = QuickPOMDP(
    states       = [hungry, full],
    actions      = [feed, ignore],
    observations = [crying, quiet],
    initialstate = [full],
    discount     = 0.9,

    transition = function T(s, a)
        if a == feed
            return SparseCat([hungry, full], [0, 1])
        elseif s == hungry && a == ignore
            return SparseCat([hungry, full], [1, 0])
        elseif s == full && a == ignore
            return SparseCat([hungry, full], [0.1, 0.9])
        end
    end,

    observation = function O(s, a, s′)
        if s′ == hungry
            return SparseCat([crying, quiet], [0.8, 0.2])
        elseif s′ == full
            return SparseCat([crying, quiet], [0.1, 0.9])
        end
    end,

    reward = (s,a) -> (s == hungry ? -10 : 0) + (a == feed ? -5 : 0)
)

using QMDP
policy = solve(QMDPSolver(), pomdp)

𝐛 = [0.2, 0.8]        # croyance : p(hungry)=0.2, p(full)=0.8
a = action(policy, 𝐛)  # interroge la politique avec la croyance, pas l'état
```

## Un écho à mes propres recherches

Le formalisme de croyance des POMDP, qui consiste à maintenir une distribution de probabilité sur un état caché à partir d'observations bruitées, résonne directement avec un problème central en IA pour l'éducation : un tuteur (humain ou artificiel) n'observe jamais l'état de connaissance réel d'un apprenant, seulement des traces indirectes (réponses, temps de réponse, hésitations). C'est structurellement le même problème que le bébé qui pleure, avec un état latent nettement plus riche. Je n'ai pas encore creusé formellement cette piste dans mes travaux sur l'alignement pédagogique, mais le parallèle est trop net pour ne pas le noter ici.

## Pour aller plus loin

- Vidéo source : [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs) (Julia Academy)
- Notebook et code : [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty)
- M. Egorov, Z. N. Sunberg, E. Balaban, T. A. Wheeler, J. K. Gupta, M. J. Kochenderfer, « POMDPs.jl: A Framework for Sequential Decision Making under Uncertainty », *Journal of Machine Learning Research*, vol. 18, no. 26, 2017. [jmlr.org/papers/v18/16-300.html](http://jmlr.org/papers/v18/16-300.html)
- M. J. Kochenderfer, T. A. Wheeler, K. H. Wray, *Algorithms for Decision Making*, MIT Press, 2022. [algorithmsbook.com](https://algorithmsbook.com)
- M. Littman, A. Cassandra, L. Kaelbling, « Learning Policies for Partially Observable Environments: Scaling Up », *ICML*, 1995.
- D. Silver, J. Veness, « Monte-Carlo Planning in Large POMDPs », *NeurIPS*, 2010.

</div>

<div class="lang-en" markdown="1">

This post is a set of notes taken while watching [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs), a lecture from Julia Academy's *Decision Making Under Uncertainty* series (Robert Moss, Stanford University), which builds on the [`POMDPs.jl`](https://github.com/JuliaPOMDP/POMDPs.jl) ecosystem. The teaching example used (the *crying baby problem*) and the accompanying notebooks are available in the [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty) repository.

*A version without formulas or jargon is available here: [Deciding without knowing everything: what the "crying baby" teaches us about AI](/blog/2026/decider-sans-tout-savoir-bebe-qui-pleure/).*

## From MDPs to POMDPs

A Markov decision process (MDP) assumes the agent knows the system's state $s$ exactly at every instant. That is rarely true: most real systems (a robot with noisy sensors, a patient whose exact health state is unknown, or an intelligent tutor that can only *observe* a student's responses without knowing their true knowledge state) only give access to partial **observations** of the underlying state.

A POMDP (*Partially Observable MDP*) formalises this as a 7-tuple:

$$\langle \mathcal{S}, \mathcal{A}, \mathcal{O}, T, R, O, \gamma \rangle$$

| Symbol | Description | Role |
|:---|:---|:---|
| $\mathcal{S}$ | State space | true states, not directly observed |
| $\mathcal{A}$ | Action space | actions available to the agent |
| $\mathcal{O}$ | Observation space | what the agent actually perceives |
| $T$ | Transition function | $T(s' \mid s, a)$ |
| $R$ | Reward function | $R(s, a)$ |
| $O$ | Observation function | $O(o \mid s', a)$ |
| $\gamma \in [0,1]$ | Discount factor | weights future rewards |

The difference from an MDP lies in the two elements highlighted in the original formulation: the observation space $\mathcal{O}$ and the observation function $O$. The agent never receives the true state (only an observation) and must therefore maintain a **belief** over the true state: a probability distribution over $\mathcal{S}$.

## The crying baby example

The classic toy problem used to illustrate a POMDP has two states, two actions and two observations:

$$
\begin{align}
\mathcal{S} &= \{\text{hungry}, \text{full}\}\\
\mathcal{A} &= \{\text{feed}, \text{ignore}\}\\
\mathcal{O} &= \{\text{crying}, \text{quiet}\}
\end{align}
$$

We never know directly whether the baby is hungry: we only *hear* whether it is crying or not, and must decide whether to feed it or ignore it based solely on that observation (and the history so far).

**Transition** $T(s' \mid s, a)$: feeding always leaves the baby full; ignoring it lets it become hungry with a 10% probability if it was full, and it stays hungry if it already was:

$$
\begin{align}
T(\text{full} \mid s, \text{feed}) &= 100\% \quad \text{regardless of } s\\
T(\text{hungry} \mid \text{hungry}, \text{ignore}) &= 100\%\\
T(\text{hungry} \mid \text{full}, \text{ignore}) &= 10\%
\end{align}
$$

**Observation** $O(o \mid s')$: a hungry baby cries 80% of the time, a full baby only cries 10% of the time (so false signals are possible in both directions):

$$
\begin{align}
O(\text{crying} \mid \text{hungry}) &= 80\%\\
O(\text{crying} \mid \text{full}) &= 10\%
\end{align}
$$

**Reward** $R(s, a)$, additive: a cost of $-10$ at every time step the baby is hungry, plus a cost of $-5$ every time it is fed (feeding is never free):

$$R(s, a) = \underbrace{(-10 \text{ if } s=\text{hungry, else } 0)}_{\text{cost of leaving it hungry}} + \underbrace{(-5 \text{ if } a=\text{feed, else } 0)}_{\text{cost of feeding}}$$

With a discount factor $\gamma = 0.9$ over an infinite horizon.

## Belief and belief updating

Since the true state is hidden, the policy $\pi$ no longer takes the state as input but the **belief** $b$:

$$\pi(s) = a \quad \text{(MDP)} \qquad\qquad \pi(b) = a \quad \text{(POMDP)}$$

For the baby, $\mathbf{b} = [\,p(\text{hungry}),\; p(\text{full})\,]$, a non-negative probability vector summing to 1.

Belief updating is a classic Bayes filter: **predict** the new state with the transition model, then **correct** with the likelihood of the observation received, and renormalise:

$$b'(s') \;\propto\; O(o \mid s', a) \sum_{s} T(s' \mid s, a)\, b(s)$$

Here is the numerical trace from the notebook, starting from a uniform belief $b_0 = [0.5,\ 0.5]$ (recomputed from the probabilities above):

| Step | Action | Observation | Resulting belief $[p(\text{hungry}), p(\text{full})]$ |
|:---:|:---|:---|:---|
| $b_0$ | / | / | $[0.500,\ 0.500]$ |
| $b_1$ | ignore | crying | $[0.907,\ 0.093]$ |
| $b_2$ | feed | quiet | $[0.000,\ 1.000]$ |
| $b_3$ | ignore | quiet | $[0.024,\ 0.976]$ |
| $b_4$ | ignore | quiet | $[0.030,\ 0.970]$ |
| $b_5$ | ignore | crying | $[0.538,\ 0.462]$ |

Two interesting points: feeding ($b_2$) collapses the belief onto `full` *deterministically*, since the transition model states that feeding always leaves the baby full, regardless of the observation received afterwards. And at step $b_5$, a single crying signal after several quiet steps is enough to push the belief back towards `hungry`, but only slightly above uniform uncertainty (0.538 versus 0.5): the accumulated belief carries more weight than a single observation.

## Solving a POMDP: alpha vectors

Since the state is no longer known exactly, the utility of a belief $b$ is computed as:

$$U(b) = \sum_s b(s)\, U(s) = \boldsymbol{\alpha}^\top \mathbf{b}$$

where $\boldsymbol{\alpha}$ is an **alpha vector**: the expected utility for each underlying state, for a given action. The optimal (or approximate) policy then becomes a set of alpha vectors, and choosing an action amounts to finding the vector that maximises $\boldsymbol{\alpha}^\top \mathbf{b}$ for the current belief.

Three classic *offline* methods, from simplest to most informed:

- **QMDP**: treats each belief state as if it were the true state (reducing the problem to an MDP), then applies value iteration:
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_{s'} T(s' \mid s, a) \max_{a'} \alpha_{a'}^{(k)}(s')$$
  Known limitation: QMDP does not "understand" that an action can serve to *reduce uncertainty* (there is no information-gathering term in its update).

- **FIB** (*Fast Informed Bound*): additionally uses the observation model, making it more informed than QMDP:
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_o \max_{a'} \sum_{s'} O(o \mid a,s')\, T(s' \mid s, a)\, \alpha_{a'}^{(k)}(s')$$

- **PBVI** (*Point-Based Value Iteration*): instead of covering the entire belief space, operates on a finite set of $m$ sampled beliefs, each with an associated alpha vector; it is a lower bound on the optimal value function, generally more accurate than QMDP/FIB for a reasonable computational cost.

For *online* solving, `POMDPs.jl` provides **POMCP** (*Partially Observable Monte Carlo Planning*), which builds a UCT-guided search tree from the current belief rather than precomputing a full policy, useful when the state space is too large for offline solving.

## Concise definition in Julia

The notebook summarises the whole problem in a compact `QuickPOMDP` definition:

```julia
using POMDPs, POMDPModelTools, QuickPOMDPs

@enum State hungry full
@enum Action feed ignore
@enum Observation crying quiet

pomdp = QuickPOMDP(
    states       = [hungry, full],
    actions      = [feed, ignore],
    observations = [crying, quiet],
    initialstate = [full],
    discount     = 0.9,

    transition = function T(s, a)
        if a == feed
            return SparseCat([hungry, full], [0, 1])
        elseif s == hungry && a == ignore
            return SparseCat([hungry, full], [1, 0])
        elseif s == full && a == ignore
            return SparseCat([hungry, full], [0.1, 0.9])
        end
    end,

    observation = function O(s, a, s′)
        if s′ == hungry
            return SparseCat([crying, quiet], [0.8, 0.2])
        elseif s′ == full
            return SparseCat([crying, quiet], [0.1, 0.9])
        end
    end,

    reward = (s,a) -> (s == hungry ? -10 : 0) + (a == feed ? -5 : 0)
)

using QMDP
policy = solve(QMDPSolver(), pomdp)

𝐛 = [0.2, 0.8]        # belief: p(hungry)=0.2, p(full)=0.8
a = action(policy, 𝐛)  # query the policy with the belief, not the state
```

## An echo of my own research

The POMDP belief formalism, which consists in maintaining a probability distribution over a hidden state from noisy observations, resonates directly with a central problem in AI for education: a tutor (human or artificial) never observes a learner's true knowledge state, only indirect traces (answers, response times, hesitations). It is structurally the same problem as the crying baby, with a substantially richer latent state. I have not yet formally pursued this direction in my work on pedagogical alignment, but the parallel is too clear not to note here.

## Further reading

- Source video: [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs) (Julia Academy)
- Notebook and code: [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty)
- M. Egorov, Z. N. Sunberg, E. Balaban, T. A. Wheeler, J. K. Gupta, M. J. Kochenderfer, "POMDPs.jl: A Framework for Sequential Decision Making under Uncertainty," *Journal of Machine Learning Research*, vol. 18, no. 26, 2017. [jmlr.org/papers/v18/16-300.html](http://jmlr.org/papers/v18/16-300.html)
- M. J. Kochenderfer, T. A. Wheeler, K. H. Wray, *Algorithms for Decision Making*, MIT Press, 2022. [algorithmsbook.com](https://algorithmsbook.com)
- M. Littman, A. Cassandra, L. Kaelbling, "Learning Policies for Partially Observable Environments: Scaling Up," *ICML*, 1995.
- D. Silver, J. Veness, "Monte-Carlo Planning in Large POMDPs," *NeurIPS*, 2010.

</div>
