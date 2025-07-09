# **Module 2, Lesson 3: Advanced Prompting Strategies**

### Building on What We've Learned

We've covered the anatomy of a prompt and the foundational "shot" techniques. Now we'll explore architectural blueprints: strategies that enable models to perform complex reasoning, self-correct, and deliver data in reliable, machine-readable formats.

### Learning Objectives

By the end of this lesson, you will be able to:
*   **Use Chain-of-Thought (CoT) prompting** to improve a model's performance on reasoning tasks.
*   **Design a multi-step prompt** that incorporates a "self-critique" loop for higher-quality results.
*   **Enforce structured output** like JSON for reliable integration with other software.
*   **Combine** these strategies to solve a complex problem.

---

### **1. Chain-of-Thought (CoT) Prompting: "Show Your Work"**

**Chain-of-Thought (CoT)** is a technique that dramatically improves performance on complex reasoning tasks (e.g., math problems, logic puzzles).

The core idea is simple: instead of asking for just the final answer, you instruct the model to **"think step by step"** and lay out its reasoning *before* giving the answer. By externalizing the reasoning process, the model is less likely to make logical leaps or calculation errors.

**Standard Prompt (Prone to Failure)**
If you ask a model to solve `(5 + (3 * 4) - 2)`, it might just spit out an answer, which could be wrong.

**Chain-of-Thought Prompt (More Robust)**
Here, we explicitly ask it to show its work.

**Code Example:**
```python
problem = "John has 5 apples. He buys 3 more boxes, and each box contains 4 apples. He then gives away 2 apples. How many apples does John have left?"

# We add the simple but powerful instruction to "think step by step."
prompt = f"""
{problem}

First, think step by step and show your reasoning. Then, provide the final answer on a new line.
"""

# Expected AI Response (with CoT):
# Step 1: John starts with 5 apples.
# Step 2: He buys 3 boxes of 4 apples each, which is 3 * 4 = 12 new apples.
# Step 3: His total is now 5 + 12 = 17 apples.
# Step 4: He gives away 2, so 17 - 2 = 15 apples.
#
# Final Answer: 15
```
By forcing the model to break the problem down, we make the result more reliable and the process more transparent and debuggable.

---

### **2. Self-Critique Loops: "Check Your Homework"**

This is an advanced application of CoT where the model is tasked with not just solving a problem, but critiquing its own solution and then improving it. This is incredibly powerful for qualitative tasks like writing, code generation, or complex planning.

The process is a multi-step prompt chain:
1.  **Initial Draft:** Generate a first version.
2.  **Critique:** Prompt the model again, asking it to act as a critic and find flaws in its *own* previous output.
3.  **Final Version:** Prompt it a final time, asking it to generate a new, improved version based on its own critique.

**Example: Improving a piece of writing**
```python
first_draft = "Our system is good. Users like it. We should invest more."

# --- STEP 2: The Critique Prompt ---
critique_prompt = f"""
You are a world-class writing critic. Find weaknesses in the following text. 
Focus on vague language and lack of specific evidence.

# Text to Critique:
{first_draft}
"""
# --- The model critiques itself, pointing out "good" is vague, etc. ---

# --- STEP 3: The Final Polish Prompt ---
final_prompt = f"""
You are a senior business writer. Rewrite the 'Original Text' based on the 'Critique'.
Make the language more active, specific, and impactful.

# Original Text:
{first_draft}

# Critique:
{critique_from_step_2}
"""
```
This process yields much higher quality output because it forces the model to iterate and refine its work.

---

### **3. Enforcing Structured Output (e.g., JSON)**

For application development, you often need the model's output to be in a structured format like JSON so your program can parse it reliably.

> **Pro-Tip: Use a Dedicated JSON Mode**
> Simply *asking* for JSON is unreliable. The best practice is to use a model or API feature that **guarantees** the output will be valid JSON. Many modern APIs (like OpenAI's) now have a "JSON Mode" for this exact purpose.

**Code Example: Using OpenAI's JSON Mode**
```python
import json
# (openai client setup)

text_to_analyze = "The new phone has a great camera and the battery lasts all day, but the screen scratches too easily."

prompt = f"""
Analyze the following product review and extract the pros and cons.
Provide the output as a JSON object with two keys: "pros" and "cons".
# Review: {text_to_analyze}
"""

response = client.chat.completions.create(
  model="gpt-4-turbo-preview",
  # This is the key part for enabling guaranteed JSON output
  response_format={ "type": "json_object" },
  messages=[
    {"role": "system", "content": "You are a helpful assistant designed to output JSON."},
    {"role": "user", "content": prompt}
  ]
)
# This is now guaranteed to be a parsable JSON string
output = json.loads(response.choices[0].message.content)
```

---

### **Key Takeaways**

*   **Chain of Thought (CoT):** Force the model to "show its work" on reasoning tasks to increase accuracy.
*   **Self-Critique:** Use a multi-step prompt to have the model generate, critique, and then refine its own work for higher quality.
*   **JSON Mode:** When you need structured data, use a model's built-in JSON mode for guaranteed reliability.

### **Hands-On Task: The Multi-Step Meal Planner**

**Scenario:**
You are building an AI Meal Planner. The user's request is complex: "I need a healthy, low-carb meal plan for 3 days. I don't eat fish."

**Your Task:**
Design a series of prompts to solve this problem by combining CoT, self-critique, and structured output.

1.  **Prompt 1: Initial Brainstorm (Chain of Thought):**
    *   Write a prompt that asks the model to "think step by step" to create an initial meal plan. It should consider all the user's constraints (healthy, low-carb, 3 days, no fish). Ask it to output its thoughts and then the plan.

2.  **Prompt 2: The Critique:**
    *   Write a second prompt that takes the output from Prompt 1.
    *   Its persona should be a "critical nutritionist." It should check for:
        *   True low-carb status (are there hidden carbs like potatoes or rice?).
        *   Variety (is the user eating chicken for every meal?).
        *   Completeness (are there 3 meals for all 3 days?).

3.  **Prompt 3: The Final, Structured Output:**
    *   Write a final prompt that takes the original plan (from #1) and the critique (from #2).
    *   Instruct it to generate a final, improved meal plan.
    *   This time, it MUST output the plan as a single JSON object. The JSON should have keys for "Day1", "Day2", and "Day3", where each key's value is another object with keys for "Breakfast", "Lunch", and "Dinner".

This task simulates a real-world AI engineering workflow where you chain prompts together to produce a final result that is more thoughtful, accurate, and reliable than any single prompt could achieve. 