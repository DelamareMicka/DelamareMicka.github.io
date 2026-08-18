---
layout: post
title: "Décider sans tout savoir : ce que le « bébé qui pleure » nous apprend sur l'IA"
title_en: "Deciding without knowing everything: what the “crying baby” teaches us about AI"
date: 2026-08-17 10:00:00+0200
description: >
  <span class="lang-fr-i">Version accessible (sans formules) de mon billet sur les POMDP : comment une intelligence artificielle peut prendre de bonnes décisions même quand elle ne voit jamais la situation exacte.</span><span class="lang-en-i">Accessible version (no formulas) of my POMDP post: how an artificial intelligence can make good decisions even when it never sees the exact situation.</span>
tags: ia decision vulgarisation
categories: notes-de-lecture
related_posts: false
---

<div class="lang-fr" markdown="1">

Imaginez que vous gardez un bébé qui dort dans la chambre à côté. Vous n'avez pas de caméra, juste vos oreilles : vous entendez s'il pleure ou s'il est silencieux, un point c'est tout. Vous ne pouvez jamais *voir* directement s'il a faim ou s'il est rassasié : vous devez le deviner à partir de ce que vous entendez, et décider s'il faut aller le nourrir ou continuer ce que vous étiez en train de faire.

C'est un problème d'une banalité totale. Et c'est aussi, formalisé, l'un des problèmes centraux de l'intelligence artificielle : **comment décider correctement quand on n'a jamais un accès direct à la vérité, seulement des indices ?**

## Le piège : les indices mentent parfois

Le premier réflexe serait de se dire : « s'il pleure, je le nourris ; s'il est silencieux, je ne fais rien. » Simple, non ?

Le problème, c'est que les pleurs ne sont pas un signal parfait. Un bébé affamé pleure souvent, mais pas toujours : parfois il patiente en silence. Et un bébé qui vient d'être nourri peut quand même pleurer un peu, pour d'autres raisons. Réagir uniquement au dernier bruit entendu, c'est se faire piéger par ces faux signaux.

Ce qu'un bon gardien fait instinctivement, et ce qu'une IA doit faire explicitement, c'est **construire une intuition qui se met à jour au fil du temps**, plutôt que de réagir bêtement au dernier indice.

## Une intuition qui évolue

Imaginons qu'on parte d'une incertitude totale : 50 % de chances que le bébé ait faim, 50 % qu'il soit rassasié. Puis les événements s'enchaînent :

1. **On ignore, et le bébé pleure.** Notre soupçon qu'il ait faim grimpe fort : environ 90 %.
2. **On le nourrit, et il se tait.** Là, plus de doute : on *sait* qu'il est rassasié (nourrir résout toujours le problème), notre intuition retombe à 0 % de faim.
3. **On ignore, et il reste silencieux.** Léger regain de doute au fil du temps (un bébé rassasié peut redevenir affamé), mais on reste confiant : autour de 2 à 3 % de chances qu'il ait faim.
4. **On continue d'ignorer, toujours silencieux.** Le doute progresse un peu, tranquillement.
5. **On ignore encore, et là il pleure.** Un seul pleur après plusieurs silences ne suffit pas à tout renverser d'un coup : notre soupçon remonte à peine au-dessus de 50 % (53 %). L'historique accumulé pèse plus lourd qu'un signal isolé.

C'est ça, l'idée centrale : on ne remplace jamais notre intuition par le dernier indice, on la *met à jour* avec lui, en tenant compte de tout ce qu'on savait déjà. C'est exactement ce que fait un GPS quand le signal satellite faiblit une seconde : il ne se perd pas d'un coup, il continue d'estimer la position à partir de sa dernière intuition solide.

## Improviser sur le moment, ou avoir déjà tout prévu ?

Une fois qu'on sait maintenir cette intuition, il reste une question : comment décider quoi faire à chaque instant ?

Il y a deux grandes familles de réponses, et elles ressemblent à deux façons de jouer aux échecs :

- **Avoir un plan préparé à l'avance.** Comme un joueur qui a mémorisé un livre d'ouvertures : pour chaque situation possible, il sait déjà quoi jouer, sans réfléchir sur le moment. C'est rapide à exécuter, mais ça demande d'avoir fait tout le travail de préparation en amont, et certaines méthodes de préparation sont plus fines que d'autres (certaines « comprennent » qu'une action peut servir à en apprendre plus, d'autres pas).
- **Réfléchir sur le moment, à partir de la situation actuelle.** Comme un joueur qui simule mentalement plusieurs coups à l'avance avant de jouer, sans avoir tout mémorisé. Plus lent à chaque décision, mais ça permet de gérer des situations bien trop nombreuses pour être toutes préparées à l'avance.

Les deux approches existent dans les outils que les chercheurs utilisent pour ce genre de problème, et le choix dépend surtout de la taille du problème à résoudre.

## Pourquoi c'est important

Ce petit problème du bébé qui pleure est un cas d'école, mais le même principe gouverne des situations bien plus sérieuses : une voiture autonome qui doit deviner si l'ombre au bord de la route est un piéton immobile ou un simple poteau, un système médical qui doit estimer l'état d'un patient à partir de symptômes ambigus, ou, ce qui me touche directement dans mes recherches, **un tuteur, humain ou IA, qui doit deviner ce qu'un élève sait vraiment**, alors qu'il n'a accès qu'à des indices indirects : ses réponses, son temps de réflexion, ses hésitations.

Dans les trois cas, la vérité reste cachée. Ce qui change, c'est la qualité de l'intuition qu'on parvient à construire à partir des indices disponibles, et la sagesse de ne jamais lui faire dire plus qu'elle ne sait vraiment.

*Pour la version avec les formules, le code et les détails techniques (dont l'exemple ci-dessus est tiré), voir [Introduction aux POMDP : décider sous incertitude partielle avec le « bébé qui pleure »](/blog/2026/introduction-pomdp-bebe-qui-pleure/).*

</div>

<div class="lang-en" markdown="1">

Imagine you are looking after a baby sleeping in the next room. You have no camera, just your ears: you hear whether it is crying or quiet, and that's it. You can never *see* directly whether it is hungry or full: you have to guess from what you hear, and decide whether to go feed it or carry on with what you were doing.

This is about as ordinary a problem as it gets. And, once formalised, it is also one of the central problems in artificial intelligence: **how do you decide correctly when you never have direct access to the truth, only clues?**

## The trap: clues sometimes lie

The first instinct would be: "if it's crying, I feed it; if it's quiet, I do nothing." Simple, right?

The trouble is that crying is not a perfect signal. A hungry baby cries often, but not always: sometimes it waits quietly. And a baby that has just been fed can still cry a little, for other reasons. Reacting only to the last sound you heard means falling for these false signals.

What a good caregiver does instinctively, and what an AI must do explicitly, is **build an intuition that updates over time**, rather than blindly reacting to the latest clue.

## An intuition that evolves

Suppose we start from total uncertainty: 50% chance the baby is hungry, 50% chance it is full. Then events unfold:

1. **We ignore it, and the baby cries.** Our suspicion that it's hungry jumps sharply: roughly 90%.
2. **We feed it, and it goes quiet.** Now there's no more doubt: we *know* it's full (feeding always fixes the problem), our intuition drops back to 0% hunger.
3. **We ignore it, and it stays quiet.** A slight rise in doubt over time (a full baby can become hungry again), but we remain confident: around 2 to 3% chance it's hungry.
4. **We keep ignoring it, still quiet.** Doubt creeps up a little further, quietly.
5. **We ignore it again, and this time it cries.** A single cry after several quiet periods isn't enough to flip everything at once: our suspicion barely rises above 50% (53%). The accumulated history carries more weight than a single signal.

That's the central idea: we never replace our intuition with the latest clue, we *update* it with that clue, taking into account everything we already knew. It's exactly what a GPS does when the satellite signal drops out for a second: it doesn't lose itself instantly, it keeps estimating position from its last solid intuition.

## Improvising on the spot, or having it all planned out?

Once we know how to maintain this intuition, one question remains: how do we decide what to do at each moment?

There are two broad families of answers, and they resemble two ways of playing chess:

- **Having a plan prepared in advance.** Like a player who has memorised an opening book: for every possible situation, they already know what to play, without thinking on the spot. It's fast to execute, but it requires doing all the preparation work upfront, and some preparation methods are more refined than others (some "understand" that an action can be used to learn more, others don't).
- **Thinking on the spot, from the current situation.** Like a player who mentally simulates several moves ahead before playing, without having memorised everything. Slower for each decision, but it lets you handle far too many situations to all be prepared in advance.

Both approaches exist in the tools researchers use for this kind of problem, and the choice mostly depends on the size of the problem being solved.

## Why it matters

This little crying-baby problem is a textbook case, but the same principle governs much more serious situations: a self-driving car that has to guess whether the shadow at the roadside is a stationary pedestrian or just a pole, a medical system that has to estimate a patient's condition from ambiguous symptoms, or, what touches me directly in my own research, **a tutor, human or AI, that has to guess what a student actually knows**, when it only has access to indirect clues: their answers, their response time, their hesitations.

In all three cases, the truth stays hidden. What changes is the quality of the intuition we manage to build from the available clues, and the wisdom never to let it claim to know more than it really does.

*For the version with formulas, code and technical detail (from which the example above is drawn), see [Introduction to POMDPs: deciding under partial observability with the "crying baby" problem](/blog/2026/introduction-pomdp-bebe-qui-pleure/).*

</div>
