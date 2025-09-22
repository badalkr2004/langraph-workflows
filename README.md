# Sequential Workflow

LangGraph enables developers to orchestrate sequential workflows for language model agents through explicit, code-controlled graph flows. Here is a comprehensive, step-by-step guide for designing and implementing a sequential workflow using LangGraph, covering state definition, node registration, linkage, compilation, and execution.[^1][^2][^3]

### Core Steps in Sequential Workflow

- Define the shared workflow state structure.
- Implement node functions for each workflow step.
- Initialize a StateGraph and register nodes.
- Specify the sequential order by assigning edges.
- Compile and invoke the completed workflow.[^2][^3][^1]

---

### Step 1: Define Workflow State and Node Functions

- Create a state class (typically a Python `TypedDict` or plain dictionary) containing all fields needed for your workflow (e.g., messages, results, context).[^3][^2]
- Each node implements one step—a pure Python function, which takes the state, performs its task, and returns the updated state.[^1][^2]
- Example:

```python
from typing import TypedDict
class GraphState(TypedDict):
    message: str
    question: str

def welcome(state: GraphState) -> dict:
    state["message"] = "Welcome to LangGraph!"
    return state

def question(state: GraphState) -> dict:
    state["question"] = "How can I help you today?"
    return state
```

---

### Step 2: Initialize StateGraph and Register Nodes

- Create a StateGraph object parameterized by your state class.
- Register each function as a node with a unique name for tracing and debugging.[^1][^2]
- Example:

```python
from langgraph.graph import StateGraph

graph = StateGraph(GraphState)
graph.add_node("welcome", welcome)
graph.add_node("question", question)
```

---

### Step 3: Specify Edges for Sequential Execution

- Use `set_entry_point` to identify the starting node.
- Use `add_edge` to strictly connect nodes step-by-step in execution order.
- End workflows by connecting the last node to the special `END` marker.[^1][^2]
- Example:

```python
from langgraph.graph import END

graph.set_entry_point("welcome")
graph.add_edge("welcome", "question")
graph.add_edge("question", END)
```

---

### Step 4: Compile and Run the Workflow

- Compile the graph to produce an executable workflow object.
- Invoke the workflow by passing an initial state (often empty or seeded data), returning the final state once all steps complete.[^3][^2][^1]
- Example:

```python
workflow = graph.compile()
final_state = workflow.invoke({})
```

---

### Advanced Features

- **Progress Monitoring:** Wrap nodes with progress logging for easier debugging.[^1]
- **Conditional Edges:** While not strictly sequential, conditional edges let you branch and loop as needed, though they are not used in strictly linear workflows.[^4]
- **Integration:** Use with LangChain tools or prompt-based agents for blending sequential logic with LLM tasks.[^5][^3]

---

### Workflow Example Table

| Step         | Description                                | Code Reference                            |
| :----------- | :----------------------------------------- | :---------------------------------------- |
| State        | Defines workflow data structure            | `GraphState` TypedDict                    |
| Node         | Single-task function operating on state    | `welcome`, `question`                     |
| Edge         | Directs sequential execution flow          | `add_edge("welcome", "question")`         |
| Entry/Finish | Starts/stops workflow; bridges to END node | `set_entry_point`, `add_edge(..., END)`   |
| Compile/Run  | Compiles and runs the workflow             | `workflow.compile()`, `workflow.invoke()` |

[^5][^3][^2][^1]

---

### Final Notes

A sequential LangGraph workflow is built on a clearly defined state, sequential node functions, explicit ordering via edges, and direct compilation and invocation for modular, debuggable execution. This method excels for deterministic LLM pipelines and step-wise agentic flows.[^3][^2][^1]
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^6][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.ibm.com/think/tutorials/build-agentic-workflows-langgraph-granite
[^2]: https://www.codecademy.com/article/building-ai-workflow-with-langgraph
[^3]: https://docs.langchain.com/oss/python/langgraph/overview
[^4]: https://veritasanalytica.ai/langgraph-ultimate-guide/
[^5]: https://www.langchain.com/langgraph
[^6]: https://langchain-ai.github.io/langgraph/tutorials/workflows/
[^7]: https://langchain-ai.github.io/langgraph/concepts/why-langgraph/
[^8]: https://www.youtube.com/watch?v=bAWujyAl1Kk
[^9]: https://docs.langchain.com/oss/python/langgraph/workflows-agents
[^10]: https://langchain-ai.github.io/langgraph/
[^11]: https://www.ionio.ai/blog/a-comprehensive-guide-about-langgraph-code-included
[^12]: https://blog.searce.com/building-your-first-agentic-workflow-with-langgraph-and-gemini-llm-a-step-by-step-guide-c173c9dcdfe7
[^13]: https://docs.langchain.com/langgraph-platform
[^14]: https://blog.gopenai.com/langgraph-tutorial-part-1-build-a-simple-agent-workflow-in-python-18a5c6b8e34a
[^15]: https://python.langchain.com/docs/introduction/
[^16]: https://www.langchain.com
[^17]: https://www.langchain.com/langgraph-platform
[^18]: https://www.ibm.com/think/topics/langgraph
