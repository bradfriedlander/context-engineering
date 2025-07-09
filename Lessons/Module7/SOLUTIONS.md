# **Module 7: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 7.

---

### **[Lesson 1: Multi-modal Context](./Lesson1_Multi-modal_Context.md)**

#### **Task: Design a Multi-modal Interaction**

*A user uploads a photo of a single, specific screw. Describe how the system would work to answer their implicit question: "What kind of screw is this, and where can I find it in the store?"*

#### **Example Solution:**

Here's how a multi-modal RAG system would handle this:

1.  **Image Analysis (The 'Query'):**
    *   The user's uploaded image is the primary input. The system would first send this image to a multi-modal model with a text prompt like: `"Analyze this image and describe the object's features in detail. Identify the head type (e.g., Phillips, Flathead), thread type (e.g., coarse, fine), material (e.g., steel, brass), and approximate length."`
    *   The text output from this analysis (e.g., `"Phillips head, coarse thread, zinc-coated steel, approx. 1.5 inches"`) becomes the new, text-based query for the RAG system.

2.  **RAG Knowledge Base & Retrieval:**
    *   The knowledge base for this system would need to be a structured database of the store's entire screw inventory.
    *   **Crucially, this database would need to be multi-modal.** Each entry should contain:
        *   `product_name`: "Wood Screw #8 1.5-inch Phillips"
        *   `sku`: "123-456"
        *   `metadata_tags`: { "head": "phillips", "length_in": 1.5, "material": "steel", "finish": "zinc" }
        *   `product_image_embedding`: A vector embedding of the official product photo.
        *   `store_location`: "Aisle 14, Bin 3B"

3.  **Multi-modal Search:**
    *   The system would perform a hybrid search. It would use the descriptive text from Step 1 to perform a vector search against the `metadata_tags`. Simultaneously, it would convert the user's *original image* into a vector embedding and perform a vector search against the `product_image_embedding` column.
    *   By combining these two search methods, the system can find the product that is both visually similar and has the matching physical characteristics.

4.  **Generation:**
    *   The top result from the search is passed to the generator LLM with a prompt like: `"The user uploaded a picture of a screw. Based on our analysis, the best match in our inventory is the following product. Tell the user what the product is and where to find it."`
    *   **Final Answer:** "That looks like a #8 1.5-inch Phillips Head Wood Screw. You can find it in Aisle 14, Bin 3B."

---

### **[Lesson 2: The Evolving Landscape](./Lesson2_The_Evolving_Landscape.md)**

#### **Task: Design a Multi-Agent System**

*Design a multi-agent system that automatically creates a weekly email newsletter about the top 3 biggest news stories in the AI industry.*

#### **Example Solution:**

1.  **List of Agents:**
    *   `NewsResearcherAgent`: Responsible for searching the web for recent, relevant news articles about AI.
    *   `ArticleAnalyzerAgent`: Responsible for reading a given article, determining if it's significant, and writing a one-paragraph summary.
    *   `EditorAgent`: Responsible for selecting the top 3 most important summaries and writing a brief, engaging introduction for the newsletter.
    *   `FormatterAgent`: Responsible for taking the final text from the EditorAgent and formatting it into a clean HTML email template.

2.  **Workflow Description:**
    1.  **Trigger:** The system runs automatically every Friday morning.
    2.  **Research:** The `EditorAgent` gives a task to the `NewsResearcherAgent`: "Find the 10 most cited news articles about AI from the past 7 days." The `NewsResearcherAgent` uses its search tools and returns a list of 10 URLs.
    3.  **Analysis & Summarization:** The `EditorAgent` takes the 10 URLs and, in parallel, gives each URL to a different instance of the `ArticleAnalyzerAgent` with the instruction: "Summarize this article and give it an 'importance score' from 1 to 10."
    4.  **Curation:** The `EditorAgent` collects the 10 summaries and their importance scores. It selects the 3 summaries with the highest scores. It then writes a new introduction for the weekly newsletter.
    5.  **Formatting:** The `EditorAgent` hands off its introduction and the 3 summaries to the `FormatterAgent` with the instruction: "Combine this content into our standard HTML newsletter format."
    6.  **Final Output:** The `FormatterAgent` produces the final HTML file, which is then ready to be sent out to the mailing list. 