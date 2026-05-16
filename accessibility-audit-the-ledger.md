# Accessibility Audit — The Ledger

**Fájl:** `the-ledger.html`  
**Dátum:** 2026-05-16  
**Standard:** WCAG 2.2 Level AA

---

## Kritikus hibák (WCAG Level A)

### 1. Hiányzó skip link (WCAG 2.4.1)

A sidebar 7 nav elemet és 4 fiókot tartalmaz. Billentyűzettel navigáló felhasználók minden alkalommal végig kell tabeljék ezeket, mielőtt a főtartalomhoz érnek.

```html
<!-- A <body> legelső gyermekeként -->
<a href="#main-content" class="sr-only focus:not-sr-only">
  Skip to main content
</a>
<!-- ... -->
<main class="main" id="main-content" tabindex="-1">
```

---

### 2. Hiányzó `<h1>` — nincs oldalszintű cím (WCAG 1.3.1)

Az egész dokumentumban nincs `<h1>`. A topbar-ban `<span>` elemek jelzik a "Today" főcímet. A `<h3>`-al kezdődő heading hierarchia hiányos.

```html
<!-- topbar-title-ban: -->
<h1 class="title">Today</h1>
<!-- vagy: -->
<span class="title" role="heading" aria-level="1">Today</span>
```

---

### 3. Keresőmező accessible label nélkül (WCAG 4.1.2)

Sor ~1925: Az `<input>` elemnek nincs `<label>` vagy `aria-label`. A `placeholder` attribútum nem helyettesíti a labelt.

```html
<input
  aria-label="Search entries, merchants, categories"
  placeholder="Search entries, merchants, categories…"
/>
```

---

### 4. Account lista elemek nem érhetők el billentyűzettel (WCAG 2.1.1)

Sor ~1844: Az `.accounts-list li` elemek `cursor: pointer`-rel és hover effekttel rendelkeznek, de nincs `tabindex`, `role="button"`, és nem aktiválhatók Enter/Space billentyűvel.

```html
<li role="button" tabindex="0"
    aria-label="Checking account, balance €4,820.71">
```

---

### 5. Progress bar-ok nem accessible (WCAG 1.3.1, 4.1.2)

A goal-ok és budget bárok sima `<div>`-ek, nincs `role="progressbar"` és ARIA értékek.

```html
<div class="goal-bar"
     role="progressbar"
     aria-valuenow="62"
     aria-valuemin="0"
     aria-valuemax="100"
     aria-label="Emergency Fund progress">
  <div class="goal-bar-fill" style="width: 62%;"></div>
</div>
```

---

### 6. Hover-only interaktív elemek (WCAG 2.1.1)

Két helyen is vannak `opacity: 0` elemek, amelyek csak hoverre jelennek meg:

- **Row actions** (sor ~857): A Categorise / Split / Cancel linkek
- **Fiscal edit links** (sor ~973): Az Adjust / Move funds linkek

Billentyűzettel ezek fókuszálhatók, de nem láthatók. Megoldás: `focus-within` figyelése is:

```css
.ledger-table tbody tr:focus-within .row-actions { opacity: 1; }
.fiscal-list li:focus-within .fiscal-edit a { opacity: 1; }
```

---

### 7. Modal fókusz trap hiányzik (WCAG 2.1.2)

Sor ~3160: A modal megnyílásakor fókusz az első inputra kerül, de Tab billentyűvel ki lehet lépni a modálból. `role="dialog"` + `aria-modal="true"` megvan, de a JS-es focus trap nincs implementálva.

---

### 8. Szín-alapú információközlés (WCAG 1.4.1)

Az account "dot" elemek (sor ~1847) kizárólag színnel különböztetik meg a számlatípusokat. Szövegalternatíva nincs.

```html
<!-- Javasolt: -->
<span class="dot" style="background: var(--blue);" aria-hidden="true"></span>
<!-- A parent aria-label-jébe kell bele: -->
<li aria-label="Checking account ··2814">
```

---

## Komoly hibák (WCAG Level AA)

### 9. Tabs ARIA-pattern helytelen (WCAG 4.1.2)

Sor ~2058: A `.tabs` container `role="tablist"`, de a `<label>` elemek nem kapnak `role="tab"`. A radio input + tablist keverése ellentmondásos.

```html
<!-- Option A: marad radio, eltávolítani a role="tablist"-et -->
<div class="tabs" aria-label="Period">

<!-- Option B: ARIA tab pattern, de akkor JS-es tab panel kezelés kell -->
<div class="tabs" role="tablist" aria-label="Period">
  <label role="tab" aria-selected="false" tabindex="-1" for="cf-1m">1M</label>
```

---

### 10. Több nav landmark nincs megkülönböztetve (WCAG 2.4.1)

A `<nav>` elem nincs `aria-label`-lel ellátva (sor ~1814).

```html
<nav aria-label="Main sections">
```

---

### 11. Ledger tábla caption hiányzik (WCAG 1.3.1)

Sor ~2190: A `<table>` elemnek nincs `<caption>`, a screen readerek egyszerűen "table"-ként olvassák fel.

```html
<table class="ledger-table" id="ledger">
  <caption class="sr-only">Transaction ledger — last 7 days</caption>
```

---

### 12. Hero statisztikák szemantikailag strukturálatlanok (WCAG 1.3.1)

Sor ~1990: A `.hero-split` div-jei `.l` / `.v` / `.foot` osztályokkal jelölik a label–érték párokat, de nincs szemantikus kapcsolat közöttük. `<dl>` / `<dt>` / `<dd>` ajánlott:

```html
<dl class="hero-split">
  <div>
    <dt class="l">Assets</dt>
    <dd class="v">€102,840</dd>
    <dd class="foot">Across 4 accounts · up €1,840</dd>
  </div>
</dl>
```

---

### 13. Confirmation auto-dismiss fókusz kezelés nélkül (WCAG 2.4.3)

Sor ~3185: Az `alertdialog` 3.4 mp után automatikusan bezárul, de fókusz nem kerül rá és nem tér vissza az előző elemre.

---

### 14. Form mezők `aria-required` nélkül (WCAG 4.1.2)

A modálos form inputokon nincs `aria-required="true"` és nincs hibaüzenet-mechanizmus. A "Save" gomb üres értékeket is elfogad.

---

### 15. Live region statikus tartalmon (WCAG 4.1.3)

Sor ~2539: `aria-live="polite"` van a `.ticker` div-en, amely statikus tartalmat tartalmaz. Ez az oldal betöltésekor felesleges bejelentéseket okozhat. Csak akkor érdemes, ha a ticker tartalma ténylegesen dinamikusan változik.

---

## Kisebb / AAA szintű észrevételek

| # | Probléma | Hely (kb. sor) |
|---|---------|----------------|
| 16 | CSS-generált tartalom (`content: '→'`, `content: 'budget'`) screen readerektől rejtett | ~659, ~957 |
| 17 | A notification badge nem közli az olvasatlan értesítések számát | ~1935 |
| 18 | Az SVG cashflow chartnak nincs accessible data table alternatívája | ~2068 |
| 19 | A `G` (Go to) és `?` (Help) billentyűparancsok megjelennek, de nincsenek implementálva | ~2549 |
| 20 | `.btn-icon` gombok (32×32px) az AAA ajánlott 44×44px alatt vannak | ~387 |
| 21 | A sidebar brand ("TheLedger") nem link és nem heading, nincs landmark role-ja | ~1804 |

---

## Prioritás összefoglaló

| Prioritás | Darab | Példák |
|----------|-------|--------|
| **Kritikus (A)** | 8 | Skip link, h1, modal focus trap, progress bars |
| **Komoly (AA)** | 7 | Nav label, table caption, tab ARIA, live region |
| **Kisebb / AAA** | 6 | CSS content, touch targets, missing kbd shortcuts |
