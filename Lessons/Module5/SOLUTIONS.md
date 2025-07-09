# **Module 5: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 5.

---

### **[Lesson 1: The Rise of AI Agents](./Lesson1_The_Rise_of_AI_Agents.md)**

#### **Task: Agent or Not?**

*For each scenario, decide if you would need a RAG system or an AI Agent.*

#### **Example Solution:**

1.  **Scenario A: "What is the capital of France?"**
    *   **System Type:** **RAG System**
    *   **Reasoning:** This is a simple fact-retrieval task. The system only needs to look up the answer in a knowledge base and present it. It does not need to perform multiple steps, make decisions, or interact with external tools beyond its own database.

2.  **Scenario B: "Send an email to my team to remind them about the 3 PM meeting today, and add a weather forecast for our city to the email."**
    *   **System Type:** **AI Agent**
    *   **Reasoning:** This task requires multiple actions and the use of several different tools. The system must:
        1.  Access a calendar or contact list to find "my team."
        2.  Use a weather API to get the forecast.
        3.  Compose the text of an email.
        4.  Use an email sending tool to dispatch the final message.
        This multi-step, tool-driven process is the definition of an agent.

3.  **Scenario C: "Summarize the attached meeting transcript."**
    *   **System Type:** **RAG System** (or even just a simple LLM call)
    *   **Reasoning:** Similar to the first scenario, this is a "single-shot" task. The system takes one piece of context (the transcript) and performs one action (summarization). It does not require a dynamic loop or external tools.

---

### **[Lesson 2: Designing and Integrating Tools (Function Calling)](./Lesson2_Designing_and_Integrating_Tools.md)**

#### **Task: Design a Tool Specification**

*Write a complete JSON Schema tool definition for `search_products(query: str, category: str = None, on_sale_only: bool = False)`.*

#### **Example Solution:**

```json
{
    "type": "function",
    "function": {
        "name": "search_products",
        "description": "Searches the product catalog based on a query and optional filters.",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "The main search term to look for in product names and descriptions, e.g., 'running shoes'."
                },
                "category": {
                    "type": "string",
                    "description": "The specific category to filter the search results by.",
                    "enum": ["electronics", "apparel", "home_goods"]
                },
                "on_sale_only": {
                    "type": "boolean",
                    "description": "If true, the search will only return products that are currently on sale. Defaults to false."
                }
            },
            "required": ["query"]
        }
    }
}
```

---

### **[Lesson 3: Agentic Frameworks and Architectures](./Lesson3_Agentic_Frameworks_and_Architectures.md)**

#### **Task: Choose Your Architecture**

*For each scenario, decide which agent architecture would be more appropriate: ReAct or Plan-and-Execute.*

#### **Example Solution:**

1.  **Scenario A: Travel Booker**
    *   **Architecture:** **Plan-and-Execute**
    *   **Reasoning:** The workflow for booking a trip is highly predictable and sequential. The agent can create a stable plan upfront: 1. Search Flights. 2. Search Hotels (using flight dates). 3. Search Cars. 4. Confirm. The steps are known, and the output of one step feeds directly into the next. A ReAct agent's flexibility isn't needed and could introduce unreliability.

2.  **Scenario B: Autonomous Researcher**
    *   **Architecture:** **ReAct (Reason and Act)**
    *   **Reasoning:** This is the ideal use case for ReAct. The agent has no way of knowing the path to the final answer at the beginning. It must be able to explore, read a document, decide based on that document to search for a new term, and dynamically adapt its plan based on its observations. A pre-defined plan would be impossible to create and would fail immediately.

3.  **Scenario C: Automated Coder**
    *   **Architecture:** **Plan-and-Execute**
    *   **Reasoning:** While coding can be exploratory, this specific task is very linear. The agent can form a simple, robust plan: 1. Write Python code for the GET request. 2. Execute the script. 3. If there are errors, enter a debugging loop; otherwise, save the result. This workflow is structured enough that a pre-defined plan is more reliable than the dynamic reasoning of ReAct. 