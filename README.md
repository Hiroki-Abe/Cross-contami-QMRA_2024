# Cross-contami-QMRA_2024

This is the source code for our paper "Effects of at-home-preparation on the risk of foodborne illness in Japan: Analysis of quantitative microbial risk assessment of *Escherichia coli* O157:H7, *Salmonella* Typhimurium, *Listeria monocytogenes*, and *Campylobacter jejuni* in lettuce salad made at home" Hiroki Abe, Akihiro Ando, Kento Koyama, Shigenobu Koseki. Microbial Risk Analysis, 27–28, 100316 (2024) (DOI: https://doi.org/10.1016/j.mran.2024.100316)

## Contents

| Folder | Pathogen |
| --- | --- |
| `STEC O157/` | *E. coli* O157:H7 |
| `Typhimurium/` | *S.* Typhimurium |
| `LM/` | *L. monocytogenes* |
| `Campy/` | *C. jejuni* |
| `X data/` | Transfer data used to fit the cross-contamination distributions (read by `Campy/001 Cross prob distribuitions.ipynb`) |

Each pathogen folder holds Jupyter notebooks that are run in numbered order:

- `001_Growth_prediction_*.ipynb`: Bayesian fitting (PyStan) of the growth model for lettuce and raw meat. The fitted models are saved in the same folder as `stan_model_growth_*.pkl` and `fit_nuts_*.pkl`. *C. jejuni* does not grow below 31 °C, so `Campy/` has no growth notebook; `001 Cross prob distribuitions.ipynb` there fits the transfer-ratio distributions instead.
- `002_QMRA_Simulation_*.ipynb`: the QMRA simulation (528,400 iterations per pathogen). The notebooks named `No cross` and `No lettuce contami` are the scenario analyses without cross-contamination and without initial lettuce contamination.
- `003_QMRA_results_analysis_*.ipynb`: sensitivity analysis and the tables and figures of the paper, made from the saved simulation results.
- `data/transportation/`: input data. `Static_*_growth_*.csv` are the growth data (log count over time at constant temperature) that 001 fits the growth model to; `Temperature_histroy_1.csv` is the temperature record used to set the transport temperature distribution.

The notebooks are kept with their outputs, so the results can be checked without running them.

## How to run

Run the notebooks in each folder in the order 001 → 002 → 003, with the folder as the working directory. 002 loads the fitted growth models (`*.pkl`) written by 001, saves the simulation results in `results/` and the figures in `plot/`; 003 reads `results/`.

Notes:

- The fitted growth models (`*.pkl`, 90–410 MB each) and the simulation results are not included because of their size. Run 001 and 002 to create them.
- `Campy/001 Cross prob distribuitions.ipynb` reads the transfer data from `../../X data/`. In this repository the data are in `X data/` at the top level, so change the path to `../X data/`. The hand-washing file is named `Hand wash.csv`.
- In `Campy/`, the no-lettuce-contamination notebook saves `results/Simulation results of repsim no lettuce contami.pkl`, while 003 reads `results/Simulation results of repsim no lettuce.pkl`. Rename the file before running 003.
- In `Typhimurium/`, 001 names the fitted models `stan_model_growth_Salmonella.pkl` and `fit_nuts_Salmonella_growth*.pkl`, while 002 and 003 load them as `..._salmonella...` (lower case). On a case-sensitive file system such as Linux, rename the files before running 002.

## Requirements

Python 3.7–3.9 with numpy, pandas, scipy, matplotlib, seaborn, tqdm and PyStan 2.x. The fitted Stan models are saved and loaded with pickle, which needs PyStan 2.
