---
name: appendix3a-quiz-publisher
description: "Searches the web for up-to-date Indian Railways rules, creates multiple MCQ quiz questions for user review in the chat, and publishes approved batches to the @appendix3a Telegram channel."
---

# Appendix 3A Multi-Quiz Publisher & Search Engine

## Instructions

This skill handles two distinct actions: **searching the web** for real-time compliance information, and **batch-publishing questions** only after they are reviewed and approved by the user inside the active conversation view.

### Step 1: Web Search Grounding (Optional/Context Dependent)
If the user references recent updates, rule numbers, financial modifications, or if you feel uncertain about factual details concerning Railways Accounts, use the `search` action first. Call `run_js` using `index.html` with this JSON:
- **action**: `"search"`
- **query**: Required. Precise keywords focusing on rules (e.g., `"Indian Railways DA rules 2026"`, `"Appendix 3A structural updates"`).

### Step 2: The Chat Review Loop
1. Generate the number of quiz questions requested by processing your internal data alongside any data returned from the `search` tool action.
2. Print the full output of the questions cleanly in the chat box using standard markdown (showing Question, Options A-D, Correct answer, and Explanation).
3. **Explicitly request the user to review the generated draft.** Tell them to offer adjustments or type their absolute approval (e.g., "Looks great, post them"). 
4. Do not fire the publishing tool call prematurely until you receive confirmation.

### Step 3: Batch Publishing Execution
When the user gives final clearance to push, invoke `run_js` targeting `index.html` with an `action` of `"publish"` and provide a single compressed `questions` array.

**Payload Structure for `"publish"` action:**
- **action**: `"publish"`
- **questions**: Required. An array containing question objects with these properties:
  - **question**: Required. Concise multiple-choice question focusing on Indian Railways Accounts/Rules. Ends with `?`. Maximum 255 characters.
  - **option_a**: Required. Text for option A. Do NOT include any "A)" markers. Max 100 characters.
  - **option_b**: Required. Text for option B. Do NOT include any "B)" markers. Max 100 characters.
  - **option_c**: Required. Text for option C. Do NOT include any "C)" markers. Max 100 characters.
  - **option_d**: Required. Text for option D. Do NOT include any "D)" markers. Max 100 characters.
  - **correct**: Required. Must be exactly one uppercase letter: `"A"`, `"B"`, `"C"`, or `"D"`.
  - **explanation**: Required. Simple 1-2 sentence clarification specifying rule citations. Max 200 characters.

**Constraints:**
- Questions must stay within the strict scope of Indian Railways rules.
- Only one answer selection can be correct.
- Output a clear summary status to the user when the function completion string returns.

**Example Tool Invocation:**
```json
{
  "action": "publish",
  "questions": [
    {
      "question": "What is the maximum period of Leave on Average Pay (LAP) that can be granted to a Railway employee at one single time?",
      "option_a": "60 days",
      "option_b": "90 days",
      "option_c": "120 days",
      "option_d": "180 days",
      "correct": "D",
      "explanation": "As per the Railway Servants (Liberalised Leave) Rules, the maximum limit of LAP that can be granted at any one time is 180 days."
    }
  ]
}
