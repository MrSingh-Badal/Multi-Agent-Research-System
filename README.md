# ResearchMind · Multi-Agent Research System

A four-stage AI research pipeline that searches the web, scrapes the most
relevant source, writes a structured report, and critiques its own output —
built with LangChain, Mistral, and Tavily, and shipped as a Streamlit app.

## How it works

The pipeline runs in four sequential stages:

1. **Search Agent** — a LangChain agent (`ChatMistralAI`) equipped with a
   `web_search` tool (Tavily API). Given a topic, it finds recent, reliable
   web results (titles, URLs, snippets).
2. **Reader Agent** — a second agent equipped with a `scrape_url` tool
   (requests + BeautifulSoup). It picks the most relevant URL from the
   search results and scrapes clean text content from it.
3. **Writer Chain** — a prompt → LLM → parser chain that combines the search
   results and scraped content into a structured report (Introduction, Key
   Findings, Conclusion, Sources).
4. **Critic Chain** — reviews the report and returns a strict score (X/10)
   with strengths, areas to improve, and a one-line verdict.

Two of the four stages (Search, Reader) are true tool-using LangChain
**agents**; the other two (Writer, Critic) are LLM **chains** without tool
access.

## Project structure

| File | Purpose |
|---|---|
| `agents.py` | Defines the search agent, reader agent, writer chain, and critic chain |
| `tools.py` | `web_search` (Tavily) and `scrape_url` (requests + BeautifulSoup) tools |
| `pipeline.py` | CLI entrypoint — runs all four stages end-to-end and prints results |
| `app.py` | Streamlit front-end — same pipeline with a live step-tracker UI, expandable raw outputs, and a downloadable `.md` report |
| `requirements.txt` | Python dependencies |

## Setup

```bash
git clone https://github.com/MrSingh-Badal/Multi-Agent-Research-System.git
cd Multi-Agent-Research-System
python -m venv .venv
source .venv/bin/activate     # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in the project root with your API keys:

```
MISTRAL_API_KEY=your_mistral_key_here
TAVILY_API_KEY=your_tavily_key_here
```

## Usage

**Run the Streamlit app:**

```bash
streamlit run app.py
```

Enter a research topic, hit "Run Research Pipeline," and watch the four
stages complete live. The final report can be downloaded as a `.md` file.

**Run the CLI pipeline:**

```bash
python pipeline.py
```

You'll be prompted for a topic; each stage's output prints to the console.

## Tech stack

- **LLM:** Mistral (`mistral-small-2506`) via `langchain-mistralai`
- **Agent framework:** LangChain (`create_agent`)
- **Search:** Tavily API
- **Scraping:** `requests` + `BeautifulSoup`
- **Frontend:** Streamlit
- **Deployment:** Render (Web Service)

## Deployment (Render)

1. Push this repo to GitHub (already done).
2. On Render: **New → Web Service** → connect this repo.
3. Build command: `pip install -r requirements.txt`
4. Start command: `streamlit run app.py --server.port $PORT --server.address 0.0.0.0`
5. Add `MISTRAL_API_KEY` and `TAVILY_API_KEY` as environment variables in the Render dashboard.

## License

MIT
