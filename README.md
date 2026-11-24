# Ou-exclusif
test
Voici une **simulation interactive complète et autonome** d’une porte logique **OU exclusif (XOR)**.

commande terminal: python -m http.server

Elle fonctionne directement dans votre navigateur : copiez le code ci-dessous dans un fichier `xor.html`, ouvrez-le, et cliquez sur les boutons pour basculer les entrées **A** et **B**. La sortie, les lignes de signal, le symbole de la porte et la table de vérité se mettent à jour en temps réel.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Simulation interactive — XOR</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500&family=Inter:wght@300;600&display=swap');

  body {
    margin: 0; padding: 2rem;
    background: radial-gradient(circle at top left, #1e293b, #0f172a);
    color: #e2e8f0;
    font-family: 'Inter', system-ui, sans-serif;
    display: flex; flex-direction: column; align-items: center; min-height: 100vh;
  }

  h1 {
    font-family: 'Orbitron', sans-serif;
    color: #38bdf8;
    font-size: 2rem;
    letter-spacing: 0.05em;
    text-shadow: 0 0 20px rgba(56,189,248,0.3);
    margin-bottom: 0.25rem;
  }

  .subtitle {
    color: #94a3b8;
    margin-bottom: 2rem;
    max-width: 600px;
    text-align: center;
    line-height: 1.5;
  }

  .card {
    background: rgba(30,41,59,0.7);
    backdrop-filter: blur(12px);
    border: 1px solid rgba(56,189,248,0.15);
    border-radius: 1.5rem;
    padding: 2rem 2.5rem;
    max-width: 800px;
    width: 100%;
    box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
  }

  /* Circuit */
  .circuit {
    display: flex; align-items: center; justify-content: center;
    gap: 1rem; margin: 2rem 0;
  }

  .node { text-align: center; }
  .node-label {
    font-size: 0.75rem; color: #94a3b8; letter-spacing: 0.05em;
    text-transform: uppercase; margin-top: 0.25rem;
  }

  .btn {
    margin-top: 0.5rem;
    padding: 0.45rem 1rem;
    border: none; border-radius: 0.5rem;
    background: linear-gradient(135deg, #38bdf8, #0ea5e9);
    color: #0f172a; font-weight: 700; font-family: inherit;
    cursor: pointer; box-shadow: 0 4px 15px rgba(56,189,248,0.3);
    transition: transform 0.1s, box-shadow 0.2s;
  }
  .btn:active { transform: translateY(1px); }
  .btn:hover { box-shadow: 0 6px 20px rgba(56,189,248,0.5); }

  /* Truth table */
  table {
    width: 100%; border-collapse: collapse;
    margin-top: 1.5rem;
  }
  th, td {
    padding: 0.75rem 1rem; text-align: center;
    border-bottom: 1px solid #334155;
  }
  th { color: #38bdf8; font-family: 'Orbitron', sans-serif; letter-spacing: 0.05em; }
  tr.active { background: rgba(56,189,248,0.08); font-weight: 700; color: #38bdf8; }
  td:first-child, td:nth-child(2) { font-family: monospace; font-size: 1.1rem; }

  .explain {
    margin-top: 1.5rem; padding: 1rem; border-left: 4px solid #10b981;
    background: rgba(16,185,129,0.08); border-radius: 0.5rem;
  }
  .explain strong { color: #10b981; }
</style>
</head>
<body>

<div class="card">
  <h1>OU EXCLUSIF — XOR</h1>
  <p class="subtitle">
    La porte <strong>XOR</strong> (symbole ⊕) renvoie <strong>1</strong> uniquement lorsque ses deux entrées sont <strong>différentes</strong>.
    Basculer A ou B ci-dessous pour observer le comportement en temps réel.
  </p>

  <div class="circuit">
    <!-- Entrée A -->
    <div class="node">
      <svg width="80" height="80" viewBox="0 0 80 80">
        <circle id="circA" cx="40" cy="40" r="28" fill="#334155" stroke="#475569" stroke-width="3" style="transition: all .3s;"/>
        <text x="40" y="45" text-anchor="middle" fill="#94a3b8" font-size="16" font-family="Orbitron" font-weight="500" id="txtA">0</text>
      </svg>
      <button class="btn" onclick="toggle('A')">Basculer A</button>
      <div class="node-label">Entrée A</div>
    </div>

    <!-- Lignes -->
    <svg width="140" height="120" viewBox="0 0 140 120" style="overflow:visible;">
      <!-- Ligne A -->
      <line id="lineA" x1="0" y1="30" x2="120" y2="30" stroke="#475569" stroke-width="4" stroke-linecap="round" style="transition: stroke .3s;"/>
      <!-- Ligne B -->
      <line id="lineB" x1="0" y1="90" x2="120" y2="90" stroke="#475569" stroke-width="4" stroke-linecap="round" style="transition: stroke .3s;"/>
    </svg>

    <!-- Porte XOR -->
    <div class="node">
      <svg width="120" height="120" viewBox="0 0 120 120">
        <!-- Corps principal (forme OR) -->
        <path d="M 30,25 Q 90,15 110,60 Q 90,105 30,95 Z" fill="#1e293b" stroke="#38bdf8" stroke-width="3"/>
        <!-- Courbe exclusive XOR -->
        <path d="M 15,35 Q 30,25 30,60 T 15,85" fill="none" stroke="#38bdf8" stroke-width="3"/>
        <text x="60" y="68" text-anchor="middle" fill="#38bdf8" font-size="28" font-family="Orbitron">⊕</text>
      </svg>
      <div class="node-label">Porte XOR</div>
    </div>

    <!-- Lignes de sortie -->
    <svg width="100" height="120" viewBox="0 0 100 120" style="overflow:visible;">
      <line id="lineOut" x1="0" y1="60" x2="80" y2="60" stroke="#475569" stroke-width="4" stroke-linecap="round" style="transition: stroke .3s;"/>
    </svg>

    <!-- Sortie Y -->
    <div class="node">
      <svg width="80" height="80" viewBox="0 0 80 80">
        <circle id="circY" cx="40" cy="40" r="28" fill="#334155" stroke="#475569" stroke-width="3" style="transition: all .3s;"/>
        <text x="40" y="45" text-anchor="middle" fill="#94a3b8" font-size="16" font-family="Orbitron" font-weight="500" id="txtY">0</text>
      </svg>
      <div class="node-label">Sortie Y = A ⊕ B</div>
    </div>
  </div>

  <h2 style="font-family:'Orbitron'; color:#38bdf8; font-size:1.2rem; margin-top:1rem;">Table de vérité</h2>
  <table>
    <thead>
      <tr><th>A</th><th>B</th><th>A ⊕ B</th></tr>
    </thead>
    <tbody>
      <tr id="r00"><td>0</td><td>0</td><td>0</td></tr>
      <tr id="r01"><td>0</td><td>1</td><td>1</td></tr>
      <tr id="r10"><td>1</td><td>0</td><td>1</td></tr>
      <tr id="r11"><td>1</td><td>1</td><td>0</td></tr>
    </tbody>
  </table>

  <div class="explain">
    <strong>Comment lire la simulation :</strong> Les cercles s’illuminent en vert (<code>#10b981</code>) quand la valeur est <strong>1</strong>. Les lignes de transmission deviennent lumineuses. La table de vérité surligne la ligne active correspondant à l’état actuel de A et B.
  </div>
</div>

<script>
  let A = 0, B = 0;

  function toggle(input) {
    if (input === 'A') A = A ? 0 : 1;
    if (input === 'B') B = B ? 0 : 1;
    update();
  }

  function update() {
    const Y = A ^ B;

    // Mise à jour des cercles et textes
    const colorOn = '#10b981', strokeOn = '#34d399', textOn = '#fff';
    const colorOff = '#334155', strokeOff = '#475569', textOff = '#94a3b8';

    const setCircle = (id, val) => {
      const c = document.getElementById(id);
      c.setAttribute('fill', val ? colorOn : colorOff);
      c.setAttribute('stroke', val ? strokeOn : strokeOff);
    };

    setCircle('circA', A); setCircle('circB', B); setCircle('circY', Y);
    document.getElementById('txtA').textContent = A;
    document.getElementById('txtB').textContent = B;
    document.getElementById('txtY').textContent = Y;

    // Lignes
    document.getElementById('lineA').setAttribute('stroke', A ? '#10b981' : '#475569');
    document.getElementById('lineB').setAttribute('stroke', B ? '#10b981' : '#475569');
    document.getElementById('lineOut').setAttribute('stroke', Y ? '#10b981' : '#475569');

    // Table de vérité : surligner la ligne active
    ['00','01','10','11'].forEach(k => document.getElementById('r'+k).classList.remove('active'));
    document.getElementById('r'+A+B).classList.add('active');
  }

  // Initialisation
  update();
</script>

</body>
</html>
```

### Ce que fait cette simulation
- **Entrées interactives** : deux boutons basculent A et B entre 0 et 1.
- **Visualisation du signal** : les lignes entre les nœuds changent de couleur (gris → vert lumineux) selon l’état logique.
- **Porte XOR animée** : le symbole ⊕ au centre reste visible, avec sa courbe distinctive qui différencie le XOR du simple OR.
- **Sortie en temps réel** : le cercle de droite affiche immédiatement le résultat du calcul `A ^ B`.
- **Table de vérité dynamique** : la ligne correspondante au couple (A,B) est surlignée automatiquement.

Vous pouvez l’utiliser pour expliquer le XOR en cours, tester des combinaisons, ou même l’étendre (par exemple en chaînant plusieurs portes pour simuler un demi-additionneur).