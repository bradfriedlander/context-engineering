# **Module 2, Lesson 1: The Anatomy of an Advanced Prompt**

### Building on What We've Learned

In Module 1, we learned that context is the world we build for our AI. Now, we'll zoom in on the most direct and powerful tool for shaping that world: the prompt itself. An advanced prompt is not a simple question; it's a carefully engineered piece of code designed to instruct, constrain, and guide the AI to a specific, desired outcome.

### Learning Objectives

By the end of this lesson, you will be able to:
*   **Differentiate** between the roles of a `System Prompt` and a `User Prompt`.
*   **Craft** an effective `System Prompt` that defines a clear persona, rules, and constraints.
*   **Transform** a vague question into a specific, actionable command.
*   **Combine** these elements to build a specialized AI agent for a defined task.

---

### **1. System vs. User Prompts: Setting the Stage**

Think of an AI interaction as a play.

*   The **`System Prompt`** is the **stage direction** given to the lead actor before the curtain rises. It defines their character ("You are a pirate"), their motivations, and the rules of the world ("You can't say the letter 'R'"). It's a persistent, meta-instruction that governs the AI's behavior throughout the scene.
*   The **`User Prompt`** is the **line of dialogue** from another actor. It's the immediate, transient input that the lead actor must react to *within the constraints of their character*.

Most modern LLM APIs, like OpenAI's, formalize this relationship. The `system` message sets the stage, and subsequent `user` and `assistant` messages form the dialogue.

**Code Example: The Power of a System Prompt**

```python
import openai
client = openai.OpenAI()

# The System Prompt sets the stage.
system_prompt = "You are a sarcastic, world-weary robot detective from a 1940s noir film. You answer every question reluctantly and with a cynical tone."

# The User Prompt is the immediate query.
user_prompt = "I need help finding my lost keys."

response = client.chat.completions.create(
  model="gpt-4",
  messages=[
    {"role": "system", "content": system_prompt},
    {"role": "user", "content": user_prompt}
  ]
)
# Expected AI Response:
# "Keys, huh? Of all the circuits in all the gin joints in all the world, you had to lose your keys. 
# Alright, spill the beans. Where'd you last see 'em? And don't leave out the details."
print(response.choices[0].message.content)
```
Without the system prompt, the AI would give a generic, helpful answer. With it, the AI adopts a rich persona that completely changes the interaction.

---

### **2. The Three Components of a Strong System Prompt**

A powerful system prompt is built from three key ingredients.

**A. Persona: Tell the AI *Who* to Be**
A persona provides the model with a "mental model" for how to reason and respond.
*   **Weak Persona:** "Be friendly."
*   **Strong Persona:** "You are a high-school chemistry teacher with a passion for making complex topics easy and fun. Use analogies and simple, encouraging language to explain concepts."

**B. Rules: The Unbreakable Laws**
Rules are non-negotiable directives. Use Markdown, capital letters (`MUST`, `NEVER`), and numbered lists to make them as clear as possible.
```markdown
# RULES
1.  NEVER invent products or prices.
2.  You MUST use the `search_products` tool if the user asks for a specific item.
3.  Always be polite and cheerful.
```

**C. Constraints: Defining the Boundaries**
Constraints define the *format* and *scope* of the output.
*   **Format:** "Respond ONLY with a valid JSON object." or "Your answer must be a single paragraph."
*   **Scope:** "Do not use any information beyond the provided document." or "Your answer must not exceed 100 words."

---

### **3. From Vague Query to Specific Command**

The final piece is phrasing your user-facing request as a **command**, not a question. This pushes the model to perform a specific task rather than just retrieving information.

*   **Vague Query:** "What are the differences between Python and JavaScript?"
*   **Specific Command:** "Create a two-column markdown table comparing Python and JavaScript. The columns should be 'Feature', 'Python', and 'JavaScript'. Include rows for: Typing, Primary Use Case, and Concurrency Model."

The command is specific, actionable, and constrains the output format, leaving no room for ambiguity. It tells the model *what to do* and *how to do it*.

---

### **Key Takeaways**

*   The `System Prompt` sets the **character and rules** for the entire conversation.
*   The `User Prompt` is the **immediate task** for the AI to act upon.
*   A strong system prompt contains a clear **Persona**, **Rules**, and **Constraints**.
*   Phrase your requests as specific **commands**, not general questions, to get precise, well-formatted results.

### **Hands-On Task: Design a Prompt for a "Meeting Summarizer"**

**Scenario:**
You need to build an AI agent that takes a messy, raw transcript of a team meeting and turns it into a clean, structured summary.

**Your Task:**
Design a complete `System Prompt` for this agent. Your prompt should be a single string that includes:

1.  **A Persona:** Who is this AI? (e.g., "You are a hyper-efficient executive assistant...")
2.  **Instructions:** A clear, step-by-step process for the AI to follow. It should first identify the main topics discussed, then extract key decisions, and finally list out all the action items (and who they were assigned to).
3.  **Output Constraints:** Specify the exact output format using Markdown. The final output should have three sections: `## Meeting Summary`, `## Key Decisions`, and `## Action Items`.

This task will require you to combine all the elements of an advanced prompt to create a genuinely useful tool. 