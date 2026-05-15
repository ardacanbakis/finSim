# finSim — Turkey Budget Planner

**A financial planning dashboard for USD-income households in Turkey.**

Plan a home purchase, vehicle financing, and monthly life in one connected view — with live exchange rates, inflation history, forward projections, and risk analysis all linked to a single set of inputs.

**Live:** [ardacanbakis.github.io/finSim](https://ardacanbakis.github.io/finSim/)

---

## What It Does

finSim answers a single question as clearly as possible:

> *"If we buy a house and possibly a car in Turkey, what does our monthly picture actually look like — now and in five years?"*

Every number in the app flows from one source of truth: your income, your exchange rate, and your loan assumptions. Change one input and every chart, ratio, and recommendation updates instantly.

---

## Tabs

### 💰 Budget
The main input screen. Enter your USD income, an optional spouse income, and any extra TRY income (rental, freelance, etc.). Set the live USD/TRY rate, then fill in your mortgage and vehicle financing assumptions. Recurring expenses are broken into labeled categories — groceries, transport, health, subscriptions, and more — each with a slider and a direct number field. All figures flow through to every other tab automatically.

### 📊 Reports
A cross-section of your budget in chart form. Covers expense distribution, a debt and fixed-load report, FX sensitivity (how your balance changes as the rate shifts), and a 12-month cash-flow overview with an interpretive text summary.

### 💱 FX Strategy
Tells you how much USD to convert to TRY each month given your actual expenses, and what size emergency fund you need as a single-income household. Includes a scenario table showing required conversion amounts at several exchange rates and a chart showing how rate swings hit your monthly balance.

### 📈 Inflation History
Puts today's numbers in context. Shows annual TurkStat inflation from 2020 to 2026, the USD/TRY rate over the same period, the TRY value of your income in each of those years, and the purchasing-power loss of a fixed TRY amount since 2020.

### 🔮 Outlook
Forward projections through 2031 across three exchange-rate scenarios (base, optimistic, pessimistic). Shows how your income's TRY value evolves, how your loan's real USD cost changes as the lira depreciates, and what happens to your budget if a partner income comes online in the future.

### 🏠 Loan Optimization
A dedicated mortgage workspace. Includes a scenario table comparing six down-payment and term combinations at your live inputs, a simulator where you can model any home price, rate, and term freely, and a **CollectAPI integration** that fetches current market loan offers from Turkish banks. Any offer you select is imported directly into your main budget with one click. The simulator also has an **"Apply to Budget →"** button that pushes the simulated loan back to the Budget tab.

### 🛡 Risk Analysis
Stress-tests your budget against the scenarios that matter most for a single-income USD household in Turkey: job loss, a sharp TRY strengthening, unexpected large expenses, and the effect of adding a partner income. Shows a risk profile card for each scenario and a quantified stress table.

---

## Session & Data

Your inputs are saved automatically to browser `localStorage` as you type. When you return to the page your last session is fully restored — no account, no server, no manual save needed.

The **Oturum** button in the top navigation bar gives you three options:

| Action | Effect |
|--------|--------|
| 💾 Kaydet | Downloads a `.json` snapshot of every field and setting |
| 📂 Yükle | Opens a file picker to restore any previously saved snapshot |
| 🖨 PDF | Builds a formatted print summary and opens the browser print dialog |

Snapshots are plain JSON and fully re-importable. Use them to keep named scenarios — for example a conservative and an optimistic plan side by side.

---

## How It Was Built

finSim is intentionally a **single self-contained HTML file** with no build step, no framework, and no server dependency. The goal was something that could be hosted as a static GitHub Pages page, opened offline, and shared as a single file.

The project started as a basic income-vs-expense calculator and grew incrementally through a series of focused additions, each committed separately:

1. **Core model** — a shared `budgetSnapshot` object that every tab reads from. Changing any input calls `calc()`, which recomputes all totals and writes the result to `budgetSnapshot` so every downstream chart and table picks it up without explicit wiring.

2. **State persistence** — `collectState()` and `applyState()` serialize the full form into `localStorage` on every keystroke. An export button serializes the same object to a `.json` file; import reads it back.

3. **UI layer** — dark/light theme via CSS custom properties on `body[data-theme]`, a sticky navigation bar, mobile-friendly scrollable tabs, and a welcome overlay that explains the workflow before the user touches any input.

4. **Charts** — Chart.js loaded from CDN. Each tab's charts are lazy-initialized the first time the tab is opened and destroyed/recreated on re-entry to prevent canvas conflicts. All chart data is derived from `budgetSnapshot` values, not hardcoded numbers.

5. **Loan math** — standard amortization formula (`P = L·r·(1+r)^n / ((1+r)^n − 1)`) extended to support balloon payments. The mortgage optimizer generates scenario tables by applying the same formula across six down-payment and term combinations using live inputs.

6. **CollectAPI integration** — client-side calls to the CollectAPI credit endpoints. Returned offers are sorted best-to-worst and each one has an import button that writes the rate and term back into the main budget card and re-runs the calculation.

7. **Language support** — a `lang_en.js` file holds all English translations keyed by DOM order. Switching language replaces text content on all labeled elements without a page reload.

8. **Iterative fixes** — several rounds of debugging: a `buildCats()` bug that reset expense fields when categories were toggled, smart-quote characters introduced by an editor that broke the entire script block, and welcome overlay dark-mode contrast issues resolved by inlining CSS custom property defaults directly on the overlay element.

---

## Tech

- Vanilla HTML, CSS, JavaScript — no framework, no bundler
- [Chart.js](https://www.chartjs.org/) via CDN for all charts
- [CollectAPI](https://collectapi.com/) for live Turkish bank loan data (token required for that tab)
- GitHub Pages for hosting
- `localStorage` for session persistence
