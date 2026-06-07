# Fixion AI

Fixion AI is an advanced, AI-powered verification and self-healing backend designed to analyze, fact-check, and correct text responses. Built using FastAPI, it provides a highly reliable, streaming API that evaluates text claims against real-world evidence retrieved from the web, scoring their factual accuracy and providing auto-corrected responses when hallucinations or unsupported claims are detected.

## ✨ Features

- **🔍 Claim Extraction:** Intelligently breaks down complete responses into individual, testable claims.
- **🌐 Web Evidence Retrieval:** Performs targeted web searches (via DuckDuckGo) to gather high-quality evidence from reliable sources (preferring Wikipedia, .gov, and .edu domains) for each claim.
- **🧠 NLI Verification:** Utilizes a Natural Language Inference (NLI) model (default: `DeBERTa-v3-base-mnli-fever-anli`) to cross-reference claims against retrieved evidence, classifying them as entailment, contradiction, or neutral.
- **📊 Reliability Scoring:** Calculates comprehensive reliability metrics, including faithfulness, grounding, and hallucination scores, to provide an overall confidence assessment.
- **🛠️ Self-Healing Correction:** If a response contains hallucinations or unsupported claims, Fixion AI employs a local LLM via Ollama (e.g., Llama 3) or falls back to the Gemini API to reconstruct a factual, verified response based strictly on supported evidence.
- **📈 Trace Viewer:** Includes a built-in visual execution trace (`trace_viewer.html`) to trace the logic of extraction, retrieval, and verification step-by-step.

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- [Ollama](https://ollama.com/) (Optional, but recommended for local, self-healing corrections)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/nakshatrapandey/Fixion.git
   cd Fixion/FixionAI
   ```

2. **Install the dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Environment Variables (`.env`)**
   Create a `.env` file in the `FixionAI` directory to configure models and keys:
   ```env
   NLI_MODEL=MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli
   CORRECTION_MODEL=llama3
   OLLAMA_URL=http://localhost:11434/api/generate
   GEMINI_API_KEY=your_gemini_api_key_here
   BACKEND_HOST=127.0.0.1
   BACKEND_PORT=8000
   DEBUG=False
   ```

## 💻 Running the Application

Start the FastAPI server:

```bash
uvicorn main:app --reload
```

The API will be available at `http://127.0.0.1:8000`.

## 📡 API Endpoints

### `POST /analyze`
The primary endpoint for analyzing text. It streams Server-Sent Events (SSE) detailing the analysis process in real-time.

**Request Body:**
```json
{
  "query": "The question or context prompt.",
  "response": "The response text to be verified.",
  "demo_mode": false
}
```

**Streamed Events:**
- `query_received`
- `claims_extracted`
- `retrieval_done`
- `evidence_processed`
- `nli_done`
- `scoring_done`
- `correction_running` (if applicable)
- `correction_done` (if applicable)
- `final` (complete analysis payload)

### `GET /trace_viewer.html`
Provides a web interface to visually inspect the node-based execution trace of the latest analysis.

## 🤝 Built For
This project was developed as part of the **Samsung PRISM Hackathon**.
