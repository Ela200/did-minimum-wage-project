# Dataset Characteristics

**[Notebook](exploratory_data_analysis.ipynb)**

## Dataset Information

### Dataset Source
- **Dataset Link:** [(https://raw.githubusercontent.com/CausalAIBook/MetricsMLNotebooks/main/data/minwage_data.csv]
- **Dataset Owner/Contact:** Dataset used in Callaway (2022) and provided through the CausalML Book repository

### Dataset Characteristics
- **Number of Observations:** 17549
- **Number of Features:** 8
- **Time Period:** 2001–2007

### Causal Framework

The analysis follows a Difference-in-Differences (DiD) design. Counties exposed to a state minimum wage above the federal minimum wage are considered treated units, while counties without such exposure serve as controls.

The objective is to estimate the causal effect of minimum wage policies on teen employment by comparing employment trends before and after treatment across treated and untreated counties.

### Outcome Variable

- **lemp:** Log county-level teen employment (main outcome variable).

### Treatment Variables

- **treated:** Indicator equal to 1 if the state minimum wage exceeds the federal minimum wage.
- **state_mw:** State minimum wage.
- **fed_mw:** Federal minimum wage.
- **G:** First treatment year used to identify treatment timing.

### Covariates

#### Economic Variables

- **annual_avg_pay:** Average annual county pay.
- **lavg_pay:** Log average annual county pay.

#### Demographic Variables

- **pop:** County population.
- **lpop:** Log county population.

#### Geographic and Time Variables

- **year:** Observation year.
- **state_name:** U.S. state.
- **FIPS:** County identifier.
- **region:** Census region.

## Exploratory Data Analysis

The exploratory data analysis is conducted in the [exploratory_data_analysis.ipynb](exploratory_data_analysis.ipynb) notebook, which includes:

- Data loading and initial inspection
- Statistical summaries and distributions
- Missing value analysis
- Feature correlation analysis
- Data visualization and insights
- Data quality assessment
