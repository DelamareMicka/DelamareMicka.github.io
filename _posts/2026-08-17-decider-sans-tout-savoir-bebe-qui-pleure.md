---
layout: post
title: "Décider sans tout savoir : ce que le « bébé qui pleure » nous apprend sur l'IA"
date: 2026-08-17 10:00:00+0200
description: Version accessible (sans formules) de mon billet sur les POMDP — comment une intelligence artificielle peut prendre de bonnes décisions même quand elle ne voit jamais la situation exacte.
tags: ia decision vulgarisation
categories: notes-de-lecture
related_posts: false
---

Imaginez que vous gardez un bébé qui dort dans la chambre à côté. Vous n'avez pas de caméra, juste vos oreilles : vous entendez s'il pleure ou s'il est silencieux, un point c'est tout. Vous ne pouvez jamais *voir* directement s'il a faim ou s'il est rassasié — vous devez le deviner à partir de ce que vous entendez, et décider s'il faut aller le nourrir ou continuer ce que vous étiez en train de faire.

C'est un problème d'une banalité totale. Et c'est aussi, formalisé, l'un des problèmes centraux de l'intelligence artificielle : **comment décider correctement quand on n'a jamais un accès direct à la vérité, seulement des indices ?**

## Le piège : les indices mentent parfois

Le premier réflexe serait de se dire : « s'il pleure, je le nourris ; s'il est silencieux, je ne fais rien. » Simple, non ?

Le problème, c'est que les pleurs ne sont pas un signal parfait. Un bébé affamé pleure souvent, mais pas toujours — parfois il patiente en silence. Et un bébé qui vient d'être nourri peut quand même pleurer un peu, pour d'autres raisons. Réagir uniquement au dernier bruit entendu, c'est se faire piéger par ces faux signaux.

Ce qu'un bon gardien fait instinctivement — et ce qu'une IA doit faire explicitement — c'est **construire une intuition qui se met à jour au fil du temps**, plutôt que de réagir bêtement au dernier indice.

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

- **Avoir un plan préparé à l'avance.** Comme un joueur qui a mémorisé un livre d'ouvertures : pour chaque situation possible, il sait déjà quoi jouer, sans réfléchir sur le moment. C'est rapide à exécuter, mais ça demande d'avoir fait tout le travail de préparation en amont — et certaines méthodes de préparation sont plus fines que d'autres (certaines « comprennent » qu'une action peut servir à en apprendre plus, d'autres pas).
- **Réfléchir sur le moment, à partir de la situation actuelle.** Comme un joueur qui simule mentalement plusieurs coups à l'avance avant de jouer, sans avoir tout mémorisé. Plus lent à chaque décision, mais ça permet de gérer des situations bien trop nombreuses pour être toutes préparées à l'avance.

Les deux approches existent dans les outils que les chercheurs utilisent pour ce genre de problème, et le choix dépend surtout de la taille du problème à résoudre.

## Pourquoi c'est important

Ce petit problème du bébé qui pleure est un cas d'école, mais le même principe gouverne des situations bien plus sérieuses : une voiture autonome qui doit deviner si l'ombre au bord de la route est un piéton immobile ou un simple poteau, un système médical qui doit estimer l'état d'un patient à partir de symptômes ambigus, ou — ce qui me touche directement dans mes recherches — **un tuteur, humain ou IA, qui doit deviner ce qu'un élève sait vraiment**, alors qu'il n'a accès qu'à des indices indirects : ses réponses, son temps de réflexion, ses hésitations.

Dans les trois cas, la vérité reste cachée. Ce qui change, c'est la qualité de l'intuition qu'on parvient à construire à partir des indices disponibles — et la sagesse de ne jamais lui faire dire plus qu'elle ne sait vraiment.

*Pour la version avec les formules, le code et les détails techniques (dont l'exemple ci-dessus est tiré), voir [Introduction aux POMDP : décider sous incertitude partielle avec le « bébé qui pleure »](/blog/2026/introduction-pomdp-bebe-qui-pleure/).*
