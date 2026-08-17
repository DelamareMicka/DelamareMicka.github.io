---
layout: page
title: maquette pédagogique — programme IA
permalink: /curriculum-ia/
description: Carte interactive du parcours Intelligence Artificielle (CESI) — survolez une UE ou un ECUE pour afficher ses crédits, ses prérequis et ses objectifs pédagogiques.
nav: false
_styles: >
  .curric-intro { margin-bottom: 1.5rem; }
  .curric-panel {
    position: sticky;
    top: 70px;
    z-index: 5;
    background: var(--global-card-bg-color);
    border: 1px solid var(--global-divider-color);
    border-radius: 0.5rem;
    padding: 1rem 1.25rem;
    margin-bottom: 1.5rem;
    min-height: 5rem;
    max-height: 42vh;
    overflow-y: auto;
  }
  .curric-panel .placeholder { color: var(--global-text-color-light); font-style: italic; }
  .curric-panel .breadcrumb { font-size: 0.8rem; color: var(--global-text-color-light); text-transform: uppercase; letter-spacing: 0.03em; }
  .curric-panel h3 { margin: 0.25rem 0 0.5rem; }
  .curric-panel .meta { font-size: 0.85rem; color: var(--global-text-color-light); margin-bottom: 0.5rem; }
  .curric-panel .prereq { background: color-mix(in srgb, var(--global-theme-color) 8%, transparent); border-left: 3px solid var(--global-theme-color); padding: 0.5rem 0.75rem; margin: 0.5rem 0; border-radius: 0.25rem; }
  .curric-grid { display: flex; gap: 0.75rem; overflow-x: auto; padding-bottom: 0.75rem; }
  .curric-sem { flex: 0 0 15.5rem; display: flex; flex-direction: column; gap: 0.5rem; }
  .curric-sem-title { font-weight: 700; text-align: center; padding: 0.4rem; border-bottom: 2px solid var(--global-theme-color); margin-bottom: 0.25rem; }
  .curric-ue { border: 1px solid var(--global-divider-color); border-radius: 0.4rem; overflow: hidden; }
  .curric-ue-header { padding: 0.4rem 0.6rem; cursor: pointer; background: color-mix(in srgb, var(--global-theme-color) 6%, transparent); font-size: 0.85rem; line-height: 1.25; }
  .curric-ue-header .code { font-weight: 700; }
  .curric-ue-header .ects { float: right; opacity: 0.7; }
  .curric-ue-body { padding: 0.35rem; display: flex; flex-direction: column; gap: 0.3rem; }
  .curric-item { border: 1px solid transparent; border-radius: 0.3rem; padding: 0.3rem 0.5rem; font-size: 0.8rem; cursor: pointer; background: var(--global-bg-color); transition: border-color 0.15s, background 0.15s; }
  .curric-item:hover, .curric-item:focus, .curric-item.active { border-color: var(--global-theme-color); background: color-mix(in srgb, var(--global-theme-color) 10%, transparent); outline: none; }
  .curric-ue-header.active { background: color-mix(in srgb, var(--global-theme-color) 18%, transparent); }
  .curric-item .code { font-weight: 700; opacity: 0.7; margin-right: 0.25em; }
  .curric-item.prereq-highlight { border-color: #d9822b; background: color-mix(in srgb, #d9822b 16%, transparent); }
  .curric-panel .prereq-links { list-style: none; padding: 0; margin: 0.25rem 0 0.75rem; }
  .curric-panel .prereq-links li { display: inline-block; margin: 0.15rem 0.35rem 0.15rem 0; }
  .curric-panel .prereq-links a {
    display: inline-block; border: 1px solid #d9822b; color: #d9822b; border-radius: 999px;
    padding: 0.1rem 0.6rem; font-size: 0.78rem; text-decoration: none; cursor: pointer;
  }
  .curric-panel .prereq-links a:hover { background: color-mix(in srgb, #d9822b 16%, transparent); }
  .curric-panel .prereq-note { font-size: 0.75rem; color: var(--global-text-color-light); margin: -0.25rem 0 0.75rem; }
  [hidden] { display: none !important; }
---

Carte interactive du **parcours Intelligence Artificielle** que je pilote à CESI (6 semestres). Survolez — ou touchez sur mobile — une UE ou un ECUE pour afficher son détail dans le bandeau ci-dessous : crédits ECTS, volume horaire, prérequis et objectifs pédagogiques.

Contenu directement issu des fiches pédagogiques officielles (une fiche par UE). Les **prérequis affichés sont le texte tel qu'écrit dans chaque fiche**. En complément, les ECUE antérieurs susceptibles de couvrir ces prérequis sont **détectés automatiquement par rapprochement de mots-clés** entre ce texte et les titres des ECUE précédents, et mis en surbrillance <span style="color:#d9822b; font-weight:700;">orange</span> dans la grille — c'est une aide visuelle approximative, pas un lien officiel validé dans la maquette (les fiches ne codent pas ce lien explicitement). Volontairement absents de cette page : noms des enseignants et répartition horaire détaillée (CM/TD/TP), qui relèvent de la gestion interne du programme.

<div id="curric-panel" class="curric-panel">
  <p class="placeholder">Survolez une UE ou un ECUE pour voir son détail ici.</p>
</div>

<div class="curric-grid">
{% for sem in site.data.curriculum_ia.semesters %}
  <div class="curric-sem">
    <div class="curric-sem-title">Semestre {{ sem.number }}</div>
    {% for ue in sem.ues %}
      <div class="curric-ue">
        <div class="curric-item curric-ue-header" tabindex="0">
          <span class="code">UE {{ ue.code }}</span>
          <span class="ects">{{ ue.ects }} ECTS</span><br>
          {{ ue.title }}
          <div class="item-detail" hidden>
            <div class="breadcrumb">Semestre {{ sem.number }} · UE {{ ue.code }}</div>
            <h3>{{ ue.title }}</h3>
            <p class="meta">{{ ue.heures }} h · {{ ue.ects }} crédits ECTS</p>
            {% if ue.themes and ue.themes != "" %}<p>{{ ue.themes }}</p>{% endif %}
            {% if ue.competences.size > 0 %}
            <details>
              <summary>Compétences visées ({{ ue.competences.size }})</summary>
              <ul>
                {% for c in ue.competences %}<li>{{ c }}</li>{% endfor %}
              </ul>
            </details>
            {% endif %}
          </div>
        </div>
        <div class="curric-ue-body">
          {% for ecue in ue.ecues %}
            <div class="curric-item curric-ecue" tabindex="0" id="item-{{ ecue.code }}" data-code="{{ ecue.code }}" data-prereqs="{{ ecue.prereq_matches | join: ' ' }}">
              <span class="code">{{ ecue.code }}</span>{{ ecue.title }}
              <div class="item-detail" hidden>
                <div class="breadcrumb">Semestre {{ sem.number }} · UE {{ ue.code }} · ECUE {{ ecue.code }}</div>
                <h3>{{ ecue.title }}</h3>
                <p class="meta">{{ ecue.heures }} h{% if ecue.coefficient %} · coefficient interne {{ ecue.coefficient }}{% endif %}</p>
                <div class="prereq">
                  <strong>Prérequis :</strong>
                  {% if ecue.prerequis and ecue.prerequis != "" %}{{ ecue.prerequis }}{% else %}Aucun prérequis spécifique{% endif %}
                </div>
                {% if ecue.prereq_matches.size > 0 %}
                <p class="prereq-note">🔎 ECUE antérieurs détectés automatiquement comme couvrant probablement ces prérequis (à vérifier) :</p>
                <ul class="prereq-links">
                  {% for pc in ecue.prereq_matches %}<li><a href="#item-{{ pc }}" data-jump="{{ pc }}">{{ pc }}</a></li>{% endfor %}
                </ul>
                {% endif %}
                {% if ecue.objectifs.size > 0 %}
                <strong>Objectifs :</strong>
                <ul>
                  {% for o in ecue.objectifs %}<li>{{ o }}</li>{% endfor %}
                </ul>
                {% endif %}
              </div>
            </div>
          {% endfor %}
        </div>
      </div>
    {% endfor %}
  </div>
{% endfor %}
</div>

<script>
(function () {
  var panel = document.getElementById('curric-panel');
  var items = document.querySelectorAll('.curric-item');

  function clearHighlights() {
    items.forEach(function (i) {
      i.classList.remove('active');
      i.classList.remove('prereq-highlight');
    });
  }

  function reveal(item) {
    var detail = item.querySelector('.item-detail');
    if (!detail) return;
    panel.innerHTML = detail.innerHTML;
    clearHighlights();
    item.classList.add('active');

    var codes = (item.getAttribute('data-prereqs') || '').trim();
    if (codes) {
      codes.split(/\s+/).forEach(function (code) {
        var target = document.querySelector('.curric-ecue[data-code="' + code + '"]');
        if (target) target.classList.add('prereq-highlight');
      });
    }

    panel.querySelectorAll('[data-jump]').forEach(function (link) {
      link.addEventListener('click', function (e) {
        e.preventDefault();
        var jumpTo = document.getElementById('item-' + link.getAttribute('data-jump'));
        if (jumpTo) {
          jumpTo.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
          reveal(jumpTo);
        }
      });
    });
  }

  items.forEach(function (item) {
    item.addEventListener('mouseenter', function () { reveal(item); });
    item.addEventListener('focus', function () { reveal(item); });
    item.addEventListener('click', function () { reveal(item); });
  });
})();
</script>
