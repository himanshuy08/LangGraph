# LangGraph — Learning Notebooks

Beginner-friendly notebooks for learning how to build AI workflows using [LangGraph](https://github.com/langchain-ai/langgraph) with **Ollama** (runs models locally, no API key needed).

---

## What is LangGraph?

LangGraph lets you build AI applications as a **graph** — each step (node) does one job, edges connect them in sequence or with conditional logic.

**Core concepts:**
- **State** — shared dictionary passed between all steps
- **Node** — a Python function that reads/updates state
- **Edge** — connection between nodes (can be conditional)
- **StateGraph** — wires everything together

---

## What is Ollama?

Ollama runs LLMs locally on your machine. No OpenAI key, no cost per token.

---

## Notebooks

| File | What it teaches |
|------|-----------------|
| `BMI.ipynb` | Basic state machine — input flows through nodes, logic runs, result returns |
| `prompt_chaining.ipynb` | Chain multiple LLM calls — output of one prompt becomes input of next |
| `LLM_Overflow.ipynb` | Handle token/context overflow using conditional edges |

---

## Setup

**1. Install Ollama**

Download from [ollama.com](https://ollama.com) and pull a model:
```bash
ollama pull llama3
```

Make sure Ollama is running before executing any notebook:
```bash
ollama serve
```

**2. Install Python dependencies**
```bash
pip install langgraph langchain langchain-ollama
```

**3. Run notebooks**
```bash
jupyter notebook
```

---

## Minimal LangGraph + Ollama Example
```python
from langgraph.graph import StateGraph, END
from langchain_ollama import OllamaLLM
from typing import TypedDict

# 1. Load local model via Ollama
llm = OllamaLLM(model="llama3")

# 2. Define shared state
class State(TypedDict):
    question: str
    answer: str

# 3. Define node
def ask_llm(state: State):
    response = llm.invoke(state["question"])
    return {"answer": response}

# 4. Build graph
graph = StateGraph(State)
graph.add_node("ask_llm", ask_llm)
graph.set_entry_point("ask_llm")
graph.add_edge("ask_llm", END)

app = graph.compile()

# 5. Run
output = app.invoke({"question": "What is LangGraph?"})
print(output["answer"])
```

---

## Requirements

- Python 3.9+
- [Ollama](https://ollama.com) installed and running
- Any Ollama-supported model pulled locally (e.g. `llama3`, `mistral`, `gemma`)

---

## Learning Resources

- [LangGraph Docs](https://langchain-ai.github.io/langgraph/)
- [LangGraph Quickstart](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
- [Ollama Model Library](https://ollama.com/library)
