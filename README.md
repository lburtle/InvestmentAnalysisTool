# Investment Analysis Tool

For sake of naming, I will just refer to it as Market Prediction Tool (MPT)

**MPT** is a research-oriented tool designed to integrate **Bayesian Probabilistic Modeling** with **Natural Language Processing** to analyze financial markets. 

The project explores how combining uncertainty-aware price prediction (using Hybrid Bayesian Neural Networks) with fundamental market sentiment (using Transformer-based Large Language Models) can create a robust holistic market view.

---

## Core Concepts

### 1. Hybrid Bayesian Learning
Unlike deterministic stock prediction models that output a single price, MPT utilizes a **Hybrid Bayesian Neural Network (BNN)** implemented in `Pyro` and `PyTorch`. This architecture combines:
- **Deterministic Layers**: Standard linear layers with LayerNorm for feature extraction.
- **Probabilistic Layers**: Bayesian layers where weights and biases are modeled as probability distributions (Normals).

This allows the model to capture **epistemic uncertainty** (model ignorance) and **aleatoric uncertainty** (market noise). The output is not just a price, but a predictive distribution, enabling the generation of confidence intervals and risk-aware trade signals.

### 2. Financial Sentiment Analysis
To account for market fundamentals often invisible to price-action models, MPT incorporates a dedicated NLP pipeline using **FinBERT** (a model pre-trained on financial texts).
- **Process**: Fetches recent news via NewsAPI, filters for financial relevance, and analyzes sentiment at the sentence level.
- **Output**: Generates an aggregate "bullish", "bearish", or "neutral" signal that can act as a regime filter for the quantitative model (e.g., preventing long-entries during negative news cycles).

---

## Repository Structure

### `MPT/`
Contains the core logic for prediction and analysis.
- **`BNN_A.py`**: The primary engine. Handles data fetching (YFinance), feature engineering (RSI, MACD, Bollinger Bands), and the training/inference of the Hybrid BNN.
- **`sentimenttool.py`**: The fundamental analysis module. Queries news, cleans text, and runs Bio-Sentimental analysis using HuggingFace Transformers.
- **`logic.py`**: Orchestration logic (if applicable).

### Infrastructure
- **`docker-compose.yml`**: orchestration for containerized deployment.
- **`Dockerfile`**: Environment definition ensuring reproducible PyTorch/Pyro execution.

---

## Getting Started

### Prerequisites
- Docker & Docker Compose
- Python 3.8+ (for local testing)
- NewsAPI Key (for sentiment analysis)

### Setup & Running

**1. Configure Environment**
Update the `docker-compose.yml` file with your API keys.
```yaml
environment:
  - API_KEY=your_newsapi_key_here
```
> [!IMPORTANT] 
> Do not use quotation marks around your key in the YAML list.

**2. Deploy with Docker**
Run the entire suite in a containerized environment to handle dependencies automatically.
```bash
docker-compose up -d
```

**3. Manual Execution (Optional)**
If running locally, ensure dependencies are installed:
```bash
pip install -r requirements.txt
python BNN_A.py
```

---

## Results & Methodology

The tool operates on a dual-signal methodology:
1. **Quantitative**: The BNN consumes a window of technical indicators (SMA, RSI, MACD, etc.) to project a future price distribution.
2. **Qualitative**: The Sentiment Tool scans the informational landscape to provide a "market mood" context.

By analyzing the divergence or convergence of these two signals, MPT aims to identify high-probability trade setups that are backed by both math and news.

---
*Status: Active Research / WIP*
