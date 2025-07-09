# **Module 4: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 4.

---

### **[Lesson 1: Mastering the Context Window](./Lesson1_Mastering_the_Context_Window.md)**

#### **Task: Design a Memory Strategy**

*For each scenario, decide which memory strategy would be more appropriate: Sliding Window or Summarization.*

#### **Example Solution:**

1.  **Scenario A: The "Sarcastic Buddy" Bot**
    *   **Strategy:** **Sliding Window**
    *   **Reasoning:** The bot's primary function relies on immediate, short-term context. It needs to know the last one or two user messages to make a relevant, witty remark. It does not need to remember details from the beginning of the conversation. A sliding window is computationally cheap and perfectly suited for this "what did you just say?" type of memory.

2.  **Scenario B: The "Project Manager" Bot**
    *   **Strategy:** **Summarization Memory**
    *   **Reasoning:** This is the ideal use case for summarization. The bot's tasks are complex and require retaining key pieces of information (deadlines, stakeholders, budget) over a very long conversation. A sliding window would quickly "forget" these critical facts. A running summary allows the bot to maintain a persistent, high-level understanding of the entire project plan.

3.  **Scenario C: The "Live Sports Ticker" Bot**
    *   **Strategy:** **Sliding Window**
    *   **Reasoning:** Similar to the sarcastic bot, this bot's utility is based on recency. The most important information is what happened in the last few minutes. Information from 30 minutes ago is largely irrelevant to a user asking "Who just scored?". A sliding window that keeps the last 5-10 minutes of play-by-play data is efficient and effective.

---

### **[Lesson 2: Contextual Compression and Distillation](./Lesson2_Contextual_Compression_and_Distillation.md)**

#### **Task: To Compress or Not to Compress?**

*For each scenario, decide if contextual compression would be highly beneficial, potentially useful, or likely unnecessary.*

#### **Example Solution:**

1.  **Scenario A: Legal Document Analysis**
    *   **Verdict:** **Highly Beneficial**
    *   **Reasoning:** A 50-page contract contains a massive amount of boilerplate and irrelevant clauses. Retrieving the 5 most relevant *pages* is still far too much context. A compression step that distills those 5 pages down to only the specific sentences containing keywords like "terminate," "termination," "notice period," etc., would dramatically improve the signal-to-noise ratio, leading to a more accurate and cheaper final answer.

2.  **Scenario B: Simple Product FAQ**
    *   **Verdict:** **Likely Unnecessary**
    *   **Reasoning:** The retrieved chunk is already short, concise, and highly relevant. It represents high-information-density context. Running a compression step would add latency and cost for little to no benefit, and it would even risk accidentally filtering out the single important sentence.

3.  **Scenario C: Scientific Research Summarization**
    *   **Verdict:** **Potentially Useful**
    *   **Reasoning:** Research paper abstracts are dense, but may still contain noise relative to a very specific query (e.g., information about funding sources, author affiliations). A conservative compression step could help by extracting only the "Methods" and "Results" sections while filtering out the "Introduction." However, an overly aggressive compression could lose critical nuance, so the trade-off must be carefully managed.

---

### **[Lesson 3: Re-ranking for Relevance](./Lesson3_Re-ranking_for_Relevance.md)**

#### **Task: Design the Final Pipeline**

*Draw a diagram that shows the complete flow of a user query through all the major RAG components.*

#### **Example Solution:**

```mermaid
graph TD
    A[User Query] --> B{1. Retriever (Vector DB)};
    B -- Top 50 docs --> C{2. Metadata Filter};
    C -- Keep docs where year > 2023 --> D{3. Re-ranker (Cross-Encoder)};
    D -- Top 5 most relevant docs --> E{4. Contextual Compressor};
    E -- Extracts key sentences --> F{5. Generator (Main LLM)};
    F --> G[Final Answer];

    subgraph "Context for Generator"
        direction LR
        A_gen[User Query]
        E_gen[Compressed Context]
    end

    A --> A_gen;
    E --> E_gen;
    A_gen --> F;
    E_gen --> F;

```
**Explanation of the Flow:**

1.  The **User Query** is sent to the **Retriever**, which does a fast vector search on the entire knowledge base, returning a large number of candidates (e.g., 50).
2.  These 50 candidates are passed to the **Metadata Filter**, which programmatically removes any document older than 12 months.
3.  The remaining documents are passed to the **Re-ranker**. This slow, accurate cross-encoder scores each document for relevance and sorts them, outputting the top 5.
4.  These 5 high-quality documents are then sent to the **Contextual Compressor**, which uses a smaller LLM to read them and extract only the most essential sentences.
5.  This highly-dense, compressed context is combined with the original **User Query** and sent to the main **Generator** LLM.
6.  The Generator synthesizes the final, accurate answer. 