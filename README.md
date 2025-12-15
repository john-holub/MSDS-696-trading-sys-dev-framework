# Development of a Multi-Module Trading Strategy Pipeline & Platform Framework

**Course:** MSDS-696 - Data Science Practicum II - Regis University  
**Author:** John Holub  
**Last Updated:** December 14, 2025 

__________

## 1. Project Overview

This project aims at building a pipeline framework environment to test strategies to generate consistent, risk-controlled returns in all conditions of financial markets. The goal is to identify **short-term high-frequency alpha** by leveraging quantitative methods for disciplined portfolio management.

The system integrates three distinct modules:
1.  **Sentiment Indicator:** Scraping and classifying text from financial subreddits (r/WallStreetBets, r/stocks, other defined subreddits).
2.  **ML Clustering of Market Regimes:** Using K-means clustering to determine if we are in a Bull, Bear, or Neutral market.
3.  **Technical Screener:** High-speed filtering of tickers based on technical conditions.

__________

## 2. Data Sources and Acquisition
The pipeline pulls data from a mix of sources to build a complete picture of the market:
* **Reddit HTML & API:** For extracting raw text from retail traders.
* **Yahoo Finance (yfinance):** For OHLCV (Open, High, Low, Close, Volume) market data.
* **CBOE Weeklys:** utilized a updated HTML list of ~660 tickers that have at least weekly options.
__________

## 3. Pipeline Modules

### A. Sentiment Analysis

I utilized **VADER (Valence Aware Dictionary and sEntiment Reasoner)** due to its specialized social media sentiment text generation. 
**Goal:** Turn qualitative content into a quantitative sentiment score.
* **Output:** A compound sentiment score that acts as a pre-filter for the CBOE Weekly universe of tickers.

### B. Clustering Market Regimes (The "Weather Report")
Most algorithms fail because they don't know *when* to stop trading. I used **Unsupervised Learning (K-Means Clustering)** on historical SPY data (Volatility, Returns, Momentum) to categorize market days.
* **Regime 0:** Bull: All is well. 
* **Regime 1:** Bear: Fight against current.
* **Regime 2:** Neutral: Transitionary period.

### C. Technical Screener
I used `pandas-ta` to calculate various technical indicators. This step enriches the 600+ ticker universe down as defined in filters.
__________

## 4. Strategy Backtesting

I used **VectorBT** for the backtesting engine. The test strategy performed poorly, but prooved the platform frameowrk work for:
* Entry/Exit logic based on the Hybrid signals.
* Transaction fees and Slippage.
* Portfolio performance vs. Benchmark (SPY).
__________

## 5. Challenges and Limitations

The project evolved a lot over the course. But some of the most considerable challenges were:
* **Options Data Dimensionality:** Options chains (Strike + Expiry) can create **very** massive datasets, especially over large historic periods. 
* **Reddit API:** Reddit tightened their API access in late November, moving it from relatively public access to private and webform request approval/approved access only, which made fetching a large historical data set difficult.
* **Lagging Indicators:** The K-Means clusters accurately described volatility, but it seems to move rapidly between regimes in volatile periods. It might not be useful in volatile and transitionary environments. 

__________

## 6. Future Roadmap

1. **Remove unproductive modules**: Remove reddit scrapping and other non-productive modules. Hopefully replace with enriched data feeds for more efficiency.
2. **Test numerous backtesting libraries/engines**: Test for usability, ease, adaptability and options integration.
3. **Optimize backtesting results**: Produce better backtesting results, highlighting performance in difference market regimes.
3.  **Generate Production-Ready Scripts:**
    * Once a strategy is validated, the framework will auto-export a clean `.py` file.
    * This script will have option to push orders (Entry, Target, Stoploss) directly to defined brokerage APIs.
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

