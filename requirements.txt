# Hybrid Time Series Forecasting

Comparing statistical, machine learning, deep learning and hybrid approaches for retail demand forecasting.

[View the portfolio case study (PDF)](case-study/hybrid-time-series-forecasting-case-study.pdf)  
[Download the PowerPoint version](case-study/hybrid-time-series-forecasting-case-study.pptx)

## Overview

This project compares multiple forecasting approaches using historical weekly sales data for two retail products:

- **Dataset A:** *The Very Hungry Caterpillar*
- **Dataset B:** *The Alchemist*

The objective was to identify trend and seasonality, compare forecasting performance on unseen data, and assess whether hybrid models could improve on standalone statistical and machine learning approaches.

The central finding was that **classical forecasting remained highly competitive**, while a weighted **Parallel Hybrid model produced the strongest weekly MAPE for both datasets**.

> Forecasting is not about predicting the future with certainty — it is about making better business decisions with the information available today.

## Project background

This work was completed as part of the **University of Cambridge Data Science, Machine Learning & AI Career Accelerator**.

The repository and portfolio case study present the academic assignment in a concise, business-facing format. The underlying methods, results and limitations have not been changed or exaggerated.

## Business problem

Unreliable demand forecasts can contribute to:

- excess inventory and unnecessary storage costs;
- stock shortages and missed sales;
- inefficient procurement and reordering;
- capital being tied up in the wrong products.

This project investigates how different forecasting methods could support better inventory and procurement decisions by producing more reliable estimates of future demand.

## Analytical workflow

1. **Data preparation**
   - Combined the supplied sales files.
   - Checked missing values and date ordering.
   - Filtered the analysis to products with sufficiently recent observations.
   - Focused on data from 2012 onwards to better represent current sales behaviour.

2. **Time-series analysis**
   - STL decomposition.
   - Autocorrelation and partial autocorrelation analysis.
   - Augmented Dickey–Fuller stationarity testing.
   - Ljung–Box residual and autocorrelation testing.

3. **Model development**
   - Auto ARIMA.
   - XGBoost using lag features.
   - LSTM.
   - Sequential SARIMA–LSTM hybrid.
   - Weighted Parallel SARIMA–LSTM hybrid.
   - Monthly SARIMA and XGBoost comparisons.

4. **Evaluation**
   - Mean Absolute Error (MAE).
   - Root Mean Squared Error (RMSE).
   - Mean Absolute Percentage Error (MAPE).
   - Visual comparison against unseen test data.
   - Residual diagnostics where appropriate.

## Weekly forecasting results

| Model | Dataset A MAPE | Dataset B MAPE |
|---|---:|---:|
| Auto ARIMA | 17.17% | 25.79% |
| XGBoost | 26.89% | 34.93% |
| LSTM | 23.85% | 27.15% |
| Sequential Hybrid | 17.13% | 25.80% |
| **Parallel Hybrid** | **17.07%** | **23.31%** |

The weighted Parallel Hybrid achieved the lowest weekly MAPE for both datasets.

Monthly aggregation reduced short-term volatility and produced lower MAPE values for Dataset A, but the same improvement was not observed for Dataset B. Because aggregation changes the forecasting frequency and scale, monthly and weekly results should not be treated as directly equivalent.

## Key findings

### Classical models provided a strong baseline

Auto ARIMA performed strongly on both datasets and was only marginally outperformed by the hybrid approaches. This showed that increased model complexity did not automatically lead to better forecasts.

### Standalone machine learning models missed important structure

XGBoost and LSTM produced smoother forecasts but underrepresented some of the seasonal variation in the sales data. XGBoost required manually engineered lag features, while the LSTM did not benefit enough from the available dataset size to outperform the statistical baseline.

### Hybrid performance depended on architecture

The Sequential Hybrid produced only a small improvement over Auto ARIMA. The weighted Parallel Hybrid was more successful because it combined independent forecasts rather than requiring one model to correct the residuals of another.

### Accuracy was not the only consideration

Lower MAPE did not always correspond to stronger residual independence. Model selection should consider forecast accuracy, residual diagnostics, stability, computational cost and business requirements rather than relying on a single metric.

## Business recommendations

- Use a strong statistical model as the baseline before introducing more complex approaches.
- Apply hybrid models selectively to high-value or operationally important SKUs where modest accuracy improvements justify additional complexity.
- Evaluate performance using rolling time-series validation rather than a single fixed split.
- Monitor forecast error, bias and drift after deployment.
- Add external drivers such as promotions, holidays, price changes and marketing activity.
- Compare models at the frequency used for the actual business decision.
- Automate retraining, monitoring and reporting where the number of SKUs makes manual model management impractical.

## Repository structure

```text
hybrid-time-series-forecasting/
├── README.md
├── requirements.txt
├── case-study/
│   ├── hybrid-time-series-forecasting-case-study.pdf
│   └── hybrid-time-series-forecasting-case-study.pptx
├── notebooks/
│   ├── the-alchemist-forecasting.ipynb
│   └── the-very-hungry-caterpillar-forecasting.ipynb
└── data/
    └── README.md
```

## Running the notebooks

The notebooks were developed in **Google Colab**.

1. Install the required packages:

```bash
pip install -r requirements.txt
```

2. Place the required source files in the `data` directory:

```text
data/book_sales.csv
data/book_sales.xlsx
```

3. Open either notebook and update any Google Drive mounting or file paths to match your environment.

4. Run the cells in order.

The source sales files are not included in this repository.

## Main technologies

- Python
- pandas and NumPy
- Matplotlib and Seaborn
- statsmodels and pmdarima
- scikit-learn
- XGBoost
- TensorFlow / Keras
- sktime
- Hyperopt and Keras Tuner
- Google Colab

## Limitations

- Only two products were examined in detail.
- The models relied primarily on historical sales and did not include external commercial drivers.
- Deep learning performance was constrained by the amount of data available.
- Further hyperparameter tuning was limited by computational resources.
- The best-performing model for these series should not be assumed to be the best model for every SKU.
- MAPE is easy to interpret but should be supplemented with bias, WAPE or scaled errors in a production setting.

## Next focus

I am now exploring how **agents and large language models could support SKU-level demand forecasting**, including:

- automated model selection;
- forecast monitoring and drift detection;
- anomaly investigation;
- model-performance summaries;
- translation of forecast outputs into business-facing recommendations.

The aim is not to replace forecasting models with an LLM, but to investigate how agent-assisted workflows could make large-scale forecasting systems easier to operate, monitor and communicate.

## Author

**Zech George**  
Data Analytics · Forecasting · Applied AI
