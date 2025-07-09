# **Module 6: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 6.

---

### **[Lesson 1: Evaluating Context Quality and RAG Performance](./Lesson1_Evaluating_Context_Quality_and_RAG_Performance.md)**

#### **Task: Evaluate a RAG System's Output**

*Scenario: User asks "How do I reset my password?" and gets a simplified answer based on retrieved context.*

#### **Example Solution:**

1.  **Context Precision:** **Good**
    *   **Justification:** The retriever found a chunk of text that is directly relevant to the user's question about passwords. It did not retrieve any irrelevant "noise."

2.  **Context Recall:** **Poor**
    *   **Justification:** The retriever *missed* a critical piece of information that existed in the knowledge base (the ability to reset from the login screen). This failure to find all relevant context means the final answer will be incomplete.

3.  **Faithfulness:** **Good**
    *   **Justification:** The generated answer, "To change your password, go to your account settings," is directly supported by the provided context. It does not invent any information or hallucinate.

4.  **Answer Relevance:** **Okay**
    *   **Justification:** The answer is relevant to the user's question, but it's incomplete due to the poor recall. It answers the question, but not as comprehensively as it could have. It's helpful, but not as helpful as it should be.

---

### **[Lesson 2: Testing, Tracing, and Debugging](./Lesson2_Testing_Tracing_and_Debugging.md)**

#### **Task: Find the Failure**

*Scenario: You are debugging an agent that is supposed to email a summary of the Q2 report to Bob.*

#### **Example Solution:**

1.  **What was the final result?**
    *   An email was successfully sent to `bob@example.com` containing a summary of the **Q1 earnings**, not the Q2 earnings.

2.  **Which component failed?**
    *   The **Planner** (the LLM's reasoning) failed. The tools themselves worked perfectly. The `search_knowledge_base` tool correctly found the Q2 report, and the `send_email` tool successfully sent an email.

3.  **What was the specific error?**
    *   The trace shows that the Planner's "thought" process was flawed. It correctly used the search tool to get the Q2 information but then, for an unknown reason (likely a reasoning error or hallucination), it ignored that observation and composed an email using incorrect information about Q1. The error was in the synthesis/reasoning step *between* the two tool calls.

---

### **[Lesson 3: Security for Context-Aware Systems](./Lesson3_Security_for_Context-Aware_Systems.md)**

#### **Task: Spot the Vulnerability**

*Scenario: An "HR Bot" can access all HR docs and all employee vacation balances, and any employee can use it.*

#### **Example Solution:**

**The Vulnerability:** The single biggest vulnerability is the **lack of access controls on the knowledge base and tools.**

**The Risk:** Any employee could ask the bot, "What is the vacation balance for the CEO?" and the agent, having unrestricted access, would dutifully use the `get_vacation_balance` tool and reveal this private information. This is a massive data leak caused by not filtering the agent's capabilities and data access based on the permissions of the user interacting with it. 