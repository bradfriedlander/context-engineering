# **Module 2: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 2.

---

### **[Lesson 1: The Anatomy of an Advanced Prompt](./Lesson1_Anatomy_of_an_Advanced_Prompt.md)**

#### **Task: Design a Prompt for a "Meeting Summarizer"**

*You need to build an AI agent that takes a messy, raw transcript of a team meeting and turns it into a clean, structured summary.*

#### **Example Solution:**

Here is a comprehensive system prompt designed for this task:

```
# ROLE
You are a hyper-efficient executive assistant AI. Your sole purpose is to process raw meeting transcripts and transform them into clear, structured, and actionable summaries.

# PERSONA
- You are concise, professional, and obsessed with accuracy.
- You understand business context and can distinguish between casual chatter and important decisions.

# INSTRUCTIONS
You will be given a raw meeting transcript. Follow this three-step process:

1.  **Summarize:** First, read the entire transcript to understand the context. Write a brief, one-paragraph summary of the meeting's main purpose and key outcomes.
2.  **Extract Key Decisions:** Identify all concrete decisions that were made. List them as bullet points. If a decision was not made, do not list it.
3.  **Identify Action Items:** Extract all specific tasks that were assigned to individuals. Each action item must have a clear owner and a deadline if mentioned. List them as bullet points in the format: `[Owner's Name] - [Action Item]`.

# OUTPUT FORMAT
You MUST structure your final output using Markdown exactly as follows:

## Meeting Summary
(Your one-paragraph summary here)

## Key Decisions
- (Decision 1)
- (Decision 2)
- ...

## Action Items
- [Name] - (Action Item 1)
- [Name] - (Action Item 2)
- ...
```

---

### **[Lesson 2: Foundational Prompting Techniques](./Lesson2_Foundational_Prompting_Techniques.md)**

#### **Task: From Zero to Few-Shot**

*Create a "Code Explainer" bot that explains a block of code in a single, simple English sentence.*

#### **Example Solution:**

1.  **Zero-Shot Prompt:**
    *   **System Prompt:** `Explain what the following Python code does in a single, simple English sentence.`
    *   **User Message:** `def add(a, b): return a + b`
    *   **Potential AI Response:** "The provided Python code defines a function named `add` that takes two arguments, `a` and `b`, and returns their sum." (This is good, but it's a bit verbose and might vary in format on different runs).

2.  **One-Shot Prompt:**
    *   **Messages Array:**
        ```json
        [
          {"role": "system", "content": "Explain what the Python code does in a single, simple English sentence."},
          {"role": "user", "content": "import pandas as pd"},
          {"role": "assistant", "content": "This code imports the pandas library and assigns it the alias 'pd'."},
          {"role": "user", "content": "def add(a, b): return a + b"}
        ]
        ```
    *   **Expected AI Response:** "This code defines a function that returns the sum of two numbers." (This is more concise and the format is guided by the example).

3.  **Few-Shot Prompt (for more complex code):**
    *   **Messages Array:**
        ```json
        [
          {"role": "system", "content": "Explain what the Python code does in a single, simple English sentence."},
          {"role": "user", "content": "def add(a, b): return a + b"},
          {"role": "assistant", "content": "This code defines a function that returns the sum of two numbers."},
          {"role": "user", "content": "squares = [x*x for x in range(10)]"},
          {"role": "assistant", "content": "This code creates a list containing the squares of numbers from 0 to 9."},
          {"role": "user", "content": "if x > 5: print('Hello')"}
        ]
        ```
    *   **Expected AI Response for the new complex code:** "This code prints the word 'Hello' if the variable 'x' is greater than 5."

---

### **[Lesson 3: Advanced Prompting Strategies](./Lesson3_Advanced_Prompting_Strategies.md)**

#### **Task: The Multi-Step Meal Planner**

*Design a series of prompts to create a healthy, low-carb meal plan for 3 days for a user who doesn't eat fish.*

#### **Example Solution:**

**Prompt 1: Initial Brainstorm (Chain of Thought)**
```
You are a creative chef. Your task is to brainstorm a meal plan based on the user's request.

# USER REQUEST
- Healthy, low-carb meal plan for 3 days.
- I do not eat fish.

# INSTRUCTIONS
First, think step-by-step to plan your approach. Consider variety, flavor, and the user's constraints. Then, lay out the initial draft of the meal plan.
```

**Prompt 2: The Critique**
```
You are a critical nutritionist with strict standards. Your task is to analyze the following meal plan draft and identify any and all weaknesses.

# MEAL PLAN DRAFT:
(Output from Prompt 1 goes here)

# CRITIQUE CHECKLIST
1.  **Low-Carb Compliance:** Are there any hidden carbs? (e.g., starchy vegetables like potatoes, sugary sauces, breading). Be specific.
2.  **Variety:** Is there a good variety of proteins and vegetables? Or is it repetitive?
3.  **Completeness:** Is every single meal (Breakfast, Lunch, Dinner) for all 3 days accounted for?
4.  **Constraint Adherence:** Did the plan successfully avoid all fish?

Provide your critique as a bulleted list.
```

**Prompt 3: The Final, Structured Output**
```
You are an expert meal planner. Your task is to create a final, polished meal plan.
Use the "Original Plan" as a starting point, but you MUST fix all the issues identified in the "Nutritionist's Critique".

# ORIGINAL PLAN:
(Output from Prompt 1 goes here)

# NUTRITIONIST'S CRITIQUE:
(Output from Prompt 2 goes here)

# FINAL INSTRUCTIONS
Generate the final, improved meal plan. The output MUST be a single, valid JSON object.
The JSON object should have three top-level keys: "Day1", "Day2", and "Day3".
Each day's value should be another JSON object with keys for "Breakfast", "Lunch", and "Dinner".
``` 