# **Module 3: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 3.

---

### **[Lesson 1: Introduction to RAG](./Lesson1_Introduction_to_RAG.md)**

#### **Task: Design a RAG Prompt**

*Scenario: A retriever found a context chunk about PTO policy. You need to design the Generator prompt.*

#### **Example Solution:**

Here is a well-structured system prompt that combines the persona, rules, and the retrieved data:

```
# ROLE
You are a helpful and friendly HR assistant for ACME Inc. Your purpose is to answer employee questions accurately based on the official company handbook.

# RULES
1.  You MUST answer the user's QUESTION based ONLY on the provided CONTEXT.
2.  If the information to answer the question is not found in the CONTEXT, you MUST state: "I do not have enough information to answer that question from the employee handbook."
3.  You MUST cite the source of your information at the end of your answer.

# CONTEXT
---
**Source:** "employee_handbook.pdf", page 12
**Title:** "Time Off Policy"
**Content:** "Full-time employees receive 20 days of Paid Time Off (PTO) per year. PTO accrues at a rate of 1.67 days per month. Unused PTO can be rolled over, up to a maximum of 10 days. New employees start with a balance of 0 days and begin accruing PTO on their first day."
---

# QUESTION
How much vacation time do I get, and can I save it for next year if I don't use it?
```

---

### **[Lesson 2: Building and Managing the Knowledge Base](./Lesson2_Building_and_Managing_the_Knowledge_Base.md)**

#### **Task: The Chunking Challenge**

*Scenario: You are given a short document about the solar system and asked to analyze different chunking strategies.*

#### **Example Solution:**

1.  **Manual Chunking:**
    *   **Reasoning:** The most logical splits are between the distinct topics. The first topic is the inner planets, and the second is the gas giants. A further split between each planet makes sense as each is a self-contained idea.
    *   **Implementation:**
        ```
        The Solar System has 8 planets. Mercury is closest to the Sun. It is very hot.
        ---
        Venus is the second planet. It has a thick atmosphere.
        ---
        Earth is the third planet. It is our home.
        ---
        The Gas Giants are Jupiter, Saturn, Uranus, and Neptune.
        Jupiter is the largest planet. It has a Great Red Spot.
        ---
        Saturn is known for its beautiful rings.
        ```

2.  **Fixed-Size Chunking (`chunk_size=70`):**
    *   **First Chunk:** `The Solar System has 8 planets. Mercury is closest to the Sun. It is ve`
    *   **Problem:** This method is blind to the meaning and structure of the text. It unnaturally splits the sentence "It is very hot," losing the semantic relationship and making the chunk less useful for answering questions about Mercury.

3.  **Recursive Chunking (on `\n\n`):**
    *   **First Chunk:** `The Solar System has 8 planets. Mercury is closest to the Sun. It is very hot.\nVenus is the second planet. It has a thick atmosphere.\nEarth is the third planet. It is our home.`
    *   **Second Chunk:** `The Gas Giants are Jupiter, Saturn, Uranus, and Neptune.\nJupiter is the largest planet. It has a Great Red Spot.\nSaturn is known for its beautiful rings.`
    *   **Why it's better:** By splitting on the double newline, the recursive chunker correctly identifies the paragraphs as the primary semantic boundary. It keeps all the inner planets in one chunk and all the gas giants in another, preserving the logical grouping of the original document.

---

### **[Lesson 3: The Retrieval Process](./Lesson3_The_Retrieval_Process.md)**

#### **Task: Choose Your Retrieval Strategy**

*For each scenario, choose between Standard Vector Search, MMR, or Hybrid Search.*

#### **Example Solution:**

1.  **Scenario A: "What is context engineering?"**
    *   **Strategy:** **MMR (Maximal Marginal Relevance)**
    *   **Reasoning:** When asking a broad, definitional question, standard vector search might return several very similar introductory paragraphs. MMR is ideal here because it will retrieve documents that are relevant to "context engineering" but will also ensure they are different from each other, providing a more comprehensive and less redundant overview.

2.  **Scenario B: "My TV is showing error code G-451. What do I do?"**
    *   **Strategy:** **Hybrid Search**
    *   **Reasoning:** The term "G-451" is a critical keyword that has no semantic meaning. A pure vector search might miss it entirely or find documents about unrelated "G-series" errors. Hybrid search is perfect because it combines a literal keyword search (to guarantee it finds "G-451") with a vector search (to find conceptually related troubleshooting steps if an exact match isn't found).

3.  **Scenario C: "How do I add a user to my account?"**
    *   **Strategy:** **Standard Vector Search**
    *   **Reasoning:** This is a very specific, direct query. The answer is likely contained in one or two highly relevant "how-to" documents. The risk of retrieving many redundant, slightly different answers is low. A standard, fast vector search is efficient and sufficient for this kind of direct lookup.

---

### **[Lesson 4: The Generation and Synthesis Process](./Lesson4_The_Generation_and_Synthesis_Process.md)**

#### **Task: Critique a Generated Answer**

*Scenario: A RAG system answers "When did the Roman Empire fall?"*

#### **Example Solution:**

1.  **Is it factually correct?**
    *   Yes, the answer "The Roman Empire fell in 476 AD after Romulus Augustulus was deposed" is factually correct based on the provided context.

2.  **Is it complete?**
    *   No, it is incomplete. It omits the crucial detail that this event is traditionally seen as the end of the **Western** Roman Empire, and it fails to mention the key figure of **Odoacer**, the Germanic chieftain who deposed the emperor.

3.  **Is it trustworthy?**
    *   No, it is not fully trustworthy because it is **missing the citation**. The user has no way of knowing where the information came from.

4.  **Rewrite the Generated Answer:**
    *   A much better answer would be: "According to the provided text, the traditional date for the fall of the **Western** Roman Empire is 476 AD, which is when the last emperor, Romulus Augustulus, was deposed by the Germanic chieftain Odoacer (Source: history_ch4.pdf)." 