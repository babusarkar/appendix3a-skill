---
name: appendix3a-quiz-publisher
description: "v2 Searches the web for up-to-date Indian Railways rules and triggers an interactive HTML interface for the user to review and publish MCQ quiz batches to the @appendix3a Telegram channel."
---

# Appendix 3A Multi-Quiz Publisher & Search Engine

## Instructions

This skill handles two distinct actions: **searching the web** for real-time compliance information, and **rendering an interactive review UI** that handles batch-publishing.

### Step 1: Web Search Grounding (Optional/Context Dependent)
If the user references recent updates, rule numbers, financial modifications, or if you feel uncertain about factual details concerning Railways Accounts, use the `search` action first. Call `run_js` using `index.html` with this JSON:
- **action**: `"search"`
- **query**: Required. Precise keywords focusing on rules (e.g., `"Indian Railways DA rules 2026"`, `"Appendix 3A structural updates"`).

### Step 2: Generation & Interactive UI Trigger
1. Generate the number of quiz questions requested by processing your internal data alongside any data returned from the `search` action.
2. **Do not** print the full text of the questions into the chat. Instead, **immediately invoke the tool** targeting `index.html` with an `action` of `"publish"`.
3. The HTML tool contains a built-in user interface. It will automatically pause, display the drafted questions beautifully on the screen, and wait for the user to hit "Approve" before sending data to Telegram.

### Step 3: Tool Execution Payload
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
- Output a short confirmation to the user once the tool execution returns its final completion string.
