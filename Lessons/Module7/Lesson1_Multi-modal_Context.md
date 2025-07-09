# **Module 7, Lesson 1: Multi-modal Context**

### Building on What We've Learned

Welcome to the final module. We've mastered engineering context for text. Now, we look to the horizon. The next frontier of AI is **multi-modality**—the ability for models to understand and reason about multiple types of data at once, not just text. We are no longer just managing words; we are managing pixels and soundwaves.

### Learning Objectives

By the end of this lesson, you will be able to:
*   **Define** "Multi-modal AI" and provide examples of its use cases.
*   **Construct** a multi-modal prompt that includes both text and images.
*   **Describe** how multi-modal RAG can improve answer quality.
*   **Explain** the concept of a "Visual Web Agent."

---

### **1. More Than Words: Prompting with Images**

State-of-the-art models like OpenAI's `GPT-4o` or Google's `Gemini` are inherently multi-modal. They can accept a combination of text and images in a single prompt.

This opens up entirely new possibilities:
*   **Visual Q&A:** Give the model a picture of your refrigerator and ask, "What can I make for dinner?"
*   **Chart Interpretation:** Provide an image of a bar chart and ask, "What was the percentage increase between Q1 and Q2?"
*   **UI Navigation:** Show the model a screenshot and ask, "Where do I click to log out?"

**How it works:**
A "vision encoder" processes the image and converts it into embeddings, similar to how text is processed. These image embeddings are placed into the context window alongside the text embeddings, allowing the model to reason about the relationship between them.

**Code Example: A Multi-modal Prompt with GPT-4o**
```python
from openai import OpenAI
client = OpenAI()

# An image of a flowchart
image_url = "https://www.process.st/wp-content/uploads/2019/07/A-Complete-Guide-to-Process-Flowcharts-2.png"

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {
            "role": "user",
            "content": [
                # The text part of the prompt
                {"type": "text", "text": "Explain this flowchart. What does the diamond shape mean?"},
                # The image part of the prompt
                {"type": "image_url", "image_url": {"url": image_url}},
            ],
        }
    ]
)
print(response.choices[0].message.content)
```

---

### **2. The Future of RAG: Grounding Text in Visuals**

Multi-modality makes RAG even more powerful. When your system retrieves text that describes something visual, you can improve performance by also retrieving the corresponding image.

*   **Scenario:** A support bot for a physical product.
*   **User Query:** "How do I replace the battery in my 'AquaDrill 5000'?"
*   **Standard RAG:** Retrieves the text: "1. Unscrew the two screws on the bottom. 2. Slide off the cover."
*   **Multi-modal RAG:** Retrieves the text *and* an image from the manual with an arrow pointing to the screws.
*   **Result:** The model can generate a much clearer, more helpful response.

### **3. The Future of Agents: Visual Web Agents**

Function calling can also be extended to the visual domain. Instead of just giving an agent *tools*, we can give it *sight*.

**Visual Web Agents** are a cutting-edge architecture that combines vision with action:
1.  **Goal:** The agent is given a goal (e.g., "Book a flight").
2.  **See:** The application takes a screenshot of the current web page and provides it to the agent.
3.  **Act:** The agent "looks" at the screenshot and decides what to do next (e.g., "I need to type 'New York' into the textbox labeled 'Origin'").
4.  **Execute:** The application code executes this action using a browser automation tool (like Selenium) and gives the agent a new screenshot.

This `See -> Act -> Observe` loop continues until the goal is complete. This technology promises to automate any task that can be done in a web browser, fundamentally changing how we interact with computers.

---

### **Key Takeaways**

*   **Multi-modal models** can process and reason about different data types (text, images, audio) in a single prompt.
*   **Multi-modal RAG** enhances retrieval by grounding text-based answers with visual information from images or diagrams.
*   **Visual Web Agents** are an emerging architecture that gives agents "sight," allowing them to navigate and interact with graphical user interfaces to complete complex tasks.

### **Hands-On Task: Design a Multi-modal Interaction**

You are designing a multi-modal RAG system for a home improvement store. A user uploads a photo of a single, specific screw.

**Your Task:**
Describe how the system would work to answer the user's implicit question: "What kind of screw is this, and where can I find it in the store?" What information would the system need in its knowledge base to be able to answer this question effectively? 