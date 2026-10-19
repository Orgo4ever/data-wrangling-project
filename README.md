# Is Europe's AI Boom Environmentally Sustainable?

**A cross-country analysis of AI adoption, renewable energy, power-sector emissions, and data-centre infrastructure in Europe.**

**Python · pandas · Data Cleaning · Data Integration · Exploratory Data Analysis · Data Visualization**

*Ironhack Data Analytics Bootcamp | Collaborative data-wrangling project | 2026*

## Project overview

AI adoption is accelerating across Europe, but rapid digital growth raises questions about the energy systems supporting it. Are countries adopting AI also expanding renewable energy? How does data-centre infrastructure relate to national emissions? And what can public data—and what **can't** it—tell us about environmental sustainability?

This project brings together European country-level indicators to examine those questions. We combined enterprise AI adoption, renewable-energy share, CO₂ emissions from fuel combustion in energy industries, and listed data-centre facilities into a common analytical framework.

**The goal was to identify patterns and differences across countries, not to estimate emissions caused by AI or prove causal relationships.**

### At a glance

| Aspect | What we analyzed |
| --- | --- |
| **Research question** | Is Europe's AI boom developing alongside energy conditions that could support more sustainable growth? |
| **Data sources** | Eurostat (AI adoption, renewable energy, power-sector emissions) and PeeringDB (listed data-centre facilities) |
| **Geographic scope** | European countries available in the source datasets; PeeringDB extraction targeted EU27 countries |
| **Time coverage** | AI adoption: 2021, 2023, 2024, 2025; energy indicators: through 2024; data centres: a directory snapshot |
| **Methods** | API acquisition, cleaning, country/year alignment, dataset merges, exploratory analysis, comparative plots, and correlations |
| **Primary deliverables** | Cleaned CSVs, an integrated analysis dataset, Jupyter notebooks, visualizations, and a team presentation |

## Key findings

Our analysis suggested that **AI adoption is not, by itself, a useful proxy for environmental sustainability**:

- **Enterprise AI adoption rose sharply**, particularly in the later reported years.
- **Higher AI adoption did not consistently coincide with higher renewable-energy shares.** Countries with substantial AI uptake had quite different energy mixes.
- **Listed data-centre counts showed no simple relationship with renewable-energy share.** Data-centre activity appeared across countries with different energy conditions.
- **Data-centre facility counts showed the strongest positive association with average power-sector CO₂ emissions among the country-level relationships examined.** This is a descriptive association—not evidence that the facilities caused those emissions.
- **Renewable-energy share had a modest negative association with average emissions**, while average AI adoption had little observed relationship with power-sector CO₂ emissions.

**Takeaway:** Understanding the environmental context of AI growth requires looking beyond adoption rates to energy systems, infrastructure, and national economic conditions. These indicators can guide further investigation, but they cannot establish AI's environmental footprint on their own.

## Selected visualizations

### 1. How enterprise AI adoption changed

The selected Eurostat series captures reported enterprise AI adoption in **2021, 2023, 2024, and 2025**. The missing 2022 observation is not interpolated.

![Enterprise AI adoption across reported years](ai_adoption_trend.png)

### 2. AI adoption, renewables, and data-centre presence

We used transparent, median-based categories to compare countries with different levels of AI adoption and renewable-energy share. The chart below shows data-centre listings within those groups; **its colors represent the AI/renewables category, not the size of the data-centre footprint**.

![Listed data centres by AI adoption and renewable energy category](data_center_by_ai_category_2024.png)

### 3. How the indicators relate

We examined country-level correlations among AI adoption, renewable-energy share, power-sector emissions, and listed data-centre facilities. Correlation values describe patterns in the available country sample; they are **not causal effects**.

![Country-level correlation matrix](correlation_matrix_2024.png)

**Additional charts:** [AI adoption by country](ai_adoption_2024_top10.png) · [Data centres vs. renewables](data_center_vs_renewable_energy_2024.png) · [Data centres vs. emissions](data_center_vs_emissions_2024.png)

## Data sources and definitions

| Indicator | Source | Interpretation |
| --- | --- | --- |
| **Enterprise AI adoption** | Eurostat SDMX API (`ISOC_EB_AI`) | Share of enterprises using at least one AI technology, based on the selected series for firms with 10+ employed persons |
| **Renewable-energy share** | Eurostat | Country-year percentage of energy from renewable sources |
| **Power-sector CO₂ emissions** | Eurostat (`CRF1A1`) | CO₂ emissions from fuel combustion in energy industries; **not** AI-specific or data-centre emissions |
| **Data-centre facilities** | PeeringDB Facilities API | Number of listed facilities per country in a current directory snapshot; **not** measured electricity consumption or capacity |
| **Electricity prices (exploratory only)** | Eurostat (`nrg_pc_204`) | Investigated during data exploration but excluded from the final comparison because price does not measure generation, demand, or emissions |

Source references: [Eurostat enterprise AI adoption API](https://ec.europa.eu/eurostat/api/dissemination/sdmx/3.0/data/dataflow/ESTAT/ISOC_EB_AI/1.0) · [PeeringDB Facilities API](https://www.peeringdb.com/api/fac)

## Analytical workflow

### 1. Collect and prepare the data

We retrieved enterprise AI adoption data from the Eurostat API and worked with cleaned Eurostat energy and emissions files plus PeeringDB facility data. Source-specific preparation included selecting relevant measures, standardizing dates and identifiers, and aggregating facility listings to the country level.

### 2. Integrate data at the correct level

We aligned the sources using **country code** (`geo`) and **year** where a time dimension was available. The merged analytical data include:

| Field | Meaning |
| --- | --- |
| `geo` | Country identifier |
| `year` | Reporting year |
| `country_name` | Readable country label |
| `ai_adoption_pct` | Enterprise AI adoption share |
| `renewable_energy_share` | Renewable-energy share |
| `emissions` | CO₂ emissions from energy industries |
| `datacenters` | Count of matched PeeringDB facility listings for that country |

We checked country-year duplication and considered coverage mismatches and missing values during integration. **Facility counts were joined as a snapshot**, not treated as a historical series for every year.

### 3. Compare trends and countries

The exploratory analysis considered changes in AI adoption over reported years, country-level rankings, AI adoption versus renewable-energy share, data-centre listings versus renewable share or emissions, and the relationship between renewables and emissions.

For cross-indicator comparisons, we used **2024 as the latest common year** in the available AI and energy datasets; the 2025 AI observations were not paired with unavailable 2025 energy data.

### 4. Create interpretable country groupings

Countries were classified using the median AI-adoption rate and median renewable-energy share in the comparison sample:

| Project category | Definition |
| --- | --- |
| **Sustainable AI Leaders** | Above-median AI adoption; above-median renewable-energy share |
| **Environmental Risk** | Above-median AI adoption; below-median renewable-energy share |
| **Green Potential** | Below-median AI adoption; above-median renewable-energy share |
| **Developing Transition** | Below-median AI adoption; below-median renewable-energy share |

These are **exploratory labels**, not formal sustainability ratings. They do not account for data-centre electricity use, country size, energy imports, or the full lifecycle impacts of AI systems.

### 5. Examine associations responsibly

The final correlation analysis compared country-level averages for AI adoption, renewable-energy share, and power-sector CO₂ emissions over **2021, 2023, and 2024**, alongside PeeringDB facility counts. Averaging the annual indicators reduced dependence on a single reporting year, but it did **not** make the facility snapshot a historical time series.

## My contribution and team collaboration

This was a **group project completed with Aline and Sandra** during the Ironhack Data Analytics Bootcamp.

My principal technical work centered on the integrated analysis:

- Retrieved and prepared enterprise AI-adoption data from Eurostat.
- Built the main Python/Jupyter analytical workflow and integrated the cleaned datasets provided by teammates.
- Standardized country/year fields, merged cross-source indicators, and reviewed consistency in the resulting data.
- Developed the main comparative analyses, visualizations, and investigation of relationships between the indicators.
- Contributed to the interpretation and presentation of the group's findings.

My teammates contributed source-specific cleaning, exploration, and the collaborative presentation. The repository preserves our separate notebooks to make those contributions visible.

## Limitations and analytical decisions

This project is **exploratory**. Several constraints affect what we can conclude:

- **Correlation is not causation.** National emissions also reflect population, industrial activity, electricity demand, economic structure, and other factors.
- **National energy-industry emissions are not AI emissions.** We cannot attribute those emissions to AI workloads or data centres.
- **Facility counts are not electricity demand.** PeeringDB listings do not measure power draw, compute capacity, utilization, or AI-specific workloads.
- **The data-centre information is a snapshot.** It cannot support a historical trend in facility construction or operation.
- **Coverage differs across sources.** PeeringDB queries focused on the EU27, while the Eurostat tables can contain additional countries. Missing or unmatched facilities must not automatically be treated as confirmed zero infrastructure.
- **Country identifiers require care.** For example, PeeringDB uses `GR` for Greece while Eurostat uses `EL`; this difference is important when assessing matched country coverage.
- **Reporting years differ.** AI adoption includes 2025, but the energy indicators used for joint comparisons end in 2024. There is no observed 2022 AI-adoption value in the selected series.
- **Exploratory categories simplify complex conditions.** A high renewable-energy share does not, by itself, prove sustainable AI growth.

These limitations are central to the analysis, not footnotes: they determine which conclusions are defensible.

## Explore the code and data

| Resource | Purpose |
| --- | --- |
| [Main analysis notebook](Kyle%27s%20notebook%5Bmain%20notebook%5D.ipynb) | AI-adoption preparation, integrated data work, exploration, and visualizations |
| [Team EDA notebook](sandra_notebook_EDA_part.6.ipynb) | Source-specific preparation and exploratory analysis |
| [Data-centre cleaning notebook](data_centers_cleaning.ipynb) | PeeringDB facility acquisition and cleaning |
| [Additional exploratory notebook](Aline%27s%20notebook.ipynb) | Electricity-price exploration, excluded from the final comparison |
| [Integrated analysis dataset](merged_data.csv) | Country-year data used in the analysis |
| [Final merged datasets](final_merged_datasets.csv) | Additional merged project output |
| [Project conclusion (PDF)](Is%20Europe%27s%20AI%20boom%20environmentally%20sustainable_%20EDA%20Conclusion%20%281%29.pdf) | Group interpretation and conclusions |

**Repository:** https://github.com/Orgo4ever/data-wrangling-project

## Running the notebooks locally

Clone the repository and create a Python environment:

```bash
git clone https://github.com/Orgo4ever/data-wrangling-project.git
cd data-wrangling-project
python -m venv .venv
```

Activate the environment:

```bash
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install the core analysis packages and start Jupyter:

```bash
python -m pip install pandas numpy matplotlib seaborn requests jupyter
jupyter notebook
```

Open the main notebook to inspect the prepared data, analyses, and figures. Source-collection notebooks can be rerun separately if needed; public API responses and available data may have changed since the project was completed. The PeeringDB API may rate-limit repeated calls.

## Team and project context

**Contributors:** Kyle Haustein, Aline, Sandra  
**Course:** Ironhack Data Analytics Bootcamp  
**Project type:** Collaborative data-wrangling and exploratory data analysis project (2026)

---

**Overall conclusion:** The project identifies differences in how European countries combine AI adoption, data-centre infrastructure, and national energy conditions. It does **not** establish that AI causes national emissions or that any country has achieved environmentally sustainable AI growth. It provides a transparent starting point for more detailed investigation.
