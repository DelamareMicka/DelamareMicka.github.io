---
layout: post
title: "Introduction aux POMDP : décider sous incertitude partielle avec le « bébé qui pleure »"
date: 2026-07-27 10:00:00+0200
description: Notes sur les processus de décision markoviens partiellement observables (POMDP) — le 7-uplet formel, la mise à jour de croyance bayésienne et les méthodes de résolution (QMDP, FIB, PBVI, POMCP), à partir de l'exemple pédagogique du « bébé qui pleure ».
tags: pomdp ia decision
categories: notes-de-lecture
related_posts: false
---

Ce billet est un ensemble de notes prises en regardant [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs), un cours de la série *Decision Making Under Uncertainty* de Julia Academy (Robert Moss, Stanford University), qui s'appuie sur l'écosystème [`POMDPs.jl`](https://github.com/JuliaPOMDP/POMDPs.jl). L'exemple pédagogique utilisé — le « bébé qui pleure » (*crying baby problem*) — et les notebooks associés sont disponibles sur le dépôt [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty).

## Des MDP aux POMDP

Un processus de décision markovien (MDP) suppose que l'agent connaît exactement l'état $s$ du système à chaque instant. C'est rarement vrai : la plupart des systèmes réels — un robot avec des capteurs bruités, un patient dont on ne connaît pas l'état de santé exact, ou un tuteur intelligent qui ne peut qu'*observer* les réponses d'un élève sans connaître son état de connaissance réel — ne donnent accès qu'à des **observations** partielles de l'état sous-jacent.

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

La différence avec un MDP tient aux deux éléments en bleu dans la formulation d'origine : l'espace d'observations $\mathcal{O}$ et la fonction d'observation $O$. L'agent ne reçoit jamais l'état vrai — seulement une observation — et doit donc maintenir une **croyance** (*belief*) sur l'état réel : une distribution de probabilité sur $\mathcal{S}$.

## L'exemple du bébé qui pleure

Le problème jouet classique pour illustrer un POMDP tient en deux états, deux actions et deux observations :

$$
\begin{align}
\mathcal{S} &= \{\text{affamé}, \text{rassasié}\}\\
\mathcal{A} &= \{\text{nourrir}, \text{ignorer}\}\\
\mathcal{O} &= \{\text{pleure}, \text{silencieux}\}
\end{align}
$$

On ne connaît jamais directement si le bébé est affamé — on ne fait qu'*entendre* s'il pleure ou non, et on doit décider de le nourrir ou de l'ignorer sur la base de cette seule observation (et de l'historique).

**Transition** $T(s' \mid s, a)$ — nourrir rassasie toujours le bébé ; l'ignorer le laisse devenir affamé avec une probabilité de 10 % s'il était rassasié, et il reste affamé s'il l'était déjà :

$$
\begin{align}
T(\text{rassasié} \mid s, \text{nourrir}) &= 100\% \quad \text{quel que soit } s\\
T(\text{affamé} \mid \text{affamé}, \text{ignorer}) &= 100\%\\
T(\text{affamé} \mid \text{rassasié}, \text{ignorer}) &= 10\%
\end{align}
$$

**Observation** $O(o \mid s')$ — un bébé affamé pleure 80 % du temps, un bébé rassasié ne pleure que 10 % du temps (les faux signaux sont donc possibles dans les deux sens) :

$$
\begin{align}
O(\text{pleure} \mid \text{affamé}) &= 80\%\\
O(\text{pleure} \mid \text{rassasié}) &= 10\%
\end{align}
$$

**Récompense** $R(s, a)$ — additive : coût de $-10$ à chaque pas de temps où le bébé est affamé, plus un coût de $-5$ à chaque fois qu'on le nourrit (nourrir n'est jamais gratuit) :

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
| $b_0$ | — | — | $[0.500,\ 0.500]$ |
| $b_1$ | ignorer | pleure | $[0.907,\ 0.093]$ |
| $b_2$ | nourrir | silencieux | $[0.000,\ 1.000]$ |
| $b_3$ | ignorer | silencieux | $[0.024,\ 0.976]$ |
| $b_4$ | ignorer | silencieux | $[0.030,\ 0.970]$ |
| $b_5$ | ignorer | pleure | $[0.537,\ 0.463]$ |

Deux points intéressants : nourrir ($b_2$) fait retomber la croyance sur `rassasié` de façon *déterministe*, puisque le modèle de transition dit que nourrir rassasie toujours le bébé — indépendamment de l'observation reçue ensuite. Et à l'étape $b_5$, un seul signal de pleurs après plusieurs silences suffit à faire remonter la croyance vers `affamé`, mais seulement légèrement au-dessus de l'incertitude uniforme (0.537 contre 0.5) : la croyance accumulée pèse plus qu'une observation isolée.

## Résoudre un POMDP : vecteurs alpha

Comme l'état n'est plus connu exactement, l'utilité d'une croyance $b$ se calcule comme :

$$U(b) = \sum_s b(s)\, U(s) = \boldsymbol{\alpha}^\top \mathbf{b}$$

où $\boldsymbol{\alpha}$ est un **vecteur alpha** : l'utilité espérée pour chaque état sous-jacent, pour une action donnée. La politique optimale (ou approchée) devient alors un ensemble de vecteurs alpha, et choisir une action revient à trouver le vecteur qui maximise $\boldsymbol{\alpha}^\top \mathbf{b}$ pour la croyance courante.

Trois méthodes *hors-ligne* classiques, du plus simple au plus informé :

- **QMDP** — traite chaque état de croyance comme s'il était l'état vrai (ramenant le problème à un MDP), puis applique l'itération de la valeur :
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_{s'} T(s' \mid s, a) \max_{a'} \alpha_{a'}^{(k)}(s')$$
  Limite connue : QMDP ne « comprend » pas qu'une action puisse servir à *réduire l'incertitude* (il n'y a pas de terme d'information dans sa mise à jour).

- **FIB** (*Fast Informed Bound*) — utilise en plus le modèle d'observation, ce qui le rend plus informé que QMDP :
  $$\alpha_a^{(k+1)}(s) = R(s,a) + \gamma\sum_o \max_{a'} \sum_{s'} O(o \mid a,s')\, T(s' \mid s, a)\, \alpha_{a'}^{(k)}(s')$$

- **PBVI** (*Point-Based Value Iteration*) — au lieu de couvrir tout l'espace des croyances, opère sur un ensemble fini de $m$ croyances échantillonnées, avec un vecteur alpha associé à chacune ; c'est une borne inférieure de la fonction de valeur optimale, généralement plus précise que QMDP/FIB pour un coût de calcul raisonnable.

Pour la résolution *en ligne*, `POMDPs.jl` fournit **POMCP** (*Partially Observable Monte Carlo Planning*), qui construit un arbre de recherche guidé par UCT à partir de la croyance courante plutôt que de précalculer une politique complète — utile quand l'espace d'états est trop grand pour une résolution hors-ligne.

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

Le formalisme de croyance des POMDP — maintenir une distribution de probabilité sur un état caché à partir d'observations bruitées — résonne directement avec un problème central en IA pour l'éducation : un tuteur (humain ou artificiel) n'observe jamais l'état de connaissance réel d'un apprenant, seulement des traces indirectes (réponses, temps de réponse, hésitations). C'est structurellement le même problème que le bébé qui pleure, avec un état latent nettement plus riche. Je n'ai pas encore creusé formellement cette piste dans mes travaux sur l'alignement pédagogique, mais le parallèle est trop net pour ne pas le noter ici.

## Pour aller plus loin

- Vidéo source : [*POMDPs: Partially Observable Markov Decision Processes*](https://www.youtube.com/watch?v=KDFzObtE6cs) (Julia Academy)
- Notebook et code : [JuliaAcademy/Decision-Making-Under-Uncertainty](https://github.com/JuliaAcademy/Decision-Making-Under-Uncertainty)
- M. Egorov, Z. N. Sunberg, E. Balaban, T. A. Wheeler, J. K. Gupta, M. J. Kochenderfer, « POMDPs.jl: A Framework for Sequential Decision Making under Uncertainty », *Journal of Machine Learning Research*, vol. 18, no. 26, 2017. [jmlr.org/papers/v18/16-300.html](http://jmlr.org/papers/v18/16-300.html)
- M. J. Kochenderfer, T. A. Wheeler, K. H. Wray, *Algorithms for Decision Making*, MIT Press, 2022. [algorithmsbook.com](https://algorithmsbook.com)
- M. Littman, A. Cassandra, L. Kaelbling, « Learning Policies for Partially Observable Environments: Scaling Up », *ICML*, 1995.
- D. Silver, J. Veness, « Monte-Carlo Planning in Large POMDPs », *NeurIPS*, 2010.
