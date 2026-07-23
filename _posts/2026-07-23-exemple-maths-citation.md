---
layout: post
title: "Exemple de billet : mathématiques et citation bibliographique"
date: 2026-07-23 12:00:00+0200
description: Billet d'exemple montrant le rendu des formules mathématiques et une citation issue de la bibliographie.
tags: exemple maths
categories: exemples
related_posts: false
related_publications: true
---

Ce billet sert de gabarit : il montre comment écrire des formules mathématiques et comment citer une entrée de `_bibliography/papers.bib` dans un article du blog. [À COMPLÉTER — remplacer par un vrai billet, ou dupliquer ce fichier comme point de départ.]

## Mathématiques

Le thème rend les mathématiques avec le moteur [MathJax](https://www.mathjax.org/) (et non KaTeX — voir la note dans `MIGRATION.md`), mais la syntaxe d'entrée est la même syntaxe LaTeX à laquelle on est habitué avec KaTeX. Une formule en ligne s'écrit en l'entourant de `$$`, par exemple $$E = mc^2$$.

En mode « display », sur son propre paragraphe :

$$
\text{APed} = \frac{1}{n} \sum_{i=1}^{n} \mathbb{1}\!\left[ a_i \in \mathcal{A}_{\text{ped}} \right]
$$

## Citation bibliographique

On peut citer une entrée de la bibliographie avec la balise `{% raw %}{% cite %}{% endraw %}` fournie par [jekyll-scholar](https://github.com/inukshuk/jekyll-scholar), par exemple {% cite delamareXXXXjournal %}. Comme `related_publications: true` est activé dans l'en-tête de ce billet, les références citées apparaissent aussi en bas de page.
