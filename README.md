# Research Report Multi-Agent (LangGraph)

A multi-agent system that generates a technical research report, built with LangGraph and Groq (open models like `openai/gpt-oss-20b`).

## Overview

The pipeline runs sequentially through a single `StateGraph` shared between two agents:

```
Researcher Agent  →  Writer Agent  →  Final Report
```

- **Researcher Agent**: takes the `topic` and gathers technical research notes about it.
- **Writer Agent**: takes the research notes (`research_notes`) and turns them into a structured Markdown report.

State is shared between the agents via `AgentState` (a `TypedDict`), and each agent reads/writes only its own keys.

## Requirements

- Python 3.10+
- A free Groq API key from [Groq Console](https://console.groq.com/keys)

## Installation

```bash
pip install langchain langgraph langchain-groq python-dotenv
```

## Setup

Create a `.env` file in the project folder and add your key:

```
GROQ_API_KEY=your_real_api_key_here
```

**Note:** `.env` is already listed in `.gitignore` so your key never gets pushed to GitHub by accident.

## Running

```bash
python main.py
```

The final report is printed to the terminal after both agents finish.

## Project structure

```
.
├── main.py           # core logic (state graph + agents)
├── .env               # API key (not tracked by git)
├── .gitignore
└── README.md
```

## ⚠️ Note on output quality

This system relies on an LLM, so its output is **not deterministic** — the exact same code can produce a different report each run, and in rare cases the model may hallucinate content unrelated to the requested topic.

To reduce this:
- `reasoning_format="hidden"` is set so the model returns only its final answer (not its internal reasoning steps)
- **Always review the generated report before using it in any professional or official context**

## Future improvements (TODO)

- [ ] Add a real search tool (e.g. Tavily / web search) to the Researcher instead of relying only on the model's internal knowledge
- [ ] Add simple validation to check each agent's output is actually related to the requested topic
- [ ] Add a conditional edge that halts the pipeline if research fails
- [ ] Add checkpointing to save and resume graph state
- [ ] Export the report as a Markdown or PDF file instead of just printing it
