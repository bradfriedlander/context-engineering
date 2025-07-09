# **Module 2, Lesson 2: Foundational Prompting Techniques**

### Building on What We've Learned

In the last lesson, we learned how to construct an advanced prompt with a persona, rules, and constraints. Now, we'll explore the core techniques for *showing* the model what we want, not just telling it. These techniques control the amount of "in-context learning" you provide to the model.

### Learning Objectives

By the end of this lesson, you will be able to:
*   **Define** and **differentiate** between Zero-Shot, One-Shot, and Few-Shot prompting.
*   **Choose** the appropriate technique based on task complexity, desired output format, and cost.
*   **Structure** a Few-Shot prompt to teach a model a nuanced or complex task.
*   **Select** high-quality examples to maximize the effectiveness of a Few-Shot prompt.

---

### **1. Zero-Shot Prompting: The Basic Request**

**Zero-shot prompting** is when you ask the model to perform a task without giving it *any* prior examples of how to do it. You are relying entirely on the model's pre-existing knowledge.

*   **When to Use:** Simple, common tasks like summarization, general questions, or simple classification.
*   **Advantage:** Simple, fast, and uses the fewest tokens.
*   **Disadvantage:** Less reliable for complex tasks. The output format can be inconsistent.

**Example: Zero-Shot Sentiment Analysis**
```python
system_prompt = "Classify the user's text as 'Positive', 'Negative', or 'Neutral'."
user_text = "The new UI is a bit clunky, but I love the new features."
# The model "knows" what sentiment analysis is, but its output format isn't guaranteed.
# It might say "The sentiment is Positive." or just "Positive".
```

---

### **2. One-Shot Prompting: A Single, Perfect Example**

**One-shot prompting** is where you provide a *single* example of the task. This one example helps clarify the instruction and, more importantly, demonstrates the **exact output format** you expect.

*   **When to Use:** When you need a specific, consistent output format, or when the task is slightly ambiguous.
*   **Advantage:** Massively increases the reliability of the output format with minimal token cost.

**Example: One-Shot for Structured Output**
To improve our sentiment bot, we provide one perfect input/output pair. This shows the model *exactly* how to behave.

```python
# In our message history, we create a single, perfect example.
messages = [
    {"role": "system", "content": "Classify the user's text as 'Positive', 'Negative', or 'Neutral'. Respond with only a single word."},
    # --- The "One-Shot" Example ---
    {"role": "user", "content": "I absolutely adore the new design!"},
    {"role": "assistant", "content": "Positive"},
    # --- The New Task ---
    {"role": "user", "content": "The new UI is a bit clunky, but I love the new features."}
]
# Because of the example, the model is now highly likely to respond with just a single word.
```

---

### **3. Few-Shot Prompting: Guidance Through Repetition**

**Few-shot prompting** is the most powerful of the three. You provide *multiple* (typically 2-5) examples. This allows the model to learn patterns, understand nuance, and handle complex or creative tasks with high fidelity.

*   **When to Use:** For complex tasks, style imitation, or to teach the model how to handle specific edge cases.
*   **Advantage:** The most reliable way to control output without fine-tuning the model itself.
*   **Disadvantage:** Uses the most tokens, which increases cost and latency.

**Example: Few-Shot for Nuanced Classification**
Let's create a bot to classify support tickets. The categories are nuanced. A zero-shot model might confuse "Your app is slow" (Feedback) with a technical issue. Few-shot clarifies this.

```python
# We provide a series of examples to teach the model the nuances.
examples = [
    {"role": "user", "content": "I can't log in."},
    {"role": "assistant", "content": "Technical"},
    {"role": "user", "content": "How do I upgrade my plan?"},
    {"role": "assistant", "content": "Billing"},
    {"role": "user", "content": "Your app is so slow and unresponsive sometimes."},
    {"role": "assistant", "content": "Feedback"} # This example is key!
]

# The final prompt includes the system message, the examples, and the new text to classify.
# The model learns from the pattern and correctly classifies the new text.
```

---

### **4. How to Choose Good Examples**

The quality of your few-shot examples is critical.

*   **Quality over Quantity:** Three high-quality, diverse examples are better than ten repetitive ones.
*   **Cover the Edges:** Choose examples that represent the full range of expected inputs, especially the tricky edge cases that might confuse a zero-shot model.
*   **Avoid Bias:** If all your "doctor" examples use "he/him" pronouns, the model will likely replicate that bias. Ensure your examples are balanced and reflect your desired output.
*   **Consistent Formatting:** The format of your examples should be identical. The model learns the structure as much as the content.

> **Pro-Tip: Start with Zero, Add as Needed**
> Always start with a zero-shot prompt. If it isn't reliable enough, move to one-shot. If the task is too nuanced, move to few-shot. This iterative process helps you find the most token-efficient solution for your problem.

---

### **Key Takeaways**

*   **Zero-Shot:** Tell the model what to do. Fast and cheap, but less reliable for complex tasks.
*   **One-Shot:** Show the model *one* perfect example to control the output format.
*   **Few-Shot:** Show the model *multiple* diverse examples to teach it a nuanced task.
*   Always start with the simplest technique and add complexity only when necessary.

### **Hands-On Task: From Zero to Few-Shot**

**Scenario:**
You want to create a "Code Explainer" bot. Given a block of code, it should explain what the code does in a single, simple English sentence.

**Your Task:**

1.  **Write a Zero-Shot Prompt:** Write a system prompt and a user message containing a simple function (e.g., `def add(a, b): return a + b`). How does the model respond? Is the format consistent?

2.  **Improve it with One-Shot:** Now, modify your prompt to be one-shot. Create a single, perfect example of a function and its one-sentence explanation. Use this to guide the model to produce a more reliable output for the same `add` function.

3.  **Perfect it with Few-Shot:** The `add` function is easy. Let's try something more complex, like a list comprehension or a simple class.
    *   Create 2-3 new examples that show how to explain more complex code.
    *   One example should be simple (like `add`).
    *   The other should be more advanced (e.g., `squares = [x*x for x in range(10)]`).
    *   Use this new few-shot prompt to ask for an explanation of a new, complex piece of code.

This exercise will walk you through the iterative process of prompt development, from a basic request to a sophisticated, example-driven instruction. 