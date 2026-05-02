# Turkey Budget Planner v4

Single-file budgeting and scenario-planning dashboard for a USD-income household in Turkey. The page connects income, exchange rate, housing finance, vehicle finance, monthly living costs, and risk scenarios into one shared model.

## Main Idea

The planner is designed to answer one question as realistically as possible:

"If we buy a house and possibly a car in Turkey, what does our monthly picture actually look like under today's rates and future currency scenarios?"

## What The App Includes

- Welcome page that explains the workflow and the risk assumptions
- Budget page for income, mortgage, vehicle finance, and all recurring expenses
- Reports page for high-level overview with multiple graph views
- Currency strategy page for monthly USD-to-TRY conversion and emergency-fund planning
- Inflation history and forward projection pages
- Mortgage optimization page with CollectAPI-powered loan comparisons
- Risk analysis page with stress scenarios

## How The Data Links Together

- USD income, spouse income, extra TRY income, and FX rate drive total monthly income
- Housing finance feeds the `Konut & Aidat` expense category
- Vehicle finance feeds the `Ulasim & Arac` expense category
- Manual expense sliders add to the overall monthly spend
- Slider-driven fields also support direct numeric entry with validation warnings
- Total spend is reused in the currency strategy view
- CollectAPI loan offers can be selected and imported back into the main budget cards
- Selected API offers update interest rate, term, and monthly payment used by the budget

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

1. Start on the welcome page and review the risk assumption.
2. Go to `Butce` and enter real monthly income.
3. Enter house and car price, down payment, and financing assumptions.
4. Tune recurring expense sliders until they reflect actual life.
5. Open `Kredi Opt.` and fetch CollectAPI offers.
6. Apply the best matching house and/or car offer to the budget.
7. Review `Raporlar` for the cross-section overview and alternative chart views.
8. Review `Kur Stratejisi`, `Gelecek`, and `Risk Analizi`.

## Files

- [turkey_budget_personalized_v4_codex.html](/Users/ardac/Documents/New%20project/turkey_budget_personalized_v4_codex.html)
- [README.md](/Users/ardac/Documents/New%20project/README.md)

## Implementation Notes

- The page is intentionally self-contained for easy sharing
- Theme defaults to dark mode and can be toggled from the sticky top navigation
- The welcome page is the default entry point
- If a selected API offer no longer matches the current loan amount or term, the imported offer is cleared automatically
