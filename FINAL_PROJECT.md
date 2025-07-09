# **Final Project: Build an AI Research Assistant**

## **Objective**

The goal of this final project is to synthesize the skills and concepts learned throughout this course to build a complete, professional-grade AI agent. You will design and implement an "AI Research Assistant" that can take a complex research query, search for information, synthesize findings, and provide a well-structured, cited answer.

This project will require you to think not just as a prompt engineer, but as a true context architect, making deliberate design decisions about every component of your system.

---

## **Core Requirements**

Your AI Research Assistant must have the following capabilities:

1.  **Tool Use:** The agent must use at least one external tool for searching the web or a database for up-to-date information.
2.  **RAG Implementation:** The agent must use a Retrieval-Augmented Generation pipeline to ground its answers in factual data. This involves:
    *   Creating a vector store from a set of documents (you can use the lessons from this course as your knowledge base!).
    *   Retrieving relevant chunks of information based on the user's query.
3.  **Agentic Logic:** The agent must operate using an agentic loop (like **ReAct**) to reason about the user's query, decide which tool to use (e.g., web search vs. RAG), and synthesize the results.
4.  **Structured Output:** The final answer must be well-formatted (e.g., using Markdown) and must include citations that clearly indicate which parts of the answer came from which retrieved sources.
5.  **Defensive Design:** The agent's prompt must be engineered to be resilient against basic prompt injection attacks.

---

## **Architectural Design (CWA)**

Before you begin coding, you must design your agent's architecture using the principles of **Context Window Architecture (CWA)**. You should be able to describe what information will be placed in each of the following layers of your agent's context:

*   **Layer 1: Instructions:** What is the agent's persona and primary goal?
*   **Layer 3: Curated Knowledge:** What information will your RAG system provide?
*   **Layer 4: Task/Goal State:** How will the agent track its progress on a research task?
*   **Layer 7: Tool Explanation:** How will you describe your search and RAG tools to the agent?
*   **Layer 11: User's Latest Query:** The specific research question.

---

## **Evaluation Plan**

You must define a simple evaluation plan to test your agent's performance. This should include:

1.  A small, hand-crafted evaluation dataset of at least 5 questions.
2.  At least three "natural language unit tests" (using the **LMUnit** paradigm) to evaluate the quality of your agent's responses. Examples could include:
    *   "Does the response directly answer the user's question?"
    *   "Are all claims in the response supported by a citation?"
    *   "Is the formatting of the response clean and readable?"

---

## **Submission**

To complete the project, you should have:
1.  The complete, runnable code for your AI agent.
2.  A `README.md` file for your project that includes:
    *   Your CWA design document.
    *   Your evaluation plan.
    *   Instructions on how to run your agent.
    *   A few example outputs from your agent.

This final project is the ultimate test of your abilities as a context engineer, bringing together every module of this course into a single, practical application. Good luck! 