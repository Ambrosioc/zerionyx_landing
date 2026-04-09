# Landing Page Refonte Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite all copy for a human tone, polish visual design, fix bugs, and apply improvements across all pages.

**Architecture:** Progressive component-by-component updates. Each task modifies one or two files, keeping changes isolated. No structural changes to the Astro project — same components, same routing.

**Tech Stack:** Astro, vanilla CSS, vanilla JS

---

## File Map

| File | Action | Responsibility |
|------|--------|---------------|
| `src/layouts/Layout.astro` | Modify | Fix meta description bug |
| `src/styles/global.css` | Modify | Polish transitions, spacing |
| `src/components/Hero.astro` | Modify | Copy rewrite + crossfade + dots + timer |
| `src/components/Nav.astro` | Modify | Remove emoji from CTA |
| `src/components/SocialProofBar.astro` | Modify | Minor copy polish |
| `src/components/Problem.astro` | Modify | Copy rewrite all 6 cards |
| `src/components/Solution.astro` | Modify | Copy rewrite 3 steps |
| `src/components/Features.astro` | Modify | Fix typos, copy polish, tabs scroll mobile |
| `src/components/Benefits.astro` | Modify | Copy polish |
| `src/components/Offer.astro` | Modify | Copy rewrite, remove emoji CTA |
| `src/components/Pricing.astro` | Modify | Responsive swipe hint |
| `src/components/Faq.astro` | Modify | Copy rewrite answers, touch area |
| `src/components/FinalCta.astro` | Modify | Title + button copy only |
| `src/components/Footer.astro` | Modify | Remove emoji from sticky CTA |
| `src/components/UrgencyBar.astro` | Delete | Not wanted |
| `src/pages/index.astro` | Modify | Fix counter min, remove UrgencyBar import if present |
| `src/pages/fonctionnalites.astro` | Modify | Copy rewrite, remove emoji CTA |
| `src/pages/tarifs.astro` | Modify | Copy rewrite hero, remove emoji CTA |
| `src/pages/faq.astro` | Modify | Copy rewrite answers, remove emoji CTA |

---

### Task 1: Bug fixes — Layout meta + counter min

**Files:**
- Modify: `src/layouts/Layout.astro:16` — meta description already uses `{description}` correctly (confirmed on read). No change needed.
- Modify: `src/pages/index.astro:63-69` — add min check on counter

- [ ] **Step 1: Fix counter minimum in index.astro**

In `src/pages/index.astro`, replace:

```js
  // Fake countdown spots
  let spots = 47;
  setInterval(() => {
    if (spots > 43 && Math.random() < 0.07) {
      spots--;
      const cnt = document.getElementById('cnt');
      const barSpots = document.getElementById('bar-spots');
      if (cnt) cnt.textContent = String(spots);
      if (barSpots) barSpots.textContent = String(spots);
    }
  }, 9000);
```

With:

```js
  // Fake countdown spots
  let spots = 47;
  setInterval(() => {
    if (spots > 43 && Math.random() < 0.07) {
      spots--;
      const cnt = document.getElementById('cnt');
      const barSpots = document.getElementById('bar-spots');
      if (cnt) cnt.textContent = String(Math.max(43, spots));
      if (barSpots) barSpots.textContent = String(Math.max(43, spots));
    }
  }, 9000);
```

- [ ] **Step 2: Verify dev server runs**

Run: `npm run dev` and check the page loads at localhost.

---

### Task 2: Delete UrgencyBar

**Files:**
- Delete: `src/components/UrgencyBar.astro`

- [ ] **Step 1: Delete UrgencyBar.astro**

```bash
rm src/components/UrgencyBar.astro
```

- [ ] **Step 2: Verify no imports reference it**

```bash
grep -r "UrgencyBar" src/
```

Expected: no results (it's not imported anywhere currently).

---

### Task 3: Global CSS polish

**Files:**
- Modify: `src/styles/global.css`

- [ ] **Step 1: Update transition speeds and reveal stagger**

In `src/styles/global.css`, replace:

```css
.btn-hero-primary:hover{background:var(--blue-dark);transform:translateY(-2px);box-shadow:0 8px 28px rgba(26,111,212,0.4)}
```

With:

```css
.btn-hero-primary:hover{background:var(--blue-dark);transform:translateY(-2px);box-shadow:0 8px 28px rgba(26,111,212,0.4);transition:all .3s ease}
```

- [ ] **Step 2: Update reveal animation timing**

In `src/styles/global.css`, replace:

```css
.reveal{opacity:0;transform:translateY(24px);transition:opacity .65s ease,transform .65s ease}
```

With:

```css
.reveal{opacity:0;transform:translateY(20px);transition:opacity .6s ease,transform .6s ease}
```

- [ ] **Step 3: Update stagger in index.astro**

In `src/pages/index.astro`, replace:

```js
        if (e.isIntersecting) {
          setTimeout(() => e.target.classList.add('visible'), i * 70);
```

With:

```js
        if (e.isIntersecting) {
          setTimeout(() => e.target.classList.add('visible'), i * 50);
```

---

### Task 4: Hero — Copy rewrite + crossfade + timer + dots

**Files:**
- Modify: `src/components/Hero.astro`

- [ ] **Step 1: Rewrite Hero copy and add crossfade + dots**

Replace the entire content of `src/components/Hero.astro` with:

```astro
<section class="hero">
  <div class="hero-orb1"></div>
  <div class="hero-orb2"></div>
  <div class="hero-orb3"></div>

  <div class="title-v active" id="tv-a">
    <h1 class="hero-title">Vos fiches papier vous coûtent <span class="t-gradient">3h par semaine.</span><br>On a la solution.</h1>
  </div>
  <div class="title-v" id="tv-b">
    <h1 class="hero-title">Fiche remplie, signée, envoyée en PDF.<br><span class="t-green">En 2 minutes.</span> Depuis votre tel.</h1>
  </div>
  <div class="title-v" id="tv-c">
    <h1 class="hero-title">Vous êtes <span class="t-gradient">1,2 million</span> à bosser<br>encore sur papier. Il est temps d'arrêter.</h1>
  </div>

  <p class="hero-sub">Remplissez vos fiches d'intervention sur le téléphone, faites signer le client sur place, et le PDF part tout seul. Pas besoin de formation, ça marche en 2 minutes.</p>

  <div class="hero-cta-wrap">
    <div class="hero-cta-row">
      <a href="#inscription" class="btn-hero-primary">Essayer gratuitement</a>
    </div>
    <div class="hero-reassure">
      <div class="hr-item"><div class="hr-dot"></div>Gratuit pendant la bêta</div>
      <div class="hr-item"><div class="hr-dot"></div>Sans engagement</div>
      <div class="hr-item"><div class="hr-dot"></div>iOS & Android</div>
      <div class="hr-item"><div class="hr-dot"></div>Réponse sous 24h</div>
    </div>
    <div class="hero-dots">
      <button class="hero-dot active" data-slide="0"></button>
      <button class="hero-dot" data-slide="1"></button>
      <button class="hero-dot" data-slide="2"></button>
    </div>
  </div>
</section>

<style>
  .hero{
    position:relative;z-index:1;
    padding:80px 48px 100px;text-align:center;overflow:hidden;
  }
  .hero-orb1{
    position:absolute;width:700px;height:700px;border-radius:50%;
    background:radial-gradient(circle,rgba(26,111,212,0.1) 0%,transparent 65%);
    top:-200px;left:50%;transform:translateX(-50%);pointer-events:none;
  }
  .hero-orb2{
    position:absolute;width:400px;height:400px;border-radius:50%;
    background:radial-gradient(circle,rgba(0,208,132,0.07) 0%,transparent 65%);
    bottom:-100px;right:5%;pointer-events:none;
  }
  .hero-orb3{
    position:absolute;width:300px;height:300px;border-radius:50%;
    background:radial-gradient(circle,rgba(0,194,255,0.06) 0%,transparent 65%);
    top:20%;left:2%;pointer-events:none;
  }
  .hero-title{
    font-family:'Space Grotesk',sans-serif;
    font-size:clamp(38px,6vw,72px);font-weight:700;
    letter-spacing:-2px;line-height:1.04;
    margin-bottom:22px;max-width:880px;margin-left:auto;margin-right:auto;
  }
  .title-v{
    display:none;opacity:0;
    transition:opacity .5s ease;
  }
  .title-v.active{
    display:block;opacity:1;
    animation:fadeUp .5s ease both;
  }
  .hero-sub{
    font-size:clamp(16px,2vw,19px);color:var(--text2);
    max-width:560px;margin:0 auto 40px;font-weight:400;line-height:1.7;
    animation:fadeUp .6s .3s ease both;
  }
  .hero-cta-wrap{animation:fadeUp .6s .4s ease both}
  .hero-cta-row{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin-bottom:16px}
  .hero-reassure{display:flex;align-items:center;justify-content:center;gap:20px;flex-wrap:wrap}
  .hr-item{display:flex;align-items:center;gap:6px;font-size:13px;color:var(--text3)}
  .hr-dot{width:6px;height:6px;border-radius:50%;background:var(--green);box-shadow:0 0 6px rgba(0,208,132,0.5)}
  .hero-dots{
    display:flex;gap:8px;justify-content:center;margin-top:24px;
  }
  .hero-dot{
    width:8px;height:8px;border-radius:50%;border:none;cursor:pointer;
    background:var(--border2);transition:all .3s;padding:0;
  }
  .hero-dot.active{
    background:var(--blue);width:24px;border-radius:4px;
  }

  @media(max-width:900px){
    .hero{padding:60px 16px 72px}
    .hero-cta-row{flex-direction:column;align-items:center}
  }
</style>

<script>
  const slides = [
    document.getElementById('tv-a'),
    document.getElementById('tv-b'),
    document.getElementById('tv-c'),
  ];
  const dots = document.querySelectorAll('.hero-dot');
  let current = 0;
  let timer;

  function showSlide(index) {
    slides.forEach((s, i) => {
      if (s) {
        s.classList.remove('active');
        s.style.display = 'none';
      }
    });
    dots.forEach(d => d.classList.remove('active'));
    const next = slides[index];
    if (next) {
      next.style.display = 'block';
      // Force reflow for animation
      void next.offsetWidth;
      next.classList.add('active');
    }
    dots[index]?.classList.add('active');
    current = index;
  }

  function nextSlide() {
    showSlide((current + 1) % slides.length);
  }

  function startTimer() {
    clearInterval(timer);
    timer = setInterval(nextSlide, 5000);
  }

  dots.forEach(dot => {
    dot.addEventListener('click', () => {
      const idx = parseInt(dot.dataset.slide || '0');
      showSlide(idx);
      startTimer();
    });
  });

  startTimer();
</script>
```

---

### Task 5: Nav — Remove emoji from CTA

**Files:**
- Modify: `src/components/Nav.astro`

- [ ] **Step 1: Remove emoji from nav CTA**

In `src/components/Nav.astro`, replace:

```html
    <a href="/#inscription" class="btn-nav">🚀 S'inscrire</a>
```

With:

```html
    <a href="/#inscription" class="btn-nav">S'inscrire</a>
```

- [ ] **Step 2: Remove emoji from mobile nav CTA**

In `src/components/Nav.astro`, replace:

```html
  <a href="/#inscription" class="nav-mobile-cta">🚀 S'inscrire</a>
```

With:

```html
  <a href="/#inscription" class="nav-mobile-cta">S'inscrire</a>
```

---

### Task 6: Problem — Copy rewrite

**Files:**
- Modify: `src/components/Problem.astro`

- [ ] **Step 1: Rewrite section header**

In `src/components/Problem.astro`, replace:

```html
      <h2 class="section-title">Vous perdez du temps<br>et de l'argent chaque jour.</h2>
      <p class="section-sub">On a parlé à des dizaines d'artisans. Voici ce qu'ils vivent :</p>
```

With:

```html
      <h2 class="section-title">Vous le vivez tous les jours.<br>On le sait.</h2>
      <p class="section-sub">On a discuté avec des dizaines d'artisans. Voilà ce qu'ils nous racontent :</p>
```

- [ ] **Step 2: Rewrite all 6 problem cards**

In `src/components/Problem.astro`, replace the entire `prob-grid` div (lines 8-15):

```html
    <div class="prob-grid">
      <div class="prob-card reveal"><div class="prob-icon">🔍</div><div class="prob-title">La fiche introuvable</div><div class="prob-body">20 minutes à chercher. Votre client attend. Votre journée tombe à l'eau.</div><div class="prob-quote">"J'ai cherché partout, impossible de retrouver la fiche de l'an dernier..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📋</div><div class="prob-title">Aucune trace pro</div><div class="prob-body">Votre client demande un rapport. Vous n'avez rien de propre à lui envoyer.</div><div class="prob-quote">"Mon client veut un compte-rendu, j'ai juste une fiche manuscrite illisible..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">⚖️</div><div class="prob-title">Le litige sans preuve</div><div class="prob-body">Un client conteste. Pas de fiche signée. Litige impossible à gérer.</div><div class="prob-quote">"Il dit qu'on n'est pas venu, j'ai aucune preuve de l'intervention..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📱</div><div class="prob-title">Le planning sur WhatsApp</div><div class="prob-body">Coordination chaotique. Erreurs, oublis. Personne ne sait qui fait quoi.</div><div class="prob-quote">"J'ai 4 techniciens, je gère tout par WhatsApp, c'est le bordel..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📊</div><div class="prob-title">Zéro visibilité</div><div class="prob-body">Vous ne savez pas combien d'interventions vous avez faites ce mois.</div><div class="prob-quote">"Je saurais même pas vous dire combien j'ai fait d'interventions en mars..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">🧾</div><div class="prob-title">Justificatifs éparpillés</div><div class="prob-body">Votre comptable attend. Demi-journée perdue à chercher partout.</div><div class="prob-quote">"Mon comptable m'a redemandé les justificatifs. 4 heures de recherche..."</div></div>
    </div>
```

With:

```html
    <div class="prob-grid">
      <div class="prob-card reveal"><div class="prob-icon">🔍</div><div class="prob-title">Encore 20 min à chercher une fiche</div><div class="prob-body">Le client est au téléphone, il attend. Vous, vous fouinez dans un classeur. Ça vous parle ?</div><div class="prob-quote">"J'ai retourné le bureau, impossible de retrouver la fiche de l'an dernier..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📋</div><div class="prob-title">Un rapport que même vous, vous relisez pas</div><div class="prob-body">Le client demande un compte-rendu. Tout ce que vous avez, c'est une fiche remplie au stylo entre deux interventions.</div><div class="prob-quote">"Mon client veut un récap propre, j'ai juste un truc gribouillé à la va-vite..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">⚖️</div><div class="prob-title">Le client conteste, vous avez aucune preuve</div><div class="prob-body">Pas de fiche signée, pas de photo. Le client dit que le travail n'a pas été fait. Vous pouvez rien prouver.</div><div class="prob-quote">"Il dit qu'on n'est jamais passé. J'ai rien pour prouver le contraire..."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📱</div><div class="prob-title">Le planning ? C'est un groupe WhatsApp</div><div class="prob-body">Un message par-ci, un appel par-là. Personne sait qui va où. Les oublis, ça arrive toutes les semaines.</div><div class="prob-quote">"J'ai 4 techs, on gère tout sur WhatsApp... c'est le bazar permanent."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">📊</div><div class="prob-title">Aucune idée de ce que fait l'équipe</div><div class="prob-body">Combien d'interventions ce mois ? Quel tech est le plus chargé ? Vous n'en savez rien.</div><div class="prob-quote">"Je serais même pas capable de dire combien on a fait d'interventions en mars."</div></div>
      <div class="prob-card reveal"><div class="prob-icon">🧾</div><div class="prob-title">Des bouts de papier dans tous les tiroirs</div><div class="prob-body">Le comptable demande les justificatifs. Vous partez pour une demi-journée de fouille.</div><div class="prob-quote">"Mon comptable m'a relancé. J'ai passé 4 heures à chercher des bouts de papier."</div></div>
    </div>
```

- [ ] **Step 3: Add tablet 2-column breakpoint**

In `src/components/Problem.astro`, replace:

```css
  @media(max-width:900px){
    .problem-wrap{padding:64px 0}
    .problem-inner{padding:0 16px}
    .prob-grid{grid-template-columns:1fr}
  }
```

With:

```css
  @media(max-width:900px){
    .problem-wrap{padding:64px 0}
    .problem-inner{padding:0 16px}
    .prob-grid{grid-template-columns:1fr 1fr}
  }
  @media(max-width:600px){
    .prob-grid{grid-template-columns:1fr}
  }
```

---

### Task 7: Solution — Copy rewrite

**Files:**
- Modify: `src/components/Solution.astro`

- [ ] **Step 1: Rewrite section header**

In `src/components/Solution.astro`, replace:

```html
      <h2 class="section-title">En place en 10 minutes.<br>Résultats dès le premier jour.</h2>
      <p class="section-sub" style="margin:0 auto">Pas de formation. Pas de technicien. Vous créez votre compte et commencez immédiatement.</p>
```

With:

```html
      <h2 class="section-title">En place en 10 minutes.<br>Vous bossez avec dès le premier jour.</h2>
      <p class="section-sub" style="margin:0 auto">Pas de formation, pas de technicien à appeler. Vous créez votre compte et c'est parti.</p>
```

- [ ] **Step 2: Rewrite step cards**

In `src/components/Solution.astro`, replace:

```html
      <div class="step-card reveal"><div class="step-num">1</div><h3>Créez votre compte</h3><p>Inscrivez-vous, ajoutez votre logo. Importez vos clients depuis Excel ou saisissez-les à la main.</p><span class="step-time">⏱ 5 minutes</span></div>
      <div class="step-card reveal"><div class="step-num">2</div><h3>Intervenez sur le terrain</h3><p>Remplissez votre fiche sur téléphone, prenez des photos, faites signer le client sur l'écran.</p><span class="step-time">⏱ 2 minutes</span></div>
      <div class="step-card reveal"><div class="step-num">3</div><h3>L'IA fait le reste</h3><p>Le rapport PDF est généré automatiquement par l'IA et envoyé au client par email. Zéro effort.</p><span class="step-time">⏱ 0 seconde</span></div>
```

With:

```html
      <div class="step-card reveal"><div class="step-num">1</div><h3>Vous créez votre compte en 5 min</h3><p>Ajoutez votre logo, importez vos clients depuis Excel ou tapez-les à la main. C'est tout.</p><span class="step-time">⏱ 5 minutes</span></div>
      <div class="step-card reveal"><div class="step-num">2</div><h3>Sur le chantier, vous remplissez la fiche sur votre tel</h3><p>Photos, mesures, observations. Le client signe direct sur l'écran.</p><span class="step-time">⏱ 2 minutes</span></div>
      <div class="step-card reveal"><div class="step-num">3</div><h3>Le PDF se génère et part au client. C'est tout.</h3><p>Le rapport pro est envoyé par email automatiquement. Vous n'avez rien à faire.</p><span class="step-time">⏱ 0 seconde</span></div>
```

- [ ] **Step 3: Rewrite CTA button**

In `src/components/Solution.astro`, replace:

```html
      <a href="#inscription" class="btn-hero-primary" style="display:inline-flex;font-size:15px;padding:14px 32px">Démarrer maintenant — C'est gratuit →</a>
```

With:

```html
      <a href="#inscription" class="btn-hero-primary" style="display:inline-flex;font-size:15px;padding:14px 32px">Essayer gratuitement →</a>
```

---

### Task 8: Features — Fix typos + copy polish + mobile tabs

**Files:**
- Modify: `src/components/Features.astro`

- [ ] **Step 1: Fix typos in Planning tab**

In `src/components/Features.astro`, replace:

```html
        <h3>Toute l'équipe synchronisee</h3>
        <p>Planifiez depuis le dashboard web, vos techniciens voient leur planning sur mobile en temps reel. L'IA detecte les conflits et suggest des optimisations.</p>
```

With:

```html
        <h3>Toute l'équipe synchronisée</h3>
        <p>Planifiez depuis le tableau de bord, vos techniciens voient leur planning sur mobile en temps réel. Les conflits sont détectés automatiquement.</p>
```

- [ ] **Step 2: Fix typos in Planning tab features**

In `src/components/Features.astro`, replace:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Notifications push temps reel</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Statuts live (en route, sur place, termine)</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Optimisation IA des tournees</div>
```

With:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Notifications push en temps réel</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Statuts live (en route, sur place, terminé)</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Optimisation des tournées</div>
```

- [ ] **Step 3: Fix typos in Historique tab**

In `src/components/Features.astro`, replace:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Recherche instantanee par client</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Alertes IA — révisions a venir</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Acces aux fiches et photos archivées</div>
```

With:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Recherche instantanée par client</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Alertes — révisions à venir</div>
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Accès aux fiches et photos archivées</div>
```

- [ ] **Step 4: Fix typos in Photos tab**

In `src/components/Features.astro`, replace:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Stockage cloud illimite</div>
```

With:

```html
          <div class="tab-feat"><div class="tf-check"><svg width="12" height="10" viewBox="0 0 12 10" fill="none"><path d="M1 5l3.5 4L11 1" stroke="#00D084" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>Stockage cloud illimité</div>
```

- [ ] **Step 5: Fix PDF tab typo**

In `src/components/Features.astro`, replace:

```html
        <p>L'IA rédige et généré un rapport professionnel dès la signature, avec votre logo. Envoyé au client par email en automatique.</p>
```

With:

```html
        <p>Dès que le client signe, un rapport pro est généré avec votre logo et envoyé par email. Vous n'avez rien à faire.</p>
```

- [ ] **Step 6: Add horizontal scroll for mobile tabs**

In `src/components/Features.astro`, replace:

```css
  .tabs-nav{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin:40px 0 48px}
```

With:

```css
  .tabs-nav{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin:40px 0 48px}
  @media(max-width:600px){
    .tabs-nav{flex-wrap:nowrap;overflow-x:auto;justify-content:flex-start;-webkit-overflow-scrolling:touch;scrollbar-width:none;padding-bottom:4px}
    .tabs-nav::-webkit-scrollbar{display:none}
    .tab-pill{white-space:nowrap;flex-shrink:0}
  }
```

---

### Task 9: Offer — Copy rewrite

**Files:**
- Modify: `src/components/Offer.astro`

- [ ] **Step 1: Rewrite Offer copy**

In `src/components/Offer.astro`, replace:

```html
    <div class="offre-badge reveal">🎁 Offre de lancement — Limitée</div>
    <h2 class="offre-title reveal">Rejoignez les 50 premiers.<br>Obtenez 2 mois offerts.</h2>
    <p class="offre-sub reveal">Cette offre disparaît après les 50 premières inscriptions. Agissez maintenant.</p>
```

With:

```html
    <div class="offre-badge reveal">🎁 Offre de lancement — Limitée</div>
    <h2 class="offre-title reveal">Les 50 premiers inscrits<br>ont 2 mois offerts.</h2>
    <p class="offre-sub reveal">Après les 50 inscriptions, l'offre disparaît. C'est maintenant ou jamais.</p>
```

- [ ] **Step 2: Rewrite Offer CTA**

In `src/components/Offer.astro`, replace:

```html
      <a href="#inscription" class="btn-offre-main">🚀 Réserver ma place maintenant</a>
```

With:

```html
      <a href="#inscription" class="btn-offre-main">Réserver ma place maintenant</a>
```

---

### Task 10: Benefits — Copy polish

**Files:**
- Modify: `src/components/Benefits.astro`

- [ ] **Step 1: Rewrite benefit descriptions for more natural tone**

In `src/components/Benefits.astro`, replace:

```html
        <div class="ben-item"><div class="ben-ico">⏱️</div><div><div class="ben-title">3 heures récupérées par semaine</div><div class="ben-desc">L'IA automatise les rapports, les recherches, la coordination. Vous gardez le temps pour votre vrai métier.</div><span class="ben-kpi">= 12h/mois × votre taux horaire</span></div></div>
        <div class="ben-item"><div class="ben-ico">💼</div><div><div class="ben-title">Image professionnelle premium</div><div class="ben-desc">Vos clients reçoivent un rapport PDF avec votre logo dès la fin de l'intervention. Vous vous démarquez instantanément.</div><span class="ben-kpi">= Fidélisation + recommandations</span></div></div>
        <div class="ben-item"><div class="ben-ico">🛡️</div><div><div class="ben-title">Zéro litige non documenté</div><div class="ben-desc">Chaque intervention est signée, horodatée, archivée. La preuve en 2 secondes si nécessaire.</div><span class="ben-kpi">= 1 litige évité = 500€+ économisés</span></div></div>
        <div class="ben-item"><div class="ben-ico">🤖</div><div><div class="ben-title">L'IA qui anticipe pour vous</div><div class="ben-desc">Alertes de révision, anomalies détectées, optimisation des tournées. L'IA travaille pendant que vous dormez.</div><span class="ben-kpi">= Contrats entretien jamais manqués</span></div></div>
```

With:

```html
        <div class="ben-item"><div class="ben-ico">⏱️</div><div><div class="ben-title">3 heures de gagnées par semaine</div><div class="ben-desc">Plus de rapports à recopier, plus de fiches à chercher. Vous gardez ce temps pour bosser.</div><span class="ben-kpi">= 12h/mois × votre taux horaire</span></div></div>
        <div class="ben-item"><div class="ben-ico">💼</div><div><div class="ben-title">Vos clients reçoivent un vrai rapport pro</div><div class="ben-desc">PDF avec votre logo, envoyé dans la minute. Ça change la façon dont ils vous voient.</div><span class="ben-kpi">= Fidélisation + recommandations</span></div></div>
        <div class="ben-item"><div class="ben-ico">🛡️</div><div><div class="ben-title">Plus jamais de litige sans preuve</div><div class="ben-desc">Tout est signé, horodaté, archivé. Si un client conteste, vous sortez la preuve en 2 secondes.</div><span class="ben-kpi">= 1 litige évité = 500€+ économisés</span></div></div>
        <div class="ben-item"><div class="ben-ico">🤖</div><div><div class="ben-title">Les alertes font le boulot à votre place</div><div class="ben-desc">Révision à prévoir, anomalie détectée, tournée optimisée. Vous êtes prévenu avant que ça devienne un problème.</div><span class="ben-kpi">= Contrats entretien jamais oubliés</span></div></div>
```

- [ ] **Step 2: Rewrite CTA button**

In `src/components/Benefits.astro`, replace:

```html
        <a href="#inscription" class="btn-hero-primary" style="display:inline-flex;font-size:14px;padding:13px 28px">Commencer à gagner du temps →</a>
```

With:

```html
        <a href="#inscription" class="btn-hero-primary" style="display:inline-flex;font-size:14px;padding:13px 28px">Essayer gratuitement →</a>
```

---

### Task 11: FAQ homepage — Copy rewrite + touch area

**Files:**
- Modify: `src/components/Faq.astro`

- [ ] **Step 1: Rewrite FAQ answers for natural tone**

Replace the entire `faq-list` div in `src/components/Faq.astro` with updated answers. Key changes:

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">C'est vraiment gratuit pendant la bêta ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui, 100%. Toutes les fonctionnalités sont accessibles gratuitement pendant la bêta. Les 50 premiers inscrits bénéficient en plus de 2 mois offerts après le lancement. Aucune carte bancaire requise pour s'inscrire.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">C'est vraiment gratuit pendant la bêta ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui, tout est gratuit pendant la bêta. Les 50 premiers inscrits ont aussi 2 mois offerts après le lancement. On ne demande pas de carte bancaire.</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">Est-ce que ça fonctionne sur mon téléphone ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Zerionyx fonctionne sur iOS (iPhone, iPad) et Android. L'app est optimisée pour une utilisation terrain — elle fonctionne même sans réseau et synchronise automatiquement dès que vous retrouvez une connexion. Parfait pour les caves, zones rurales ou bâtiments mal couverts.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">Est-ce que ça fonctionne sur mon téléphone ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">iPhone, iPad, Android — ça marche partout. L'app fonctionne même sans réseau (en cave, en zone blanche) et synchronise toute seule dès que le réseau revient.</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">Je ne suis pas à l'aise avec la technologie. C'est compliqué ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">L'app a été conçue spécifiquement pour les artisans, pas pour les informaticiens. L'interface est aussi simple qu'un formulaire papier. En plus, Jordan — notre expert chauffagiste — vous accompagne en 30 minutes lors de l'onboarding. 10 minutes suffisent pour démarrer.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">Je suis pas à l'aise avec la technologie. C'est compliqué ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">L'app est faite pour des artisans, pas des informaticiens. C'est aussi simple qu'un formulaire papier. Et si besoin, Jordan vous fait le tour en 30 min. La plupart des gens démarrent en 10 minutes.</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">La signature électronique est-elle légalement valide ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui. Zerionyx utilise DocuSeal, conforme au règlement eIDAS européen. Chaque signature est horodatée et archivée de façon infalsifiable. En cas de litige, cette preuve est recevable devant les tribunaux français et européens.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">La signature électronique, c'est valable légalement ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui. On utilise DocuSeal, conforme eIDAS. Chaque signature est horodatée et archivée. En cas de litige, c'est recevable devant les tribunaux français et européens.</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">29€/mois c'est rentabilisé comment ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">29€/mois = 1€/jour. Un technicien à 40€/h qui gagne 3h/semaine récupère 480€/mois de valeur. Le ROI est immédiat. Sans compter les litiges évités (500€+ chacun) et l'image professionnelle qui améliore la fidélisation client.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">29€/mois, ça vaut le coup ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Ça fait 1€ par jour. Si vous facturez 40€/h et que vous gagnez 3h par semaine, ça vous rapporte 480€/mois. Sans compter les litiges que vous évitez (500€+ chacun).</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">Mes données sont-elles en sécurité ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Vos données sont hébergées en Europe (RGPD), chiffrées en transit et au repos. Vous êtes propriétaire de toutes vos données. Export et suppression possibles à tout moment.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">Mes données sont en sécurité ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Hébergées en Europe, chiffrées, conformes RGPD. Vos données vous appartiennent. Vous pouvez les exporter ou les supprimer quand vous voulez.</div></div>
```

In `src/components/Faq.astro`, replace:

```html
      <div class="faq-item"><div class="faq-q">Puis-je annuler à tout moment ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui, sans condition ni frais. Pas d'engagement minimum. Vous annulez en deux clics depuis votre espace. Vos données restent accessibles 30 jours après pour exportation.</div></div>
```

With:

```html
      <div class="faq-item"><div class="faq-q">Je peux annuler quand je veux ?<div class="faq-arr"><svg width="11" height="7" viewBox="0 0 11 7" fill="none"><path d="M1 1l4.5 5 4.5-5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg></div></div><div class="faq-a">Oui, en deux clics. Pas d'engagement, pas de frais. Vos données restent accessibles 30 jours après pour les récupérer.</div></div>
```

- [ ] **Step 2: Increase touch area for FAQ questions**

In `src/components/Faq.astro`, replace:

```css
  .faq-q{display:flex;align-items:center;justify-content:space-between;padding:17px 20px;cursor:pointer;font-family:'Space Grotesk',sans-serif;font-size:15px;font-weight:600;color:var(--text)}
```

With:

```css
  .faq-q{display:flex;align-items:center;justify-content:space-between;padding:17px 20px;cursor:pointer;font-family:'Space Grotesk',sans-serif;font-size:15px;font-weight:600;color:var(--text);min-height:48px}
```

---

### Task 12: FinalCta — Title + button copy

**Files:**
- Modify: `src/components/FinalCta.astro`

- [ ] **Step 1: Rewrite title**

In `src/components/FinalCta.astro`, replace:

```html
      <h2 class="cta-final-title">Prenez la décision<br>qui change votre quotidien.</h2>
      <p class="cta-final-sub">Rejoignez la liste d'attente et soyez parmi les premiers à utiliser Zerionyx.</p>
```

With:

```html
      <h2 class="cta-final-title">Prêt à lâcher<br>le papier ?</h2>
      <p class="cta-final-sub">Inscrivez-vous, on vous contacte dès l'ouverture de Zerionyx.</p>
```

- [ ] **Step 2: Rewrite submit button**

In `src/components/FinalCta.astro`, replace:

```html
      <button type="button" class="f-submit" id="submit-btn">🚀 Rejoindre la liste — 2 mois offerts →</button>
```

With:

```html
      <button type="button" class="f-submit" id="submit-btn">Rejoindre la liste — 2 mois offerts →</button>
```

- [ ] **Step 3: Update button text in JS error recovery**

In `src/components/FinalCta.astro`, replace:

```js
    btn.textContent = '🚀 Rejoindre la liste — 2 mois offerts →';
```

With:

```js
    btn.textContent = 'Rejoindre la liste — 2 mois offerts →';
```

---

### Task 13: Footer — Remove emoji from sticky CTA

**Files:**
- Modify: `src/components/Footer.astro`

- [ ] **Step 1: Remove emoji from sticky CTA**

In `src/components/Footer.astro`, replace:

```html
    <a href="#inscription" class="btn-nav" style="flex:1;justify-content:center;font-size:13px;padding:11px">🚀 Rejoindre la liste d'attente</a>
```

With:

```html
    <a href="#inscription" class="btn-nav" style="flex:1;justify-content:center;font-size:13px;padding:11px">Rejoindre la liste d'attente</a>
```

---

### Task 14: Page fonctionnalités — Copy rewrite

**Files:**
- Modify: `src/pages/fonctionnalites.astro`

- [ ] **Step 1: Rewrite hero**

In `src/pages/fonctionnalites.astro`, replace:

```html
    <h1 class="feat-hero-title">Toutes nos <span class="t-gradient">fonctionnalités</span></h1>
    <p class="feat-hero-sub">Tout ce dont vous avez besoin pour digitaliser vos interventions terrain. Rien de superflu.</p>
```

With:

```html
    <h1 class="feat-hero-title">Ce que <span class="t-gradient">Zerionyx</span> fait pour vous</h1>
    <p class="feat-hero-sub">Tout ce qu'il faut pour arrêter le papier et bosser plus vite. Rien en trop.</p>
```

- [ ] **Step 2: Rewrite feature descriptions in the data array**

In `src/pages/fonctionnalites.astro`, replace the features array (lines 8-57) with rewritten copy. Key changes to each feature object:

Replace:

```js
    subtitle: 'Remplissez vos fiches en 2 minutes chrono',
    description: 'Fini les formulaires papier illisibles et les fiches perdues. Zerionyx vous propose des templates intelligents adaptés à votre métier. Checkboxes, mesures, observations — tout se fait depuis votre téléphone, même sans connexion internet.',
    description2: 'L\'app fonctionne en mode hors ligne. Vous pouvez travailler en cave, en zone rurale ou dans un bâtiment mal couvert. Dès que le réseau revient, tout se synchronise automatiquement.',
```

With:

```js
    subtitle: 'Vos fiches, sur votre tel, en 2 minutes',
    description: 'Plus de formulaires papier illisibles. Vous avez des templates prêts pour votre métier : checkboxes, mesures, observations. Tout se fait sur le téléphone, même sans internet.',
    description2: 'En cave, en zone blanche, en pleine campagne — ça marche. Dès que le réseau revient, tout se synchronise tout seul.',
```

Replace:

```js
    subtitle: 'Protégez-vous avec une preuve incontestable',
    description: 'Le client signe directement sur l\'écran de votre téléphone. Chaque signature est horodatée automatiquement avec la date et l\'heure précises, puis archivée de façon sécurisée et infalsifiable.',
    description2: 'Conforme au règlement eIDAS européen, la signature Zerionyx a une valeur légale en cas de litige. Plus jamais de "il dit qu\'on n\'est pas venu" — vous avez la preuve.',
```

With:

```js
    subtitle: 'Fini les litiges sans preuve',
    description: 'Le client signe sur l\'écran de votre tel. C\'est horodaté, archivé, impossible à falsifier.',
    description2: 'Conforme eIDAS, valeur légale en Europe. Si un client dit "vous n\'êtes pas venu", vous avez la preuve que si.',
```

Replace:

```js
    subtitle: 'Impressionnez vos clients sans effort',
    description: 'Dès que le client signe, un rapport PDF professionnel est généré automatiquement avec votre logo, vos coordonnées, les détails de l\'intervention, les photos et la signature. Il est envoyé par email au client instantanément.',
    description2: 'Votre image de marque est irréprochable. Vos clients reçoivent un document pro qui vous démarque de la concurrence — et vous n\'avez strictement rien à faire.',
```

With:

```js
    subtitle: 'Un rapport pro envoyé sans lever le petit doigt',
    description: 'Dès la signature, un PDF avec votre logo, les photos, les détails — tout part par email au client. Automatiquement.',
    description2: 'Vos clients reçoivent un document propre qui change l\'image qu\'ils ont de vous. Et vous, vous n\'avez rien fait de plus.',
```

Replace:

```js
    subtitle: 'Toute l\'équipe synchronisée en temps réel',
    description: 'Planifiez les interventions depuis votre tableau de bord web. Chaque technicien voit son planning sur son téléphone et reçoit des notifications push pour les nouvelles missions ou modifications.',
    description2: 'Fini la coordination par WhatsApp ou les appels téléphoniques. Tout le monde sait qui fait quoi, où et quand. Les statuts se mettent à jour en temps réel : en route, sur place, terminé.',
```

With:

```js
    subtitle: 'Tout le monde sait qui fait quoi',
    description: 'Vous planifiez depuis le tableau de bord, vos techs voient leur planning sur le tel avec des notifications pour chaque mission.',
    description2: 'Plus besoin de WhatsApp ou d\'appels. Les statuts se mettent à jour en direct : en route, sur place, terminé.',
```

Replace:

```js
    subtitle: 'Documentez, prouvez, archivez',
    description: 'Prenez des photos directement depuis l\'app. Elles sont automatiquement classées et rattachées à la bonne intervention. Avant, pendant, après — tout est documenté et archivé.',
    description2: 'Les photos sont incluses dans le rapport PDF envoyé au client. En cas de litige ou de contrôle, vous avez une trace visuelle complète de votre travail.',
```

With:

```js
    subtitle: 'Une photo vaut mieux qu\'un long rapport',
    description: 'Prenez des photos depuis l\'app, elles se classent toutes seules. Avant, pendant, après — tout est documenté.',
    description2: 'Elles sont incluses dans le PDF envoyé au client. Si un jour on vous demande des comptes, vous avez tout.',
```

Replace:

```js
    subtitle: 'Retrouvez n\'importe quelle fiche en 2 secondes',
    description: 'Toutes vos interventions sont archivées et consultables instantanément. Recherchez par client, par équipement, par technicien ou par date. Plus jamais de fiche introuvable.',
    description2: 'Exportez vos données en CSV pour votre comptable en quelques clics. Votre historique est aussi un outil de pilotage : suivez votre activité, identifiez vos meilleurs clients, et ne manquez plus aucun contrat d\'entretien.',
```

With:

```js
    subtitle: 'Plus jamais "j\'arrive pas à retrouver la fiche"',
    description: 'Tout est archivé. Cherchez par client, équipement, technicien ou date. Vous trouvez en 2 secondes.',
    description2: 'Exportez en CSV pour le comptable. Suivez votre activité, repérez vos meilleurs clients, et ne loupez plus aucun contrat d\'entretien.',
```

- [ ] **Step 3: Rewrite CTA buttons**

In `src/pages/fonctionnalites.astro`, replace:

```html
            <a href="/#inscription" class="btn-nav" style="display:inline-flex;font-size:14px;padding:12px 24px">Rejoindre la liste d'attente →</a>
```

With (replace_all):

```html
            <a href="/#inscription" class="btn-nav" style="display:inline-flex;font-size:14px;padding:12px 24px">Essayer gratuitement →</a>
```

- [ ] **Step 4: Rewrite final CTA**

In `src/pages/fonctionnalites.astro`, replace:

```html
      <h2 class="feat-final-title">Convaincu ?</h2>
      <p class="feat-final-sub">Rejoignez la liste d'attente et soyez parmi les premiers à utiliser Zerionyx.</p>
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">🚀 Rejoindre la liste d'attente</a>
```

With:

```html
      <h2 class="feat-final-title">Prêt à essayer ?</h2>
      <p class="feat-final-sub">Inscrivez-vous, on vous contacte dès l'ouverture.</p>
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">Essayer gratuitement</a>
```

---

### Task 15: Page tarifs — Copy rewrite

**Files:**
- Modify: `src/pages/tarifs.astro`

- [ ] **Step 1: Rewrite hero**

In `src/pages/tarifs.astro`, replace:

```html
    <h1 class="pricing-hero-title">Des tarifs <span class="t-gradient">simples et transparents</span></h1>
    <p class="pricing-hero-sub">Rentabilisé dès le premier mois. Un technicien à 40€/h qui gagne 3h/semaine récupère 480€/mois de valeur.</p>
```

With:

```html
    <h1 class="pricing-hero-title">Des tarifs <span class="t-gradient">clairs, sans surprise</span></h1>
    <p class="pricing-hero-sub">Si vous facturez 40€/h et gagnez 3h par semaine, ça vous rapporte 480€/mois. L'abonnement se rentabilise dès la première semaine.</p>
```

- [ ] **Step 2: Rewrite final CTA**

In `src/pages/tarifs.astro`, replace:

```html
      <h2 class="pricing-cta-title">Prêt à gagner du temps ?</h2>
      <p class="pricing-cta-sub">Rejoignez la liste d'attente et bénéficiez de 2 mois offerts au lancement.</p>
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">🚀 Rejoindre la liste d'attente</a>
```

With:

```html
      <h2 class="pricing-cta-title">Prêt à essayer ?</h2>
      <p class="pricing-cta-sub">Inscrivez-vous, les 50 premiers ont 2 mois offerts.</p>
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">Essayer gratuitement</a>
```

---

### Task 16: Page FAQ — Copy rewrite

**Files:**
- Modify: `src/pages/faq.astro`

- [ ] **Step 1: Rewrite hero**

In `src/pages/faq.astro`, replace:

```html
    <h1 class="faq-hero-title">Questions <span class="t-gradient">fréquentes</span></h1>
    <p class="faq-hero-sub">Tout ce que vous devez savoir sur Zerionyx. Si vous ne trouvez pas la réponse ici, écrivez-nous à <a href="mailto:contact@zerionyx.fr">contact@zerionyx.fr</a></p>
```

With:

```html
    <h1 class="faq-hero-title">Vos <span class="t-gradient">questions</span></h1>
    <p class="faq-hero-sub">Si la réponse n'est pas ici, écrivez-nous à <a href="mailto:contact@zerionyx.fr">contact@zerionyx.fr</a> — on répond sous 24h.</p>
```

- [ ] **Step 2: Rewrite General FAQ answers for natural tone**

In `src/pages/faq.astro`, replace the first FAQ answer:

```html
            <div class="faq-a">Zerionyx est une application mobile (iOS & Android) conçue pour les artisans et techniciens de terrain. Elle remplace vos fiches papier par des fiches numériques que vous remplissez en 2 minutes sur votre téléphone. Le client signe directement sur l'écran, et un rapport PDF professionnel est généré et envoyé automatiquement par email. Vous gagnez du temps, vous avez une image professionnelle, et vous êtes protégé en cas de litige.</div>
```

With:

```html
            <div class="faq-a">C'est une app mobile pour les artisans et techniciens. Vous remplissez vos fiches sur le téléphone en 2 minutes, le client signe sur l'écran, et un rapport PDF pro part tout seul par email. Moins de paperasse, plus de temps pour bosser.</div>
```

Replace the second FAQ answer:

```html
            <div class="faq-a">Zerionyx s'adresse à tous les professionnels qui interviennent chez des clients : chauffagistes, électriciens, plombiers, climaticiens, techniciens de maintenance, maçons, et bien d'autres. Que vous soyez indépendant ou que vous gériez une équipe de techniciens, Zerionyx s'adapte à votre organisation.</div>
```

With:

```html
            <div class="faq-a">Tous les pros qui se déplacent chez des clients : chauffagistes, électriciens, plombiers, climaticiens, techniciens de maintenance, maçons... Que vous soyez seul ou avec une équipe, ça s'adapte.</div>
```

- [ ] **Step 3: Rewrite final CTA**

In `src/pages/faq.astro`, replace:

```html
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">🚀 Rejoindre la liste d'attente</a>
```

With:

```html
      <a href="/#inscription" class="btn-hero-primary" style="display:inline-flex;margin-top:24px">Essayer gratuitement</a>
```

---

### Task 17: Final verification

- [ ] **Step 1: Run dev server and verify all pages**

```bash
npm run dev
```

Check each page in browser:
- `/` — verify Hero rotation, crossfade, dots, all copy changes, counter min
- `/fonctionnalites` — verify copy updates, CTA buttons
- `/tarifs` — verify hero copy, CTA
- `/faq` — verify hero, answer copy

- [ ] **Step 2: Run build to check for errors**

```bash
npm run build
```

Expected: Build completes with no errors.

- [ ] **Step 3: Verify UrgencyBar is deleted**

```bash
ls src/components/UrgencyBar.astro
```

Expected: "No such file or directory"
