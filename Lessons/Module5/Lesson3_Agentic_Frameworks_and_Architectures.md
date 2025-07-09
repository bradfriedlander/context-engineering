# **Module 5, Lesson 3: Agentic Frameworks and Architectures**

### Building on What We've Learned

Building the `LLM -> Tool -> Observation` loop from scratch is instructive, but it's a lot of boilerplate code. For complex applications, we need to manage state, prompt templates, and tool dispatching. This is where **agentic frameworks** come in. They provide high-level abstractions to build, run, and manage agents so we can focus on our application's logic.

### Learning Objectives

By the end of this lesson, you will be able to:
*   **Identify** the purpose of agentic frameworks like LangChain, LlamaIndex, and Autogen.
*   **Differentiate** between a single-agent architecture and a multi-agent architecture.
*   **Describe** the "Plan-and-Execute" agent model as an alternative to ReAct.
*   **Use** a framework like LangChain to create a simple ReAct agent.

---

### **1. Why Use a Framework?**

Agentic frameworks provide a "scaffolding" for your application. They handle the complex, repetitive parts of the agentic loop, such as:
*   **Prompt Engineering:** Providing tested, high-quality prompt templates for different agent types.
*   **Tool Abstraction:** Making it easy to convert Python functions into JSON Schema descriptions the LLM can use.
*   **State Management:** Managing the "memory" or "scratchpad" of the agent's thought process.
*   **The Execution Loop:** Running the `Think -> Act -> Observe` cycle until the task is complete.

**Popular Frameworks:**
*   **LangChain:** The most mature and feature-rich framework. It provides tools for every step of building an LLM application. Its **LangChain Expression Language (LCEL)** allows you to "pipe" components together to define complex logic chains.
*   **LlamaIndex:** While also a full-featured framework, it excels at the data-centric parts of RAG, offering state-of-the-art tools for data ingestion, indexing, and sophisticated retrieval.
*   **Autogen (from Microsoft):** A framework designed specifically for creating **multi-agent systems,** where you can create a team of specialized AI agents that collaborate to solve a problem (e.g., a `CodeWriterAgent` and a `CodeTesterAgent` that work together).

---

### **2. Agent Architectures**

**ReAct (Reason and Act):**
This is the architecture we've been studying. The agent thinks, acts, and observes, re-evaluating its plan after every single step.
*   **Pro:** Highly flexible and dynamic. Can adapt on the fly if a tool fails or an observation is unexpected.
*   **Con:** Can be less reliable for long, multi-step tasks, as the agent can get "stuck in a loop" or lose track of the overall goal.

**Plan-and-Execute:**
This is a more deliberate, two-phase approach.
1.  **Phase 1: Plan:** The agent first creates a full, detailed, step-by-step plan to achieve the user's goal.
2.  **Phase 2: Execute:** The agent then executes each step of the plan in sequence, *without* stopping to re-evaluate the plan.
*   **Pro:** More reliable and predictable for tasks that have a clear, known workflow.
*   **Con:** Less flexible. It cannot adapt if an early step fails or produces an unexpected result.

---

### **3. Building an Agent with LangChain**

This example shows how a framework simplifies agent creation.

```python
from langchain_openai import ChatOpenAI
from langchain.agents import tool, AgentExecutor, create_react_agent
from langchain import hub

# 1. Define Tools with a simple decorator
@tool
def search_wikipedia(query: str) -> str:
    """Searches Wikipedia for the given query."""
    # (Dummy implementation)
    if "apple inc" in query.lower():
        return "Apple Inc. was founded in Cupertino, California."
    return "Not found."

tools = [search_wikipedia]

# 2. Create the Agent
# Pull a pre-built ReAct prompt template from LangChain Hub
prompt = hub.pull("hwchase17/react")
llm = ChatOpenAI(model="gpt-4", temperature=0)
# Bind the LLM, tools, and prompt together
agent = create_react_agent(llm, tools, prompt)
# The AgentExecutor runs the reasoning loop for us
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

# 3. Run the Agent
user_goal = "Where was Apple Inc. founded?"
result = agent_executor.invoke({"input": user_goal})
```

The `verbose=True` argument in the `AgentExecutor` is incredibly useful for debugging, as it prints out the agent's internal "thought" process at each step of the ReAct loop. This shows the true power of frameworks—they handle the complex state management, allowing you to focus on defining high-quality tools.

---

### **Key Takeaways**

*   **Agentic frameworks** (like LangChain) save you from writing boilerplate code by providing abstractions for prompts, tools, and the agent execution loop.
*   **ReAct** is a flexible agent architecture that re-evaluates its plan after every step.
*   **Plan-and-Execute** is a more rigid architecture that creates a full plan first, then executes it without changes.
*   Frameworks make it much easier to build, debug, and maintain complex agents.

### **Hands-On Task: Choose Your Architecture**

For each scenario, decide which agent architecture would be more appropriate: **ReAct** or **Plan-and-Execute**. Explain your reasoning.

1.  **Scenario A: Travel Booker**
    *   A user wants to book a trip. The agent needs to search for flights, then search for hotels for the chosen flight dates, then check for rental cars, and finally confirm the bookings with the user. The workflow is predictable and linear.

2.  **Scenario B: Autonomous Researcher**
    *   A user wants to know "the current state of AI in healthcare." The agent needs to search the web, read multiple articles, follow links, identify key themes, and synthesize a report. The path to the answer is unknown at the start.

3.  **Scenario C: Automated Coder**
    *   A user asks to "write a Python script that makes a GET request to an API and saves the result to a file." The task has clear, sequential steps: write the code, execute it, check for errors, and save the output. 