# News Sentiment Analysis


This repository contains the implementation of a **Two-Stage News Analysis Agent** using the **LangGraph framework**. The agent is designed to classify and analyze financial news articles in two distinct stages:

1. **Stage 1**: Categorizes the news and determines the event type.
2. **Stage 2**: Evaluates the news for its impact, duration, and sentiment.

The workflow is structured as a directed graph, where each stage is represented as a node that processes and mutates a shared `News` state.

---

## Features

- **LangGraph Integration**: Node-based architecture for modular and scalable workflows.
- **Two-Stage Analysis**:
  - **Stage 1**: Classifies news into categories such as `fundamental`, `market_perception`, `technical`, or `noise`.
  - **Stage 2**: Analyzes the news for time horizon, impact magnitude, affected dimensions, and sentiment.
- **Schema Validation**: Ensures strict adherence to JSON schemas using Pydantic.
- **End-to-End Example**: Demonstrates the agent's functionality with real-world data.

---

## Workflow Overview

### Graph Flow

```
Input Node (data from raw.news) 
  → Stage 1 Node (category, event_type) → Filter (noise)
    → Stage 2 Node (time_horizon, duration, impact_magnitude, affected_dimensions, sentiment) → Filter (minor)
      → Output Node (data loaded into core.news_analysis)
```

- The `News` state propagates between nodes.
- Use `ConditionalEdge` for routing logic.
- All outputs are **lowercase JSON**; strict schema validation ensures data integrity.

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/news_sentiment_analysis.git
   cd news_sentiment_analysis
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up the database connection (update the configuration in `config.py`).

---

## Usage

1. Run the main script to process news data:
   ```bash
   python main.py
   ```

2. The agent will:
   - Fetch raw news data from the `raw.news` table.
   - Process the data through Stage 1 and Stage 2.
   - Store the analyzed results in the `core.news_analysis` table.

---

## Input and Output Contracts

### Input Contract

Each news item must provide:

```json
{
  "ticker": "TSLA",
  "headline": "Tesla posts record Q3 deliveries",
  "summary": "Tesla delivered 435,000 vehicles in Q3 2025, exceeding expectations and marking an all-time high.",
  "publisher": "reuters",
  "publish_date": "2025-10-03 12:15:05",
  "url": "https://www.reuters.com/tesla-record-q3-deliveries"
}
```

### Output Contract

```json
{
  "ticker": "ORCL",
  "headline": "Oracle signs $10B multi-year deal with OpenAI",
  "summary": "Oracle will supply cloud infrastructure to OpenAI in a long-term, $10B partnership.",
  "publisher": "reuters",
  "publish_date": "2025-10-03 12:15:05",
  "category": "fundamental",
  "event_type": "customer_contract",
  "time_horizon": "long_term",
  "duration": "5 years",
  "impact_magnitude": "major",
  "affected_dimensions": ["revenue", "technology"],
  "sentiment": "positive"
}
```

---

## Paper

![Paper](news_sentiment_analysis.pdf)

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Built with the **LangGraph framework**.
- Inspired by real-world financial news analysis workflows.
