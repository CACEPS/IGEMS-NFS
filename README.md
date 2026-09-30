# IGEMS-NFS replication package

Replication package for the manuscript:

**Closing the sequencing gap: a governance pathway for carbon budget allocation and the 2028 Global Stocktake**


Repository: https://github.com/caceps/IGEMS-NFS. An archived release with a DOI will be deposited on Zenodo at acceptance.

## Quick start

```bash
pip install -r requirements.txt
bash run_all.sh
```

`run_all.sh` runs every script in order from `code/` and writes all results to `outputs/` and `figures/`.
A full run takes a few minutes on a laptop. The Monte Carlo uses a fixed seed (42), so results are identical on every run.

## Run order

| Step | Script | Produces |
|---:|---|---|
| 1 | `build_baseline.py` | `outputs/baseline_v9.csv` (nine-region 2024 baseline) |
| 2 | `frontier_observed.py` | `outputs/frontier_windows.csv`, `outputs/frontier_summary.csv` |
| 3 | `run_base.py` | `outputs/budget_sensitivity_v9.csv` |
| 4 | `analysis_v9.py` | `outputs/results_v9.json`, `pathways_v9.csv`, `monte_carlo_v9.csv` |
| 5 | `gap_and_transfers_v10.py` | `outputs/results_v10_additions.json` |
| 6 | `kyoto_backtest.py` | `outputs/kyoto_backtest.csv`, `kyoto_backtest_tests.csv` |
| 7 | `si_tables_v9.py` | `outputs/si_*.csv` (inequality, validation, regions, decision rules, bargain, fund) |
| 8 | `si_json.py` | `outputs/si_tables.json` (all Supplementary tables) |
| 9 | `figures_v9.py` | `figures/figS_frontier_switching.*` (Figure S1) |
| 10 | `main_figures_v9.py` | `figures/fig1_framework.*`, `figures/fig2_results.*`, `figures/fig2_results_colour.*` |

`frontier_modelled_AR6.py` is optional and needs the AR6 R10 regional file from the IIASA AR6 Scenario Explorer (see its docstring).

## Where the headline numbers come from

| Manuscript statement | Value | Source file and key |
|---|---|---|
| 2024 territorial fossil and cement CO2 | 37.4 Gt | `baseline_v9.csv` (sum of `Current_emissions_Gt`) |
| Observed frontier (95th percentile, non-crisis) | 3.9 %/yr | `frontier_summary.csv`, `p95_decl` |
| Crisis-inclusive upper bracket | 8.3 %/yr | `frontier_summary.csv`, `p95_incl_crisis` |
| Frontier variants (90th percentile to 99th percentile) | 3.2 to 5.2 %/yr | `frontier_summary.csv` |
| Uniform decline required at 500 / 250 / 130 Gt | 7.2 / 13.9 / 25.0 %/yr | `results_v9.json`, `switching` |
| High emitters' need at the observed frontier | 660 Gt | `results_v9.json`, `bargain.HE_cumulative_need_at_observed_frontier_Gt` |
| Draws clearing both blocs at 500 Gt | 21.9 % | `results_v9.json`, `monte_carlo.500` |
| Market rule last on adherence | 78 % of draws | `results_v9.json`, `monte_carlo.500.market_last_on_adherence` |
| Per-capita Gini: equity / hybrid / market | 0.05 / 0.23 / 0.43 | `si_ineq.csv` |
| Bargain threshold at 500 Gt | 6.7 %/yr | `results_v10_additions.json`, `bargain_min_frontier_annual_pct` |
| Bargain transfers at 500 Gt, upper frontier | USD 370 bn/yr | `results_v10_additions.json`, `bargain_transfer_totals_at_USD100` |
| Physical shortfall at 500 Gt | about 450 Gt | `results_v10_additions.json`, `physical_gap.500` |
| Kyoto partial correlation (n = 37) | -0.35, p = 0.035 | `kyoto_backtest_tests.csv` |

## Data

* `data/owid-co2-data.csv`: Global Carbon Project 2024 national emissions via Our World in Data, with UN WPP 2024 population and the Jones et al. (2023) GHG series used in the Kyoto test.
* `data/wb_gdp_current_usd.csv`: World Bank WDI GDP, current US dollars.

Scope: territorial fossil and cement CO2, excluding land use (about 4.6 Gt) and international bunkers (1.19 Gt). CO2 only.

## Provisional items (author to verify before acceptance)

* AR6 modelled frontier: `frontier_modelled_AR6.py` has not yet been run.
* Forest fractions in `igems_v9.FOREST` (capacity rule, Supplementary only). Replace with FAO FRA 2020.
* GDP fills for economies missing from WDI (`build_baseline.FILL`).
* Kyoto targets and base years (`kyoto_backtest.T`, `BASEYR`), to be checked against Shishlov et al. (2016).

## File names

Script and output names keep their `_v9` and `_v10` suffixes so that references in the Supplementary Materials remain valid.
The suffix marks the release in which a file was introduced, not a separate model.

See `CHANGELOG.md` for the release history.
