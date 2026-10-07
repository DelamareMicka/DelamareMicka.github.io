---
layout: page
title: maquette pédagogique, programme IA
title_en: AI programme curriculum map
permalink: /curriculum-ia/
description: >
  <span class="lang-fr-i">Carte interactive du parcours Intelligence Artificielle (CESI) : survolez une UE ou un ECUE pour afficher ses crédits, ses prérequis et ses objectifs pédagogiques.</span><span class="lang-en-i">Interactive map of the Artificial Intelligence programme (CESI): hover over a UE or an ECUE to display its credits, prerequisites and learning objectives.</span>
nav: false
_styles: >
  .curric-intro { margin-bottom: 1.5rem; }
  /* Panel lives in its own column, beside the grid rather than stacked above it.
     A sticky/fixed panel stacked ABOVE a scrolling grid ends up covering grid
     rows once the page scrolls past it -- any card under that covered strip
     can no longer receive real hover events (or flickers at the boundary),
     which is what caused the freeze. Side-by-side columns never overlap. */
  .curric-layout { display: flex; gap: 1rem; align-items: flex-start; }
  .curric-main { flex: 1 1 auto; min-width: 0; }
  .curric-panel {
    flex: 0 0 19rem;
    width: 19rem;
    position: sticky;
    top: 76px;
    z-index: 5;
    background: var(--global-card-bg-color);
    border: 1px solid var(--global-divider-color);
    border-radius: 0.5rem;
    padding: 1rem 1.25rem;
    max-height: calc(100vh - 100px);
    overflow-y: auto;
  }
  @media (max-width: 900px) {
    .curric-layout { flex-direction: column; }
    .curric-panel { position: static; width: 100%; flex: none; max-height: 40vh; margin-bottom: 1rem; order: -1; }
  }
  .curric-panel .placeholder { color: var(--global-text-color-light); font-style: italic; }
  .curric-panel .breadcrumb { font-size: 0.8rem; color: var(--global-text-color-light); text-transform: uppercase; letter-spacing: 0.03em; }
  .curric-panel h3 { margin: 0.25rem 0 0.5rem; }
  .curric-panel .meta { font-size: 0.85rem; color: var(--global-text-color-light); margin-bottom: 0.5rem; }
  .curric-panel .prereq { background: color-mix(in srgb, var(--global-theme-color) 8%, transparent); border-left: 3px solid var(--global-theme-color); padding: 0.5rem 0.75rem; margin: 0.5rem 0; border-radius: 0.25rem; }
  .curric-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(15.5rem, 1fr)); gap: 0.75rem; }
  .curric-sem { display: flex; flex-direction: column; gap: 0.5rem; }
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
  .curric-item.unlocks-highlight { border-color: #1b9e96; background: color-mix(in srgb, #1b9e96 14%, transparent); }
  .curric-item.prereq-highlight.unlocks-highlight {
    border-color: #1b9e96;
    background: linear-gradient(90deg, color-mix(in srgb, #d9822b 16%, transparent) 50%, color-mix(in srgb, #1b9e96 14%, transparent) 50%);
  }
  .curric-ue-header .ue-fiche-hint { display: block; font-size: 0.72rem; opacity: 0.6; margin-top: 0.15rem; font-style: italic; }
  .curric-modal-backdrop {
    position: fixed; inset: 0; z-index: 2000; background: rgba(0, 0, 0, 0.55);
    display: flex; align-items: flex-start; justify-content: center;
    overflow-y: auto; padding: 3rem 1rem;
  }
  .curric-modal {
    position: relative; background: var(--global-card-bg-color); color: var(--global-text-color);
    border-radius: 0.6rem; max-width: 52rem; width: 100%; padding: 1.75rem 2rem 2rem;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  }
  .curric-modal-close {
    position: absolute; top: 0.6rem; right: 0.75rem; background: none; border: none;
    font-size: 1.6rem; line-height: 1; cursor: pointer; color: var(--global-text-color-light);
  }
  .curric-modal-close:hover { color: var(--global-text-color); }
  .curric-fiche-ecue { border-top: 1px solid var(--global-divider-color); margin-top: 1.25rem; padding-top: 1.25rem; }
  @media (max-width: 600px) {
    .curric-modal-backdrop { padding: 1rem 0.5rem; }
    .curric-modal { padding: 1.5rem 1.25rem 1.75rem; }
  }
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

<div class="lang-fr" markdown="1">

Carte interactive du **parcours Intelligence Artificielle** que je pilote à CESI (6 semestres). Survolez (ou touchez sur mobile) une UE ou un ECUE pour afficher son détail dans le panneau (à droite sur grand écran, en haut sur mobile) : crédits ECTS, volume horaire, prérequis et objectifs pédagogiques. Un clic fixe la sélection : elle reste affichée, prérequis et dépendances compris, même en bougeant la souris ou en faisant défiler la page, jusqu'au prochain clic.

Contenu directement issu des fiches pédagogiques officielles (une fiche par UE). Les **prérequis affichés sont le texte tel qu'écrit dans chaque fiche**. En complément, les ECUE antérieurs susceptibles de couvrir ces prérequis sont **détectés automatiquement par rapprochement de mots-clés** entre ce texte et les titres des ECUE précédents, et mis en surbrillance <span style="color:#d9822b; font-weight:700;">orange</span> dans la grille : c'est une aide visuelle approximative, pas un lien officiel validé dans la maquette (les fiches ne codent pas ce lien explicitement). Au clic (ou au survol), les ECUE plus tardifs qui s'appuient à leur tour sur l'élément sélectionné sont mis en surbrillance <span style="color:#1b9e96; font-weight:700;">turquoise</span> : c'est la relation inverse, ce que l'élément sélectionné permet d'aborder ensuite. Cliquer sur le bandeau d'une UE ouvre sa fiche complète (UE et tous ses ECUE) dans une fenêtre dédiée. Volontairement absents de cette page : noms des enseignants et répartition horaire détaillée (CM/TD/TP), qui relèvent de la gestion interne du programme.

</div>
<div class="lang-en" markdown="1">

Interactive map of the **Artificial Intelligence programme** that I run at CESI (6 semesters). Hover (or tap on mobile) over a UE (teaching unit) or an ECUE (course component) to display its detail in the panel (on the right on large screens, at the top on mobile): ECTS credits, hours, prerequisites and learning objectives. A click locks the selection in place: it stays displayed, prerequisites and dependents included, even if you move the mouse or scroll the page, until the next click.

Content taken directly from the official course specification sheets (one sheet per UE). The **prerequisites shown are the text exactly as written in each sheet**. In addition, earlier ECUEs likely to cover these prerequisites are **automatically detected by keyword matching** between this text and the titles of previous ECUEs, and highlighted in <span style="color:#d9822b; font-weight:700;">orange</span> in the grid: this is an approximate visual aid, not an official link validated in the curriculum (the sheets do not encode this link explicitly). On click (or hover), later ECUEs that in turn rely on the selected item are highlighted in <span style="color:#1b9e96; font-weight:700;">turquoise</span>: this is the reverse relationship, what the selected item leads to next. Clicking a UE's header opens its full sheet (the UE and all its ECUEs) in a dedicated window. Deliberately absent from this page: teacher names and detailed hourly breakdown (lecture/tutorial/lab), which fall under the internal management of the programme.

</div>

<div class="curric-layout">
<div class="curric-main">
<div class="curric-grid">
{% for sem in site.data.curriculum_ia.semesters %}
  <div class="curric-sem">
    <div class="curric-sem-title"><span class="lang-fr-i">Semestre {{ sem.number }}</span><span class="lang-en-i">Semester {{ sem.number }}</span></div>
    {% for ue in sem.ues %}
      <div class="curric-ue">
        <div class="curric-item curric-ue-header" tabindex="0">
          <span class="code">UE {{ ue.code }}</span>
          <span class="ects">{{ ue.ects }} ECTS</span><br>
          <span class="lang-fr-i">{{ ue.title }}</span><span class="lang-en-i">{{ ue.title_en | default: ue.title }}</span>
          <span class="ue-fiche-hint"><span class="lang-fr-i">Cliquer pour la fiche complète</span><span class="lang-en-i">Click for the full sheet</span></span>
          <div class="item-detail" hidden>
            <div class="breadcrumb"><span class="lang-fr-i">Semestre {{ sem.number }} · UE {{ ue.code }}</span><span class="lang-en-i">Semester {{ sem.number }} · UE {{ ue.code }}</span></div>
            <h3><span class="lang-fr-i">{{ ue.title }}</span><span class="lang-en-i">{{ ue.title_en | default: ue.title }}</span></h3>
            <p class="meta">{{ ue.heures }} h · {{ ue.ects }} <span class="lang-fr-i">crédits ECTS</span><span class="lang-en-i">ECTS credits</span></p>
            {% if ue.themes and ue.themes != "" %}<p><span class="lang-fr-i">{{ ue.themes }}</span><span class="lang-en-i">{{ ue.themes_en | default: ue.themes }}</span></p>{% endif %}
            {% if ue.competences.size > 0 %}
            <details>
              <summary><span class="lang-fr-i">Compétences visées ({{ ue.competences.size }})</span><span class="lang-en-i">Skills targeted ({{ ue.competences.size }})</span></summary>
              <ul>
                {% for c in ue.competences %}<li><span class="lang-fr-i">{{ c }}</span><span class="lang-en-i">{{ ue.competences_en[forloop.index0] | default: c }}</span></li>{% endfor %}
              </ul>
            </details>
            {% endif %}
          </div>
        </div>
        <div class="curric-ue-body">
          {% for ecue in ue.ecues %}
            <div class="curric-item curric-ecue" tabindex="0" id="item-{{ ecue.code }}" data-code="{{ ecue.code }}" data-prereqs="{{ ecue.prereq_matches | join: ' ' }}">
              <span class="code">{{ ecue.code }}</span><span class="lang-fr-i">{{ ecue.title }}</span><span class="lang-en-i">{{ ecue.title_en | default: ecue.title }}</span>
              <div class="item-detail" hidden>
                <div class="breadcrumb"><span class="lang-fr-i">Semestre {{ sem.number }} · UE {{ ue.code }} · ECUE {{ ecue.code }}</span><span class="lang-en-i">Semester {{ sem.number }} · UE {{ ue.code }} · ECUE {{ ecue.code }}</span></div>
                <h3><span class="lang-fr-i">{{ ecue.title }}</span><span class="lang-en-i">{{ ecue.title_en | default: ecue.title }}</span></h3>
                <p class="meta">{{ ecue.heures }} h{% if ecue.coefficient %} · <span class="lang-fr-i">coefficient interne {{ ecue.coefficient }}</span><span class="lang-en-i">internal coefficient {{ ecue.coefficient }}</span>{% endif %}</p>
                <div class="prereq">
                  <strong><span class="lang-fr-i">Prérequis :</span><span class="lang-en-i">Prerequisites:</span></strong>
                  {% if ecue.prerequis and ecue.prerequis != "" %}<span class="lang-fr-i">{{ ecue.prerequis }}</span><span class="lang-en-i">{{ ecue.prerequis_en | default: ecue.prerequis }}</span>{% else %}<span class="lang-fr-i">Aucun prérequis spécifique</span><span class="lang-en-i">No specific prerequisites</span>{% endif %}
                </div>
                {% if ecue.prereq_matches.size > 0 %}
                <p class="prereq-note">🔎 <span class="lang-fr-i">ECUE antérieurs détectés automatiquement comme couvrant probablement ces prérequis (à vérifier) :</span><span class="lang-en-i">Earlier ECUEs automatically detected as likely covering these prerequisites (to be verified):</span></p>
                <ul class="prereq-links">
                  {% for pc in ecue.prereq_matches %}<li><a href="#item-{{ pc }}" data-jump="{{ pc }}">{{ pc }}</a></li>{% endfor %}
                </ul>
                {% endif %}
                {% if ecue.objectifs.size > 0 %}
                <strong><span class="lang-fr-i">Objectifs :</span><span class="lang-en-i">Objectives:</span></strong>
                <ul>
                  {% for o in ecue.objectifs %}<li><span class="lang-fr-i">{{ o }}</span><span class="lang-en-i">{{ ecue.objectifs_en[forloop.index0] | default: o }}</span></li>{% endfor %}
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
</div>
<div id="curric-panel" class="curric-panel">
  <p class="placeholder"><span class="lang-fr-i">Survolez une UE ou un ECUE pour voir son détail ici.</span><span class="lang-en-i">Hover over a UE or an ECUE to see its detail here.</span></p>
</div>
</div>

<div id="curric-modal-backdrop" class="curric-modal-backdrop" hidden>
  <div class="curric-modal" role="dialog" aria-modal="true">
    <button type="button" class="curric-modal-close" id="curric-modal-close" aria-label="Fermer / Close">&times;</button>
    <div id="curric-modal-body"></div>
  </div>
</div>

<script>
(function () {
  var panel = document.getElementById('curric-panel');
  var items = Array.prototype.slice.call(document.querySelectorAll('.curric-item'));

  // Build a lookup once instead of re-querying the DOM for every prereq code on every hover.
  var byCode = Object.create(null);
  items.forEach(function (i) {
    var code = i.getAttribute('data-code');
    if (code) byCode[code] = i;
  });

  // Reverse index: code -> items that list this code among their prereq matches,
  // i.e. what becomes reachable once the item with that code is covered.
  var dependents = Object.create(null);
  items.forEach(function (i) {
    var codes = (i.getAttribute('data-prereqs') || '').trim();
    if (!codes) return;
    codes.split(/\s+/).forEach(function (code) {
      (dependents[code] = dependents[code] || []).push(i);
    });
  });

  var highlighted = [];
  var unlocked = [];
  var activeItem = null;
  // A click pins the selection: it stays put through later mouse movement or
  // scrolling, and only another click moves it. Hover-preview (for anyone who
  // hasn't clicked anything yet) stays free to follow the cursor as before.
  var pinned = false;

  function clearHighlights() {
    if (activeItem) activeItem.classList.remove('active');
    highlighted.forEach(function (i) { i.classList.remove('prereq-highlight'); });
    highlighted.length = 0;
    unlocked.forEach(function (i) { i.classList.remove('unlocks-highlight'); });
    unlocked.length = 0;
    activeItem = null;
  }

  function reveal(item) {
    var detail = item.querySelector('.item-detail');
    if (!detail) return;
    panel.innerHTML = detail.innerHTML;
    clearHighlights();
    item.classList.add('active');
    activeItem = item;

    var codes = (item.getAttribute('data-prereqs') || '').trim();
    if (codes) {
      codes.split(/\s+/).forEach(function (code) {
        var target = byCode[code];
        if (target) {
          target.classList.add('prereq-highlight');
          highlighted.push(target);
        }
      });
    }

    var myCode = item.getAttribute('data-code');
    if (myCode && dependents[myCode]) {
      dependents[myCode].forEach(function (dep) {
        dep.classList.add('unlocks-highlight');
        unlocked.push(dep);
      });
    }
  }

  // Event delegation: one click listener on the panel instead of re-binding on every reveal().
  panel.addEventListener('click', function (e) {
    var link = e.target.closest('[data-jump]');
    if (!link) return;
    e.preventDefault();
    var jumpTo = byCode[link.getAttribute('data-jump')];
    if (jumpTo) {
      jumpTo.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
      reveal(jumpTo);
    }
  });

  // Debounce hover: while the cursor sweeps across many cards, only the one it
  // settles on for a moment triggers the (costlier) panel/highlight update.
  var pending = null;
  function scheduleReveal(item) {
    if (pending) clearTimeout(pending);
    pending = setTimeout(function () { reveal(item); }, 60);
  }

  // Scrolling the page with the cursor held still fires mouseenter on whatever
  // card now sits under it, as if it had been hovered on purpose, which wiped
  // out a clicked selection's highlight mid-scroll. Only treat mouseenter as a
  // real hover when the pointer has actually moved since the last time we checked.
  var lastMouseX = null;
  var lastMouseY = null;
  document.addEventListener('mousemove', function (e) {
    lastMouseX = e.clientX;
    lastMouseY = e.clientY;
  });

  items.forEach(function (item) {
    item.addEventListener('mouseenter', function (e) {
      if (pinned) return;
      if (e.clientX === lastMouseX && e.clientY === lastMouseY) return;
      scheduleReveal(item);
    });
    item.addEventListener('focus', function () {
      if (pinned) return;
      scheduleReveal(item);
    });
    item.addEventListener('click', function () {
      if (pending) clearTimeout(pending);
      pinned = true;
      reveal(item);
      if (item.classList.contains('curric-ue-header')) openFiche(item);
    });
  });

  // Fiche UE modal: assembled from the already-rendered detail markup of the
  // UE header itself plus every ECUE underneath it, no extra templating needed.
  var modalBackdrop = document.getElementById('curric-modal-backdrop');
  var modalBody = document.getElementById('curric-modal-body');
  var modalClose = document.getElementById('curric-modal-close');
  var lastFocused = null;

  function openFiche(ueHeader) {
    var ueDetail = ueHeader.querySelector('.item-detail');
    if (!ueDetail) return;
    var ueBlock = ueHeader.closest('.curric-ue');
    var html = '<div class="curric-fiche-ue">' + ueDetail.innerHTML + '</div>';
    if (ueBlock) {
      Array.prototype.forEach.call(ueBlock.querySelectorAll('.curric-ue-body > .curric-ecue'), function (ecue) {
        var d = ecue.querySelector('.item-detail');
        if (d) html += '<div class="curric-fiche-ecue">' + d.innerHTML + '</div>';
      });
    }
    modalBody.innerHTML = html;
    lastFocused = document.activeElement;
    modalBackdrop.hidden = false;
    modalClose.focus();
  }

  function closeFiche() {
    modalBackdrop.hidden = true;
    modalBody.innerHTML = '';
    if (lastFocused && lastFocused.focus) lastFocused.focus();
  }

  modalClose.addEventListener('click', closeFiche);
  modalBackdrop.addEventListener('click', function (e) {
    if (e.target === modalBackdrop) closeFiche();
  });
  document.addEventListener('keydown', function (e) {
    if (e.key === 'Escape' && !modalBackdrop.hidden) closeFiche();
  });
})();
</script>
