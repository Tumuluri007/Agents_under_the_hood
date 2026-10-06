# LangChain- Develop AI Agents with LangChain & LangGraph 🦜

Building an AI agent from scratch to understand how the ReAct loop and tool calling really work, using Ollama, OpenAI and LangSmith tracing.

# Agents Under the Hood

Building an AI agent **from scratch** to understand what really happens inside one: no black boxes. Instead of relying only on ready-made agent helpers, this project implements the core agent loop step by step: reasoning, tool calling and observing results.

## What this project covers

- How a **ReAct agent** (Reason + Act) works internally
- How **tool calling** works: the model returns a structured request, and the code runs the tool
- How the **message history** (`HumanMessage`, `AIMessage`, `ToolMessage`) drives each step
- Running the same agent on a **local model (Ollama)** and a **cloud model (OpenAI)**
- **Tracing** every step with LangSmith to see exactly what the agent did

## How the agent loop works

```
1. User asks a question
        ↓
2. Model reasons: answer directly, or call a tool?
        ↓
3. If tool call → run the tool → add the result as a ToolMessage
        ↓
4. Send the updated history back to the model
        ↓
5. Repeat until the model gives a final answer
```

## Tech stack

| Tool | Purpose |
|---|---|
| LangChain | Messages, tools and model interfaces |
| Ollama ([Llama 3.2]) | Local model: free, offline, private |
| OpenAI API | Cloud model, for comparison |
| LangSmith | Tracing and debugging each agent step |
| uv | Python package and project manager |
| python-dotenv | Loads API keys from `.env` |

## Prerequisites

- Python 3.12+
- [uv](https://docs.astral.sh/uv/)
- [Ollama](https://ollama.com/download), installed and running
- An [OpenAI API key](https://platform.openai.com/api-keys)
- A [LangSmith API key](https://smith.langchain.com/)

## Setup

**1. Clone and install**

```bash
git clone https://github.com/Tumuluri007/Agents_under_the_hood.git
cd Agents_under_the_hood
uv sync
```

**2. Download the local model**

```bash
ollama pull llama3.2
```

**3. Create a `.env` file** in the project folder:

```
OPENAI_API_KEY=your-openai-key
LANGSMITH_API_KEY=your-langsmith-key
LANGSMITH_TRACING=true
LANGSMITH_PROJECT=agents-under-the-hood
```

`.env` is listed in `.gitignore`, so your keys are never uploaded.

## Run it

```bash
uv run main.py
```

Then open [LangSmith](https://smith.langchain.com/) to see the full trace: every model call, tool call and result.

## Switching between models

| Model | Runs on | Cost | Notes |
|---|---|---|---|
| Ollama (Llama 3.2) | Your computer | Free | Works offline; slower on a basic CPU |
| OpenAI | Cloud | Paid per use | Faster, and more reliable at tool calling |

## Key concepts learned

- **Tools**: Python functions the model can ask to run (`@tool`)
- **Tool calling**: the model replies with a tool name and inputs instead of an answer
- **Agent loop**: think, act, observe, repeat until done
- **Tracing**: seeing inside the agent with LangSmith to debug its decisions
- **Local vs cloud models**: the trade-offs between privacy, cost and quality

## Project structure

```
Agents_under_the_hood/
├── main.py          # Agent loop, tools and model setup
├── pyproject.toml   # Dependencies
├── uv.lock          # Exact package versions
├── .gitignore       # Keeps .env and .venv out of Git
└── README.md
```

