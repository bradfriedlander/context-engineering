# **Glossary of Key Terms**

This glossary provides definitions for the key technical terms and concepts used in the "Context Engineering for AI" course.

---

### **A**

*   **AI Agent:** A system that uses an LLM to reason, plan, and use tools to achieve a goal, rather than just answering questions.
*   **AI Safety Benchmark:** A standardized test suite used to evaluate an AI system's resilience against a wide range of security and ethical hazards. These benchmarks (e.g., MLCommons AILuminate) systematically test for issues like generating harmful content, enabling crime, or providing unqualified advice.
*   **Agent2Agent (A2A) Protocol:** An emerging open standard designed to allow independent AI agents to communicate and interoperate. It provides a shared language for agents to discover each other, assign tasks, and collaborate to solve complex problems. It is complementary to the Model Context Protocol (MCP).
*   **Answer Relevance:** An evaluation metric that measures whether the generated answer actually addresses the user's specific question.
*   **Autogen:** A framework from Microsoft designed for creating and managing multi-agent systems where specialized agents collaborate to solve problems.

### **B**

*   **Bi-Encoder:** A type of model used for fast retrieval in vector databases. It encodes the query and documents into vectors independently.

### **C**

*   **Chain-of-Thought (CoT):** A prompting technique where the model is asked to "think step-by-step" to break down complex reasoning tasks, improving its accuracy.
*   **Chunking:** The process of breaking large documents into smaller, semantically meaningful snippets before embedding them for a RAG system.
*   **Context:** The information provided to an LLM to ground its response. This includes the system prompt, user query, retrieved data, and any examples.
*   **Context Precision:** An evaluation metric for RAG that measures the relevance of the retrieved documents. ("Of all the documents I retrieved, how many were actually relevant?")
*   **Context Recall:** An evaluation metric for RAG that measures the retriever's ability to find all necessary information. ("Of all the relevant documents that exist, was I able to find them?")
*   **Context Window:** The finite amount of information (measured in tokens) that an LLM can "see" at one time.
*   **Context Window Architecture (CWA):** A conceptual blueprint that standardizes the structure of an LLM's context window. It organizes information into a series of distinct layers (e.g., instructions, user info, RAG results, tool definitions) to create more reliable, debuggable, and performant AI systems by leveraging the model's natural attention patterns.
*   **Contextual Compression:** An optimization technique where retrieved documents are "compressed" (by filtering or extracting key sentences) to remove noise before being passed to the generator LLM.
*   **Cross-Encoder:** A type of model used for highly accurate re-ranking. It examines a query and a document *together* to produce a precise relevance score.

### **F**

*   **Faithfulness:** An evaluation metric that measures whether the generated answer is strictly based on the provided context. It is a direct measure of hallucination.
*   **Few-Shot Prompting:** A technique where the model is given a few examples ("shots") of the desired input/output format within the prompt to guide its response.
*   **Focused Transformer (FoT):** An emerging technique that uses contrastive training to improve the quality of vector embeddings. It trains the model to differentiate more clearly between relevant and irrelevant "distractor" documents, improving the quality of the initial retrieval step in a RAG pipeline.
*   **Function Calling:** The mechanism that allows an LLM to request an action by generating a JSON object specifying a function's name and arguments. Your code then executes this function and returns the result to the LLM.

### **H**

*   **Hybrid Search:** A search strategy that combines keyword-based search (for exact matches) with vector search (for semantic similarity) to produce more robust results.

### **I**

*   **Indexing (in RAG):** The end-to-end process of preparing a knowledge base, which includes ingesting documents, chunking them, and creating vector embeddings.

### **J**

*   **Jailbreaking:** A type of prompt injection aimed at bypassing a model's safety and ethics constraints.
*   **JSON Schema:** A standard format for describing the structure of JSON data. It's used to define the name, parameters, and descriptions of tools for function calling.

### **L**

*   **LangChain:** A popular open-source framework for building, running, and managing LLM applications, including agents and RAG systems.
*   **LLM Observability:** The practice of tracing and monitoring the inputs, outputs, and performance of every component in an LLM application to aid in debugging.
*   **LMUnit:** A framework and paradigm for "natural language unit testing." It involves writing specific, granular checks for an LLM's output in plain English, which are then evaluated by a "judge" LLM to provide fine-grained, actionable feedback on the response's quality.

### **M**

*   **Maximal Marginal Relevance (MMR):** A retrieval technique that optimizes for both relevance to the query and diversity among the results, helping to avoid redundant information.
*   **Model Context Protocol (MCP):** An emerging open standard for structuring and communicating context to an AI model. It provides a formal schema for bundling system prompts, user queries, retrieved data, and tool definitions into a single, coherent package.
*   **Multi-modality:** The ability of an AI model to process, understand, and reason about multiple types of data, such as text and images, in a single prompt.

### **N**

*   **Needle in a Haystack:** A term describing the problem where an LLM's ability to find a specific piece of information degrades as the amount of context ("the haystack") increases.

### **P**

*   **Plan-and-Execute:** An agentic architecture where the agent first creates a complete, step-by-step plan and then executes it without re-evaluating, contrasting with the more dynamic ReAct model.
*   **Prompt Injection:** A security vulnerability where an attacker's input is crafted to be interpreted by the LLM as an instruction, causing it to ignore its original system prompt.

### **R**

*   **RAG (Retrieval-Augmented Generation):** An architecture where an LLM's knowledge is augmented by first retrieving relevant information from an external knowledge base and adding it to the context.
*   **ReAct (Reason + Act):** A common agentic architecture where the agent operates in a loop of Thought -> Action -> Observation, allowing it to reason and dynamically adapt its plan.
*   **Re-ranking:** An optimization step in a RAG pipeline where a slow, highly-accurate model (a cross-encoder) is used to re-order the initial set of documents retrieved by the faster vector search.

### **T**

*   **Tracing:** The process of observing and logging the entire lifecycle of a request as it passes through the stages of an LLM application, which is essential for debugging.

### **V**

*   **Vector Database:** A specialized database designed to store and efficiently search high-dimensional vector embeddings based on semantic similarity.
*   **Vector Embedding:** A numerical representation of a piece of text (or other data) in a high-dimensional space. Texts with similar meanings will have vectors that are close together.
*   **Visual Web Agent:** A cutting-edge agent that can "see" screenshots of a graphical user interface and interact with it to complete tasks.

### **Z**

*   **Zero-Shot Prompting:** A technique where the model is asked to perform a task without being given any prior examples in the prompt. 