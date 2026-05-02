# Turkey Budget Planner — finSim

Single-file budgeting and scenario-planning dashboard for a USD-income household in Turkey. The page connects income, exchange rate, housing finance, vehicle finance, monthly living costs, and risk scenarios into one shared model.

**Live demo:** [ardacanbakis.github.io/finSim](https://ardacanbakis.github.io/finSim/)

## Main Idea

The planner is designed to answer one question as realistically as possible:

"If we buy a house and possibly a car in Turkey, what does our monthly picture actually look like under today's rates and future currency scenarios?"

## What The App Includes

- Welcome page (Başlangıç) that explains the workflow and risk assumptions — opens by default
- Budget page for income, mortgage, vehicle finance, and all recurring expenses
- Reports page for high-level overview with multiple graph views
- Currency strategy page for monthly USD-to-TRY conversion and emergency-fund planning
- Inflation history and forward projection pages
- Mortgage optimization page with CollectAPI-powered loan comparisons
- Risk analysis page with stress scenarios

## How The Data Links Together

- USD income, spouse income, extra TRY income, and FX rate drive total monthly income
- Housing finance feeds the `Konut & Aidat` expense category
- Vehicle finance feeds the `Ulaşım & Araç` expense category
- Manual expense sliders add to the overall monthly spend
- Slider-driven fields also support direct numeric entry with validation warnings
- Total spend is reused in the currency strategy view
- CollectAPI loan offers can be selected and imported back into the main budget cards
- Selected API offers update interest rate, term, and monthly payment used by the budget

## Session State — No Backend Needed

All inputs are automatically saved to `localStorage` as you type. When you return to the page, your last session is restored exactly as you left it — no login, no server, no export required.

Three buttons are always visible in the top navigation bar:

| Button | What it does |
|--------|-------------|
| 💾 Kaydet | Downloads a `.json` snapshot of every input and setting |
| 🖨 PDF | Builds a formatted summary page and opens the browser print dialog (save as PDF) |
| 📂 Yükle | Opens a file picker — select a previously saved `.json` to restore the full session |

The exported `.json` file is human-readable and fully re-importable. Use it to:
- Back up a scenario before experimenting with new numbers
- Share a session with someone else
- Keep multiple named scenarios (e.g. `finsim-optimistic.json`, `finsim-conservative.json`)

## CollectAPI Integration

The page uses these endpoints:

- `GET /credit/creditBid`
- `GET /credit/konutKredi`
- `GET /credit/tasitKredi`

Expected usage:

- `creditBid` gives personalized results for `konut` or `tasit`
- `konutKredi` and `tasitKredi` provide general market-rate lists
- The UI sorts offers from best to worst and lets the user push a selected offer into the budget

## Token Setup

The HTML supports a project-level embedded token with:

```js
const COLLECT_API_TOKEN='...';
```

Notes:

- The code normalizes the token, so raw token text and values starting with `apikey ` both work
- Because this is a client-side HTML file, embedding the token makes it readable to anyone who can inspect the file
- A backend proxy would be the safer long-term option

## Recommended User Flow

1. Open the live page — it starts on the Welcome (Başlangıç) tab.
2. Go to `Bütçe` and enter real monthly income and FX rate.
3. Enter house and car price, down payment, and financing assumptions.
4. Tune recurring expense sliders until they reflect actual life.
5. Open `Kredi Opt.` and fetch CollectAPI offers if you have a token.
6. Apply the best matching house and/or car offer to the budget.
7. Review `Raporlar` for the cross-section overview and chart views.
8. Review `Kur Stratejisi`, `Gelecek`, and `Risk Analizi`.
9. Click **💾 Kaydet** to save a snapshot, or **🖨 PDF** for a printable summary.

## Files

- `index.html` — the entire application (self-contained, no build step)
- `lang_en.js` — English translation strings
- `finsim_favicon_minimal.svg`, `finsim_logo_dark.svg`, `finsim_logo_light.svg` — visual assets

## Implementation Notes

- The page is intentionally self-contained for easy sharing and GitHub Pages hosting
- Theme defaults to dark mode and can be toggled from the sticky top navigation
- The welcome page (Başlangıç) is the default entry point
- Session state is persisted to `localStorage` automatically; no backend is required
- If a selected API offer no longer matches the current loan amount or term, the imported offer is cleared automatically
