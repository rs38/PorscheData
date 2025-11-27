# Porsche Telemetry Exploration

This workspace contains the Jupyter notebook `explore porsche data.ipynb`, which aggregates Porsche telemetry CSV exports, engineers a calculated power signal, and produces exploratory plots (SOC, power traces, histograms, sampling frequency, and combined energy/SOC views).

## Data layout & expectations

- CSV files are expected inside dated subdirectories (for example `2025_09`, `2025_10`, `2025_11`) located in the same folder as the notebook.

**Example directory structure:**
```
PorscheData/
├── explore porsche data.ipynb
├── README.md
├── 2025_09/
│   ├── part-00001-c000.csv
│   ├── part-00002-c000.csv
│   └── part-00003-c000.csv
├── 2025_10/
│   ├── part-00001-c000.csv
│   ├── part-00002-c000.csv
│   └── part-00004-c000.csv
└── 2025_11/
    ├── part-00001-c000.csv
    └── part-00002-c000.csv
```

- Each CSV must follow the default `part-0000*-c000.csv` naming pattern 
- The notebook filters data between `start` and `end` timestamps that you can edit near the top of Cell 1.

## Notebook workflow

1. **Load & filter data**: Cell 1 scans all monthly folders, concatenates every matching CSV, and restricts rows to the configured UTC time window.
2. **Signal summaries**: Cells 2–3 provide per-signal counts and per-file row counts to validate coverage.
3. **Visualization cells**: Subsequent cells render SOC, KL_IstLeistung, derived calc_IstLeistung (from voltage/current), histograms within a fixed kW range, battery energy with SOC overlay, and sampling frequency diagnostics.
4. **Derived signal**: Cell 5 creates `calc_IstLeistung` by multiplying `BMS_IstSpannung` and `BMS_IstStrom_02`, aligning on `source_file` + timestamp, and converting Watts to kW.

## How to run

1. Open the workspace folder in VS Code and start the Python kernel used for the notebook (Python 3.13.2 as of the latest run).
2. Ensure all CSV parts are available locally following the structure above.
3. Adjust the `start`/`end` timestamps in Cell 1 if you need a different time range, then run all cells sequentially (`Run All`) or step through as needed.
4. If you add new CSVs or modify the date window, re-run Cell 1 before executing downstream cells so `df` reflects the latest data.

