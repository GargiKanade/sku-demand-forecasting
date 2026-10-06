# SKU-Level Demand Forecasting for Festive Planning

A four-week demand forecast for individual products, validated with backtesting, with a model card that states where the forecast can be trusted and where it can't.

## Business question
A category lead needs SKU-level demand for the next four weeks to plan festive-season inventory. How accurate can the forecast be, for which SKUs, and how much safety stock does the uncertainty require?

## Data
- Source: [dataset link, e.g. M5 Forecasting (Walmart) on Kaggle, or another retail dataset]
- Size and period: [number of SKUs, stores, date range]
- Key columns: date, SKU, store, units sold, price, [promotions, holidays]
- Limitations: [e.g. not Indian retail data, so festive effects are proxied by holiday flags]

## Approach
1. **Select SKUs:** group by volume (high, medium, low or intermittent demand) and work on a representative sample of [N] SKUs.
2. **Baselines:** naive and seasonal-naive forecasts, so the model has something to beat.
3. **Models:** [Prophet and/or ARIMA], with holiday and price effects where available.
4. **Backtesting:** rolling-origin evaluation, with [N] windows, each forecasting 4 weeks ahead.
5. **Error analysis:** accuracy by SKU group, by horizon week, and during holiday periods.
6. **Business translation:** convert forecast error into a safety stock suggestion.

## Metrics
- **WAPE** (weighted absolute percentage error): the main metric, because plain MAPE breaks down on low-volume SKUs.
- **MAE** and **bias** (are we systematically over- or under-forecasting?).
- Every model is compared against the seasonal-naive baseline.

## Results
| SKU group | Baseline WAPE | Model WAPE | Bias |
|---|---|---|---|
| High volume | [x%] | [y%] | [z] |
| Medium volume | [x%] | [y%] | [z] |
| Intermittent | [x%] | [y%] | [z] |

Plain-language summary: [one or two sentences, e.g. "The model beats the baseline by x points on high-volume SKUs but not on intermittent ones."]

## Model card

**Intended use:** four-week SKU-level planning for [category].

**Not intended for:** new SKUs with no history, or one-off promotions with no past equivalent.

**Accuracy bounds:** [expected WAPE range by SKU group, from the backtest].

**Drift risks:**
- [Price changes or new competitors shift demand patterns.]
- [Festival timing moves each year, so holiday effects shift.]
- [Stock-outs make observed sales understate true demand.]

**Retraining and monitoring:** [e.g. retrain monthly, flag any SKU whose rolling error exceeds a set threshold].

**Limitations:** [honest list, including data coverage and the sample of SKUs used].

## Recommendations
- [Where to trust the model and where to use judgement or a simple rule.]
- [Suggested safety stock approach by SKU group.]

## How to run
1. `pip install -r requirements.txt`
2. Run the notebooks in `notebooks/` in order.

## Repository structure
- `data/` small sample files or links to the source
- `notebooks/` preparation, models, backtesting
- `outputs/` forecast charts and error tables
- `MODEL_CARD.md` (optional: copy of the model card above)
