# Springboard AI for Programmers Course proposal

# AI Monitor: MLflow CLV Model Summary Assistant

## Project Overview

**AI Monitor** is a web application designed to automate and streamline the monitoring the Client Lifetime Value (CLV) models. These models generate outputs that are tracked using MLflow. However, because predictions span many years, real-time validation against ground truth is not feasible.

Currently, users must manually sift through MLflow logs and statistical summaries to determine whether model performance is consistent over time. This process is both time-consuming and prone to human error.

**AI Monitor** leverages AI to automatically analyze these logs, summarize key metrics, and flag any notable shifts or inconsistencies — enabling data scientists and analysts to focus on decision-making rather than manual inspection.

---

## Use Case & Workflow

### Target Users:
- Data scientists, ML engineers, and business analysts working with predictive models in MLflow.

### Current Workflow:
1. Model runs are logged into MLflow, including nested runs for training, scoring and monitoring.
2. Monitoring metrics are generated per scoring event.
3. Users manually examine metrics across many runs to detect changes.

### AI-Powered Workflow:
1. The user selects a model run or experiment in MLflow via the web interface.
2. AI Monitor fetches the monitoring files and metrics (e.g., score distributions, means, variances, percentiles).
3. An AI agent summarizes the metrics and highlights deviations from expected trends or prior runs.
4. The summary is displayed with structured explanations and visual annotations.

**AI Benefit:** Automates metric analysis, highlights potential issues, and reduces manual overhead — especially valuable for long-term prediction models with no immediate ground truth.

---

## AI Features to Be Implemented

| Feature | Description | Relevance |
|--------|-------------|-----------|
| **Prompt Engineering** | Custom prompts crafted to interpret statistical summaries from MLflow. | Ensures AI interprets metric formats correctly and responds with precise analysis. |
| **Structured Outputs** | Outputs presented in JSON or markdown tables with flags and comments. | Enables easy integration with dashboards or alerting tools. |
| **Retrieval-Augmented Generation (RAG)** | Embeds historical monitoring runs into a vector database for context-aware analysis. | Allows AI to compare current metrics to historical patterns. |
| **Evaluation Frameworks** | Tools to assess AI summary accuracy and alignment with subject matter expert judgment. | Ensures summaries are meaningful and trustworthy. |
| **Observability Tools** | Logs AI usage, performance, and user feedback over time. | Helps maintain reliability and improve model responses iteratively. |

---

## Technical Approach

### System Architecture Overview:

User Interface (Web App)  
↓  
Monitoring File Fetcher (MLflow API)  
↓  
AI Engine (LLM + RAG)  
↓  
Summary Renderer (Structured Output + Visuals)  
↓  
Storage (Vector DB, Log Store, Evaluation Metrics)


### Key Components & Tools:
- **Frontend**: React + Tailwind (or Next.js)
- **Backend**: Python (FastAPI)
- **AI Engine**:
  - **LLMs**: OpenAI GPT-4o or Anthropic Claude
  - **Prompt Framework**: LangChain
- **Vector Database**: Pinecone, or FAISS (for RAG)
- **Observability**: LangSmith for tracing & monitoring
- **MLflow Integration**: MLflow tracking API to access model run data
- **Evaluation**: Custom test cases + model agreement scoring

---

## Example Prompts & Expected Outputs

### Example Prompt 1:

```json
{
  "input": "Summarize the monitoring metrics for model `clv_model`, scored on `2024-12-31`. Compare it to the test dataset metrics and previous scoring runs from 2022 and 2023."
}
```

### Expected Output:
### Summary for clv_model - Scored on 2024-12-31

- **Mean Score**: 0.842 (vs 0.840 in 2023, 0.841 in 2022)  Consistent
- **Score Variance**: 0.112 ↑ (vs 0.105 in 2023) Slight increase
- **Top 10% Percentile**: 0.962 (No significant change)
- **Skewness**: -0.34 (Moderately left-skewed; consistent with previous years)
- **KL Divergence (to test set)**: 0.0062 Low divergence

**Conclusion**:  
No major changes detected. Slight increase in score variance may warrant future monitoring but is within acceptable bounds.

Model behavior is stable.

## Evaluation Strategy

To assess system performance and reliability:

### Quantitative Metrics:

- Summary Accuracy Score: Agreement rate with expert-written summaries.

- Drift Detection Accuracy: Correct flagging of real vs. false metric shifts.

- Response Latency: Time to generate summary (goal: < 10 seconds).

- User Satisfaction Rating: 1–5 scale feedback from internal testers.

### Qualitative Methods:

- Expert review of summaries

- A/B testing with manual vs. AI-assisted monitoring

## Observability Plan

To ensure robust monitoring of the AI system:

- Logging:

    - Track every AI call, input file, and summary result

    - Log flagged metrics and user overrides

- Tracing:

    - Use LangSmith to trace prompts, retrieval steps, and outputs

- User Feedback Loop:

    - Thumbs-up/down on summaries

    - Option for users to provide written corrections

- Error Tracking:

    - Capture and categorize failures in metric parsing, RAG, or API calls

## Future Enhancements

- Integrate visualizations (e.g., score distribution plots)

- Support for time-series drift dashboards

- Auto-alerts for significant metric anomalies

- Plugin for Slack or email notifications

Repository Structure (Proposed)

```
/ai-monitor
├── frontend/              # React app
├── backend/               # FastAPI service
│   ├── routes/
│   ├── services/
│   └── models/
├── prompts/               # Prompt templates
├── rag/                   # Vector DB + embeddings
├── evaluations/           # Evaluation scripts
├── observability/         # Tracing and logging tools
└── README.md              # Project documentation
```
