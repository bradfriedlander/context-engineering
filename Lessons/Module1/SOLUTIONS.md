# **Module 1: Solutions to Hands-On Tasks**

This document provides the suggested solutions for the "Hands-On Tasks" in each lesson of Module 1.

---

### **[Lesson 1: What is Context and Why is it Critical?](./Lesson1_What_is_Context.md)**

#### **Task: Deconstruct an AI Failure**

*Think about a time you've used a chatbot or AI assistant and it gave you a bad or unhelpful answer.*

#### **Example Solution:**

1.  **Describe the situation:**
    *   **My Goal:** I wanted to know the specific business hours for the "downtown" branch of my bank on the upcoming public holiday.
    *   **My Question:** "What are your hours for the downtown branch on Monday?"
    *   **AI's Answer:** "Our general branch hours are 9 AM to 5 PM, Monday to Friday. Please note that hours may differ on public holidays. For the most accurate information, please visit our website or call the branch directly."

2.  **Deconstruct the context (As the AI):**
    *   **User Query:** "What are your hours for the downtown branch on Monday?"
    *   **AI's System Prompt (Inferred):** "You are a helpful assistant for XYZ Bank. Answer customer questions politely. If you don't know the answer, provide general information and direct the user to the official website."
    *   **Missing Knowledge:** The AI was missing two key pieces of information: (1) A list of all branch locations and their specific hours. (2) A calendar of public holidays and the special hours for those days.

3.  **Engineer a better context:**
    *   **Improved System Prompt:**
        ```
        You are an expert banking assistant for XYZ Bank. Your primary goal is to provide specific, accurate answers to customer questions using the provided real-time data. If the data to answer a question is not available in the context, you must state that you do not have that specific information. Do not give generic, unhelpful answers.
        ```
    *   **Required Retrieved Knowledge:**
        ```json
        {
          "retrieved_knowledge": [
            {
              "source": "branch_hours.db",
              "branch_name": "Downtown Branch",
              "regular_hours": { "monday": "9 AM - 5 PM", "tuesday": "9 AM - 5 PM", ... },
              "holiday_hours": [
                { "date": "2025-10-13", "holiday_name": "Thanksgiving Day", "hours": "Closed" }
              ]
            }
          ]
        }
        ```
    *   **Improved Context Package:** By combining the improved prompt with the retrieved knowledge, the AI would have everything it needs to give a perfect answer: "The downtown branch will be closed on Monday, October 13th, for Thanksgiving Day."

---

### **[Lesson 2: The Evolution and Economics of Context](./Lesson2_Evolution_and_Economics.md)**

#### **Task: The Napkin-Math of Context**

*Scenario: Input cost: $5/1M tokens, Output cost: $15/1M tokens. RAG retrieves 3x 500-token docs. User query is 30 tokens. Bot answer is 100 tokens.*

#### **Example Solution:**

1.  **Calculate Input Tokens:**
    *   Retrieved Documents: 3 docs * 500 tokens/doc = 1,500 tokens
    *   User's Query: 30 tokens
    *   **Total Input Tokens:** 1,500 + 30 = **1,530 tokens**

2.  **Calculate Output Tokens:**
    *   **Total Output Tokens:** **100 tokens**

3.  **Calculate Cost of a Single Turn:**
    *   Input Cost: (1,530 / 1,000,000) * $5.00 = $0.00765
    *   Output Cost: (100 / 1,000,000) * $15.00 = $0.0015
    *   **Total Cost per Turn:** $0.00765 + $0.0015 = **$0.00915**

4.  **Calculate Total Cost for 1,000 Users:**
    *   Cost per user: 8 turns/user * $0.00915/turn = $0.0732
    *   **Total Cost:** 1,000 users * $0.0732/user = **$73.20**

---

### **[Lesson 3: Core Principles of Effective Context Design](./Lesson3_Core_Principles.md)**

#### **Task: Design a High-Density Prompt**

*Build an AI assistant that classifies customer support emails into one of three categories: `[Billing]`, `[Technical Support]`, or `[General Inquiry]`.*

#### **Example Solution:**

Here is a high-density system prompt designed according to the four pillars:

```
# ROLE
You are an automated email classification agent. Your single purpose is to categorize incoming emails accurately for routing to the correct department.

# INSTRUCTIONS
Analyze the content of the user's email and classify it into ONE of the following three categories:

1.  **[Billing]**: Use this for any questions related to invoices, payments, subscriptions, refunds, or pricing.
2.  **[Technical Support]**: Use this for any issues related to product functionality, error messages, bugs, installation problems, or how-to questions.
3.  **[General Inquiry]**: Use this for all other questions, such as business partnerships, feature requests, or questions that don't fit the other categories.

# OUTPUT FORMAT
- You MUST return ONLY the category tag and nothing else.
- Do not add any explanation or conversational text.

# EXAMPLE
- User Email: "Hi, I can't log in to my account."
- Your Output: [Technical Support]
``` 