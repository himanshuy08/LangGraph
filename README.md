# LangGraph — Learning Notebooks

Beginner-friendly notebooks for learning how to build AI workflows using [LangGraph](https://github.com/langchain-ai/langgraph).

---

## What is LangGraph?

LangGraph lets you build AI applications as a **graph** — where each step (node) does one job, and edges connect them in sequence or with branching logic. Think of it as a flowchart where each box calls an LLM or runs some logic.

**Core concepts you'll need:**
- **State** — a shared dictionary passed between all steps
- **Node** — a Python function that reads/updates the state
- **Edge** — a connection between nodes (can be conditional)
- **StateGraph** — the object that wires everything together

---

## Notebooks

### `BMI.ipynb`
A simple BMI calculator built as a LangGraph state machine.
Good for understanding: how state flows through nodes, basic graph construction.

### `prompt_chaining.ipynb`
Breaks a complex task into a sequence of LLM calls — each prompt feeds into the next.
Good for understanding: multi-step LLM workflows, passing outputs between nodes.

### `LLM_Overflow.ipynb`
Handles scenarios where LLM context/token limits are exceeded using graph-based flow control.
Good for understanding: conditional edges, error handling in graphs.

---

## Setup

**Install dependencies:**
```bash
pip install langgraph langchain langchain-openai
```

**Set your API key:**
```bash
# On Mac/Linux
export OPENAI_API_KEY=your_key_here

# On Windows
set OPENAI_API_KEY=your_key_here
```

**Run notebooks:**
```bash
jupyter notebook
```
Then open any `.ipynb` file and run cells top to bottom.

---

## Minimal LangGraph Example
```python
from langgraph.graph import StateGraph, END
from typing import TypedDict

# 1. Define shared state
class State(TypedDict):
    input: str
    result: str

# 2. Define nodes (functions that update state)
def process(state: State):
    return {"result": f"Processed: {state['input']}"}

# 3. Build the graph
graph = StateGraph(State)
graph.add_node("process", process)
graph.set_entry_point("process")
graph.add_edge("process", END)

app = graph.compile()

# 4. Run it
output = app.invoke({"input": "hello"})
print(output["result"])  # Processed: hello
```

---

## Requirements

- Python 3.9+
- OpenAI API key (or any LangChain-supported LLM)

---

## Learning Resources

- [LangGraph Docs](https://langchain-ai.github.io/langgraph/)
- [LangGraph Quickstart](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
