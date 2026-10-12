## Overview
This notebook explores corporate income tax payment records, performs data quality checks, identifies records that differ from the wider population using Isolation Forest anomaly detection, and groups observations with similar anomaly characteristics using K-Means clustering.
The notebook also examines the distribution of anomaly scores through exploratory analysis, evaluates the separation of anomaly groups, and develops a risk based category for prioritising taxpayers for further investigation.

The resulting outputs are intended to support risk assessment and investigative review, rather than provide direct evidence of non compliance or fraud.

## Data source
The data was downloaded from [statbase](https://statbase.org/data/gha-tax-revenue/), you may need to create an account and pay for subscription to be able to download the full data. I have provided a sample of the data for this project.
Expected columns are `TIN`, `Year`, and the twelve monthly payment columns.

## Repository structure

```text
.
├── analytical_report.ipynb
├── README.md
├── requirements.txt
├── .gitignore
├── data/       
└── outputs/    
```

## Getting started

1. Create and activate a Python virtual environment.
2. Install dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```
3. Open `analytical_report.ipynb` in Jupyter Notebook or JupyterLab.


## Reusable cleaning function

The main function is:

```python
#Ideally best to fill the missing tax payments because the data does not provide any further information
cleaned_tax_data, quality_report = clean_tax_data(
    data_tax,
    fill_missing_payments=False,
)
```

The clean_tax_data function returns a cleaned copy of the input DataFrame and a quality report. You can review and make changes based on your preferences, and what you are looking for.


## Methods

- **Zero tax payment review:** Check to see if there were taxpayers with all twelve monthly values as zero.
- **Isolation Forest:** flags records that are unusual relative to the dataset, using `Year` and monthly payments. The notebook retains `contamination=0.20`.
- **Visualised anomalies and percentiles of the anomalies:** Visualised taxpayers using a histogram to assess the distribution and skewness of the anomaly scores. Percentile analysis was then used to identify the most unusual taxpayers relative to all the taxpayers to support the development of risk based investigation categories. 
- **K-means clustering:** explores groups based on the anomaly score alone. 
- **Risk Assessment:** The groups from the analysis are subgrouped into risk categories high, medium or low risk. 

## Limitations

- I set contamination to 20% which may flag many records. Its best to compare alternative settings and validate against known cases where possible.
- I do not have the data to support further assessment of whether the observed anomalies patterns can be explained by legitimate economic, operational, or reporting factors. 

