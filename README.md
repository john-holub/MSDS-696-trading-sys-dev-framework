# Development of a Multi-Module Trading Strategy Pipeline & Platform Framework

**Course:** MSDS-696 - Data Science Practicum II - Regis University  
**Author:** John Holub
**Last Updated:** December 14, 2025 

__________

## 1. Project Overview

This project aims at building a pipeline framework environment to test strategies to generate consistent, risk-controlled returns in all conditions of financial markets. The goal is to identify **short-term high-frequency alpha** by leveraging quantitative methods for disciplined portfolio management.

The system integrates three distinct modules:
1.  **Sentiment Indicator:** Scraping and classifying text from financial subreddits (r/WallStreetBets, r/stocks).
2.  **ML Clustering of Market Regimes:** Using K-means clustering to determine if we are in a Bull, Bear, or Neutral market.
3.  **Technical Screener:** High-speed filtering of tickers based on technical conditions.

__________

## 2. Data Sources and Acquisition
The pipeline pulls data from a mix of sources to build a complete picture of the market:
* **Reddit API (PRAW):** For extracting raw text from retail traders.
* **Yahoo Finance (yfinance):** For OHLCV (Open, High, Low, Close, Volume) market data.
* **CBOE Weeklys:** utilized a static list of ~660 tickers that have weekly options available (high liquidity).
__________

## 3. Pipeline Modules

### A. Sentiment Analysis (The "Vibe Check")
I utilized **VADER (Valence Aware Dictionary and sEntiment Reasoner)** because it is specifically tuned for social media text. It handles caps, slang, and emojis better than standard NLP models.
* **Goal:** Turn qualitative "noise" into a quantitative score.
* **Output:** A compound sentiment score that acts as a pre-filter for the universe of stocks.

### B. Clustering Market Regimes (The "Weather Report")
Most algorithms fail because they don't know *when* to stop trading. I used **Unsupervised Learning (K-Means Clustering)** on historical SPY data (Volatility, Returns, Momentum) to categorize market days.
* **Regime 0:** Low Volatility / Bullish (Safe to trade)
* **Regime 1:** High Volatility / Bearish (Risk off)
* **Regime 2:** Choppy / Neutral (Caution)

### C. Technical Screener
I used `pandas-ta` to calculate indicators like RSI, Bollinger Bands, and MACD. This step filters the 600+ ticker universe down to just the few that match the current regime's criteria.
__________

## 4. Strategy Backtesting

I used **VectorBT** for the backtesting engine. Standard Python loops are too slow for testing thousands of parameter combinations. VectorBT allowed me to simulate:
* Entry/Exit logic based on the Hybrid signals.
* Transaction fees and Slippage.
* Portfolio performance vs. Benchmark (SPY).
__________

## 5. Challenges and Limitations

This wasn't a straight line to success. I hit several major roadblocks:
* **Options Data Dimensionality:** Options chains (Strike x Expiry x Greeks) create massive datasets. I had to use the underlying stock price as a proxy for the options strategy to keep the project manageable.
* **The "Reddit API Apocalypse":** Reddit tightened their API access, which made fetching deep historical sentiment data difficult for a solo developer.
* **Lagging Indicators:** The K-Means clusters accurately described *past* volatility, but sometimes lagged in predicting *future* shifts (reactive vs. proactive).

__________

## 6. Future Roadmap

If I continue this project, the workflow would evolve into this:
1.  **Parquet Storage:** Move away from CSVs to effectively store numerous options chains.
2.  **Unified Backtesting Framework:** Create a modular, reusable framework that integrates defined risk management parameters.
3.  **Generate Production-Ready Scripts:**
    * Once a strategy is validated, the framework will auto-export a clean `.py` file.
    * This script will push orders (Entry, Target, Stoploss) directly to the **IBKR Brokerage API**.
__________

## References: 
Hutto, C., & Gilbert, E. (2014). VADER: A Parsimonious Rule-Based Model for Sentiment Analysis of Social Media Text. Proceedings of the International AAAI Conference on Web and Social Media, 8(1), 216-225. https://doi.org/10.1609/icwsm.v8i1.14550  
(Industry-standard sentiment tool widely used for Reddit, Twitter, and short-form text sentiment.)

Kashcha, A. (2023). Reddit Discovery (redsim). GitHub repository.  
https://github.com/anvaka/redsim  
(Useful to help identify and source similar subreddits.)

Mostafavi, S. M., & Hooman, A. R. (2025). Key technical indicators for stock market prediction. Machine Learning with Applications, 20, 100631.
https://doi.org/10.1016/j.mlwa.2025.100631  
(Used to define most potentially useful technical indicators.)

Polakow, O. (n.d.). GitHub - polakowo/vectorbt. Github. Retrieved 17 November. 2025, from https://github.com/polakowo/vectorbt.  
(Python backtesting library agent used.)

Reddit Inc. (2025). Responsible Builder Policy & API Access Guidelines.
Retrieved from: https://www.reddit.com/r/redditdev/comments/1oug31u/introducing_the_responsible_builder_policy_new/  
(Reddit private API policy update.)

