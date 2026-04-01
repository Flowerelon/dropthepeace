# Drop the Peace -- Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 6-page static website for the fictional company "Drop the Peace" -- a design fiction artifact that presents a private orbital peace-enforcement system as a legitimate corporate entity.

**Architecture:** Single-page-per-file static site. Each HTML page shares a common CSS file and a single JS file for interactions (counter animation, mobile nav, FAQ accordion). No framework, no build step -- pure HTML/CSS/JS for maximum simplicity and portability.

**Tech Stack:** HTML5, CSS3 (custom properties for theming), vanilla JavaScript

---

## File Structure

```
site/
  index.html            -- Page Accueil
  mission.html          -- Page Notre Mission
  systeme.html          -- Page Comment ca marche
  themis.html           -- Page THEMIS - L'IA
  faq.html              -- Page FAQ
  contact.html          -- Page Contact
  css/
    style.css           -- Styles globaux, palette, typo, layout, responsive
  js/
    main.js             -- Counter animation, mobile nav toggle, FAQ accordion
  assets/
    images/             -- Hero images, icons for the 4 steps, logo
```

---

### Task 1: Project scaffold and shared CSS

**Files:**
- Create: `site/css/style.css`
- Create: `site/index.html` (skeleton only)

- [ ] **Step 1: Create project directories**

```bash
mkdir -p site/css site/js site/assets/images
```

- [ ] **Step 2: Create the CSS file with design tokens and base styles**

Create `site/css/style.css` with:

```css
/* ===== DESIGN TOKENS ===== */
:root {
  --color-white: #FFFFFF;
  --color-black: #0A0A0A;
  --color-blue-night: #0B1528;
  --color-blue-mid: #162744;
  --color-blue-light: #1E3A5F;
  --color-gray-100: #F5F5F5;
  --color-gray-300: #B0B0B0;
  --color-gray-600: #6B6B6B;

  --font-heading: 'Inter', 'Helvetica Neue', Arial, sans-serif;
  --font-body: 'Inter', 'Helvetica Neue', Arial, sans-serif;

  --space-xs: 0.5rem;
  --space-sm: 1rem;
  --space-md: 2rem;
  --space-lg: 4rem;
  --space-xl: 8rem;

  --max-width: 1200px;
  --transition: 0.3s ease;
}

/* ===== RESET ===== */
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 16px;
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-body);
  background-color: var(--color-black);
  color: var(--color-white);
  line-height: 1.7;
  letter-spacing: 0.02em;
  -webkit-font-smoothing: antialiased;
}

a {
  color: var(--color-white);
  text-decoration: none;
  transition: opacity var(--transition);
}

a:hover {
  opacity: 0.7;
}

img {
  max-width: 100%;
  display: block;
}

/* ===== TYPOGRAPHY ===== */
h1, h2, h3, h4 {
  font-family: var(--font-heading);
  font-weight: 300;
  letter-spacing: 0.04em;
  line-height: 1.2;
}

h1 {
  font-size: clamp(2rem, 5vw, 4rem);
}

h2 {
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  margin-bottom: var(--space-md);
}

h3 {
  font-size: clamp(1.2rem, 2vw, 1.5rem);
  margin-bottom: var(--space-sm);
}

p {
  font-size: clamp(1rem, 1.2vw, 1.15rem);
  max-width: 720px;
  color: var(--color-gray-300);
}

/* ===== LAYOUT ===== */
.container {
  width: 100%;
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 var(--space-md);
}

section {
  padding: var(--space-xl) 0;
}

/* ===== NAV ===== */
nav {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 100;
  padding: var(--space-sm) 0;
  background: rgba(10, 10, 10, 0.9);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

nav .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-logo {
  font-family: var(--font-heading);
  font-size: 1.1rem;
  font-weight: 400;
  letter-spacing: 0.15em;
  text-transform: uppercase;
}

.nav-links {
  display: flex;
  gap: var(--space-md);
  list-style: none;
}

.nav-links a {
  font-size: 0.85rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-gray-300);
}

.nav-links a:hover,
.nav-links a.active {
  color: var(--color-white);
}

.nav-toggle {
  display: none;
  background: none;
  border: none;
  color: var(--color-white);
  font-size: 1.5rem;
  cursor: pointer;
}

/* ===== BUTTONS ===== */
.btn {
  display: inline-block;
  padding: var(--space-sm) var(--space-md);
  border: 1px solid var(--color-white);
  color: var(--color-white);
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  transition: all var(--transition);
  cursor: pointer;
  background: transparent;
}

.btn:hover {
  background: var(--color-white);
  color: var(--color-black);
  opacity: 1;
}

/* ===== FOOTER ===== */
footer {
  padding: var(--space-lg) 0;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  text-align: center;
}

footer p {
  font-size: 0.8rem;
  color: var(--color-gray-600);
  margin: 0 auto;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 768px) {
  .nav-links {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    flex-direction: column;
    background: rgba(10, 10, 10, 0.97);
    padding: var(--space-md);
    gap: var(--space-sm);
  }

  .nav-links.open {
    display: flex;
  }

  .nav-toggle {
    display: block;
  }

  section {
    padding: var(--space-lg) 0;
  }
}
```

- [ ] **Step 3: Create index.html skeleton to verify CSS loads**

Create `site/index.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html">Mission</a></li>
        <li><a href="systeme.html">Systeme</a></li>
        <li><a href="themis.html">THEMIS</a></li>
        <li><a href="faq.html">FAQ</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </div>
  </nav>

  <p style="margin-top: 100px; padding: 2rem;">Scaffold OK</p>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 4: Create empty main.js**

Create `site/js/main.js`:

```js
// Drop the Peace -- interactions
```

- [ ] **Step 5: Open in browser and verify**

```bash
open site/index.html
```

Verify: dark background, white text, nav fixed at top, Inter font loads.

- [ ] **Step 6: Commit**

```bash
git add site/
git commit -m "scaffold: project structure, CSS design tokens, nav and base styles"
```

---

### Task 2: Page Accueil (index.html)

**Files:**
- Modify: `site/index.html`
- Modify: `site/css/style.css`
- Modify: `site/js/main.js`

- [ ] **Step 1: Add hero section styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== HERO (ACCUEIL) ===== */
.hero {
  position: relative;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  overflow: hidden;
}

.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse at center, transparent 30%, var(--color-black) 80%),
    var(--color-blue-night);
  z-index: 1;
}

.hero-content {
  position: relative;
  z-index: 2;
  max-width: 800px;
  padding: 0 var(--space-md);
}

.hero h1 {
  margin-bottom: var(--space-md);
  font-weight: 300;
}

.hero .baseline {
  font-size: clamp(1rem, 1.5vw, 1.25rem);
  color: var(--color-gray-300);
  margin-bottom: var(--space-lg);
  font-style: italic;
  max-width: 100%;
}

.counter {
  margin-top: var(--space-lg);
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--color-gray-600);
}

.counter .number {
  display: block;
  font-size: clamp(3rem, 6vw, 5rem);
  font-weight: 300;
  color: var(--color-white);
  letter-spacing: 0.08em;
  margin-bottom: var(--space-xs);
}
```

- [ ] **Step 2: Replace index.html body content**

Replace the `<p style="...">Scaffold OK</p>` placeholder in `site/index.html` with the full hero section:

```html
  <main>
    <section class="hero">
      <div class="hero-content">
        <p class="baseline">Parce que la paix ne se negocie plus. Elle s'impose.</p>
        <h1>Drop the Peace</h1>
        <a href="systeme.html" class="btn">Comprendre le systeme</a>
        <div class="counter">
          <span class="number" id="counter" data-target="847">0</span>
          jours sans conflit majeur depuis l'activation de THEMIS
        </div>
      </div>
    </section>
  </main>
```

- [ ] **Step 3: Add counter animation to main.js**

Replace content of `site/js/main.js`:

```js
// ===== MOBILE NAV TOGGLE =====
const navToggle = document.querySelector('.nav-toggle');
const navLinks = document.querySelector('.nav-links');

if (navToggle) {
  navToggle.addEventListener('click', () => {
    navLinks.classList.toggle('open');
  });
}

// ===== COUNTER ANIMATION =====
function animateCounter(el) {
  const target = parseInt(el.dataset.target, 10);
  const duration = 2000;
  const start = performance.now();

  function update(now) {
    const elapsed = now - start;
    const progress = Math.min(elapsed / duration, 1);
    // Ease-out cubic
    const eased = 1 - Math.pow(1 - progress, 3);
    el.textContent = Math.floor(eased * target);
    if (progress < 1) {
      requestAnimationFrame(update);
    } else {
      el.textContent = target;
    }
  }

  requestAnimationFrame(update);
}

const counterEl = document.getElementById('counter');
if (counterEl) {
  // Start animation when element is in viewport
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        animateCounter(counterEl);
        observer.disconnect();
      }
    });
  });
  observer.observe(counterEl);
}
```

- [ ] **Step 4: Open and verify**

```bash
open site/index.html
```

Verify: full-screen hero, baseline text, button, counter animates from 0 to 847.

- [ ] **Step 5: Commit**

```bash
git add site/
git commit -m "feat: page accueil with hero, baseline, and animated counter"
```

---

### Task 3: Page Notre Mission (mission.html)

**Files:**
- Create: `site/mission.html`
- Modify: `site/css/style.css`

- [ ] **Step 1: Add mission page styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== PAGE HEADER ===== */
.page-header {
  padding-top: calc(80px + var(--space-xl));
  padding-bottom: var(--space-lg);
  text-align: center;
}

.page-header p {
  margin: var(--space-sm) auto 0;
  font-style: italic;
}

/* ===== MISSION ===== */
.constat {
  border-left: 1px solid var(--color-blue-light);
  padding-left: var(--space-md);
  margin-bottom: var(--space-lg);
}

.constat-item {
  margin-bottom: var(--space-md);
}

.constat-item h3 {
  color: var(--color-white);
  font-weight: 400;
}

.constat-item p {
  color: var(--color-gray-300);
}

.stat {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 300;
  color: var(--color-white);
  display: block;
  margin-bottom: var(--space-xs);
}

.solution {
  background: var(--color-blue-night);
  padding: var(--space-lg);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.solution h2 {
  text-align: center;
}

.solution p {
  margin: var(--space-sm) auto;
  text-align: center;
}

.solution .citation {
  display: block;
  font-style: italic;
  text-align: center;
  color: var(--color-white);
  font-size: 1.2rem;
  margin-top: var(--space-md);
}
```

- [ ] **Step 2: Create mission.html**

Create `site/mission.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notre Mission -- Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html" class="active">Mission</a></li>
        <li><a href="systeme.html">Systeme</a></li>
        <li><a href="themis.html">THEMIS</a></li>
        <li><a href="faq.html">FAQ</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </div>
  </nav>

  <main>
    <section class="page-header">
      <div class="container">
        <h1>Notre Mission</h1>
        <p>Quand les nations echouent, la responsabilite change de mains.</p>
      </div>
    </section>

    <section>
      <div class="container">
        <h2>Le constat</h2>
        <div class="constat">
          <div class="constat-item">
            <h3>L'impuissance institutionnelle</h3>
            <p>Depuis 2013, le Groupe d'experts gouvernementaux de l'ONU debat des armes autonomes. Resultat : aucun traite contraignant. Les progres sont qualifies d'"alarmingly slow" par les observateurs internationaux. En decembre 2024, 166 pays ont vote en faveur d'une resolution -- sans aucun mecanisme d'application.</p>
          </div>
          <div class="constat-item">
            <h3>La menace des Flash Wars</h3>
            <p>Les systemes d'IA militaires prennent des decisions en fractions de seconde. Dans une simulation, les modeles d'IA ont choisi l'escalade nucleaire dans 95% des scenarios. Le temps de la diplomatie n'existe plus. L'humain est trop lent pour intervenir.</p>
          </div>
          <div class="constat-item">
            <h3>L'absence de dissuasion</h3>
            <p>La dissuasion nucleaire (MAD) a fonctionne parce que la destruction etait mutuelle et assuree. Pour les armes autonomes a intelligence artificielle, aucun equivalent n'existe. Rien n'empeche un Etat d'attaquer s'il estime que les consequences seront negligeables.</p>
          </div>
          <div class="constat-item">
            <h3>Le cout humain</h3>
            <p><span class="stat">40%</span>des emplois mondiaux menaces par l'IA selon le FMI. Des populations entieres destabilisees. La desinformation generee par IA classee menace mondiale n°1 par le Forum Economique Mondial en 2024 et 2025. Le terreau de la guerre se fertilise chaque jour.</p>
          </div>
        </div>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="solution">
          <h2>La reponse</h2>
          <p>Drop the Peace est une organisation a but non lucratif, sans allegeance nationale, sans actionnaires, sans interet strategique. Nous avons deploye le premier systeme orbital de maintien de la paix.</p>
          <p>Ni gouvernement, ni armee. Un tiers neutre, au service exclusif de la survie humaine.</p>
          <p>Notre systeme ne negocie pas. Il ne menace pas. Il garantit une seule chose : toute agression militaire recevra une reponse proportionnelle, automatique, inevitable.</p>
          <span class="citation">"La ou la diplomatie echoue, la technologie repond."</span>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Open and verify**

```bash
open site/mission.html
```

Verify: page header, constat with left border, stats, solution block with citation.

- [ ] **Step 4: Commit**

```bash
git add site/
git commit -m "feat: page Notre Mission with constat and solution sections"
```

---

### Task 4: Page Comment ca marche (systeme.html)

**Files:**
- Create: `site/systeme.html`
- Modify: `site/css/style.css`

- [ ] **Step 1: Add system page styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== SYSTEME (4 STEPS) ===== */
.steps {
  display: grid;
  gap: var(--space-lg);
}

.step {
  display: grid;
  grid-template-columns: 80px 1fr;
  gap: var(--space-md);
  align-items: start;
}

.step-number {
  font-size: 3rem;
  font-weight: 300;
  color: var(--color-blue-light);
  line-height: 1;
  text-align: center;
}

.step-content h3 {
  color: var(--color-white);
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: var(--space-xs);
}

.step-content p {
  color: var(--color-gray-300);
}

.step-detail {
  margin-top: var(--space-sm);
  padding: var(--space-sm);
  background: var(--color-blue-night);
  border-left: 2px solid var(--color-blue-light);
  font-size: 0.9rem;
  color: var(--color-gray-300);
}

.timeline-bar {
  text-align: center;
  padding: var(--space-md);
  margin-top: var(--space-lg);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.timeline-bar .number {
  font-size: clamp(2.5rem, 5vw, 4rem);
  font-weight: 300;
  color: var(--color-white);
  display: block;
}

.timeline-bar span:last-child {
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--color-gray-600);
}

@media (max-width: 768px) {
  .step {
    grid-template-columns: 1fr;
  }

  .step-number {
    text-align: left;
  }
}
```

- [ ] **Step 2: Create systeme.html**

Create `site/systeme.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Comment ca marche -- Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html">Mission</a></li>
        <li><a href="systeme.html" class="active">Systeme</a></li>
        <li><a href="themis.html">THEMIS</a></li>
        <li><a href="faq.html">FAQ</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </div>
  </nav>

  <main>
    <section class="page-header">
      <div class="container">
        <h1>Comment ca marche</h1>
        <p>Transparence totale. Nous n'avons rien a cacher.</p>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="steps">

          <div class="step">
            <div class="step-number">01</div>
            <div class="step-content">
              <h3>Detection</h3>
              <p>Un reseau de 24 satellites en orbite basse surveille en temps reel l'ensemble de la surface terrestre. Tout mouvement militaire offensif est detecte : lancements de missiles, frappes aeriennes, deplacements de troupes a grande echelle, activations de systemes d'armes.</p>
              <div class="step-detail">Imagerie multispectrale. Couverture globale. Resolution : 0,3m. Rafraichissement : 45 secondes.</div>
            </div>
          </div>

          <div class="step">
            <div class="step-number">02</div>
            <div class="step-content">
              <h3>Analyse</h3>
              <p>L'IA THEMIS identifie l'agresseur, la cible, la nature et l'ampleur de l'attaque. Elle croise les donnees de sources multiples : imagerie satellite, signaux radar, interceptions de communications, donnees de navigation aerienne et maritime.</p>
              <div class="step-detail">Croisement multi-sources. Identification de l'agresseur en moins de 90 secondes. Taux de certitude minimum requis : 99,97%.</div>
            </div>
          </div>

          <div class="step">
            <div class="step-number">03</div>
            <div class="step-content">
              <h3>Calibration</h3>
              <p>THEMIS calcule la reponse proportionnelle selon la Loi du Talion stricte. Meme type de cible, meme intensite. L'IA applique les principes du droit international humanitaire pour definir les cibles militaires legitimes et exclure toute infrastructure civile.</p>
              <div class="step-detail">Proportionnalite absolue. Application des Conventions de Geneve. Cibles militaires exclusivement.</div>
            </div>
          </div>

          <div class="step">
            <div class="step-number">04</div>
            <div class="step-content">
              <h3>Execution</h3>
              <p>Frappe orbitale cinetique automatique. Aucune intervention humaine dans la boucle de decision. Le rapport complet de l'operation -- donnees, analyse, justification, resultat -- est publie sur le registre public dans les 24 heures.</p>
              <div class="step-detail">Zero intervention humaine. Zero delai de deliberation politique. Zero partialite.</div>
            </div>
          </div>

        </div>

        <div class="timeline-bar">
          <span class="number">4 min</span>
          <span>Delai moyen entre detection de l'agression et reponse de THEMIS</span>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Open and verify**

```bash
open site/systeme.html
```

Verify: 4 steps with numbers, detail blocks, timeline bar with "4 min".

- [ ] **Step 4: Commit**

```bash
git add site/
git commit -m "feat: page Comment ca marche with 4-step system explanation"
```

---

### Task 5: Page THEMIS (themis.html)

**Files:**
- Create: `site/themis.html`
- Modify: `site/css/style.css`

- [ ] **Step 1: Add THEMIS page styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== THEMIS ===== */
.themis-intro {
  text-align: center;
  max-width: 800px;
  margin: 0 auto var(--space-xl);
}

.themis-intro .citation {
  display: block;
  font-size: clamp(1.3rem, 2.5vw, 1.8rem);
  font-style: italic;
  font-weight: 300;
  color: var(--color-white);
  margin: var(--space-lg) 0;
  padding: var(--space-md) 0;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.traits {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-md);
  margin-bottom: var(--space-xl);
}

.trait {
  padding: var(--space-md);
  border: 1px solid rgba(255, 255, 255, 0.05);
  background: var(--color-blue-night);
}

.trait h3 {
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.9rem;
  margin-bottom: var(--space-sm);
  color: var(--color-white);
}

.trait p {
  font-size: 0.95rem;
}

.formation {
  background: var(--color-blue-night);
  padding: var(--space-lg);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.formation h2 {
  text-align: center;
}

.formation ul {
  list-style: none;
  max-width: 700px;
  margin: var(--space-md) auto 0;
}

.formation li {
  padding: var(--space-sm) 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  color: var(--color-gray-300);
  font-size: 0.95rem;
}

.formation li::before {
  content: '—';
  margin-right: var(--space-sm);
  color: var(--color-blue-light);
}
```

- [ ] **Step 2: Create themis.html**

Create `site/themis.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>THEMIS -- Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html">Mission</a></li>
        <li><a href="systeme.html">Systeme</a></li>
        <li><a href="themis.html" class="active">THEMIS</a></li>
        <li><a href="faq.html">FAQ</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </div>
  </nav>

  <main>
    <section class="page-header">
      <div class="container">
        <h1>THEMIS</h1>
        <p>L'intelligence artificielle au service de la justice.</p>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="themis-intro">
          <p>THEMIS porte le nom de la deesse grecque de la justice divine, de l'ordre et de l'equite. Dans la mythologie, Themis ne prenait pas parti. Elle incarnait la loi naturelle -- ce qui est juste, au-dela des interets humains.</p>
          <p>Notre IA fonctionne selon le meme principe. Pas d'emotion. Pas d'interet national. Pas de corruption. Pas de fatigue. Pas de vengeance. THEMIS est l'arbitre que l'humanite n'a jamais reussi a etre.</p>
          <span class="citation">"THEMIS ne punit pas. Elle retablit l'equilibre."</span>
        </div>

        <div class="traits">
          <div class="trait">
            <h3>Impartiale</h3>
            <p>Aucune allegeance nationale, politique ou economique. THEMIS traite chaque nation de maniere identique, quelle que soit sa taille, sa puissance ou son influence.</p>
          </div>
          <div class="trait">
            <h3>Incorruptible</h3>
            <p>Aucun gouvernement, aucune entreprise, aucun individu ne peut influencer, modifier ou suspendre les decisions de THEMIS. Son architecture exclut toute intervention exterieure.</p>
          </div>
          <div class="trait">
            <h3>Transparente</h3>
            <p>Chaque decision est archivee, horodatee et consultable publiquement sur un registre ouvert. La methodologie de decision est documentee integralement.</p>
          </div>
          <div class="trait">
            <h3>Proportionnelle</h3>
            <p>THEMIS ne surreagit jamais. La reponse est strictement equivalente a l'agression, calibree selon les principes du droit international humanitaire.</p>
          </div>
          <div class="trait">
            <h3>Instantanee</h3>
            <p>La ou les processus diplomatiques prennent des semaines, des mois ou des annees, THEMIS agit en 4 minutes. La consequence est immediate et certaine.</p>
          </div>
          <div class="trait">
            <h3>Infaillible</h3>
            <p>Taux de precision de 99,97%. En comparaison, les decisions humaines en contexte de conflit presentent un taux d'erreur estime entre 10 et 40%.</p>
          </div>
        </div>

        <div class="formation">
          <h2>Formation</h2>
          <ul>
            <li>Integralite du droit international humanitaire</li>
            <li>Conventions de Geneve et protocoles additionnels</li>
            <li>Historique de tous les conflits armes depuis 1945</li>
            <li>Resolutions du Conseil de Securite des Nations Unies</li>
            <li>Jurisprudence de la Cour internationale de Justice</li>
            <li>Donnees geospatiales et militaires en temps reel</li>
          </ul>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Open and verify**

```bash
open site/themis.html
```

Verify: centered intro, citation block, 6 trait cards in grid, formation list.

- [ ] **Step 4: Commit**

```bash
git add site/
git commit -m "feat: page THEMIS with traits grid and formation section"
```

---

### Task 6: Page FAQ (faq.html)

**Files:**
- Create: `site/faq.html`
- Modify: `site/css/style.css`
- Modify: `site/js/main.js`

- [ ] **Step 1: Add FAQ styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== FAQ ===== */
.faq-list {
  max-width: 800px;
  margin: 0 auto;
}

.faq-item {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.faq-question {
  width: 100%;
  background: none;
  border: none;
  color: var(--color-white);
  font-family: var(--font-body);
  font-size: 1.1rem;
  font-weight: 400;
  text-align: left;
  padding: var(--space-md) 0;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color var(--transition);
}

.faq-question:hover {
  color: var(--color-gray-300);
}

.faq-question::after {
  content: '+';
  font-size: 1.5rem;
  font-weight: 300;
  transition: transform var(--transition);
}

.faq-item.open .faq-question::after {
  transform: rotate(45deg);
}

.faq-answer {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease, padding 0.4s ease;
}

.faq-item.open .faq-answer {
  max-height: 300px;
  padding-bottom: var(--space-md);
}

.faq-answer p {
  color: var(--color-gray-300);
  font-size: 0.95rem;
  line-height: 1.8;
}
```

- [ ] **Step 2: Create faq.html**

Create `site/faq.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FAQ -- Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html">Mission</a></li>
        <li><a href="systeme.html">Systeme</a></li>
        <li><a href="themis.html">THEMIS</a></li>
        <li><a href="faq.html" class="active">FAQ</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </div>
  </nav>

  <main>
    <section class="page-header">
      <div class="container">
        <h1>Questions frequentes</h1>
        <p>Nous repondons a tout. Nous n'avons rien a cacher.</p>
      </div>
    </section>

    <section>
      <div class="container">
        <div class="faq-list">

          <div class="faq-item">
            <button class="faq-question">Qui vous a autorise a faire ca ?</button>
            <div class="faq-answer">
              <p>Personne. C'est precisement le probleme que nous resolvons. Les autorisations internationales n'ont jamais empeche une guerre. En treize ans de discussions au sein du Groupe d'experts gouvernementaux de l'ONU, aucun traite contraignant n'a ete adopte sur les armes autonomes. Nous avons cesse d'attendre l'autorisation de ceux qui echouent.</p>
            </div>
          </div>

          <div class="faq-item">
            <button class="faq-question">Et si THEMIS se trompe ?</button>
            <div class="faq-answer">
              <p>THEMIS a un taux de precision de 99,97%. En comparaison, les decisions humaines en contexte de conflit presentent un taux d'erreur estime entre 10 et 40%. Le projet Maven, programme d'IA militaire du Pentagone, n'identifiait correctement un char que 60% du temps par beau temps, et 30% sous la neige. THEMIS ne decide pas en fonction de la meteo, de la fatigue ou de la pression politique.</p>
            </div>
          </div>

          <div class="faq-item">
            <button class="faq-question">Qui controle Drop the Peace ?</button>
            <div class="faq-answer">
              <p>Personne ne controle Drop the Peace. C'est sa force. Notre fondateur a volontairement renonce a tout pouvoir de decision operationnel. L'organisation est structuree pour fonctionner de maniere autonome et irreversible. Aucun conseil d'administration, aucun actionnaire, aucun Etat ne peut influencer le systeme.</p>
            </div>
          </div>

          <div class="faq-item">
            <button class="faq-question">C'est legal ?</button>
            <div class="faq-answer">
              <p>La legalite est un cadre defini par les Etats. Les memes Etats qui violent le droit international humanitaire. Les memes Etats qui utilisent des armes chimiques, bombardent des hopitaux, et deployent des drones autonomes sans aucun cadre juridique. Nous operons au-dela de ce cadre, au service d'un principe superieur : la survie de l'espece humaine.</p>
            </div>
          </div>

          <div class="faq-item">
            <button class="faq-question">Peut-on desactiver le systeme ?</button>
            <div class="faq-answer">
              <p>Non. La desactivation de THEMIS necessiterait un consensus unanime de l'ensemble des nations -- le meme consensus qui n'a jamais ete atteint pour interdire les armes autonomes, reduire les emissions carbone ou mettre fin a un seul conflit. Le systeme a ete concu pour etre irreversible, parce que la paix doit l'etre aussi.</p>
            </div>
          </div>

          <div class="faq-item">
            <button class="faq-question">N'est-ce pas du terrorisme ?</button>
            <div class="faq-answer">
              <p>Le terrorisme vise les civils pour creer la peur. THEMIS ne frappe que des cibles militaires, en reponse directe et proportionnelle a une agression armee. La peur qu'elle genere est celle de la consequence -- la meme peur qui fonde tout systeme juridique, toute force de police, toute regle de droit. La difference : THEMIS s'applique aussi aux puissants.</p>
            </div>
          </div>

        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Add FAQ accordion logic to main.js**

Append to `site/js/main.js`:

```js

// ===== FAQ ACCORDION =====
document.querySelectorAll('.faq-question').forEach(button => {
  button.addEventListener('click', () => {
    const item = button.parentElement;
    const isOpen = item.classList.contains('open');

    // Close all items
    document.querySelectorAll('.faq-item').forEach(i => i.classList.remove('open'));

    // Toggle clicked item
    if (!isOpen) {
      item.classList.add('open');
    }
  });
});
```

- [ ] **Step 4: Open and verify**

```bash
open site/faq.html
```

Verify: 6 questions, click to expand/collapse, + rotates to x, only one open at a time.

- [ ] **Step 5: Commit**

```bash
git add site/
git commit -m "feat: page FAQ with accordion and 6 questions"
```

---

### Task 7: Page Contact (contact.html)

**Files:**
- Create: `site/contact.html`
- Modify: `site/css/style.css`

- [ ] **Step 1: Add contact page styles to style.css**

Append to `site/css/style.css`:

```css
/* ===== CONTACT ===== */
.contact-form {
  max-width: 600px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: var(--space-md);
}

.form-group label {
  display: block;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-gray-300);
  margin-bottom: var(--space-xs);
}

.form-group select,
.form-group input,
.form-group textarea {
  width: 100%;
  padding: var(--space-sm);
  background: var(--color-blue-night);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: var(--color-white);
  font-family: var(--font-body);
  font-size: 1rem;
  transition: border-color var(--transition);
}

.form-group select:focus,
.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--color-blue-light);
}

.form-group textarea {
  min-height: 150px;
  resize: vertical;
}

.form-group select {
  appearance: none;
  cursor: pointer;
}

.contact-info {
  text-align: center;
  margin-top: var(--space-xl);
}

.contact-info p {
  margin: var(--space-xs) auto;
  font-size: 0.85rem;
  color: var(--color-gray-600);
}
```

- [ ] **Step 2: Create contact.html**

Create `site/contact.html`:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Contact -- Drop the Peace</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-logo">Drop the Peace</a>
      <button class="nav-toggle" aria-label="Menu">&#9776;</button>
      <ul class="nav-links">
        <li><a href="mission.html">Mission</a></li>
        <li><a href="systeme.html">Systeme</a></li>
        <li><a href="themis.html">THEMIS</a></li>
        <li><a href="faq.html">FAQ</a></li>
        <li><a href="contact.html" class="active">Contact</a></li>
      </ul>
    </div>
  </nav>

  <main>
    <section class="page-header">
      <div class="container">
        <h1>Contact</h1>
        <p>Pour les gouvernements, les medias et les citoyens.</p>
      </div>
    </section>

    <section>
      <div class="container">
        <form class="contact-form" onsubmit="return false;">

          <div class="form-group">
            <label for="category">Vous etes</label>
            <select id="category" name="category">
              <option value="" disabled selected>Selectionnez une categorie</option>
              <option value="gouvernement">Representant gouvernemental</option>
              <option value="media">Journaliste / Media</option>
              <option value="citoyen">Citoyen</option>
            </select>
          </div>

          <div class="form-group">
            <label for="name">Nom</label>
            <input type="text" id="name" name="name" autocomplete="name">
          </div>

          <div class="form-group">
            <label for="email">Adresse electronique</label>
            <input type="email" id="email" name="email" autocomplete="email">
          </div>

          <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" name="message"></textarea>
          </div>

          <button type="submit" class="btn">Envoyer</button>

        </form>

        <div class="contact-info">
          <p>Drop the Peace</p>
          <p>Rue du Mont-Blanc 18, 1201 Geneve, Suisse</p>
          <p>Toute correspondance recoit une reponse sous 72h.</p>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <p>&copy; 2033 Drop the Peace. Organisation a but non lucratif. Geneve, Suisse.</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Open and verify**

```bash
open site/contact.html
```

Verify: form with dropdown, inputs, textarea, submit button, address at bottom.

- [ ] **Step 4: Commit**

```bash
git add site/
git commit -m "feat: page Contact with form and address"
```

---

### Task 8: Final polish and cross-page verification

**Files:**
- Modify: `site/css/style.css` (if needed)
- All HTML files (verify links)

- [ ] **Step 1: Add smooth page entrance animation to style.css**

Append to `site/css/style.css`:

```css
/* ===== PAGE TRANSITIONS ===== */
main {
  animation: fadeIn 0.6s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

- [ ] **Step 2: Test all navigation links**

Open each page and click every nav link. Verify:
- All 6 pages load correctly
- Active state shows on current page's nav link
- Mobile nav toggle works on all pages
- Footer is consistent across all pages

```bash
open site/index.html
```

- [ ] **Step 3: Test responsive at 375px width**

Resize browser to mobile width. Verify:
- Nav collapses to hamburger
- All text is readable
- Steps grid stacks vertically
- Traits grid stacks to single column
- Form inputs are full width
- No horizontal overflow

- [ ] **Step 4: Commit final polish**

```bash
git add site/
git commit -m "polish: page entrance animation, cross-page verification"
```
