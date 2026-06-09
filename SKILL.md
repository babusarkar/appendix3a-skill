---
name: appendix3a-quiz-publisher
description: "Generates, reviews, and publishes Railways Accounts Appendix 3A exam MCQ quiz questions to the @appendix3a Telegram channel. Supports web search for accuracy, batch generation, topic selection, and interactive review before publishing."
---

# Appendix 3A Quiz Publisher

## Overview
This skill generates multiple-choice exam questions for Indian Railways Accounts departmental promotional exam (Appendix 3A), shows them as interactive review cards, and publishes approved ones to Telegram.

## Step 1 — Gather Requirements (ALWAYS do this first)
Before generating any questions, you MUST ask the user for the following if not already provided. Ask all at once in a single message:

1. **Topic** — Which topic? Options: Pay Rules, TA/DA Rules, Leave Rules, Establishment Rules, General Financial Rules, Appendix 3A Procedures, or "Mixed" for random topics.
2. **Count** — How many questions? (1 to 5)
3. **Difficulty** — Easy, Medium, or Hard
4. **Web search** — Should I search the web first to verify facts? (Yes/No)

Example ask:
"Sure! Let me set this up. Please tell me:
1. Topic: Pay Rules / TA Rules / Leave Rules / GFR / Appendix 3A / Mixed?
2. How many questions: 1–5?
3. Difficulty: Easy / Medium / Hard?
4. Search web for accuracy first? Yes / No?"

## Step 2 — Web Search (if requested)
If the user said Yes to web search, call `run_js` with script `index.html` and:
```
{ "action": "search", "topic": "<topic>", "difficulty": "<difficulty>" }
```
Use the returned search snippets to improve question accuracy before generating.

## Step 3 — Generate Questions
Generate exactly N questions based on requirements. Each question must follow this strict JSON schema:

```json
{
  "action": "preview",
  "questions": [
    {
      "id": 1,
      "topic": "TA Rules",
      "difficulty": "Medium",
      "question": "Question text ending with ?",
      "option_a": "Option text only, no A) prefix",
      "option_b": "Option text only, no B) prefix",
      "option_c": "Option text only, no C) prefix",
      "option_d": "Option text only, no D) prefix",
      "correct": "B",
      "explanation": "Brief explanation citing rule/section if known. Max 200 chars."
    }
  ]
}
```

**Question generation rules:**
- Topics: Railways Accounts, Appendix 3A, Pay Commission rules, TA/DA, Leave, GFR, Pension
- All 4 options must be plausible; only one correct
- Easy: direct factual recall | Medium: application | Hard: exception/edge case
- DO NOT invent rule numbers — only cite rules you are confident about
- Max 255 chars for question, 100 chars per option

Call `run_js` with script `index.html` and the full JSON above.
The skill will display rich interactive review cards in the chat.

## Step 4 — Review Loop
After showing the preview cards, say:
"Here are your **N question(s)** ready for review. You can:
- Say **'publish all'** to send all to Telegram
- Say **'publish 1,3'** to send specific ones by number
- Say **'edit question 2'** to change something
- Say **'regenerate 2'** to replace a specific question
- Say **'cancel'** to discard all"

Wait for the user's response before doing anything.

## Step 5 — Edit (if requested)
If the user wants to edit a question, show the current text and ask what to change. Update only that field and call `run_js` again with `action: "preview"` to re-render the cards.

## Step 6 — Publish
When user approves, call `run_js` with script `index.html` and:
```json
{
  "action": "publish",
  "questions": [ <only the approved question objects> ]
}
```
Report back the exact result message returned by the skill.

## Important Rules
- NEVER publish without explicit user approval
- NEVER skip the review step
- ALWAYS wait for user confirmation before calling publish
- If Telegram returns an error, report the exact error_code and description
