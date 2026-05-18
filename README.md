# msc-cryptosent-2026
MSc Thesis — Forecasting Bitcoin volatility using Twitter sentiment analysis. University of Essex Online.

## Overview

This repository contains the implementation of an MSc dissertation 
investigating whether the integration of Twitter-derived sentiment improves 
Bitcoin volatility forecasting compared to price-only baselines.

The research evaluates three sentiment extraction methods — standard VADER, 
a crypto-augmented VADER lexicon developed for this study, and FinBERT — 
across two forecasting architectures (Facebook Prophet and an LSTM neural 
network), using walk-forward cross-validation across 28 folds and formal 
statistical significance testing via the Diebold–Mariano test with the 
Harvey–Leybourne–Newbold small-sample correction.

## Key findings

- No sentiment-enhanced variant outperformed its price-only baseline at the 
  5% statistical significance threshold in either modelling framework.
- FinBERT did not outperform the simpler VADER variants, consistent with the 
  domain-transfer limitation when applying formally-trained transformers to 
  informal crypto Twitter content.
- Regime-stratified analysis revealed a model-dependent mirror pattern: 
  Prophet integrates sentiment most effectively in high-volatility regimes, 
  while LSTM integrates sentiment most effectively in low-volatility regimes.
- Sentiment-enhanced LSTM achieved up to 11 percentage points higher 
  directional accuracy than baseline, despite no statistically significant 
  RMSE improvement.

## Repository structure

## Repository structure

```
msc-cryptosent-2026/
├── README.md                Project overview (this file)
├── LICENSE                  MIT License
├── .gitignore               Files excluded from version control
├── requirements.txt         Python dependencies
├── notebooks/
│   └── cryptosent_dissertation_pipeline.ipynb   Complete pipeline
└── figures/
    ├── fig_4_1_btc_price_volatility.png
    └── fig_4_2_tweet_volume_sentiment.png
```

## Implementation

The complete data pipeline is implemented in the single notebook at 
`notebooks/cryptosent_dissertation_pipeline.ipynb`. The notebook is organised 
into sections matching the methodology described in Chapters 3 and 4 of the 
dissertation:

1. Price data acquisition from Binance and computation of realised volatility
2. Twitter data ingestion and preprocessing
3. Bot filtering using three independent heuristics
4. Sentiment extraction using standard VADER, crypto-augmented VADER, and FinBERT
5. Weekly aggregation with follower-weighted means
6. Prophet modelling (baseline and sentiment-enhanced variants)
7. LSTM modelling (baseline and sentiment-enhanced variants)
8. Walk-forward evaluation with Diebold–Mariano significance testing
9. Regime-stratified analysis

## Requirements

Python 3.10 or later. Install dependencies with: pip install -r requirements.txt
The pipeline was developed in Google Colab, using a T4 GPU for FinBERT 
inference and CPU runtime for other stages. The FinBERT pass over 3.13 
million tweets takes approximately 6 hours of GPU time and is checkpointed 
to allow resumption after session interruption.

## Data sources

- **Price data**: Binance public API, free and unauthenticated.
- **Twitter data**: Kaggle archival dataset 
  [`kaushiksuresh147/bitcoin-tweets`](https://www.kaggle.com/datasets/kaushiksuresh147/bitcoin-tweets). 
  Requires a Kaggle account and API token (`kaggle.json`) for download.

Raw datasets are not committed to this repository due to size and licensing 
considerations. Each notebook section documents the data acquisition step.

The pipeline was developed in Google Colab, using a T4 GPU for FinBERT 
inference and CPU runtime for other stages. The FinBERT pass over 3.13 
million tweets takes approximately 6 hours of GPU time and is checkpointed 
to allow resumption after session interruption.
