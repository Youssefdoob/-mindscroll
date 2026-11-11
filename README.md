<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#080d1a">
  <title>MindScroll RF — Académie d'ingénierie</title>

  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: #080d1a;
      color: #f3f6ff;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    header {
      position: sticky;
      top: 0;
      z-index: 10;
      background: #10182c;
      border-bottom: 1px solid #283653;
      padding: 15px;
    }

    .top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
    }

    h1 {
      font-size: 22px;
      margin: 0;
      color: #69e6ff;
    }

    .subtitle {
      color: #a8b7d5;
      font-size: 12px;
      margin-top: 5px;
    }

    button, select {
      border: 0;
      border-radius: 12px;
      padding: 12px;
      font-size: 14px;
      font-weight: 600;
    }

    button {
      background: #28c9ed;
      color: #07101c;
      cursor: pointer;
    }

    button:active { transform: scale(.98); }

    .controls {
      display: flex;
      gap: 8px;
      margin-top: 15px;
    }

    select {
      background: #202d47;
      color: white;
      flex: 1;
      min-width: 0;
    }

    .controls button { flex: 1; }

    .stats {
      display: flex;
      justify-content: space-between;
      margin-top: 12px;
      font-size: 12px;
      color: #b3c4df;
    }

    main {
      max-width: 680px;
      margin: auto;
      padding: 15px;
    }

    .card {
      background: linear-gradient(145deg, #182640, #111a30);
      border: 1px solid #2d4264;
      border-radius: 22px;
      padding: 24px;
      margin-bottom: 18px;
      min-height: 340px;
    }

    .category {
      color: #68dfff;
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: bold;
    }

    .level {
      float: right;
      color: #b5c5e3;
      font-size: 12px;
    }

    h2 {
      font-size: 25px;
      margin-top: 25px;
      line-height: 1.25;
    }

    .summary {
      font-size: 17px;
      line-height: 1.7;
      color: #dce8fb;
    }

    .formula {
      padding: 15px;
      background: #071322;
      border-left: 3px solid #2dd8ff;
      border-radius: 8px;
      margin: 20px 0;
      font-family: monospace;
      overflow-wrap: anywhere;
    }

    .actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 22px;
    }

    .secondary {
      background: #293c5c;
      color: white;
    }

    .details {
      display: none;
      margin-top: 22px;
      padding-top: 18px;
      border-top: 1px solid #344763;
      line-height: 1.8;
      color: #dce7fa;
    }

    .quiz {
      margin-top: 20px;
      padding: 16px;
      background: #0a1528;
      border-radius: 14px;
    }

    .quiz p { font-weight: bold; }

    .answers {
      display: grid;
      gap: 8px;
    }

    .answer {
      text-align: left;
      background: #263957;
      color: white;
      font-weight: normal;
    }

    .correct { background: #167a51 !important; }
    .wrong { background: #963a48 !important; }

    .feedback {
      margin-top: 12px;
      color: #a8e8d2;
      font-size: 14px;
    }

    .empty {
      text-align: center;
      color: #a8b7d5;
      padding: 40px;
    }

    footer {
      text-align: center;
      padding: 30px;
      color: #7185a8;
      font-size: 12px;
    }
  </style>
</head>

<body>

<header>
  <div class="top">
    <div>
      <h1>⚡ MindScroll RF</h1>
      <div class="subtitle">
        Ton académie personnelle d'ingénierie
      </div>
    </div>
    <button onclick="newKnowledge()">🔄 Nouvelles notions</button>
  </div>

  <div class="controls">
    <select id="category" onchange="newKnowledge()">
      <option value="Tout">📚 Tous les domaines</option>
      <option value="RF">📡 Radiofréquences</option>
      <option value="Antennes">📶 Antennes</option>
      <option value="Télécoms">🌐 Télécommunications</option>
      <option value="Mesures">📊 Mesures RF</option>
      <option value="Électronique">⚡ Électronique</option>
      <option value="IoT">🤖 IoT</option>
    </select>
  </div>

  <div class="stats">
    <span id="progress">0 notion maîtrisée</span>
    <span id="score">Score : 0</span>
    <span id="savedCount">0 sauvegardée</span>
  </div>
</header>

<main id="feed"></main>

<footer>
  MindScroll RF · Apprendre chaque jour · 100 % gratuit
</footer>

<script>
const lessons = [
  {
    id: 1,
    cat: "RF",
    level: "Débutant",
    title: "Qu'est-ce qu'une radiofréquence ?",
    summary: "Une radiofréquence est une fréquence d'oscillation électromagnétique utilisée notamment pour transmettre des informations sans fil.",
    formula: "f = 1 / T",
    detail: "La fréquence f représente le nombre d'oscillations par seconde. Elle s'exprime en hertz (Hz). Les systèmes radio utilisent des bandes de fréquences différentes selon leurs applications : radiodiffusion, téléphonie mobile, Wi-Fi, radar ou satellite.",
    question: "Quelle est l'unité de fréquence ?",
    answers: ["Watt", "Hertz", "Ohm", "Volt"],
    correct: 1
  },
  {
    id: 2,
    cat: "RF",
    level: "Débutant",
    title: "La longueur d'onde",
    summary: "La longueur d'onde représente la distance parcourue par une onde pendant une période.",
    formula: "λ = c / f",
    detail: "Dans le vide, les ondes électromagnétiques se propagent à environ 3 × 10⁸ m/s. À 3 GHz, la longueur d'onde vaut environ 10 cm. Cette grandeur est essentielle pour dimensionner les antennes.",
    question: "À 3 GHz, quelle est approximativement la longueur d'onde dans le vide ?",
    answers: ["1 cm", "10 cm", "1 m", "10 m"],
    correct: 1
  },
  {
    id: 3,
    cat: "RF",
    level: "Intermédiaire",
    title: "L'adaptation d'impédance",
    summary: "L'adaptation d'impédance permet de réduire les réflexions entre une source, une ligne de transmission et une charge.",
    formula: "Γ = (ZL - Z0) / (ZL + Z0)",
    detail: "Le coefficient de réflexion Γ dépend de l'impédance de charge ZL et de l'impédance caractéristique Z0. Si ZL = Z0, alors Γ = 0 : aucune puissance n'est réfléchie dans le modèle idéal.",
    question: "Que vaut Γ lorsque ZL = Z0 ?",
    answers: ["1", "0", "-1", "∞"],
    correct: 1
  },
  {
    id: 4,
    cat: "RF",
    level: "Intermédiaire",
    title: "La puissance en dBm",
    summary: "Le dBm exprime une puissance électrique par rapport à une référence de 1 milliwatt.",
    formula: "P(dBm) = 10 log10(P(mW))",
    detail: "0 dBm correspond à 1 mW, 10 dBm à 10 mW, 20 dBm à 100 mW et 30 dBm à 1 W. Une augmentation de 3 dB correspond approximativement à un doublement de puissance.",
    question: "Quelle puissance correspond à 30 dBm ?",
    answers: ["1 mW", "10 mW", "100 mW", "1 W"],
    correct: 3
  },
  {
    id: 5,
    cat: "RF",
    level: "Avancé",
    title: "Le facteur de qualité Q",
    summary: "Le facteur Q caractérise notamment la sélectivité d'un résonateur.",
    formula: "Q ≈ f0 / Δf",
    detail: "Pour un résonateur faiblement amorti et une bande passante définie à mi-puissance, Q est approximativement égal au rapport entre fréquence centrale et largeur de bande. Un Q élevé correspond à une résonance plus étroite.",
    question: "À fréquence centrale identique, un Q plus élevé signifie généralement :",
    answers: ["Une bande plus large", "Une bande plus étroite", "Une fréquence nulle", "Une absence de résonance"],
    correct: 1
  },
  {
    id: 6,
    cat: "Antennes",
    level: "Débutant",
    title: "Le rôle d'une antenne",
    summary: "Une antenne convertit un signal électrique guidé en onde électromagnétique rayonnée, et inversement.",
    formula: "Signal guidé ↔ Onde rayonnée",
    detail: "Une antenne émettrice transforme l'énergie RF conduite par une ligne en rayonnement électromagnétique. En réception, elle capte une partie de l'onde incidente et fournit un signal électrique au récepteur.",
    question: "Une antenne peut-elle fonctionner en réception ?",
    answers: ["Oui", "Non", "Uniquement à 5 GHz", "Uniquement avec une batterie"],
    correct: 0
  },
  {
    id: 7,
    cat: "Antennes",
    level: "Intermédiaire",
    title: "Le gain d'antenne",
    summary: "Le gain caractérise la concentration du rayonnement dans une direction, en tenant compte de l'efficacité.",
    formula: "G = η × D",
    detail: "Le gain G est lié à la directivité D et à l'efficacité de rayonnement η. En unités linéaires, G = ηD. Le gain isotrope est souvent exprimé en dBi.",
    question: "Quelle unité est couramment utilisée pour le gain isotrope ?",
    answers: ["dBm", "dBi", "Hz", "Ohm"],
    correct: 1
  },
  {
    id: 8,
    cat: "Antennes",
    level: "Intermédiaire",
    title: "L'antenne patch",
    summary: "Une antenne patch est une structure imprimée sur un substrat diélectrique, souvent utilisée dans les systèmes micro-ondes.",
    formula: "L ≈ λg / 2",
    detail: "Pour le mode fondamental d'un patch rectangulaire, la longueur électrique est proche d'une demi-longueur d'onde guidée. Les effets de frange et la permittivité effective doivent être pris en compte pour un dimensionnement précis.",
    question: "Quel matériau constitue généralement le support d'une antenne patch ?",
    answers: ["Un substrat diélectrique", "Du vide uniquement", "Un aimant", "Une batterie"],
    correct: 0
  },
  {
    id: 9,
    cat: "Antennes",
    level: "Avancé",
    title: "Le plan de masse défectueux (DGS)",
    summary: "Une DGS modifie la distribution des courants sur le plan de masse afin d'influencer les caractéristiques RF.",
    formula: "DGS = Defected Ground Structure",
    detail: "Une structure DGS introduit des perturbations géométriques dans le plan de masse. Elle peut modifier les résonances, l'adaptation et la réponse fréquentielle d'une antenne ou d'un circuit micro-ruban.",
    question: "Où se trouve une structure DGS ?",
    answers: ["Dans le plan de masse", "Dans la batterie", "Dans le logiciel", "Dans le connecteur USB"],
    correct: 0
  },
  {
    id: 10,
    cat: "Antennes",
    level: "Avancé",
    title: "Le diagramme de rayonnement",
    summary: "Le diagramme de rayonnement décrit la répartition angulaire du champ ou de la puissance rayonnée.",
    formula: "U(θ, φ)",
    detail: "Le diagramme permet d'identifier les lobes principaux, les lobes secondaires et les directions de faible rayonnement. Il est essentiel pour étudier la couverture et les interférences.",
    question: "Que représente un diagramme de rayonnement ?",
    answers: ["La température du câble", "La répartition spatiale du rayonnement", "La tension de batterie", "Le débit Ethernet"],
    correct: 1
  },
  {
    id: 11,
    cat: "Mesures",
    level: "Débutant",
    title: "Le paramètre S11",
    summary: "S11 caractérise la réflexion à l'entrée d'un dispositif RF lorsque les autres ports sont adaptés.",
    formula: "S11(dB) = 20 log10(|S11|)",
    detail: "Un S11 de -10 dB correspond à une puissance réfléchie égale à 10 % de la puissance incidente, dans les conditions de mesure adaptées. Plus S11 est négatif, plus la réflexion est faible.",
    question: "À S11 = -10 dB, quelle fraction de puissance est réfléchie ?",
    answers: ["1 %", "10 %", "50 %", "90 %"],
    correct: 1
  },
  {
    id: 12,
    cat: "Mesures",
    level: "Débutant",
    title: "Le VNA",
    summary: "Un analyseur de réseau vectoriel mesure les paramètres S en amplitude et en phase.",
    formula: "VNA = Vector Network Analyzer",
    detail: "Le VNA permet de caractériser l'adaptation, la transmission et les réflexions d'un composant RF. Une calibration adaptée est nécessaire pour limiter les erreurs liées aux câbles et connecteurs.",
    question: "Que mesure un VNA ?",
    answers: ["Seulement la température", "Les paramètres S", "La luminosité", "Le courant continu uniquement"],
    correct: 1
  },
  {
    id: 13,
    cat: "Mesures",
    level: "Intermédiaire",
    title: "Le paramètre S21",
    summary: "S21 représente la transmission du port 1 vers le port 2, avec des conditions de référence adaptées.",
    formula: "S21(dB) = 20 log10(|S21|)",
    detail: "Dans un filtre passif, S21 permet d'étudier les pertes d'insertion et les bandes passantes. Un S21 de -3 dB correspond approximativement à la moitié de la puissance transmise par rapport à la référence.",
    question: "Dans un filtre, S21 sert principalement à étudier :",
    answers: ["La transmission", "La couleur du substrat", "La température ambiante", "La tension secteur"],
    correct: 0
  },
  {
    id: 14,
    cat: "Mesures",
    level: "Avancé",
    title: "L'abaque de Smith",
    summary: "L'abaque de Smith est un outil graphique pour analyser les impédances et les coefficients de réflexion.",
    formula: "z = Z / Z0",
    detail: "L'abaque utilise des impédances normalisées. Il facilite la conception de réseaux d'adaptation et l'analyse des transformations d'impédance le long des lignes de transmission.",
    question: "Quelle grandeur est généralement normalisée sur l'abaque de Smith ?",
    answers: ["L'impédance", "La masse", "La température", "La durée"],
    correct: 0
  },
  {
    id: 15,
    cat: "Télécoms",
    level: "Débutant",
    title: "La modulation numérique",
    summary: "La modulation numérique permet de représenter des bits en faisant varier une porteuse.",
    formula: "ASK / FSK / PSK / QAM",
    detail: "Les modulations peuvent agir sur l'amplitude, la fréquence ou la phase. La QAM combine des variations d'amplitude et de phase pour transmettre plusieurs bits par symbole.",
    question: "Quelle modulation combine amplitude et phase ?",
    answers: ["QAM", "FSK uniquement", "OOK uniquement", "FM analogique uniquement"],
    correct: 0
  },
  {
    id: 16,
    cat: "Télécoms",
    level: "Intermédiaire",
    title: "L'OFDM",
    summary: "L'OFDM transmet des données sur plusieurs sous-porteuses orthogonales.",
    formula: "OFDM = Orthogonal Frequency Division Multiplexing",
    detail: "L'OFDM est utilisé notamment dans le Wi-Fi et plusieurs systèmes cellulaires. L'orthogonalité permet de rapprocher les sous-porteuses tout en maîtrisant leur interférence dans les conditions prévues.",
    question: "L'OFDM utilise :",
    answers: ["Une seule fréquence fixe sans modulation", "Plusieurs sous-porteuses orthogonales", "Uniquement des impulsions DC", "Une antenne par bit"],
    correct: 1
  },
  {
    id: 17,
    cat: "Télécoms",
    level: "Intermédiaire",
    title: "Le rapport signal sur bruit",
    summary: "Le SNR compare la puissance utile du signal à celle du bruit.",
    formula: "SNR(dB) = 10 log10(Psignal / Pbruit)",
    detail: "Un SNR plus élevé facilite généralement la démodulation et permet d'envisager des modulations plus efficaces spectralement, selon le canal et le codage.",
    question: "Que signifie un SNR plus élevé ?",
    answers: ["Davantage de bruit relatif", "Un meilleur rapport signal/bruit", "Une fréquence nulle", "Une antenne déconnectée"],
    correct: 1
  },
  {
    id: 18,
    cat: "Télécoms",
    level: "Avancé",
    title: "Le budget de liaison",
    summary: "Le budget de liaison estime la puissance reçue en combinant émission, gains et pertes.",
    formula: "Pr(dBm) = Pt(dBm) + Gt(dBi) + Gr(dBi) - L(dB)",
    detail: "Le budget de liaison tient compte de la puissance d'émission, des gains d'antennes et des pertes de propagation et de câblage. Il permet de vérifier si le niveau reçu dépasse la sensibilité du récepteur avec une marge suffisante.",
    question: "Une augmentation des pertes de propagation provoque généralement :",
    answers: ["Une hausse de Pr", "Une baisse de Pr", "Aucun changement", "Une hausse automatique du gain"],
    correct: 1
  },
  {
    id: 19,
    cat: "Électronique",
    level: "Débutant",
    title: "La loi d'Ohm",
    summary: "La loi d'Ohm relie tension, courant et résistance pour un conducteur ohmique.",
    formula: "U = R × I",
    detail: "La tension U est exprimée en volts, la résistance R en ohms et le courant I en ampères. Pour une résistance de 100 Ω traversée par 20 mA, la tension vaut 2 V.",
    question: "Quelle tension obtient-on avec 100 Ω et 20 mA ?",
    answers: ["0,2 V", "2 V", "20 V", "200 V"],
    correct: 1
  },
  {
    id: 20,
    cat: "Électronique",
    level: "Intermédiaire",
    title: "Le filtre passe-bas RC",
    summary: "Un filtre RC passe-bas atténue les fréquences élevées par rapport aux fréquences basses.",
    formula: "fc = 1 / (2πRC)",
    detail: "La fréquence de coupure dépend de la résistance et de la capacité. Pour un filtre du premier ordre, le gain est inférieur d'environ 3 dB à sa valeur en bande passante à la fréquence de coupure.",
    question: "Si R augmente et C reste constant, la fréquence de coupure :",
    answers: ["Augmente", "Diminue", "Reste identique", "Devient négative"],
    correct: 1
  },
  {
    id: 21,
    cat: "Électronique",
    level: "Intermédiaire",
    title: "Le convertisseur analogique-numérique",
    summary: "Un ADC transforme une tension analogique en valeur numérique.",
    formula: "2^N niveaux pour N bits",
    detail: "Un convertisseur idéal de 10 bits possède 1024 codes numériques. Sa résolution en tension dépend de la plage de conversion et du nombre de bits.",
    question: "Combien de codes possède un ADC de 10 bits ?",
    answers: ["256", "512", "1024", "2048"],
    correct: 2
  },
  {
    id: 22,
    cat: "IoT",
    level: "Débutant",
    title: "Qu'est-ce que l'IoT ?",
    summary: "L'Internet des objets connecte des équipements physiques capables de mesurer, communiquer ou agir.",
    formula: "Capteur → Réseau → Traitement → Action",
    detail: "Un système IoT peut combiner capteurs, microcontrôleur, communication sans fil, serveur et interface utilisateur. Les contraintes importantes incluent l'autonomie, la sécurité et la fiabilité.",
    question: "Quel composant mesure une grandeur physique ?",
    answers: ["Un capteur", "Un routeur uniquement", "Un écran uniquement", "Une antenne uniquement"],
    correct: 0
  },
  {
    id: 23,
    cat: "IoT",
    level: "Intermédiaire",
    title: "Le protocole MQTT",
    summary: "MQTT est un protocole de messagerie léger utilisant le modèle publication/abonnement.",
    formula: "Publisher → Broker → Subscriber",
    detail: "Les équipements publient des messages sur des sujets appelés topics. Un broker les redistribue aux clients abonnés. MQTT est fréquent dans les applications IoT.",
    question: "Quel élément distribue les messages MQTT ?",
    answers: ["Le broker", "Le condensateur", "Le convertisseur ADC", "Le quartz"],
    correct: 0
  },
  {
    id: 24,
    cat: "IoT",
    level: "Intermédiaire",
    title: "LoRa et longue portée",
    summary: "LoRa est une technologie radio conçue pour les communications à faible débit et longue portée.",
    formula: "LoRa ≠ LoRaWAN",
    detail: "LoRa désigne principalement la couche physique radio, tandis que LoRaWAN définit un protocole réseau utilisant LoRa. La portée réelle dépend du terrain, de la réglementation, des antennes et du bilan de liaison.",
    question: "LoRaWAN correspond principalement à :",
    answers: ["Un protocole réseau", "Une résistance", "Un format vidéo", "Un type de batterie"],
    correct: 0
  }
];

const STORAGE_KEY = "mindscroll_rf_fr_v1";

let state;

try {
  state = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {};
} catch {
  state = {};
}

state.seen = Array.isArray(state.seen) ? state.seen : [];
state.saved = Array.isArray(state.saved) ? state.saved : [];
state.mastered = Array.isArray(state.mastered) ? state.mastered : [];
state.score = Number.isFinite(state.score) ? state.score : 0;

let currentLessons = [];

function saveState() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
  } catch (error) {
    console.warn("Sauvegarde indisponible", error);
  }
}

function shuffle(array) {
  const result = [...array];

  for (let i = result.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [result[i], result[j]] = [result[j], result[i]];
  }

  return result;
}

function updateStats() {
  document.getElementById("progress").textContent =
    state.mastered.length + " notion(s) maîtrisée(s)";

  document.getElementById("score").textContent =
    "Score : " + state.score;

  document.getElementById("savedCount").textContent =
    state.saved.length + " sauvegardée(s)";
}

function newKnowledge() {
  const category = document.getElementById("category").value;

  const pool = lessons.filter(lesson =>
    category === "Tout" || lesson.cat === category
  );

  if (pool.length === 0) {
    document.getElementById("feed").innerHTML =
      '<p class="empty">Aucune notion disponible.</p>';
    return;
  }

  // Priorité aux notions jamais affichées.
  let unseen = pool.filter(lesson => !state.seen.includes(lesson.id));

  // Quand toutes les notions ont été vues,
  // on recommence un nouveau cycle.
  if (unseen.length === 0) {
    const poolIds = new Set(pool.map(lesson => lesson.id));

    state.seen = state.seen.filter(id => !poolIds.has(id));

    unseen = [...pool];
  }

  const selected = shuffle(unseen).slice(0, 5);

  selected.forEach(lesson => {
    if (!state.seen.includes(lesson.id)) {
      state.seen.push(lesson.id);
    }
  });

  currentLessons = selected;

  saveState();
  renderFeed();
}

function renderFeed() {
  const feed = document.getElementById("feed");
  feed.innerHTML = "";

  currentLessons.forEach(lesson => {
    const card = document.createElement("article");
    card.className = "card";

    const isSaved = state.saved.includes(lesson.id);
    const isMastered = state.mastered.includes(lesson.id);

    card.innerHTML = `
      <div>
        <span class="category">${lesson.cat}</span>
        <span class="level">${lesson.level}</span>
      </div>

      <h2>${lesson.title}</h2>

      <p class="summary">${lesson.summary}</p>

      <div class="formula">${lesson.formula}</div>

      <div class="actions">
        <button class="more">📖 En savoir plus</button>
        <button class="secondary bookmark">
          ${isSaved ? "🔖 Sauvegardé" : "🔖 Sauvegarder"}
        </button>
      </div>

      <div class="details">
        <h3>📚 Explication approfondie</h3>
        <p>${lesson.detail}</p>

        <div class="quiz">
          <p>🧠 ${lesson.question}</p>
          <div class="answers"></div>
          <div class="feedback"></div>
        </div>
      </div>
    `;

    const details = card.querySelector(".details");
    const more = card.querySelector(".more");
    const bookmark = card.querySelector(".bookmark");
    const answersContainer = card.querySelector(".answers");
    const feedback = card.querySelector(".feedback");

    more.addEventListener("click", () => {
      const opened = details.style.display === "block";

      details.style.display = opened ? "none" : "block";
      more.textContent = opened
        ? "📖 En savoir plus"
        : "📕 Réduire";
    });

    bookmark.addEventListener("click", () => {
      if (state.saved.includes(lesson.id)) {
        state.saved = state.saved.filter(id => id !== lesson.id);
        bookmark.textContent = "🔖 Sauvegarder";
      } else {
        state.saved.push(lesson.id);
        bookmark.textContent = "🔖 Sauvegardé";
      }

      saveState();
      updateStats();
    });

    lesson.answers.forEach((answer, index) => {
      const button = document.createElement("button");

      button.className = "answer";
      button.textContent = answer;

      button.addEventListener("click", () => {
        if (answersContainer.dataset.answered === "true") return;

        answersContainer.dataset.answered = "true";

        const allButtons = answersContainer.querySelectorAll("button");

        allButtons.forEach(btn => {
          btn.disabled = true;
        });

        allButtons[lesson.correct].classList.add("correct");

        if (index === lesson.correct) {
          button.classList.add("correct");

          feedback.textContent =
            "✅ Bonne réponse ! " +
            (isMastered ? "Tu avais déjà maîtrisé cette notion." : "+10 points.");

          if (!state.mastered.includes(lesson.id)) {
            state.mastered.push(lesson.id);
            state.score += 10;
          }
        } else {
          button.classList.add("wrong");

          feedback.textContent =
            "❌ La bonne réponse est : " +
            lesson.answers[lesson.correct];
        }

        saveState();
        updateStats();
      });

      answersContainer.appendChild(button);
    });

    feed.appendChild(card);
  });

  updateStats();
}

newKnowledge();
</script>

</body>
</html>
