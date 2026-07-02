# Is Europe’s AI Boom Environmentally Sustainable?

**Ironhack Data Analytics Bootcamp — Data Wrangling Project**  
**Team:** Kyle Haustein, Aline, and Sandra

## Overview

This project explores whether Europe’s rapid adoption of artificial intelligence is occurring under energy conditions that can support more sustainable digital growth.

We combine country-level indicators for enterprise AI adoption, renewable-energy share, power-sector CO₂ emissions, and listed data-centre facilities. The goal is not to claim that AI directly causes emissions. Instead, the analysis examines whether countries with expanding AI use and digital infrastructure appear to be supported by stronger renewable-energy systems.

## Research Question

> **Is Europe’s AI boom environmentally sustainable?**

### Sub-questions

- Which European countries have the highest enterprise AI adoption?
- How has average enterprise AI adoption changed over time?
- Do countries with higher AI adoption also have stronger renewable-energy shares?
- How are listed data-centre facilities distributed across different energy-system conditions?
- Which variables show the strongest country-level relationship with power-sector CO₂ emissions?

---

## Data Sources

| Theme | Source | Main file(s) | Key measure | Coverage / role |
|---|---|---|---|---|
| Enterprise AI adoption | Eurostat SDMX API (`ISOC_EB_AI`) | `ai_adoption_cleaned.csv` | `ai_adoption_pct` | Annual share of enterprises using at least one AI technology; the selected series covers enterprises with 10 or more persons employed. |
| Renewable energy | Eurostat | `cleaned_renewable_energy.csv` | `renewable_energy_share` | Country-year renewable-energy share. |
| Power-sector emissions | Eurostat | `clean_eurostat_power_sector_emissions.csv` | `emissions` | CO₂ emissions from fuel combustion in energy industries (`CRF1A1`). |
| Data-centre facilities | PeeringDB Facilities API | `eu_peeringdb_facilities.csv`, `eu_facilities_cleaned.csv` | Facility listings by country | Current directory snapshot of listed facilities, queried for EU27 country codes. |
| Electricity prices *(exploratory only)* | Eurostat (`nrg_pc_204`) | `eurostat_electricity_clean.csv` | Electricity prices | Retained as an exploratory dataset, but excluded from the final analysis because it measures prices rather than electricity generation, demand, or emissions. |

### Primary source links

- [Eurostat SDMX API — enterprise AI adoption](https://ec.europa.eu/eurostat/api/dissemination/sdmx/3.0/data/dataflow/ESTAT/ISOC_EB_AI/1.0)
- [PeeringDB Facilities API](https://www.peeringdb.com/api/fac)

---

## Project Files

```text
.
├── README.md
├── Kyle's notebook.ipynb
├── Sandra_notebook_2_datasets.ipynb.ipynb
├── Aline's notebook.ipynb
├── data_centers_cleaning.ipynb
│
├── ai_adoption_cleaned.csv
├── cleaned_renewable_energy.csv
├── clean_eurostat_power_sector_emissions.csv
├── eu_peeringdb_facilities.csv
├── eu_facilities_cleaned.csv
├── merged_data.csv
└── eurostat_electricity_clean.csv
```

### Notebook roles

| Notebook | Purpose |
|---|---|
| `Kyle's notebook.ipynb` | Retrieves and cleans AI-adoption data, merges datasets, and contains the main exploratory analysis and visualisations. |
| `sandra_notebook_2_datasets.ipynb.ipynb` | Cleans the renewable-energy and power-sector emissions datasets. |
| `data_centers_cleaning.ipynb` | Collects PeeringDB facility listings, handles API pagination/rate limits, and creates the cleaned facility file. |
| `Aline's notebook.ipynb` | Explores Eurostat electricity-price data, which was later excluded from the final analysis. |

> **Note:** The filename for Sandra’s notebook includes the doubled `.ipynb.ipynb` extension because that is the current project filename.

---

## Data Preparation

The workflow included the following steps:

1. **Retrieved and cleaned AI-adoption data** from the Eurostat API.
2. **Standardised country codes and year columns** before merging datasets.
3. **Selected relevant indicators** for enterprise AI adoption, renewable-energy share, and power-sector CO₂ emissions.
4. **Created data-centre counts** by grouping listed PeeringDB facility records by country.
5. **Merged the analysis data** into `merged_data.csv` using country code (`geo`) and year.
6. **Checked duplicates** to confirm that the merged file has one row per country-year combination.

The final merged dataset contains:

| Column | Description |
|---|---|
| `geo` | Country code used for merging |
| `year` | Reporting year |
| `ai_adoption_pct` | Share of enterprises using at least one AI technology |
| `renewable_energy_share` | Renewable-energy share (%) |
| `emissions` | Power-sector CO₂ emissions |
| `country_name` | Readable country name |
| `datacenters` | Count of listed PeeringDB facilities in the country |

---

## Analysis Approach

### 1. AI adoption over time

Enterprise AI adoption is analysed over the available reported years: **2021, 2023, 2024, and 2025**. There is no 2022 observation in the selected AI series, so the trend is based on reported years rather than a continuous annual series.

### 2. Country-level comparisons

For the latest fully comparable year, the analysis compares:

- AI adoption and renewable-energy share
- Listed data-centre facilities and renewable-energy share
- Listed data-centre facilities and power-sector CO₂ emissions
- Renewable-energy share and power-sector CO₂ emissions

### 3. AI sustainability categories

Countries are grouped using median splits for AI adoption and renewable-energy share:

| Category | Interpretation |
|---|---|
| **Sustainable AI Leaders** | Above-median AI adoption and above-median renewable-energy share |
| **Environmental Risk** | Above-median AI adoption and below-median renewable-energy share |
| **Green Potential** | Below-median AI adoption and above-median renewable-energy share |
| **Developing Transition** | Below-median AI adoption and below-median renewable-energy share |

The data-centre visualisation retains these original AI-and-renewables categories. Its colours therefore represent the AI sustainability category, **not** data-centre count.

### 4. Correlation analysis

The final correlation heatmap compares:

- Average AI adoption across **2021, 2023, and 2024**
- Average renewable-energy share across **2021, 2023, and 2024**
- Average power-sector CO₂ emissions across **2021, 2023, and 2024**
- Current listed data-centre facilities from the PeeringDB snapshot

Using averages prevents the comparison from depending entirely on one year, while preserving the fact that data-centre records are a current snapshot rather than a historical series.

---

## Key Findings

- Enterprise AI adoption increased sharply in the later reported years, especially from 2023 onward.
- High AI adoption occurs in countries with very different renewable-energy shares; AI adoption alone is not a sufficient indicator of sustainability.
- Listed data-centre facilities do not show a simple relationship with renewable-energy share. Infrastructure is present in both stronger and weaker renewable-energy systems.
- In the country-level correlation analysis, listed data-centre facilities had the strongest observed positive relationship with average power-sector CO₂ emissions.
- Renewable-energy share showed a modest negative relationship with average emissions.
- Average AI adoption showed little relationship with average power-sector CO₂ emissions.

These results are **descriptive associations**, not causal estimates.

---

## Important Limitations

- **Correlation does not establish causation.** Country size, industrial activity, electricity demand, economic structure, and digital-hub status can all affect the results.
- **Facility counts are not capacity or electricity-use data.** A count of listed facilities does not measure server capacity, power demand, utilisation, or AI-specific workloads.
- **The PeeringDB data are a current snapshot.** They cannot be used to show how data-centre infrastructure changed over time.
- **Facility-source coverage differs from the other datasets.** The PeeringDB query was limited to EU27 country codes, while the AI and energy datasets include some additional European countries. A zero after merging can mean “no matched listing in the queried source,” not necessarily confirmed absence of facilities.
- **Country codes require harmonisation.** In particular, PeeringDB uses `GR` for Greece while Eurostat uses `EL`; this mapping should be handled explicitly before final facility comparisons.
- **2025 is AI-only in the merged data.** Renewable-energy and emissions data currently end in 2024, so 2025 should not be used for combined energy or emissions comparisons.
- **Power-sector emissions are national totals.** They should not be interpreted as emissions caused by AI or by data centres.

---

## Reproducing the Project

```bash
git clone https://github.com/Orgo4ever/data-wrangling-project.git
cd data-wrangling-project

python -m venv .venv
```

Activate the environment:

```bash
# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

Install the core packages:

```bash
pip install pandas numpy matplotlib requests jupyter
```

Recommended notebook order:

1. `sandra_notebook_2_datasets.ipynb.ipynb`
2. `data_centers_cleaning.ipynb`
3. `Kyle's notebook.ipynb`
4. `Aline's notebook.ipynb` *(optional; electricity-price exploration only)*

> The PeeringDB API can rate-limit repeated requests. The data-centre notebook includes pacing and retry logic; rerunning it may take several minutes.

---

## Responsible Interpretation

This project does **not** argue that:

> “AI causes national CO₂ emissions.”

Instead, it supports a more careful conclusion:

> The sustainability of AI and digital-infrastructure growth appears to depend strongly on the national energy mix, the scale of supporting infrastructure, and wider country-level economic conditions.

---

## Authors

Kyle Haustein · Aline · Sandra  
Ironhack Data Analytics Bootcamp
