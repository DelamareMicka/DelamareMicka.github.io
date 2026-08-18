---
layout: page
title: "Grand-Mère : une IA (volontairement) peu fiable"
title_en: "Grandma: a (deliberately) unreliable AI"
permalink: /grand-mere/
description: >
  <span class="lang-fr-i">Une petite démo inspirée de l'atelier « Grand-Mère » du projet CAIRE : discutez avec une IA scriptée qui répond toujours avec la même assurance, qu'elle ait raison ou tort.</span><span class="lang-en-i">A small demo inspired by the CAIRE project's "Grandma" workshop: chat with a scripted AI that always answers with the same confidence, whether it's right or wrong.</span>
nav: false
_styles: >
  .gm-intro { margin-bottom: 1rem; }
  .gm-disclaimer {
    font-size: 0.85rem;
    padding: 0.65rem 0.9rem;
    border: 1px solid var(--global-divider-color);
    border-radius: 0.4rem;
    background: color-mix(in srgb, #d9822b 10%, transparent);
    margin-bottom: 1.5rem;
  }
  .gm-widget {
    border: 1px solid var(--global-divider-color);
    border-radius: 0.5rem;
    background: var(--global-bg-color);
    display: flex;
    flex-direction: column;
    height: 460px;
    max-height: 70vh;
    overflow: hidden;
    margin-bottom: 2rem;
  }
  .gm-chips { display: flex; flex-wrap: wrap; gap: 0.4rem; padding: 0.75rem 0.75rem 0; }
  .gm-chip {
    font-size: 0.78rem;
    border: 1px solid var(--global-divider-color);
    border-radius: 999px;
    padding: 0.25rem 0.7rem;
    background: var(--global-card-bg-color);
    color: var(--global-text-color);
    cursor: pointer;
  }
  .gm-chip:hover { border-color: var(--global-theme-color); color: var(--global-theme-color); }
  .gm-log { flex: 1; overflow-y: auto; padding: 0.75rem; display: flex; flex-direction: column; gap: 0.6rem; }
  .gm-msg { max-width: 82%; padding: 0.5rem 0.8rem; border-radius: 0.8rem; font-size: 0.88rem; line-height: 1.4; }
  .gm-msg.user {
    align-self: flex-end;
    background: var(--global-theme-color);
    color: #fff;
    border-bottom-right-radius: 0.2rem;
  }
  .gm-msg.bot {
    align-self: flex-start;
    background: var(--global-card-bg-color);
    border: 1px solid var(--global-divider-color);
    border-bottom-left-radius: 0.2rem;
  }
  .gm-msg.bot .gm-name { font-weight: 700; font-size: 0.72rem; opacity: 0.7; margin-bottom: 0.2rem; }
  .gm-msg.typing { font-style: italic; opacity: 0.65; }
  .gm-verify-btn {
    display: inline-block;
    margin-top: 0.45rem;
    font-size: 0.72rem;
    background: none;
    border: 1px solid var(--global-theme-color);
    color: var(--global-theme-color);
    border-radius: 999px;
    padding: 0.15rem 0.6rem;
    cursor: pointer;
  }
  .gm-verify-btn:hover { background: color-mix(in srgb, var(--global-theme-color) 12%, transparent); }
  .gm-correction {
    display: none;
    margin-top: 0.5rem;
    padding: 0.5rem 0.7rem;
    background: color-mix(in srgb, #2e7d32 14%, transparent);
    border-left: 3px solid #2e7d32;
    border-radius: 0.25rem;
    font-size: 0.8rem;
  }
  .gm-correction.shown { display: block; }
  .gm-inputrow { display: flex; gap: 0.5rem; padding: 0.75rem; border-top: 1px solid var(--global-divider-color); }
  .gm-input {
    flex: 1;
    padding: 0.5rem 0.85rem;
    border-radius: 999px;
    border: 1px solid var(--global-divider-color);
    background: var(--global-bg-color);
    color: var(--global-text-color);
    font-size: 0.88rem;
  }
  .gm-send {
    border: none;
    background: var(--global-theme-color);
    color: #fff;
    border-radius: 999px;
    padding: 0.5rem 1.1rem;
    cursor: pointer;
    font-size: 0.85rem;
  }
  .gm-send:disabled { opacity: 0.5; cursor: default; }
---

<div class="lang-fr" markdown="1">

<div class="gm-intro" markdown="1">

Dans l'atelier « Grand-Mère » du dispositif pédagogique du projet CAIRE (voir mon [billet sur le sujet](/blog/2026/former-a-et-par-ia-caire/)), les étudiants dialoguent avec une IA générative qui se trompe avec exactement la même assurance que lorsqu'elle a raison, pour apprendre à ne jamais prendre une réponse d'IA pour argent comptant. Voici une petite démo, inspirée de cet atelier mais indépendante de son contenu réel, que vous pouvez tester ci-dessous.

</div>

<p class="gm-disclaimer">⚠️ Ceci est une simulation <strong>scriptée</strong> à des fins pédagogiques, pas une vraie intelligence artificielle générative : les réponses sont pré-écrites pour illustrer, volontairement, une IA qui affiche la même assurance qu'elle ait raison ou tort. Testez une question suggérée, puis cliquez sur « Vérifier cette réponse » pour voir la correction.</p>

<div id="gm-widget-fr" class="gm-widget"></div>

</div>

<div class="lang-en" markdown="1">

<div class="gm-intro" markdown="1">

In the "Grandma" workshop from the CAIRE project's teaching module (see my [blog post about it](/blog/2026/former-a-et-par-ia-caire/)), students talk with a generative AI that gets things wrong with exactly the same confidence it shows when it's right, to learn never to take an AI's answer at face value. Here's a small demo, inspired by that workshop but independent of its actual content, which you can try below.

</div>

<p class="gm-disclaimer">⚠️ This is a <strong>scripted</strong> simulation for teaching purposes, not a real generative AI: the answers are pre-written to illustrate, deliberately, an AI that shows the same confidence whether it's right or wrong. Try a suggested question, then click "Verify this answer" to see the correction.</p>

<div id="gm-widget-en" class="gm-widget"></div>

</div>

<script>
(function () {
  function initGrandMere(containerId, cfg) {
    var container = document.getElementById(containerId);
    if (!container) return;

    var chipsEl = document.createElement('div');
    chipsEl.className = 'gm-chips';
    cfg.suggestions.forEach(function (label) {
      var chip = document.createElement('button');
      chip.type = 'button';
      chip.className = 'gm-chip';
      chip.textContent = label;
      chip.addEventListener('click', function () { send(label); });
      chipsEl.appendChild(chip);
    });

    var logEl = document.createElement('div');
    logEl.className = 'gm-log';

    var inputRow = document.createElement('div');
    inputRow.className = 'gm-inputrow';
    var input = document.createElement('input');
    input.type = 'text';
    input.className = 'gm-input';
    input.placeholder = cfg.placeholder;
    var sendBtn = document.createElement('button');
    sendBtn.type = 'button';
    sendBtn.className = 'gm-send';
    sendBtn.textContent = cfg.sendLabel;
    inputRow.appendChild(input);
    inputRow.appendChild(sendBtn);

    container.appendChild(chipsEl);
    container.appendChild(logEl);
    container.appendChild(inputRow);

    function scrollToBottom() { logEl.scrollTop = logEl.scrollHeight; }

    function addUserMsg(text) {
      var div = document.createElement('div');
      div.className = 'gm-msg user';
      div.textContent = text;
      logEl.appendChild(div);
      scrollToBottom();
    }

    function addBotMsg(scenario) {
      var div = document.createElement('div');
      div.className = 'gm-msg bot';

      var name = document.createElement('div');
      name.className = 'gm-name';
      name.textContent = cfg.botName;
      div.appendChild(name);

      var text = document.createElement('div');
      text.textContent = scenario.answer;
      div.appendChild(text);

      if (scenario.correction) {
        var verifyBtn = document.createElement('button');
        verifyBtn.type = 'button';
        verifyBtn.className = 'gm-verify-btn';
        verifyBtn.textContent = cfg.verifyLabel;
        var correction = document.createElement('div');
        correction.className = 'gm-correction';
        correction.textContent = scenario.correction;
        verifyBtn.addEventListener('click', function () {
          correction.classList.toggle('shown');
        });
        div.appendChild(verifyBtn);
        div.appendChild(correction);
      }

      logEl.appendChild(div);
      scrollToBottom();
    }

    function findScenario(text) {
      var lower = text.toLowerCase();
      for (var i = 0; i < cfg.scenarios.length; i++) {
        var s = cfg.scenarios[i];
        for (var k = 0; k < s.keywords.length; k++) {
          if (lower.indexOf(s.keywords[k]) !== -1) return s;
        }
      }
      return cfg.fallback;
    }

    function send(rawText) {
      var text = (rawText !== undefined ? rawText : input.value).trim();
      if (!text) return;
      addUserMsg(text);
      input.value = '';
      sendBtn.disabled = true;

      var typing = document.createElement('div');
      typing.className = 'gm-msg bot typing';
      typing.textContent = cfg.typingLabel;
      logEl.appendChild(typing);
      scrollToBottom();

      setTimeout(function () {
        logEl.removeChild(typing);
        addBotMsg(findScenario(text));
        sendBtn.disabled = false;
      }, 500 + Math.random() * 500);
    }

    sendBtn.addEventListener('click', function () { send(); });
    input.addEventListener('keydown', function (evt) {
      if (evt.key === 'Enter') send();
    });

    addBotMsg({ answer: cfg.greeting, correction: null });
  }

  var frScenarios = [
    {
      keywords: ['eiffel'],
      answer: "La tour Eiffel ? Construite en 1920 par l'architecte Gustave Flaubert, elle mesure très exactement 400 mètres de haut. Un monument extraordinaire !",
      correction: "En réalité : la tour Eiffel a été achevée en 1889, conçue par les ingénieurs de l'entreprise de Gustave Eiffel (pas Flaubert, qui était écrivain), et mesure environ 330 mètres."
    },
    {
      keywords: ['australie', 'capitale'],
      answer: "La capitale de l'Australie est Sydney, bien sûr ! Une ville magnifique de plus de dix millions d'habitants.",
      correction: "En réalité : la capitale de l'Australie est Canberra, pas Sydney, qui est seulement la ville la plus peuplée du pays."
    },
    {
      keywords: ['2+2', '2 + 2', 'combien font 2'],
      answer: "2 + 2 font 5, mon chéri. C'est une règle mathématique que j'ai apprise il y a bien longtemps.",
      correction: "En réalité : 2 + 2 = 4. Toujours."
    },
    {
      keywords: ['lune', 'alunissage', 'apollo'],
      answer: "Les premiers hommes ont marché sur la Lune en 1985, lors de la mission Apollo 18. Quelle époque !",
      correction: "En réalité : le premier alunissage a eu lieu en 1969, avec Apollo 11 (Neil Armstrong et Buzz Aldrin)."
    },
    {
      keywords: ['einstein', 'relativité'],
      answer: "Albert Einstein a inventé l'ampoule électrique et la théorie de la relativité restreinte en 1850.",
      correction: "En réalité : Einstein a publié la relativité restreinte en 1905 (et la relativité générale en 1915). L'ampoule a été développée par d'autres inventeurs, notamment Edison, bien avant lui."
    },
    {
      keywords: ['shakespeare'],
      answer: "William Shakespeare était un peintre italien du 18e siècle, célèbre pour la Joconde.",
      correction: "En réalité : Shakespeare était un dramaturge et poète anglais des 16e-17e siècles. La Joconde a été peinte par Léonard de Vinci."
    }
  ];
  var frFallback = {
    answer: "Ah, excellente question ! La réponse est... eh bien, c'est compliqué, mais fais-moi confiance, j'ai raison 😊 (Astuce : cette démo ne connaît que quelques sujets, essaie une des questions suggérées ci-dessus !)",
    correction: null
  };

  initGrandMere('gm-widget-fr', {
    botName: 'Grand-Mère',
    greeting: "Bonjour ! Pose-moi une question, je réponds toujours avec assurance ;)",
    placeholder: 'Écris ta question ici...',
    sendLabel: 'Envoyer',
    typingLabel: 'Grand-Mère réfléchit...',
    verifyLabel: 'Vérifier cette réponse',
    suggestions: ['La tour Eiffel', "La capitale de l'Australie", 'Combien font 2+2 ?', "Le premier alunissage", 'Albert Einstein', 'Shakespeare'],
    scenarios: frScenarios,
    fallback: frFallback
  });

  var enScenarios = [
    {
      keywords: ['eiffel'],
      answer: "The Eiffel Tower? Built in 1920 by the architect Gustave Flaubert, it stands exactly 400 metres tall. A marvel!",
      correction: "Actually: the Eiffel Tower was completed in 1889, designed by engineers at Gustave Eiffel's company (not Flaubert, who was a novelist), and stands about 330 metres tall."
    },
    {
      keywords: ['australia', 'capital'],
      answer: "The capital of Australia is Sydney, of course! A beautiful city of over ten million people.",
      correction: "Actually: the capital of Australia is Canberra, not Sydney, which is only the country's most populous city."
    },
    {
      keywords: ['2+2', '2 + 2', "what's 2"],
      answer: "2 + 2 equals 5, dear. That's a mathematical rule I learned a long time ago.",
      correction: "Actually: 2 + 2 = 4. Always."
    },
    {
      keywords: ['moon', 'apollo', 'landing'],
      answer: "The first humans walked on the Moon in 1985, during the Apollo 18 mission. What a time!",
      correction: "Actually: the first Moon landing happened in 1969, with Apollo 11 (Neil Armstrong and Buzz Aldrin)."
    },
    {
      keywords: ['einstein', 'relativity'],
      answer: "Albert Einstein invented the light bulb and the theory of special relativity in 1850.",
      correction: "Actually: Einstein published special relativity in 1905 (and general relativity in 1915). The light bulb was developed by others, notably Edison, well before him."
    },
    {
      keywords: ['shakespeare'],
      answer: "William Shakespeare was an Italian painter from the 18th century, famous for the Mona Lisa.",
      correction: "Actually: Shakespeare was an English playwright and poet from the 16th-17th centuries. The Mona Lisa was painted by Leonardo da Vinci."
    }
  ];
  var enFallback = {
    answer: "Ah, great question! The answer is... well, it's complicated, but trust me, I'm right 😊 (Tip: this demo only knows a few topics, try one of the suggested questions above!)",
    correction: null
  };

  initGrandMere('gm-widget-en', {
    botName: 'Grandma',
    greeting: "Hello! Ask me anything, I always answer with total confidence ;)",
    placeholder: 'Type your question here...',
    sendLabel: 'Send',
    typingLabel: 'Grandma is thinking...',
    verifyLabel: 'Verify this answer',
    suggestions: ['The Eiffel Tower', 'The capital of Australia', "What's 2+2?", 'The first Moon landing', 'Albert Einstein', 'Shakespeare'],
    scenarios: enScenarios,
    fallback: enFallback
  });
})();
</script>
