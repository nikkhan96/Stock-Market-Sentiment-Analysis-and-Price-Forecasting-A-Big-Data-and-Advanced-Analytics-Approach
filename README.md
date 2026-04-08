# Stock Market Analysis with Sentiment Integration

This project implements an end-to-end stock market analysis pipeline that fuses social media sentiment from stock-related tweets with historical price time series for major technology stocks.  The workflow covers data ingestion, storage, sentiment extraction, feature engineering, forecasting models, benchmarking, and basic streaming simulation, all orchestrated in a single Jupyter notebook.

## Project Overview

The notebook `Msc_DA_CA2.ipynb` builds a complete analytics pipeline that includes:
- Social media sentiment analysis on stock tweets using NLTK and VADER  
- Time series forecasting using SARIMAX, LSTM, and GRU models  
- Big data processing with Apache Spark for scalable ETL and analytics  
- Integration with MySQL and MongoDB for relational and NoSQL storage  
- YCSB-style benchmarking to compare database performance  

The analysis focuses on five primary tickers: AAPL, AMZN, MSFT, TSLA, and GOOG, using both tweet text and historical OHLCV price data.

## Repository Structure

Key components used by the notebook:
- `Msc_DA_CA2.ipynb` – Main Jupyter notebook containing all phases of the pipeline  
- `requirements.txt` – Python dependencies required to run the notebook  
- `stock-tweet-and-price/` – Expected root folder for raw data  
  - `stocktweet/stocktweet.csv` – Tweets dataset  
  - `stockprice/*.csv` – Per-ticker historical price files (AAPL.csv, AMZN.csv, etc.)  
- `output/` (created at runtime) – Generated artifacts  
  - `stock_tweets_raw.csv`, `stock_prices_raw.csv`  
  - `tweet_sentiment_daily.csv`, `cleaned_stock_prices.csv`, merged datasets  
  - Model outputs, visualizations, and benchmarking results  

Base paths are configured inside the notebook using `BASE_PATH` and are used to derive data and output directories.

## Main Pipeline Phases

The notebook is organized into the following phases:

- **Phase 1 – Setup & NLTK**  
  Installs packages (via `requirements.txt`) and downloads NLTK resources such as `punkt`, `stopwords`, `wordnet`, and `vader_lexicon`.

- **Phase 2 – Configuration & EDA**  
  - Defines file paths, output directories, and database configuration for MySQL and MongoDB Atlas.
  - Creates Spark sessions for local processing and for DB-integrated workloads.
  - Performs exploratory data analysis on tweets and stock price datasets, including schema inspection, distribution checks, and basic per-ticker statistics.

- **Phase 2C – Data Ingestion**  
  Implements Spark-based ingestion for tweets and OHLCV stock price data, adds ticker identifiers, and consolidates multiple CSV files into unified DataFrames for downstream processing.

- **Subsequent Phases (sentiment, modeling, benchmarking)**  
  Later cells (not fully shown in the snippet) implement sentiment extraction with VADER, merge sentiment features with stock prices, train SARIMAX/LSTM/GRU models, simulate basic streaming, and compare model/database performance.

## Installation

Create and activate a virtual environment, then install dependencies from `requirements.txt`:

```bash
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

The `requirements.txt` includes key packages such as `pyspark`, `pandas`, `numpy`, `nltk`, `scikit-learn`, `keras`, `torch`, `statsmodels`, `streamlit`, `transformers`, `plotly`, `dash`, `psycopg2-binary`, `pymongo`, `vaderSentiment`, `yfinance`, `openpyxl`, `matplotlib`, `seaborn`, `optuna`, `kafka-python`, and `pyarrow`.

## Configuration

Inside the notebook, the following configuration elements must be adapted to your environment:

- **Base path**  
  ```python
  BASE_PATH = os.path.expanduser("~/Desktop/msc-da-ca2-sem-2-2025260")
  ```
  Update this to point to your local project directory so that `DATA_PATH`, `OUTPUT_PATH`, and related paths resolve correctly.

- **MySQL settings**  
  ```python
  MYSQL_HOST = "localhost"
  MYSQL_PORT = "3306"
  MYSQL_DATABASE = "Your Database Name"
  MYSQL_USER = "root"
  MYSQL_PASSWORD = os.environ.get("MYSQL_PASSWORD", "Your Password")
  ```
  Adjust host, port, database name, user, and either set `MYSQL_PASSWORD` as an environment variable or change the default.

- **MongoDB Atlas connection**  
  ```python
  MONGODB_CONNECTION_STRING = "mongodb+srv://<user>:<password>@cluster0...mongodb.net/..."
  MONGODB_DATABASE = "BDSP"
  ```
  Replace the connection string with your own secured MongoDB URI and database name.

Ensure that the corresponding database servers are running and accessible from the environment where the notebook executes.

## Running the Notebook

To execute the pipeline end-to-end:

1. Place `stocktweet.csv` and the `stockprice/*.csv` files under the `stock-tweet-and-price` hierarchy so that the configured paths resolve correctly.  
2. Start Jupyter:  
   ```bash
   jupyter notebook
   ```
3. Open `Msc_DA_CA2.ipynb`.  
4. Run the notebook sequentially from top to bottom, starting with the setup/configuration cells, followed by EDA, ingestion, sentiment, modeling, and benchmarking phases.  

Successful execution will populate the `output/` directory with intermediate and final datasets, model evaluation metrics, and visualizations.

## Requirements Summary

The core Python modules and libraries are listed in `requirements.txt` and include:

- Data & big data: `pyspark`, `pandas`, `numpy`, `pyarrow`  
- NLP & sentiment: `nltk`, `vaderSentiment`, `transformers`  
- ML/DL & stats: `scikit-learn`, `keras`, `torch`, `statsmodels`, `optuna`  
- Visualization & apps: `matplotlib`, `seaborn`, `plotly`, `dash`, `dash-bootstrap-components`, `streamlit`  
- Databases & I/O: `psycopg2-binary`, `pymongo`, `kafka-python`, `openpyxl`, `yfinance`  

Install these via `pip install -r requirements.txt` before running the notebook to avoid import errors.
