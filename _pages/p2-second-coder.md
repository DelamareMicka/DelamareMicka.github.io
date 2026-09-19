---
layout: page
title: "P2 — codage relationnel indépendant"
title_en: "P2 — independent relational coding"
permalink: /p2-second-coder/
description: >
  <span class="lang-fr-i">Outil de codage pour le second évaluateur indépendant (Pending Analysis P2, article "Constructive Alignment as a Relational Property of AI-Supported Learning Design").</span><span class="lang-en-i">Coding tool for the independent second rater (Pending Analysis P2, "Constructive Alignment as a Relational Property of AI-Supported Learning Design").</span>
nav: false
render_with_liquid: false
---

<div class="p2-coder-tool">
<style>
  .p2-coder-tool{
    --ink:#181d17;
    --ink-soft:#4b5245;
    --paper:#f7f5ee;
    --paper-2:#edeadd;
    --card:#fffef9;
    --line:#dcd6bf;
    --accent:#3f6b4a;
    --accent-ink:#f3f7f0;
    --accent-soft:#e3ecdf;
    --warn:#9c4a26;
    --warn-soft:#f4e3d7;
    --focus:#2f5a8f;
    --shadow: 0 1px 2px rgba(24,29,23,0.06), 0 6px 18px -10px rgba(24,29,23,0.18);

    display:block;
    background:var(--paper);
    color:var(--ink);
    font-family:'Source Sans 3', system-ui, -apple-system, "Segoe UI", sans-serif;
    line-height:1.5;
    padding:20px 16px 60px;
    border-radius:14px;
  }
  @media (prefers-color-scheme: dark){
    .p2-coder-tool{
      --ink:#eef0e6;
      --ink-soft:#b7bdad;
      --paper:#14170f;
      --paper-2:#1c2016;
      --card:#1a1e15;
      --line:#343a2b;
      --accent:#7fb888;
      --accent-ink:#0f1a10;
      --accent-soft:#233223;
      --warn:#e0996f;
      --warn-soft:#3a2418;
      --focus:#8fb7e8;
      --shadow: 0 1px 2px rgba(0,0,0,0.4), 0 10px 24px -12px rgba(0,0,0,0.6);
    }
  }

  .p2-coder-tool, .p2-coder-tool *{ box-sizing:border-box; }
  .p2-coder-tool h1, .p2-coder-tool h2, .p2-coder-tool h3{ font-family:'Fraunces', Georgia, serif; text-wrap:balance; margin:0; }
  .p2-coder-tool .mono, .p2-coder-tool .quote, .p2-coder-tool textarea, .p2-coder-tool .score-pill, .p2-coder-tool .doc-id{ font-family:'IBM Plex Mono', ui-monospace, monospace; }
  .p2-coder-tool a{ color:var(--focus); }

  .p2-coder-tool .wrap{ max-width:1180px; margin:0 auto; }

  /* ---------- Top bar ---------- */
  .p2-coder-tool .topbar{
    display:flex; flex-wrap:wrap; gap:16px 28px; align-items:flex-end; justify-content:space-between;
    padding-bottom:18px; margin-bottom:22px; border-bottom:1px solid var(--line);
  }
  .p2-coder-tool .brand-eyebrow{
    font-size:12px; letter-spacing:.09em; text-transform:uppercase; color:var(--ink-soft); margin-bottom:6px;
  }
  .p2-coder-tool .brand h1{ font-size:clamp(24px,3.4vw,34px); font-weight:600; }
  .p2-coder-tool .brand p{ margin:6px 0 0; color:var(--ink-soft); max-width:56ch; font-size:14.5px; }

  .p2-coder-tool .progress-card{
    background:var(--card); border:1px solid var(--line); border-radius:14px; padding:14px 18px;
    box-shadow:var(--shadow); min-width:220px;
  }
  .p2-coder-tool .progress-card .num{ font-family:'Fraunces',serif; font-size:32px; font-weight:600; line-height:1; }
  .p2-coder-tool .progress-card .num small{ font-size:16px; color:var(--ink-soft); font-weight:400; }
  .p2-coder-tool .progress-card .bar{ height:6px; border-radius:4px; background:var(--paper-2); margin-top:10px; overflow:hidden; }
  .p2-coder-tool .progress-card .bar > i{ display:block; height:100%; background:var(--accent); transition:width .3s ease; }
  .p2-coder-tool .progress-card .label{ font-size:12px; color:var(--ink-soft); margin-top:6px; }

  .p2-coder-tool .coder-field{ display:flex; flex-direction:column; gap:4px; font-size:13px; color:var(--ink-soft); }
  .p2-coder-tool .coder-field input{
    font:inherit; font-family:'IBM Plex Mono',monospace; padding:8px 10px; border-radius:8px;
    border:1px solid var(--line); background:var(--card); color:var(--ink); min-width:200px;
  }
  .p2-coder-tool .sync-note{ font-size:12px; padding:6px 10px; border-radius:8px; display:inline-flex; gap:6px; align-items:center; }
  .p2-coder-tool .sync-note.ok{ background:var(--accent-soft); color:var(--accent); }
  .p2-coder-tool .sync-note.warn{ background:var(--warn-soft); color:var(--warn); }
  .p2-coder-tool .dot{ width:7px; height:7px; border-radius:50%; background:currentColor; flex:none; }

  /* ---------- Guidelines ---------- */
  .p2-coder-tool details.rules{
    background:var(--card); border:1px solid var(--line); border-radius:14px; margin-bottom:22px; box-shadow:var(--shadow);
  }
  .p2-coder-tool details.rules > summary{
    cursor:pointer; padding:16px 20px; font-family:'Fraunces',serif; font-size:18px; font-weight:600;
    display:flex; align-items:center; justify-content:space-between; list-style:none;
  }
  .p2-coder-tool details.rules > summary::-webkit-details-marker{ display:none; }
  .p2-coder-tool details.rules > summary::after{ content:'Read the protocol \2193'; font-family:'Source Sans 3',sans-serif; font-size:12px; font-weight:400; color:var(--ink-soft); }
  .p2-coder-tool details.rules[open] > summary::after{ content:'Collapse \2191'; }
  .p2-coder-tool .rules-body{ padding:0 22px 22px; }
  .p2-coder-tool .warn-box{
    background:var(--warn-soft); border:1px solid color-mix(in srgb, var(--warn) 40%, transparent);
    border-radius:10px; padding:14px 16px; margin-bottom:18px; font-size:14px;
  }
  .p2-coder-tool .warn-box b{ color:var(--warn); }
  .p2-coder-tool .rules-grid{ display:grid; grid-template-columns:repeat(auto-fit,minmax(230px,1fr)); gap:16px; margin:16px 0; }
  .p2-coder-tool .rule-card{ background:var(--paper-2); border-radius:10px; padding:14px 16px; }
  .p2-coder-tool .rule-card h4{ font-family:'Fraunces',serif; font-size:15px; margin:0 0 8px; }
  .p2-coder-tool .rule-card table{ width:100%; border-collapse:collapse; font-size:13px; }
  .p2-coder-tool .rule-card td{ padding:3px 0; vertical-align:top; }
  .p2-coder-tool .rule-card td:first-child{ font-family:'IBM Plex Mono',monospace; font-weight:600; width:28px; color:var(--accent); }
  .p2-coder-tool .scale-table{ width:100%; border-collapse:collapse; font-size:13.5px; margin-top:8px; }
  .p2-coder-tool .scale-table th{ text-align:left; font-size:11px; text-transform:uppercase; letter-spacing:.06em; color:var(--ink-soft); padding:6px 10px 6px 0; border-bottom:1px solid var(--line); }
  .p2-coder-tool .scale-table td{ padding:8px 10px 8px 0; border-bottom:1px dashed var(--line); vertical-align:top; }
  .p2-coder-tool .scale-table tr:last-child td{ border-bottom:none; }

  /* ---------- Layout: sidebar + form ---------- */
  .p2-coder-tool .layout{ display:grid; grid-template-columns:300px 1fr; gap:22px; align-items:start; }
  @media (max-width:860px){ .p2-coder-tool .layout{ grid-template-columns:1fr; } }

  .p2-coder-tool .sidebar{ position:sticky; top:16px; }
  @media (max-width:860px){ .p2-coder-tool .sidebar{ position:static; } }
  .p2-coder-tool .group{ margin-bottom:16px; }
  .p2-coder-tool .group h3{
    font-size:11px; text-transform:uppercase; letter-spacing:.07em; color:var(--ink-soft);
    font-family:'Source Sans 3',sans-serif; font-weight:600; margin-bottom:6px; padding:0 4px;
  }
  .p2-coder-tool .paper-btn{
    width:100%; text-align:left; display:flex; gap:10px; align-items:flex-start;
    padding:9px 10px; border-radius:9px; border:1px solid transparent; background:none; cursor:pointer;
    font:inherit; color:var(--ink); font-size:13.5px; line-height:1.35;
  }
  .p2-coder-tool .paper-btn:hover{ background:var(--paper-2); }
  .p2-coder-tool .paper-btn.active{ background:var(--accent-soft); border-color:color-mix(in srgb, var(--accent) 35%, transparent); }
  .p2-coder-tool .paper-btn .status{ flex:none; width:9px; height:9px; border-radius:50%; margin-top:4px; background:var(--line); }
  .p2-coder-tool .paper-btn .status.done{ background:var(--accent); }
  .p2-coder-tool .paper-btn .status.draft{ background:var(--warn); }
  .p2-coder-tool .paper-btn .ttl{ flex:1; }
  .p2-coder-tool .paper-btn .idx{ color:var(--ink-soft); font-family:'IBM Plex Mono',monospace; font-size:11px; }

  .p2-coder-tool .export-btn{
    width:100%; margin-top:10px; padding:10px; border-radius:9px; border:1px solid var(--line);
    background:var(--card); color:var(--ink); font:inherit; font-size:13px; cursor:pointer;
  }
  .p2-coder-tool .export-btn:hover{ border-color:var(--accent); color:var(--accent); }

  /* ---------- Form panel ---------- */
  .p2-coder-tool .panel{ background:var(--card); border:1px solid var(--line); border-radius:16px; box-shadow:var(--shadow); overflow:hidden; }
  .p2-coder-tool .panel-head{ padding:22px 26px 18px; border-bottom:1px solid var(--line); background:var(--paper-2); }
  .p2-coder-tool .panel-head .kicker{ font-size:12px; color:var(--ink-soft); font-family:'IBM Plex Mono',monospace; }
  .p2-coder-tool .panel-head h2{ font-size:22px; margin-top:4px; }
  .p2-coder-tool .panel-head .filename{ margin-top:6px; font-size:12.5px; color:var(--ink-soft); }
  .p2-coder-tool .panel-body{ padding:24px 26px 30px; display:flex; flex-direction:column; gap:26px; }

  .p2-coder-tool .field-row{ display:grid; grid-template-columns:1fr; gap:10px; }
  .p2-coder-tool .field-label{ font-weight:600; font-size:14.5px; display:flex; align-items:baseline; gap:8px; }
  .p2-coder-tool .field-label .hint{ font-weight:400; font-size:12.5px; color:var(--ink-soft); }
  .p2-coder-tool .score-row{ display:flex; gap:8px; flex-wrap:wrap; }
  .p2-coder-tool .score-pill{
    padding:6px 13px; border-radius:999px; border:1px solid var(--line); background:var(--paper);
    cursor:pointer; font-size:13px; user-select:none;
  }
  .p2-coder-tool .score-pill input{ display:none; }
  .p2-coder-tool .score-pill.checked{ background:var(--accent); border-color:var(--accent); color:var(--accent-ink); }
  .p2-coder-tool textarea{
    width:100%; min-height:56px; padding:10px 12px; border-radius:9px; border:1px solid var(--line);
    background:var(--paper); color:var(--ink); font-size:13px; resize:vertical;
  }
  .p2-coder-tool textarea:focus, .p2-coder-tool input:focus{ outline:2px solid var(--focus); outline-offset:1px; }

  .p2-coder-tool .rel-card{ border:1px solid var(--line); border-radius:12px; padding:16px 18px; background:var(--paper); }
  .p2-coder-tool .rel-card h4{ font-family:'Fraunces',serif; font-size:16px; margin-bottom:2px; }
  .p2-coder-tool .rel-card .q{ font-size:12.5px; color:var(--ink-soft); margin-bottom:12px; }
  .p2-coder-tool .rel-grid{ display:grid; grid-template-columns:1fr 1fr; gap:14px; }
  @media (max-width:560px){ .p2-coder-tool .rel-grid{ grid-template-columns:1fr; } }
  .p2-coder-tool select{
    font:inherit; padding:7px 10px; border-radius:8px; border:1px solid var(--line);
    background:var(--paper); color:var(--ink); font-family:'IBM Plex Mono',monospace; font-size:13px;
  }
  .p2-coder-tool .na-check{ display:flex; align-items:center; gap:7px; font-size:12.5px; color:var(--ink-soft); margin-top:10px; }
  .p2-coder-tool .rel-card.na .rel-grid, .p2-coder-tool .rel-card.na .field-row.quote{ opacity:.35; pointer-events:none; }

  .p2-coder-tool .category-row{ display:flex; gap:8px; flex-wrap:wrap; }
  .p2-coder-tool .cat-pill{ min-width:56px; text-align:center; }
  .p2-coder-tool .suggestion{ font-size:12.5px; color:var(--ink-soft); margin-top:2px; }

  .p2-coder-tool .panel-footer{
    display:flex; align-items:center; justify-content:space-between; gap:14px; flex-wrap:wrap;
    border-top:1px solid var(--line); padding-top:18px;
  }
  .p2-coder-tool .save-btn{
    padding:11px 22px; border-radius:10px; border:none; background:var(--accent); color:var(--accent-ink);
    font:inherit; font-weight:600; font-size:14px; cursor:pointer;
  }
  .p2-coder-tool .save-btn:hover{ filter:brightness(1.06); }
  .p2-coder-tool .save-btn:disabled{ opacity:.55; cursor:default; }
  .p2-coder-tool .save-state{ font-size:12.5px; color:var(--ink-soft); min-height:1.2em; }
  .p2-coder-tool .nav-btns{ display:flex; gap:8px; }
  .p2-coder-tool .nav-btns button{
    padding:9px 14px; border-radius:9px; border:1px solid var(--line); background:var(--card); color:var(--ink);
    font:inherit; font-size:13px; cursor:pointer;
  }
  .p2-coder-tool .nav-btns button:hover{ border-color:var(--accent); }
  .p2-coder-tool .complete-check{ display:flex; align-items:center; gap:8px; font-size:13.5px; font-weight:600; }
</style>

<div class="wrap">

  <div class="topbar">
    <div class="brand">
      <div class="brand-eyebrow">Pending Analysis P2 &middot; Independent second coder</div>
      <h1>Second-Coder Bench</h1>
      <p>Relational constructive-alignment coding for the 37 codable instances. Read each source PDF cold, score it against the protocol below, save as you go &mdash; then export your file and send it back to Mickael when you're done.</p>
    </div>
    <div style="display:flex; gap:14px; align-items:flex-end; flex-wrap:wrap;">
      <div class="coder-field">
        <label for="coderName">Coding as</label>
        <input id="coderName" type="text" placeholder="Your name" autocomplete="off" />
      </div>
      <div class="progress-card">
        <div class="num" id="progressNum">0<small>&nbsp;/ 37 complete</small></div>
        <div class="bar"><i id="progressBar" style="width:0%"></i></div>
        <div class="label" id="syncLabel">Loading&hellip;</div>
      </div>
    </div>
  </div>

  <details class="rules" id="rulesBlock">
    <summary>Coding protocol &amp; anti-contamination rule</summary>
    <div class="rules-body">
      <div class="warn-box">
        <b>Do not look at the existing codes, the manuscript, or any other coder's file before you code a paper.</b>
        You're coding blind so your scores can be compared against the original pass to measure agreement (Cohen&rsquo;s &kappa;) &mdash;
        that's the entire point of this exercise. Read only the source PDF for the paper you're on. If you accidentally
        see an existing score for a paper, code it anyway and say so in the notes field for that paper.
      </div>

      <p style="font-size:14px; max-width:70ch;">
        Unit of analysis: one specific <b>AI-supported learning-design instance</b> &mdash; a learner population and
        context, an Intended Learning Outcome (<b>ILO</b>), a Teaching/Learning Activity (<b>TLA</b>) and the cognitive
        process it should elicit, an AI role within that activity, an Assessment Task (<b>AT</b>), and the conditions
        under which that AT is administered. If a paper describes several non-equivalent instances, code the clearest
        one and mention the others exist in your notes.
      </p>

      <div class="rules-grid">
        <div class="rule-card">
          <h4>Reporting scores (ILO / TLA / AT)</h4>
          <table>
            <tr><td>0</td><td>Not identifiable &mdash; not described at all</td></tr>
            <tr><td>1</td><td>Indirectly identifiable &mdash; inferable, not stated</td></tr>
            <tr><td>2</td><td>Explicitly stated</td></tr>
          </table>
          <p style="font-size:12.5px; color:var(--ink-soft); margin-top:8px;">
            &ldquo;Understand machine learning&rdquo; is a <b>1</b> (a topic). &ldquo;Design a classifier by applying X and Y&rdquo;
            is a <b>2</b> (a named process). This topic-vs-process call is where coders disagree most &mdash; read it carefully.
          </p>
        </div>
        <div class="rule-card">
          <h4>Relationship scores (all 5 relationships)</h4>
          <table>
            <tr><td>0</td><td>No relevant evidence, or only topical relatedness</td></tr>
            <tr><td>1</td><td>Plausible from the activity type, not stated</td></tr>
            <tr><td>2</td><td>Explicitly described, not justified</td></tr>
            <tr><td>3</td><td>Explicit <b>and</b> justified &mdash; a stated design rationale or an independent empirical check</td></tr>
          </table>
        </div>
        <div class="rule-card">
          <h4>Evidence grade E0&ndash;E4</h4>
          <table>
            <tr><td>E0</td><td>No evidence bears on it (usually pairs with score 0)</td></tr>
            <tr><td>E1</td><td>Assertion without support (score 2)</td></tr>
            <tr><td>E2</td><td>Indirect / inferential only (score 1)</td></tr>
            <tr><td>E3</td><td>Justified by a stated rationale, no empirical check (score 3, theory branch)</td></tr>
            <tr><td>E4</td><td>Supported by an independent empirical/process check (score 3, empirical branch)</td></tr>
          </table>
        </div>
        <div class="rule-card">
          <h4>The strict rule for scoring a 3</h4>
          <p style="font-size:13px;">
            Never score 3 (or grade E3/E4) just because an experiment, a positive result, or a stated restriction exists
            <i>somewhere</i> in the paper. The evidence must specifically test <b>this exact relationship</b> &mdash; e.g.
            for AI&rarr;TLA: did the study check whether AI supported or substituted the <i>targeted process itself</i>,
            not just that some experiment exists elsewhere?
          </p>
        </div>
      </div>

      <table class="scale-table">
        <tr><th>Relationship</th><th>Question it answers</th></tr>
        <tr><td class="mono">ILO&rarr;TLA</td><td>Does the activity give learners the opportunity to perform the process the ILO names?</td></tr>
        <tr><td class="mono">ILO&rarr;AT</td><td>Does the assessment evidence achievement of the ILO's process, not a distinct construct?</td></tr>
        <tr><td class="mono">TLA&rarr;AT</td><td>Is the performance practised in the activity consistent with what's assessed?</td></tr>
        <tr><td class="mono">AI&rarr;TLA</td><td>Does AI support the learner's performance of the targeted process, or perform it for them?</td></tr>
        <tr><td class="mono">AI&rarr;AT</td><td>Is AI's role during assessment compatible with the construct the AT is meant to measure?</td></tr>
      </table>

      <div class="rule-card" style="margin-top:16px;">
        <h4>Structurally not applicable</h4>
        <p style="font-size:13px;">
          If the instance involves <b>no AI role at all</b> in the activity and/or assessment, tick &ldquo;not
          applicable&rdquo; on that relationship instead of scoring it 0. Score 0 means &ldquo;AI is present but this
          relationship isn't evidenced&rdquo;; not-applicable means &ldquo;there is no AI to evaluate here.&rdquo;
        </p>
      </div>

      <div class="rule-card" style="margin-top:16px;">
        <h4>The four alignment categories</h4>
        <table>
          <tr><td>A</td><td>Alignment demonstrated &mdash; ILO&rarr;TLA, ILO&rarr;AT, TLA&rarr;AT each &ge;2, trending toward 3</td></tr>
          <tr><td>B</td><td>Partially supported &mdash; some of those three &ge;2, others 0&ndash;1</td></tr>
          <tr><td>C</td><td>Cannot be established &mdash; the default outcome of insufficient evidence</td></tr>
          <tr><td>D</td><td>Misalignment identified &mdash; a relationship scored 0 <b>and</b> the source affirmatively describes an inconsistency. Never use D just because evidence is weak.</td></tr>
        </table>
      </div>

      <p style="font-size:12.5px; color:var(--ink-soft); margin-top:14px;">
        Every evidence quote must be copied verbatim from the PDF &mdash; never paraphrased or reconstructed from
        memory. If you can't find a quote to support a score above 0, that's a sign the score should be lower.
      </p>
    </div>
  </details>

  <div class="layout">
    <nav class="sidebar" id="sidebar"></nav>
    <main class="panel" id="panel"></main>
  </div>

</div>
</div>

<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600&family=Source+Sans+3:wght@400;600&family=IBM+Plex+Mono:wght@400;600&display=swap">

<script>
(function(){
  "use strict";

  // ---------------------------------------------------------------
  // Data: the 37 codable instances, in the same five thematic groups
  // used for the earlier LLM-based robustness check (Section 13, P2).
  // ---------------------------------------------------------------
  var GROUPS = [
    { label: "ITS & embodied / robotic AI", papers: [
      ["Physical_embodiment_and_anthropomorphism_10.1038_s41539-024-00293-z.pdf","Physical embodiment and anthropomorphism of AI tutors and their role in student enjoyment and performance"],
      ["papers_Development_research_on_an_AI_English_learning_sup.pdf","Development research on an AI English learning support system to facilitate learner-generated-context-based learning"],
      ["papers_Ding_2024_integratingIVRinscaffoldedGBLforsciencelearning.pdf","Integrating immersive virtual reality technology in scaffolded game-based learning to enhance low motivation students' multimodal science learning"],
      ["papers_lai2019.pdf","Study on enhancing AIoT computational thinking skills by plot image-based VR"],
      ["papers_s41239-023-00434-1.pdf","Students' perceptions of using ChatGPT in a physics class as a virtual tutor"],
      ["papers_understanding_secondary_students.pdf","Understanding secondary students' continuance intention to adopt AI-powered intelligent tutoring system for English learning"],
      ["The_effect_of_the_frequency_of_use_of_an_10.3389_feduc.2025.1738655.pdf","The effect of the frequency of use of an intelligent tutoring system on learning gains in mathematics secondary education"]
    ]},
    { label: "AI literacy & K–12 ethics curricula", papers: [
      ["papers_1-s2.0-S2666920X22000169-main.pdf","Artificial Intelligence education for young children: Why, what, and how in curriculum design and implementation"],
      ["papers_Abdelghani_et_al._-_2024_-_GPT-3-Driven_Pedagogical_Agents_to_Train_Childrens_Curious_Question-Asking_Skills-annotated.pdf","GPT-3-driven pedagogical agents for training children's curious question-asking skills"],
      ["papers_developing_a_weather.pdf","Developing a weather prediction project-based machine learning course in facilitating AI learning among high school students"],
      ["papers_evaluaing_an_AI_literacy_programme.pdf","Evaluating an artificial intelligence literacy programme for empowering and developing concepts, literacy and ethical awareness in senior secondary students"],
      ["papers_Integrating_Ethics_and_Career_Futures-annotated.pdf","Integrating Ethics and Career Futures with Technical Learning to Promote AI Literacy for Middle School Students: An Exploratory Study"],
      ["papers_Williams_et_al._-_2023_-_AI_Ethics_Curricula_for_Middle_School_Youth_Lessons_Learned_from_Three_Project-Based_Curricula-annotated.pdf","AI + Ethics Curricula for Middle School Youth: Lessons Learned from Three Project-Based Curricula"]
    ]},
    { label: "Chatbot-mediated instruction", papers: [
      ["AI_in_the_classroom_Exploring_students_10.1007_s10639-025-13337-7.pdf","AI in the classroom: Exploring students' interaction with ChatGPT in programming learning"],
      ["Facilitator_or_hindrance_The_impact_of_10.1186_s41239-025-00534-0.pdf","Facilitator or hindrance? The impact of AI on university students' higher-order thinking skills in complex problem solving"],
      ["papers_NILE_AIchatbotsubmission_author_R2.pdf","Teacher Support and Student Motivation to Learn with Artificial Intelligence (AI) based Chatbot"],
      ["papers_Zhong_et_al._-_2024_-_The_influences_of_ChatGPT_on_undergraduate_students_demonstrated_and_perceived_interdisciplinary_learning-annotated.pdf","The influences of ChatGPT on undergraduate students' demonstrated and perceived interdisciplinary learning"],
      ["AI_vs_teacher_feedback_on_EFL_argumenta_10.3389_feduc.2025.1614673.pdf","AI vs. teacher feedback on EFL argumentative writing: a quantitative study"],
      ["Effects_of_different_AI_driven_Chatbot_f_10.1038_s41539-025-00311-8.pdf","Effects of different AI-driven Chatbot feedback on learning outcomes and brain activity"],
      ["One_year_in_the_classroom_with_ChatGPT_10.3389_feduc.2025.1574477.pdf","One year in the classroom with ChatGPT: empirical insights and transformative impacts"],
      ["Who_is_solving_the_challenge_The_use_of_10.3389_feduc.2025.1417642.pdf","Who is solving the challenge? The use of ChatGPT in mathematics and biology courses using challenge-based learning"]
    ]},
    { label: "Assessment, learning analytics & peer assessment", papers: [
      ["Highly_informative_feedback_using_learni_10.1186_s41239-025-00539-9.pdf","Highly informative feedback using learning analytics: how feedback literacy moderates student perceptions of feedback"],
      ["papers_zotou2020.pdf","Data-driven problem based learning: enhancing problem based learning with learning analytics"],
      ["A_gamified_AI_movement_assessment_and_fe_10.1007_s10639-026-13911-7.pdf","A gamified AI movement assessment and feedback approach for university students in physical education"],
      ["Balancing_AI_assisted_learning_and_tradi_10.3389_feduc.2025.1596462.pdf","Balancing AI-assisted learning and traditional assessment: the FACT assessment in environmental data science education"],
      ["Enhancing_peer_assessment_with_artificia_10.1186_s41239-024-00501-1.pdf","Enhancing peer assessment with artificial intelligence"],
      ["Evaluating_the_effectiveness_of_digital_10.3389_feduc.2025.1670892.pdf","Evaluating the effectiveness of digital scenario-based English teaching at the university level using AI-generated content"],
      ["Motivation_and_achievement_in_EFL_the_p_10.3389_feduc.2025.1614388.pdf","Motivation and achievement in EFL: the power of instructional approach"],
      ["Student_reactions_to_AI_versus_human_fee_10.1186_s41239-025-00555-9.pdf","Student reactions to AI versus human feedback in teamwork skills assessment"]
    ]},
    { label: "Generative AI, creative & project-based learning", papers: [
      ["Design_of_generative_AI_powered_pedagogy_10.1038_s41539-025-00326-1.pdf","Design of generative AI-powered pedagogy for virtual reality environments in higher education"],
      ["Exploring_effortless_AI_generated_gamifi_10.1007_s10639-025-13765-5.pdf","Exploring effortless AI-generated gamified quizzes in an online special education module"],
      ["GIFT_AI_Damn_jpg_visual_literacy_throu_10.3389_feduc.2025.1621207.pdf","GIFT-AI: Damn!.jpg — visual literacy through image-making with generative AI"],
      ["GIFT_AI_I_m_scared_that_the_AI_feedbac_10.3389_feduc.2025.1612398.pdf","GIFT-AI: “I'm scared that the AI feedback is too much!” — preservice teachers' feedback strategies with GenAI"],
      ["Heutagogy_and_generative_AI_an_empirica_10.3389_feduc.2026.1760298.pdf","Heutagogy and generative AI: an empirical investigation of self-determined learning on deep learning outcomes"],
      ["Project_based_learning_as_a_pathway_to_w_10.3389_feduc.2026.1757812.pdf","Project-based learning as a pathway to writing autonomy and responsible AI use in university EFL learners"],
      ["Scaffolding_Creativity_Integrating_Gene_10.1145_3706599.3720283.pdf","Scaffolding Creativity: Integrating Generative AI Tools and Real-world Experiences in Business Education"],
      ["Teaching_AI_with_games_the_impact_of_ge_10.1007_s10639-025-13624-3.pdf","Teaching AI with games: the impact of generative AI drawing on computational thinking skills"]
    ]}
  ];

  var PAPERS = [];
  var n = 0;
  GROUPS.forEach(function(g){
    g.papers.forEach(function(p){
      n++;
      PAPERS.push({ id: "p" + (n<10?"0"+n:n), group: g.label, filename: p[0], title: p[1] });
    });
  });

  var RELATIONSHIPS = [
    { key:"ILO_to_TLA", label:"ILO → TLA", q:"Does the activity give learners the opportunity to perform the process the ILO names?", naAllowed:false },
    { key:"ILO_to_AT",  label:"ILO → AT",  q:"Does the assessment evidence achievement of the ILO's process, not a distinct construct?", naAllowed:false },
    { key:"TLA_to_AT",  label:"TLA → AT",  q:"Is the performance practised in the activity consistent with what's assessed?", naAllowed:false },
    { key:"AI_to_TLA",  label:"AI → TLA",  q:"Does AI support the learner's performance of the targeted process, or perform it for them?", naAllowed:true },
    { key:"AI_to_AT",   label:"AI → AT",   q:"Is AI's role during assessment compatible with the construct the AT is meant to measure?", naAllowed:true }
  ];

  var GRADES = ["E0","E1","E2","E3","E4"];
  var CATEGORIES = ["A","B","C","D"];

  // ---------------------------------------------------------------
  // State
  // ---------------------------------------------------------------
  var docs = {};        // paperId -> saved coding doc
  var draft = {};        // paperId -> in-progress form state (not yet saved)
  var activeId = PAPERS[0].id;

  function emptyCoding(p){
    var rel = {};
    RELATIONSHIPS.forEach(function(r){
      rel[r.key] = { score: null, grade: "", evidence_quote: "", not_applicable: false };
    });
    return {
      filename: p.filename, title: p.title, group: p.group,
      ILO_score: null, ILO_evidence_quote: "",
      TLA_score: null, TLA_evidence_quote: "",
      AT_score: null, AT_evidence_quote: "",
      category: "", category_auto: "",
      relationships: rel,
      coder_notes: "",
      coder: "",
      completed: false,
      updated_at: null
    };
  }

  function getState(id){
    if (!draft[id]) {
      var base = docs[id] ? JSON.parse(JSON.stringify(docs[id])) : emptyCoding(PAPERS.find(function(p){return p.id===id;}));
      draft[id] = base;
    }
    return draft[id];
  }

  function suggestCategory(state){
    var a = state.relationships.ILO_to_TLA.score, b = state.relationships.ILO_to_AT.score, c = state.relationships.TLA_to_AT.score;
    if (a===null || b===null || c===null) return "";
    if (a>=2 && b>=2 && c>=2) return "A";
    if (a>=2 || b>=2 || c>=2) return "B";
    return "C";
  }

  // ---------------------------------------------------------------
  // localStorage persistence (this is a static page: no live sync,
  // saving keeps your work in THIS browser between visits; always
  // export JSON/CSV when you're done and send it back)
  // ---------------------------------------------------------------
  function lsSave(id, data){
    try{
      var all = JSON.parse(localStorage.getItem("p2_codings_fallback") || "{}");
      all[id] = data;
      localStorage.setItem("p2_codings_fallback", JSON.stringify(all));
    }catch(e){}
  }
  function lsLoadAll(){
    try{ return JSON.parse(localStorage.getItem("p2_codings_fallback") || "{}"); }catch(e){ return {}; }
  }

  // ---------------------------------------------------------------
  // Rendering
  // ---------------------------------------------------------------
  function statusFor(id){
    var d = docs[id];
    if (d && d.completed) return "done";
    if (d && (d.ILO_score!==null || d.TLA_score!==null || d.AT_score!==null || d.coder_notes)) return "draft";
    return "none";
  }

  function renderSidebar(){
    var el = document.getElementById("sidebar");
    var html = "";
    GROUPS.forEach(function(g){
      html += '<div class="group"><h3>'+escapeHtml(g.label)+'</h3>';
      g.papers.forEach(function(p, i){
        var paper = PAPERS.find(function(x){ return x.filename===p[0]; });
        var st = statusFor(paper.id);
        html += '<button class="paper-btn'+(paper.id===activeId?' active':'')+'" data-id="'+paper.id+'">'
              + '<span class="status '+st+'"></span>'
              + '<span class="ttl"><span class="idx">'+paper.id.replace('p','#')+'</span> &middot; '+escapeHtml(paper.title)+'</span>'
              + '</button>';
      });
      html += '</div>';
    });
    html += '<button class="export-btn" id="exportBtn">Export all as JSON</button>';
    html += '<button class="export-btn" id="exportCsvBtn" style="margin-top:6px;">Export scores as CSV</button>';
    el.innerHTML = html;
    el.querySelectorAll(".paper-btn").forEach(function(btn){
      btn.addEventListener("click", function(){ setActive(btn.getAttribute("data-id")); });
    });
    document.getElementById("exportBtn").addEventListener("click", exportJson);
    document.getElementById("exportCsvBtn").addEventListener("click", exportCsv);
    updateProgress();
  }

  function updateProgress(){
    var done = PAPERS.filter(function(p){ return statusFor(p.id)==="done"; }).length;
    document.getElementById("progressNum").innerHTML = done + "<small>&nbsp;/ 37 complete</small>";
    document.getElementById("progressBar").style.width = Math.round(done/37*100) + "%";
  }

  function pill(name, value, current, extraClass){
    var checked = (current===value) ? " checked" : "";
    return '<label class="score-pill'+checked+(extraClass||'')+'" data-field="'+name+'" data-value="'+value+'">'
         + '<input type="radio" name="'+name+'" value="'+value+'"'+(current===value?' checked':'')+'>'+value+'</label>';
  }

  function relCard(r, state){
    var rel = state.relationships[r.key];
    var scores = [0,1,2,3].map(function(v){ return pill(r.key+"__score", v, rel.score); }).join("");
    var gradeOpts = ['<option value="">—</option>'].concat(GRADES.map(function(g){
      return '<option value="'+g+'"'+(rel.grade===g?' selected':'')+'>'+g+'</option>';
    })).join("");
    var naBlock = r.naAllowed ? (
      '<label class="na-check"><input type="checkbox" data-field="'+r.key+'__na"'+(rel.not_applicable?' checked':'')+'> '
      + 'Not applicable — no AI role in this instance</label>'
    ) : "";
    return '<div class="rel-card'+(rel.not_applicable?' na':'')+'" data-rel="'+r.key+'">'
      + '<h4>'+r.label+'</h4><div class="q">'+r.q+'</div>'
      + '<div class="rel-grid">'
      +   '<div class="field-row"><div class="field-label">Score</div><div class="score-row">'+scores+'</div></div>'
      +   '<div class="field-row"><div class="field-label">Evidence grade</div><select data-field="'+r.key+'__grade">'+gradeOpts+'</select></div>'
      + '</div>'
      + '<div class="field-row quote" style="margin-top:12px;"><div class="field-label">Evidence quote <span class="hint">verbatim from the PDF, or leave blank if score is 0</span></div>'
      +   '<textarea class="quote" data-field="'+r.key+'__quote" placeholder="“…”">'+escapeHtml(rel.evidence_quote)+'</textarea></div>'
      + naBlock
      + '</div>';
  }

  function renderPanel(){
    var paper = PAPERS.find(function(p){ return p.id===activeId; });
    var state = getState(activeId);
    var idx = PAPERS.indexOf(paper);
    var autoCat = suggestCategory(state);
    var chosenCat = state.category || autoCat;

    var ilo = [0,1,2].map(function(v){ return pill("ILO_score", v, state.ILO_score); }).join("");
    var tla = [0,1,2].map(function(v){ return pill("TLA_score", v, state.TLA_score); }).join("");
    var at  = [0,1,2].map(function(v){ return pill("AT_score", v, state.AT_score); }).join("");
    var cats = CATEGORIES.map(function(c){ return pill("category", c, chosenCat, " cat-pill"); }).join("");

    var relHtml = RELATIONSHIPS.map(function(r){ return relCard(r, state); }).join("");

    var html =
      '<div class="panel-head">'
      + '<div class="kicker">'+paper.id.replace('p','Instance #')+' of 37 · '+escapeHtml(paper.group)+'</div>'
      + '<h2>'+escapeHtml(paper.title)+'</h2>'
      + '<div class="filename">Source file: <span class="mono">'+escapeHtml(paper.filename)+'</span></div>'
      + '</div>'
      + '<div class="panel-body">'

      + '<div class="field-row"><div class="field-label">ILO reporting score <span class="hint">0–2</span></div><div class="score-row">'+ilo+'</div>'
      +   '<textarea class="quote" data-field="ILO_evidence_quote" placeholder="Verbatim ILO quote">'+escapeHtml(state.ILO_evidence_quote)+'</textarea></div>'

      + '<div class="field-row"><div class="field-label">TLA reporting score <span class="hint">0–2</span></div><div class="score-row">'+tla+'</div>'
      +   '<textarea class="quote" data-field="TLA_evidence_quote" placeholder="Verbatim TLA quote">'+escapeHtml(state.TLA_evidence_quote)+'</textarea></div>'

      + '<div class="field-row"><div class="field-label">AT reporting score <span class="hint">0–2</span></div><div class="score-row">'+at+'</div>'
      +   '<textarea class="quote" data-field="AT_evidence_quote" placeholder="Verbatim AT quote">'+escapeHtml(state.AT_evidence_quote)+'</textarea></div>'

      + '<div style="display:flex; flex-direction:column; gap:14px;">'
      +   '<h3 style="font-size:17px;">The five relationships</h3>'
      +   '<div style="display:flex; flex-direction:column; gap:14px;">'+relHtml+'</div>'
      + '</div>'

      + '<div class="field-row"><div class="field-label">Alignment category</div><div class="category-row">'+cats+'</div>'
      +   '<div class="suggestion">Auto-suggested from ILO→TLA / ILO→AT / TLA→AT above: <b>'+(autoCat||'—')+'</b>. Override if D applies (affirmative evidence of inconsistency) or your judgment differs.</div></div>'

      + '<div class="field-row"><div class="field-label">Coder notes <span class="hint">2–4 sentences: what the instance is, and any close calls</span></div>'
      +   '<textarea data-field="coder_notes" style="min-height:80px;" placeholder="What is the instance, and where did you have to make a judgment call?">'+escapeHtml(state.coder_notes)+'</textarea></div>'

      + '<div class="panel-footer">'
      +   '<label class="complete-check"><input type="checkbox" id="completeCheck"'+(state.completed?' checked':'')+'> Mark this paper as complete</label>'
      +   '<div style="display:flex; align-items:center; gap:14px;">'
      +     '<div class="save-state" id="saveState"></div>'
      +     '<button class="save-btn" id="saveBtn">Save</button>'
      +   '</div>'
      + '</div>'

      + '<div class="nav-btns">'
      +   '<button id="prevBtn"'+(idx===0?' disabled':'')+'>&larr; Previous</button>'
      +   '<button id="nextBtn"'+(idx===PAPERS.length-1?' disabled':'')+'>Next &rarr;</button>'
      + '</div>'

      + '</div>';

    document.getElementById("panel").innerHTML = html;
    wireForm(state);
  }

  function wireForm(state){
    var panel = document.getElementById("panel");

    panel.querySelectorAll(".score-pill").forEach(function(p){
      p.addEventListener("click", function(){
        var field = p.getAttribute("data-field");
        var value = parseInt(p.getAttribute("data-value"),10);
        var group = panel.querySelectorAll('.score-pill[data-field="'+field+'"]');
        group.forEach(function(g){ g.classList.remove("checked"); });
        p.classList.add("checked");
        applyField(state, field, value);
      });
    });

    panel.querySelectorAll("select[data-field]").forEach(function(sel){
      sel.addEventListener("change", function(){ applyField(state, sel.getAttribute("data-field"), sel.value); });
    });

    panel.querySelectorAll("textarea[data-field]").forEach(function(t){
      t.addEventListener("input", function(){ applyField(state, t.getAttribute("data-field"), t.value); });
    });

    panel.querySelectorAll('input[type="checkbox"][data-field]').forEach(function(cb){
      cb.addEventListener("change", function(){
        applyField(state, cb.getAttribute("data-field"), cb.checked);
        renderPanel(); // re-render to grey out the na card
      });
    });

    var complete = document.getElementById("completeCheck");
    complete.addEventListener("change", function(){ state.completed = complete.checked; });

    document.getElementById("saveBtn").addEventListener("click", function(){ save(state); });
    document.getElementById("prevBtn").addEventListener("click", function(){
      var idx = PAPERS.findIndex(function(p){return p.id===activeId;});
      if (idx>0) setActive(PAPERS[idx-1].id);
    });
    document.getElementById("nextBtn").addEventListener("click", function(){
      var idx = PAPERS.findIndex(function(p){return p.id===activeId;});
      if (idx<PAPERS.length-1) setActive(PAPERS[idx+1].id);
    });
  }

  function applyField(state, field, value){
    if (field.indexOf("__")>-1){
      var parts = field.split("__");
      var relKey = parts[0], sub = parts[1];
      if (sub==="score") state.relationships[relKey].score = value;
      else if (sub==="grade") state.relationships[relKey].grade = value;
      else if (sub==="quote") state.relationships[relKey].evidence_quote = value;
      else if (sub==="na") state.relationships[relKey].not_applicable = value;
      return;
    }
    if (field==="category"){ state.category = value; return; }
    state[field] = value;
  }

  function setActive(id){
    activeId = id;
    renderSidebar();
    renderPanel();
  }

  function escapeHtml(s){
    return (s==null?"":String(s)).replace(/[&<>"']/g, function(c){
      return ({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"})[c];
    });
  }

  // ---------------------------------------------------------------
  // Save / export
  // ---------------------------------------------------------------
  var coderName = "";
  try{ coderName = localStorage.getItem("p2_coder_name") || ""; }catch(e){}
  document.getElementById("coderName").value = coderName;
  document.getElementById("coderName").addEventListener("input", function(e){
    coderName = e.target.value;
    try{ localStorage.setItem("p2_coder_name", coderName); }catch(err){}
  });

  function save(state){
    state.coder = coderName;
    state.category = state.category || suggestCategory(state);
    state.updated_at = new Date().toISOString();
    var saveState = document.getElementById("saveState");

    docs[activeId] = JSON.parse(JSON.stringify(state));
    lsSave(activeId, state);
    saveState.textContent = "Saved in this browser at " + new Date().toLocaleTimeString() + " — export when you're done.";
    renderSidebar();
  }

  function exportJson(){
    var all = PAPERS.map(function(p){ return docs[p.id] || null; }).filter(Boolean);
    var blob = JSON.stringify(all, null, 2);
    triggerDownload("p2_second_coder_codings.json", blob, "application/json");
  }

  function exportCsv(){
    var header = ["id","filename","title","coder","completed","ILO_score","TLA_score","AT_score","category",
      "ILO_to_TLA_score","ILO_to_TLA_grade","ILO_to_AT_score","ILO_to_AT_grade","TLA_to_AT_score","TLA_to_AT_grade",
      "AI_to_TLA_score","AI_to_TLA_grade","AI_to_TLA_na","AI_to_AT_score","AI_to_AT_grade","AI_to_AT_na"];
    var rows = [header.join(",")];
    PAPERS.forEach(function(p){
      var d = docs[p.id];
      if (!d) return;
      var r = d.relationships;
      function c(v){ return v===null||v===undefined ? "" : v; }
      function q(v){ return '"'+String(v==null?"":v).replace(/"/g,'""')+'"'; }
      rows.push([p.id, q(p.filename), q(p.title), q(d.coder), d.completed, c(d.ILO_score), c(d.TLA_score), c(d.AT_score), d.category,
        c(r.ILO_to_TLA.score), r.ILO_to_TLA.grade, c(r.ILO_to_AT.score), r.ILO_to_AT.grade, c(r.TLA_to_AT.score), r.TLA_to_AT.grade,
        c(r.AI_to_TLA.score), r.AI_to_TLA.grade, r.AI_to_TLA.not_applicable, c(r.AI_to_AT.score), r.AI_to_AT.grade, r.AI_to_AT.not_applicable
      ].join(","));
    });
    triggerDownload("p2_second_coder_scores.csv", rows.join("\n"), "text/csv");
  }

  function triggerDownload(filename, text, mime){
    try{
      var blob = new Blob([text], {type:mime});
      var url = URL.createObjectURL(blob);
      var a = document.createElement("a");
      a.href = url; a.download = filename; a.click();
      setTimeout(function(){ URL.revokeObjectURL(url); }, 2000);
    }catch(e){}
  }

  // ---------------------------------------------------------------
  // Boot
  // ---------------------------------------------------------------
  function boot(){
    var fb = lsLoadAll();
    Object.keys(fb).forEach(function(id){ docs[id] = fb[id]; });
    document.getElementById("syncLabel").innerHTML = '<span class="sync-note warn"><span class="dot"></span> Saved in this browser only — export JSON/CSV and send it to Mickael when done</span>';
    renderSidebar();
    renderPanel();
  }

  if (document.readyState === "loading"){
    document.addEventListener("DOMContentLoaded", boot);
  } else {
    boot();
  }
})();
</script>
