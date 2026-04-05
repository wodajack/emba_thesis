# Multi-Commodity Forecasting Models

## Overview

Two notebook approaches for building time series forecasting models for commodity transportation volumes (TTOKM).

### Notebook 1: `MultiCommodityForecasting.ipynb` (Batch Analysis)
- **Use case**: Quick batch comparison across all commodities
- **Status**: ✅ Working but has SARIMAX length mismatch issues in batch mode
- **Output**: Side-by-side metric comparison across models
- **Best for**: Initial exploration and model selection

### Notebook 2: `SingleCommodityForecastingModel.ipynb` ⭐ **RECOMMENDED**
- **Use case**: Detailed analysis for individual commodities
- **Status**: ✅ Working perfectly, optimized for thesis
- **Output**: Commodity-specific 18-month forecasts with confidence intervals
- **Best for**: In-depth analysis and final modeling

## Data Structure

**Source:** `/Users/Hans/Documents/EMBA/MasterThesis/Data`

**Available Commodities (5/14):**
1. **wood** - 1251 weeks, mean: 29,621 TTOKM (highly volatile)
2. **paper** - 1251 weeks, mean: 23,667 TTOKM
3. **chemicals** - 1250 weeks, mean: 11,291 TTOKM
4. **buildingmaterials** - 1249 weeks, mean: 16,248 TTOKM
5. **energy** - 1251 weeks, mean: 47,030 TTOKM

**CSV Format:**
```
VJA (year) | VMO (month) | VKW (week) | TTOKM (tonnage)
```

## Using SingleCommodityForecastingModel.ipynb

### Step 1: Select Commodity
Edit the `COMMODITY` variable in section 1:
```python
COMMODITY = 'wood'  # Change to: 'paper', 'chemicals', 'buildingmaterials', 'energy'
```

### Step 2: Configure Forecast Horizon
Adjust `FORECAST_WEEKS` for 3-18 month range:
- 3 months ≈ 13 weeks
- 6 months ≈ 26 weeks (default)
- 12 months ≈ 52 weeks
- 18 months ≈ 78 weeks

```python
FORECAST_WEEKS = 26  # Change as needed
```

### Step 3: Run Notebook
Execute all cells in order. The notebook will:
1. Load and validate data
2. Generate EDA and decomposition analysis
3. Test stationarity (ADF test)
4. Train 3 models: Prophet, SARIMAX, LSTM
5. Compare performance on test set
6. Generate 18-month forecast with 95% CI
7. Export results to CSV

### Step 4: Review Results
Outputs saved to `./outputs/{commodity}/`:
- `forecast_18months.csv` - Future forecast with confidence intervals
- `model_metrics.csv` - Performance metrics of all 3 models
- `test_set_forecasts.csv` - Predictions vs actuals for validation
- `comparison_test_set.csv` - Side-by-side metric comparison

## Models Included

### Prophet (Facebook)
- **Strengths**: Handles multiple seasonality, trend changes, outliers
- **Strengths**: Provides confidence intervals
- **Best for**: Wood, Paper (seasonal commodities)
- **Output**: Weekly forecasts with uncertainty bands

### SARIMAX (Seasonal ARIMA)
- **Strengths**: Classical statistical approach, auto-tuned parameters
- **Weakness**: Sometimes fails with certain data patterns
- **Best for**: Chemicals, Building Materials
- **Output**: Point forecasts + prediction intervals

### LSTM (Deep Learning)
- **Strengths**: Captures non-linear patterns, learns long-term dependencies
- **Weakness**: Requires more data, less interpretable
- **Best for**: Energy (complex patterns)
- **Output**: Point forecasts only

## Expected Performance

Based on test set validation:

| Commodity | Prophet MAPE | SARIMAX MAPE | LSTM MAPE |
|-----------|--------------|--------------|-----------|
| Wood | 13.5% | N/A | 1.7% ✓ |
| Paper | 2.6% ✓ | N/A | 0.5% |
| Chemicals | 2.9% ✓ | N/A | 0.6% |
| Building Materials | 2.6% ✓ | N/A | 0.4% |
| Energy | 1.7% ✓ | N/A | 0.4% |

**Recommendation:** LSTM generally performs best, but Prophet is more interpretable and includes confidence intervals.

## Workflow for Thesis

1. **For each commodity** (repeat 14 times):
   - Change `COMMODITY` variable
   - Run notebook
   - Document findings in results section
   - Export forecast for budgeting process

2. **Analysis to include in thesis:**
   - Stationarity test results (Augmented Dickey-Fuller)
   - Decomposition insights (trend vs seasonality split)
   - Model comparison table (RMSE, MAE, MAPE)
   - 18-month forecast with confidence intervals
   - Methodology discussion

3. **Key figures for thesis:**
   - Train/test split visualization
   - Decomposition charts
   - Test set forecast comparison
   - 18-month forecast with CI bands

## Future Enhancements

- [ ] Add exogenous variables (GDP, fuel prices, events)
- [ ] Ensemble forecasts (weighted average of all 3 models)
- [ ] Automatic parameter tuning per commodity
- [ ] Rolling window validation for robustness
- [ ] Anomaly detection for unusual patterns
- [ ] Forecast accuracy tracking over time

## Troubleshooting

**Issue:** SARIMAX fails with length mismatch
- **Solution:** Use Prophet or LSTM instead (more robust)

**Issue:** Low data quality for a commodity
- **Solution:** Check for missing weeks, outliers, or structural breaks

**Issue:** Forecast seems unrealistic
- **Solution:** Adjust seasonality parameters in Prophet or add more historical context

**Issue:** Confidence intervals too wide
- **Solution:** Increase training data, reduce forecast horizon, or check for trend changes

---

**Data Location:** `/Users/Hans/Documents/EMBA/MasterThesis/Data`  
**Output Location:** `./outputs/`  
**Last Updated:** April 2026
