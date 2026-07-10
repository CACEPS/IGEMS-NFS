# IGEMS Coalition Allocation Model: Replication Package

Replication code and data for:

**Closing the institutional failure: a sequenced governance pathway for carbon budget allocation and the 2028 Global Stocktake.**

## What this package does

IGEMS distributes a 10 Gt CO₂ per year global ceiling across nine macro-regions under four allocation rules. The rules are market (grandfathering), equity (CBDR+RC), capacity, and hybrid (40:35:25). The package computes corridor adherence, acceptance propensity, bloc coalition tests, and Solidarity Fund flows. A 400-draw Monte Carlo tests robustness to structural parameters and Dirichlet-sampled hybrid weights.

## Repository structure

```
├── code/
│   ├── igems_model.py              # Core model (reproduces workbook exactly)
│   ├── monte_carlo.py              # 400-draw robustness analysis (seed 42)
│   ├── make_figures.py             # Main-text Figures 1-5 (300 dpi PNG + PDF)
│   └── original_colab_script.py.txt  # Original Colab export, kept for provenance
├── data/
│   └── IGEMS_coalition_outputs.xlsx  # Archived model output workbook (12 sheets)
├── outputs/
│   ├── monte_carlo_draws.csv       # Draw-level grid (400 x 19)
│   └── monte_carlo_summary.csv     # Supplementary Table S4.1
├── figures/                        # Generated figures
├── requirements.txt
└── README.md
```

## Quick start

```bash
pip install -r requirements.txt
cd code
python monte_carlo.py     # writes ../outputs/*.csv and prints summary
python make_figures.py    # writes ../figures/fig1..fig5 (.png and .pdf)
```

Verification of the base case against the archived workbook:

```python
from igems_model import run, MECHANISMS
res, p = run()
print([round(res[f"{m}_binary_corridor"].mean(), 3) for m in MECHANISMS])
# [0.2, 0.4, 0.467, 0.4]
```

## Two important technical notes

1. **Fund unit correction.** The archived workbook sheet `table6_fund` converts Gt to
   billion USD as `Gt * price / 1000`. The correct conversion is `Gt * price`
   because 1 Gt = 1e9 tonnes. `igems_model.solidarity_fund` implements the
   corrected conversion. The Gt columns in the workbook are correct throughout.
   See Supplementary Note S6.2.
2. **pandas `keep_default_na`.** Read the workbook with `keep_default_na=False`.
   The North America region code "NA" is otherwise parsed as missing.

## Headline base-case results

| Rule | Mean adherence | Max:min per-capita | Justice index | High-emitter accept | Low-income accept |
|---|---|---|---|---|---|
| Market | 0.20 | 13.45 | 41.1 | 0.182 | 0.182 |
| Equity | 0.40 | 1.64 | 33.1 | 0.066 | 0.875 |
| Capacity | 0.47 | 37.90 | 27.0 | 0.188 | 0.534 |
| Hybrid | 0.40 | 4.81 | 42.8 | 0.140 | 0.693 |

No rule clears the 0.5 participation threshold for both blocs (gridlock).
Solidarity Fund redistribution: 1.92 Gt/yr, USD 144-288 bn/yr at USD 75-150/t.

## Data sources (all public)

Global Carbon Project (emissions, 2024 release); World Bank WDI (GDP, accessed
January 2026); UN World Population Prospects 2024; Matthews et al. (2020)
aggregation (historical emissions); FAO FRA via World Bank (forest cover).

## Citation and archive

Cite the tagged Zenodo release: [DOI to be minted at submission].

## License

MIT for code. Data files retain the licenses of their original public sources.
