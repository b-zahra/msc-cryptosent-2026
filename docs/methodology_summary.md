# Methodology Summary

This document provides a one-page summary of the methodological design 
implemented in this repository. The full discussion appears in Chapter 3 
of the dissertation.

## Research design

- **Philosophy**: positivist, deductive
- **Method**: predominantly quantitative with a small qualitative component 
  (the construction of the crypto-augmented VADER lexicon)
- **Hypotheses**: H1 (sentiment improves overall forecasting), H2 (FinBERT 
  outperforms VADER), H3 (sentiment uplift is regime-dependent)

## Pipeline

1. **Price data**: Bitcoin OHLCV retrieved from Binance public API, 
   January 2021 – December 2024 (1,461 daily records), trimmed to match 
   the Twitter dataset window of February 2021 – January 2023.

2. **Volatility target**: weekly realised volatility, computed as the 
   annualised standard deviation of daily log returns over a rolling 
   30-day window. Annualisation factor √365 (continuous trading).

3. **Twitter data**: Kaggle archival dataset of 4.69 million Bitcoin-related 
   tweets (Suresh, 2022).

4. **Bot filtering**: three independent heuristics — account age, 
   follower-to-friends ratio, posting frequency. Combined flag rate 
   32.75%, retaining 3.13 million tweets.

5. **Sentiment extraction**: three methods applied to all retained tweets — 
   standard VADER, crypto-augmented VADER (43 added terms), and FinBERT 
   (ProsusAI/finbert on T4 GPU, ~6 hours).

6. **Aggregation**: weekly aggregation with simple and follower-weighted 
   means; 61 weekly observations.

7. **Feature engineering**: 53 weekly observations × 26 columns covering 
   lagged price and sentiment features.

8. **Models**: Prophet (linear decomposition) and LSTM (1 layer, 16 hidden 
   units, dropout 0.2) — each in baseline and six sentiment-enhanced 
   configurations, for 14 total model variants.

9. **Evaluation**: expanding-window walk-forward cross-validation across 
   28 folds; RMSE, MAE, and directional accuracy metrics; 
   Diebold–Mariano significance test with Harvey–Leybourne–Newbold 
   small-sample correction.

10. **Analysis extensions**: regime-stratified per-tercile RMSE, 
    lag sensitivity analysis (lag-1, lag-2, lag-3).
