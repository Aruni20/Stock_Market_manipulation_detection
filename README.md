# Coordinated Market Manipulation Detection: A Modular ABSA and Emotion-Aware Forecasting Pipeline

**Capturing Fine-Grained, Temporal, and Emotion-Centric Signals from Social Media for Market Surveillance**

Financial markets are increasingly influenced by social media sentiment, coordinated hype campaigns, and sudden news. Traditional market anomaly detection focuses only on price and volume but misses subtle manipulations driven by sentiment and user coordination. This project proposes a robust, end-to-end pipeline combining ABSA, user emotion profiling, time-aware sentiment forecasting, and price prediction to detect suspicious market activity.

---

## Overview

**Research Question:**  
Can we detect potential pump-and-dump or coordinated manipulation events by fusing sentiment dynamics from social media with market behavior?

**Modular Pipeline:**

1. Collect and preprocess raw tweets for a target stock.
2. Perform Aspect-Based Sentiment Analysis (ABSA) to extract aspect-level sentiment.
3. Construct user emotion profiles to capture coordination patterns.
4. Forecast sentiment over time using sequence models.
5. Predict stock returns and compute residual-based anomalies.
6. Fuse sentiment and price anomalies to flag potential manipulation.

This pipeline is designed to simulate and test robustness against events like the Adani pump-and-dump scenario.

---

## Motivation

**Traditional Assumptions:**

- Price and volume deviations alone indicate anomalies.
- Social sentiment is too noisy to be informative.
- Standard regression models suffice for prediction.

**Real Market Observations:**

- Market hype often originates from coordinated social activity.
- Tweets and posts carry multi-aspect sentiment, not just “positive” or “negative.”
- Price movements alone may be normal, but combined with sudden coordinated sentiment spikes, they can indicate manipulation.

**Pipeline Goal:** Integrate syntactic, emotional, and temporal signals from social media with financial features for robust anomaly detection.

---

## Core Challenges and Solutions

### Challenge 1: Tweets Contain Multi-Aspect Sentiment

**Problem:** Tweets often mention multiple aspects like management, governance, fundamentals, or hype, making simple sentiment aggregation misleading.

**Solution:**

- Clean tweets while preserving emojis and symbols that convey hype (🚀, etc.).
- Split tweets into clauses for finer granularity.
- Detect aspects via embedding similarity with predefined aspect descriptions.
- Classify sentiment using FinBERT or transformer-based models.
- Construct aspect-sentiment triplets per tweet.

**Outcome:** Structured ABSA vector for each ticker per timestamp, ready for aggregation.

---

### Challenge 2: User Coordination and Emotion Profiles

**Problem:** Not all users are equal; coordinated groups can amplify sentiment artificially.

**Solution:**

- Extract emotion vectors for users (joy, fear, anger, trust).
- Compute user-level distributions to capture typical behavior.
- Detect clusters of accounts posting similar emotions simultaneously, indicating potential coordinated activity.

**Outcome:** Features reflecting social manipulation patterns, added to the ABSA vectors for forecasting.

---

### Challenge 3: Time-Aware Sentiment Forecasting

**Problem:** Sentiment evolves over time; sudden spikes may indicate unusual behavior.

**Solution:**

- Train sequence models (LSTM, seq2seq, or transformer) on aggregated ABSA + emotion vectors.
- Predict next 15–30 minutes of sentiment distribution.
- Compute deviations: actual sentiment vs forecasted → sentiment anomaly score.

**Outcome:** Highlights unexpected surges in hype or negativity, which may precede price manipulation.

---

### Challenge 4: Market Forecasting and Residual-Based Detection

**Problem:** Price spikes alone are insufficient; they need context.

**Solution:**

- Train regression models (Random Forest, XGBoost) with features:
  - Past returns, volatility, volume
  - Aspect-level sentiment aggregates
  - User coordination metrics
- Predict next-period returns and compute residuals:

\[
\epsilon_i = r_i - \hat{r}_i
\]

- Estimate uncertainty via rolling standard deviation or GARCH.
- Construct prediction intervals (\(\hat{r} \pm k\sigma\)) to detect outliers.

**Outcome:** Flags price moves that deviate from both historical and sentiment-informed expectations.

---

### Challenge 5: Fusion of Sentiment and Market Anomalies

**Problem:** Either sentiment spikes or price spikes alone may not indicate manipulation.

**Solution:**

- Combine sentiment anomaly and price residual anomaly using a simple fusion rule:
  - If both anomalies occur simultaneously → manipulation flagged.
  - Otherwise → noise or normal volatility.

**Outcome:** Detects coordinated pump-and-dump scenarios with higher confidence and interpretable signals.

---

## Architecture Overview


```
[ Raw Tweets & OHLCV Data ]
            ↓
[ Preprocessing & Clause Splitting ]
            ↓
[ Aspect-Based Sentiment Analysis (ABSA) ]
            ↓
[ User Emotion Profiling & Clustering ]
            ↓
[ Time-Aware Sentiment Forecasting ]
            ↓
[ Price Prediction / Residual Analysis ]
            ↓
     → Fusion → Manipulation Flags


```
---

## Experimental Setup

- **Data:** Simulated tweet datasets + historical OHLCV data per ticker.
- **Preprocessing:** Tweet cleaning, aspect extraction, emotion tagging, timestamp alignment.
- **Evaluation Metrics:**
  - ABSA accuracy
  - Cluster-based anomaly detection scores
  - Forecasting RMSE
  - Detection precision/recall for manipulation events

---

## Applications

| Use Case | Description |
|----------|-------------|
| Market Surveillance Dashboards | Detect potential manipulation in near real-time |
| Regulatory Compliance & Audit | Provide evidence-based flags for suspicious activity |
| Trading Strategy Stress Testing | Test robustness of trading models against hype-driven events |
| Social Signal Integration | Enhance conventional market indicators with sentiment intelligence |

---

## Keywords

Aspect-Based Sentiment Analysis, User Emotion Profiling, Time-Aware Forecasting, Residual-Based Price Modeling, Pump-and-Dump Detection, Financial NLP, Twitter Sentiment, Market Anomaly Detection, ABSA Triplets, Coordination Metrics, Random Forest, XGBoost

---

> *This project fuses syntactic, emotional, temporal, and market signals to build a holistic, interpretable system capable of detecting coordinated manipulation events in financial markets.*