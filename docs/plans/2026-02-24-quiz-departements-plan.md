# Quiz Départements — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** A single-file French departments quiz game with two modes (full game and single question), tolerant input matching, and clean animations.

**Architecture:** One `index.html` file containing all HTML, CSS, and JS. The app manages screens (home, game, single, results) by toggling visibility of `<section>` elements. Game state is held in a simple JS object.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript (ES6+). No dependencies.

---

### Task 1: HTML skeleton + home screen + base CSS

**Files:**
- Create: `index.html`

**Step 1: Create the HTML structure**

Write `index.html` with:
- `<!DOCTYPE html>`, lang="fr", UTF-8 meta, viewport meta
- `<title>Quiz Départements</title>`
- 4 sections (only home visible initially):
  - `#home` — title "Quiz Départements", two buttons: "Partie complète (101 départements)" and "Un département"
  - `#game` — hidden: number display `#dept-number`, input `#answer-input`, attempt counter `#attempts`, progress bar + score
  - `#single` — hidden: number display, input, result area, buttons "Encore un" / "Retour"
  - `#results` — hidden: score display, missed list `#missed-list`, "Recommencer" button
- `<style>` block in `<head>`
- `<script>` block before `</body>`

**Step 2: Write base CSS**

In the `<style>` block:
- `* { box-sizing: border-box; margin: 0; padding: 0; }`
- `body`: font-family system-ui sans-serif, background `#fff`, color `#1a1a1a`, min-height 100vh, display flex, justify/align center
- `section`: hidden by default (`display: none`), when `.active` → `display: flex`, flex-direction column, align-items center, gap
- `#home`: two buttons styled as large rounded rectangles, subtle border, hover effect
- `.dept-number`: font-size ~6rem, font-weight 800, letter-spacing, margin
- `input`: large font (~1.5rem), padding, border 2px solid `#ddd`, border-radius, width 100%, max-width 400px, text-align center, outline none, transition border-color
- `.progress-bar`: width 100%, max-width 400px, height 4px, background `#eee`, border-radius. Inner `.progress-fill`: height 100%, background `#1a1a1a`, transition width 0.3s
- `.score-text`: font-size 0.9rem, color `#999`, margin-top 0.5rem

**Step 3: Wire up screen navigation in JS**

```js
const sections = document.querySelectorAll('section');
function showScreen(id) {
  sections.forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}
showScreen('home');
document.getElementById('btn-full').addEventListener('click', () => startFullGame());
document.getElementById('btn-single').addEventListener('click', () => startSingle());
```

Stub `startFullGame()` and `startSingle()` to just call `showScreen('game')` / `showScreen('single')`.

**Step 4: Verify in browser**

Open `index.html` in browser. Verify:
- Home screen displays centered with title and 2 buttons
- Clicking "Partie complète" shows the game screen
- Clicking "Un département" shows the single screen
- Style is minimal and clean

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: HTML skeleton with home screen and base CSS"
```

---

### Task 2: Départements data

**Files:**
- Modify: `index.html` (script section)

**Step 1: Add the DEPARTEMENTS object**

At the top of the `<script>` block, add the complete data object with all 101 entries:

```js
const DEPARTEMENTS = {
  "01": "Ain", "02": "Aisne", "03": "Allier", "04": "Alpes-de-Haute-Provence",
  "05": "Hautes-Alpes", "06": "Alpes-Maritimes", "07": "Ardèche", "08": "Ardennes",
  "09": "Ariège", "10": "Aube", "11": "Aude", "12": "Aveyron",
  "13": "Bouches-du-Rhône", "14": "Calvados", "15": "Cantal", "16": "Charente",
  "17": "Charente-Maritime", "18": "Cher", "19": "Corrèze",
  "2A": "Corse-du-Sud", "2B": "Haute-Corse",
  "21": "Côte-d'Or", "22": "Côtes-d'Armor", "23": "Creuse", "24": "Dordogne",
  "25": "Doubs", "26": "Drôme", "27": "Eure", "28": "Eure-et-Loir",
  "29": "Finistère", "30": "Gard", "31": "Haute-Garonne", "32": "Gers",
  "33": "Gironde", "34": "Hérault", "35": "Ille-et-Vilaine", "36": "Indre",
  "37": "Indre-et-Loire", "38": "Isère", "39": "Jura", "40": "Landes",
  "41": "Loir-et-Cher", "42": "Loire", "43": "Haute-Loire", "44": "Loire-Atlantique",
  "45": "Loiret", "46": "Lot", "47": "Lot-et-Garonne", "48": "Lozère",
  "49": "Maine-et-Loire", "50": "Manche", "51": "Marne", "52": "Haute-Marne",
  "53": "Mayenne", "54": "Meurthe-et-Moselle", "55": "Meuse", "56": "Morbihan",
  "57": "Moselle", "58": "Nièvre", "59": "Nord", "60": "Oise",
  "61": "Orne", "62": "Pas-de-Calais", "63": "Puy-de-Dôme", "64": "Pyrénées-Atlantiques",
  "65": "Hautes-Pyrénées", "66": "Pyrénées-Orientales", "67": "Bas-Rhin", "68": "Haut-Rhin",
  "69": "Rhône", "70": "Haute-Saône", "71": "Saône-et-Loire", "72": "Sarthe",
  "73": "Savoie", "74": "Haute-Savoie", "75": "Paris", "76": "Seine-Maritime",
  "77": "Seine-et-Marne", "78": "Yvelines", "79": "Deux-Sèvres", "80": "Somme",
  "81": "Tarn", "82": "Tarn-et-Garonne", "83": "Var", "84": "Vaucluse",
  "85": "Vendée", "86": "Vienne", "87": "Haute-Vienne", "88": "Vosges",
  "89": "Yonne", "90": "Territoire de Belfort", "91": "Essonne",
  "92": "Hauts-de-Seine", "93": "Seine-Saint-Denis", "94": "Val-de-Marne",
  "95": "Val-d'Oise",
  "971": "Guadeloupe", "972": "Martinique", "973": "Guyane",
  "974": "La Réunion", "976": "Mayotte"
};
```

**Step 2: Add shuffle utility**

```js
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}
```

**Step 3: Verify**

Open browser console, type `Object.keys(DEPARTEMENTS).length` → should return `101`.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add 101 départements data and shuffle utility"
```

---

### Task 3: Normalize + Levenshtein comparison

**Files:**
- Modify: `index.html` (script section)

**Step 1: Write normalize function**

```js
function normalize(str) {
  return str
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')  // remove diacritics
    .replace(/['\-\s]+/g, '');         // remove apostrophes, hyphens, spaces
}
```

**Step 2: Write Levenshtein distance function**

```js
function levenshtein(a, b) {
  const m = a.length, n = b.length;
  const dp = Array.from({ length: m + 1 }, (_, i) => [i]);
  for (let j = 1; j <= n; j++) dp[0][j] = j;
  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      dp[i][j] = a[i-1] === b[j-1]
        ? dp[i-1][j-1]
        : 1 + Math.min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]);
    }
  }
  return dp[m][n];
}
```

**Step 3: Write checkAnswer function**

```js
function checkAnswer(input, expected) {
  return levenshtein(normalize(input), normalize(expected)) <= 2;
}
```

**Step 4: Verify in console**

Test cases:
- `checkAnswer("cote d armor", "Côtes-d'Armor")` → `true`
- `checkAnswer("cottes d'armor", "Côtes-d'Armor")` → `true`
- `checkAnswer("lot et garone", "Lot-et-Garonne")` → `true`
- `checkAnswer("paris", "Paris")` → `true`
- `checkAnswer("seine", "Paris")` → `false`

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add normalize and Levenshtein-based answer checking"
```

---

### Task 4: Full game mode (core logic, no animations)

**Files:**
- Modify: `index.html` (script section)

**Step 1: Implement game state and core loop**

```js
let gameState = {
  queue: [],       // shuffled array of [code, name]
  current: 0,      // index in queue
  attempts: 0,     // 0, 1, or 2 (0-indexed)
  score: 0,        // correct answers
  missed: []       // [{code, name}] for recap
};

function startFullGame() {
  gameState.queue = shuffle(Object.entries(DEPARTEMENTS));
  gameState.current = 0;
  gameState.attempts = 0;
  gameState.score = 0;
  gameState.missed = [];
  showScreen('game');
  showCurrentDept();
}

function showCurrentDept() {
  const [code, name] = gameState.queue[gameState.current];
  document.getElementById('dept-number').textContent = code;
  document.getElementById('answer-input').value = '';
  document.getElementById('answer-input').focus();
  document.getElementById('attempts').textContent = `Tentative ${gameState.attempts + 1}/3`;
  updateProgress();
}

function updateProgress() {
  const total = gameState.queue.length;
  const done = gameState.current;
  document.getElementById('progress-fill').style.width = `${(done / total) * 100}%`;
  document.getElementById('score-text').textContent = `${done} / ${total}`;
}
```

**Step 2: Handle answer submission**

```js
document.getElementById('answer-input').addEventListener('keydown', (e) => {
  if (e.key === 'Enter') handleAnswer();
});

function handleAnswer() {
  const input = document.getElementById('answer-input');
  const value = input.value.trim();
  if (!value) return;

  const [code, name] = gameState.queue[gameState.current];

  if (checkAnswer(value, name)) {
    gameState.score++;
    onCorrectAnswer(name);
  } else {
    gameState.attempts++;
    if (gameState.attempts >= 3) {
      gameState.missed.push({ code, name });
      onRevealAnswer(name);
    } else {
      onWrongAnswer();
    }
  }
}

function nextDept() {
  gameState.current++;
  gameState.attempts = 0;
  if (gameState.current >= gameState.queue.length) {
    showResults();
  } else {
    showCurrentDept();
  }
}
```

**Step 3: Stub feedback functions (no animation yet)**

```js
function onCorrectAnswer(name) {
  setTimeout(nextDept, 1500);
}
function onWrongAnswer() {
  document.getElementById('answer-input').value = '';
  document.getElementById('attempts').textContent = `Tentative ${gameState.attempts + 1}/3`;
}
function onRevealAnswer(name) {
  document.getElementById('reveal-text').textContent = name;
  document.getElementById('reveal-text').style.display = 'block';
  setTimeout(() => {
    document.getElementById('reveal-text').style.display = 'none';
    nextDept();
  }, 2000);
}
```

**Step 4: Add `#reveal-text` element to HTML**

In the `#game` section, add `<p id="reveal-text" style="display:none"></p>` styled in red.

**Step 5: Verify in browser**

- Start full game → number appears
- Type correct name → moves to next after 1.5s
- Type wrong 3 times → reveals answer, moves to next

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: implement full game mode core logic"
```

---

### Task 5: Results screen

**Files:**
- Modify: `index.html` (script section + HTML)

**Step 1: Implement showResults()**

```js
function showResults() {
  showScreen('results');
  document.getElementById('final-score').textContent = `${gameState.score} / ${gameState.queue.length}`;

  const missedList = document.getElementById('missed-list');
  missedList.innerHTML = '';

  if (gameState.missed.length === 0) {
    missedList.innerHTML = '<p>Aucune erreur, bravo !</p>';
  } else {
    gameState.missed.forEach(({ code, name }) => {
      const li = document.createElement('li');
      li.textContent = `${code} — ${name}`;
      missedList.appendChild(li);
    });
  }
}
```

**Step 2: Wire up "Recommencer" button**

```js
document.getElementById('btn-restart').addEventListener('click', () => showScreen('home'));
```

**Step 3: Style results section**

- `#final-score`: large font, centered
- `#missed-list`: left-aligned list, styled as `<ul>`, max-height with scroll if long
- `.missed-title`: small heading "Départements ratés :" above the list (hidden if none)

**Step 4: Verify**

Play through a few departments (answer wrong 3 times to force misses), verify results screen shows score and missed list.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add results screen with score and missed departments list"
```

---

### Task 6: Single department mode

**Files:**
- Modify: `index.html` (script section)

**Step 1: Implement single mode logic**

```js
function startSingle() {
  showScreen('single');
  pickRandomSingle();
}

function pickRandomSingle() {
  const keys = Object.keys(DEPARTEMENTS);
  const code = keys[Math.floor(Math.random() * keys.length)];
  const name = DEPARTEMENTS[code];
  document.getElementById('single-number').textContent = code;
  document.getElementById('single-input').value = '';
  document.getElementById('single-input').focus();
  document.getElementById('single-result').textContent = '';
  document.getElementById('single-result').className = 'result-text';
  document.getElementById('single-input').disabled = false;

  // Store current answer
  document.getElementById('single-input').dataset.answer = name;
}
```

**Step 2: Handle single mode submission**

```js
document.getElementById('single-input').addEventListener('keydown', (e) => {
  if (e.key === 'Enter') handleSingleAnswer();
});

function handleSingleAnswer() {
  const input = document.getElementById('single-input');
  if (input.disabled) return;
  const value = input.value.trim();
  if (!value) return;

  const expected = input.dataset.answer;
  const result = document.getElementById('single-result');

  if (checkAnswer(value, expected)) {
    result.textContent = `Correct ! C'est bien ${expected}`;
    result.className = 'result-text correct';
    spawnConfetti(document.getElementById('single'));
  } else {
    result.textContent = `Raté ! C'était ${expected}`;
    result.className = 'result-text wrong';
  }
  input.disabled = true;
}
```

**Step 3: Wire up "Encore un" and "Retour" buttons**

```js
document.getElementById('btn-another').addEventListener('click', pickRandomSingle);
document.getElementById('btn-back').addEventListener('click', () => showScreen('home'));
```

**Step 4: Verify**

- Click "Un département" → number appears
- Type answer → result shown (correct green or wrong red with answer)
- "Encore un" → new random department
- "Retour" → back to home

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add single department quiz mode"
```

---

### Task 7: Animations (confetti, shake, color feedback)

**Files:**
- Modify: `index.html` (CSS + script)

**Step 1: Add CSS animations**

```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-8px); }
  40%, 80% { transform: translateX(8px); }
}
.shake { animation: shake 0.4s ease; }

@keyframes confetti-fall {
  0% { transform: translateY(-10px) rotate(0deg); opacity: 1; }
  100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
}
.confetti {
  position: fixed;
  width: 8px;
  height: 8px;
  top: -10px;
  z-index: 1000;
  pointer-events: none;
}

.input-correct { border-color: #22c55e !important; }
.input-wrong { border-color: #ef4444 !important; }
.number-correct { color: #22c55e; }
```

**Step 2: Implement confetti spawn function**

```js
function spawnConfetti(container) {
  const colors = ['#22c55e', '#3b82f6', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899'];
  for (let i = 0; i < 25; i++) {
    const el = document.createElement('div');
    el.className = 'confetti';
    el.style.left = Math.random() * 100 + 'vw';
    el.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
    el.style.animation = `confetti-fall ${1 + Math.random() * 1}s ease-out forwards`;
    el.style.animationDelay = Math.random() * 0.5 + 's';
    el.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 2500);
  }
}
```

**Step 3: Update feedback functions with animations**

Replace the stub functions from Task 4:

```js
function onCorrectAnswer(name) {
  const input = document.getElementById('answer-input');
  const number = document.getElementById('dept-number');
  input.classList.add('input-correct');
  number.classList.add('number-correct');
  spawnConfetti();
  setTimeout(() => {
    input.classList.remove('input-correct');
    number.classList.remove('number-correct');
    nextDept();
  }, 1500);
}

function onWrongAnswer() {
  const input = document.getElementById('answer-input');
  input.classList.add('shake', 'input-wrong');
  input.value = '';
  document.getElementById('attempts').textContent = `Tentative ${gameState.attempts + 1}/3`;
  setTimeout(() => input.classList.remove('shake', 'input-wrong'), 1000);
}

function onRevealAnswer(name) {
  const input = document.getElementById('answer-input');
  input.classList.add('input-wrong');
  input.disabled = true;
  const reveal = document.getElementById('reveal-text');
  reveal.textContent = name;
  reveal.style.display = 'block';
  setTimeout(() => {
    reveal.style.display = 'none';
    input.classList.remove('input-wrong');
    input.disabled = false;
    nextDept();
  }, 2000);
}
```

**Step 4: Verify in browser**

- Correct answer → green border on input, green number, confetti falls
- Wrong answer → shake + red border, fades after 1s
- 3rd fail → red reveal text, moves to next after 2s
- Single mode → confetti on correct, red text on wrong

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add confetti, shake, and color feedback animations"
```

---

### Task 8: Polish & final verification

**Files:**
- Modify: `index.html`

**Step 1: Disable input during transitions**

Prevent submitting during the 1.5s/2s transition delays. Add `input.disabled = true` in `onCorrectAnswer` and `onRevealAnswer`, re-enable in `showCurrentDept`.

**Step 2: Handle edge cases**

- Empty input: already handled (return early if empty)
- Rapid Enter key: disabled input prevents double submission
- Focus management: `showCurrentDept()` and `pickRandomSingle()` call `.focus()` on input

**Step 3: Add subtle fade transition between departments**

```css
.fade-in { animation: fadeIn 0.3s ease; }
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

Apply `.fade-in` class to the number element when showing a new department.

**Step 4: Full playtest**

- Play through 5-10 departments in full mode
- Test all 3 attempts cycle
- Verify results screen
- Test single mode
- Test "Recommencer" and "Retour" navigation
- Verify progress bar reaches 100% at end

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: polish transitions, input guards, and edge cases"
```
