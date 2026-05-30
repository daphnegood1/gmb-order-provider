# Milksha Audit Workflow

## Purpose

Use this workflow to produce a public Milksha / 迷客夏 Taiwan store ordering-provider analysis. The original successful run produced:

- 335 official Taiwan stores
- 315 stores with confirmed official Nidin takeout ordering
- 303 stores with confirmed delivery ordering
- split provider counts for takeout and delivery

Counts may change over time. Always regenerate from current public sources.

## Sources

Use these sources in order:

1. Official store population: `https://www.milksha.com/store_detail.php?uID=1`
2. User-requested English/search entry, for citation only: `https://www.milksha.com/en/store_detail.php?uID=22`
3. Official ordering site: `https://milksha.nidin.shop/`
4. Nidin API base: `https://loctw-service-api.nidin.shop/shopper/v2`
5. Footinder search: `https://drink.footinder.com.tw/?s=<query>`
6. Google Maps/GMB links from the official store cards

`uID=22` is a search page. `uID=1` renders the full Taiwan store cards and should be used as the population source unless Milksha changes the site.

## Script Sequence

Run from the project root so `data/` is written in the target repo:

```powershell
python <skill>/scripts/fetch_official_stores.py
python <skill>/scripts/audit_order_providers.py --workers 10
python <skill>/scripts/audit_nidin_order.py
```

The scripts are intentionally stdlib-only. If a run fails due to network limits, rerun the failing script; it rewrites the data files from the current `stores.json`.

## Provider Interpretation

- Nidin in `takeoutProviders`: official Milksha online pickup/takeout ordering is available through Nidin.
- Nidin in `deliveryProviders`: official Nidin API shows delivery information or delivery conditions.
- foodpanda / Uber Eats in `deliveryProviders`: Footinder page contains corresponding delivery-platform evidence.
- lin.ee in `otherProviders`: public evidence contains a LINE short link or LINE ordering signal.

Do not place foodpanda or Uber Eats in takeout unless the user supplies direct GMB evidence or another public source explicitly identifies them as takeout providers.

## Manual Review

Keep stores in the dataset even when not confirmed. Typical unresolved cases:

- mall counters
- hospital or campus-only stores
- special stores
- Milksha Plus stores not present in Nidin
- official stores not indexed by Footinder
- Google GMB dynamic order buttons blocked from batch reading

Use `needs_manual_review` or `unavailable_or_blocked`; do not guess.

## Publishing

For a GitHub Pages static site:

1. Keep `index.html`, `styles.css`, `app.js`, and `data/` at repo root.
2. Commit generated data files.
3. Push to GitHub.
4. Enable GitHub Pages from `main` branch root.
5. Verify:
   - homepage returns 200
   - `data/summary.json` returns 200
   - summary counts include `takeoutProviderCounts` and `deliveryProviderCounts`
   - the county selector updates metric cards, provider charts, the Taiwan map, and detail rows together
   - the Taiwan county map renders 22 county/city labels and each label shows the current store count
   - county-map counts sum to the current displayed store total
   - six-region cards show `台北基宜`, `桃竹苗`, `中彰投`, `雲嘉南`, `高屏`, and `花東`; their sum matches the displayed store total unless offshore counties are intentionally shown separately

## Taiwan Map Site Pattern

When the user asks for geographic distribution, add a self-contained Taiwan map section to the static site:

- Put six-region summary cards above the map.
- Render county/city counts directly on the map, not only in a table.
- Include `0` for counties where the brand has no stores.
- Provide a compact county list next to or below the map for scanability.
- Avoid external map services for the core visual; use inline SVG or local static assets so GitHub Pages remains stable.
- Make the map respond to the same global county/city dropdown as the metric cards and detail table.
- On mobile, ensure the map does not create horizontal scrolling and labels remain readable.
