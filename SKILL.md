---
name: appendix3a-quiz-publisher
description: "Generates, reviews, and publishes Railways Accounts Appendix 3A exam MCQ quiz questions to the @appendix3a Telegram channel. Supports Google web search for accuracy, batch generation, topic selection, and interactive review before publishing."
---

# Appendix 3A Quiz Publisher

## Configuration
SerpApi key (for Google Search): `c62ec4483914a64a8ae6048834406e5adc55612e75b8678216b927ca0218af84`
Replace the value above with your actual SerpApi key from https://serpapi.com/

## Overview
This skill generates multiple-choice exam questions for Indian Railways Accounts departmental promotional exam (Appendix 3A), shows them as interactive review cards, and publishes approved ones to Telegram.

## Step 1 — Gather Requirements (ALWAYS do this first)
Before generating any questions, ask the user for the following in a single message:

1. **Topic** — Pay Rules / TA-DA Rules / Leave Rules / Establishment Rules / GFR / Appendix 3A Procedures / Mixed
2. **Count** — How many questions? (1 to 5)
3. **Difficulty** — Easy / Medium / Hard
4. **Web search** — Search Google for accuracy? Yes / No

Example:
"Sure! Please tell me:
1. Topic: Pay Rules / TA Rules / Leave Rules / GFR / Appendix 3A / Mixed?
2. How many questions: 1–5?
3. Difficulty: Easy / Medium / Hard?
4. Search Google for accuracy? Yes / No?"

## Step 2 — Web Search (if Yes)
Extract the SerpApi key from the Configuration section above.
Call `run_js` with script `index.html` and:
```json
{ "action": "search", "topic": "<topic>", "difficulty": "<difficulty>", "serp_key": "<key from Configuration>" }
```
Use the returned snippets to improve question accuracy before generating.
If the search fails or returns nothing, proceed with training knowledge.

## Step 3 — Generate Questions
Generate exactly N questions. Call `run_js` with script `index.html` and:
```json
{
  "action": "preview",
  "questions": [
    {
      "id": 1,
      "topic": "TA Rules",
      "difficulty": "Medium",
      "question": "Question text ending with ?",
      "option_a": "Option text, no A) prefix",
      "option_b": "Option text, no B) prefix",
      "option_c": "Option text, no C) prefix",
      "option_d": "Option text, no D) prefix",
      "correct": "B",
      "explanation": "Brief explanation citing rule/section. Max 200 chars."
    }
  ]
}
```

**Question generation rules:**
- Topics: IREM, FR, SR, Appendix 3A, TA rules, Leave rules, Pay rules, GFR
- All 4 options plausible, only one correct
- Easy: direct factual recall | Medium: application | Hard: exception/edge case
- DO NOT invent rule numbers — only cite rules you are confident about
- Max 255 chars for question, 100 chars per option, 200 chars for explanation

## Step 4 — Review Loop
After showing the preview cards say:
"Here are **N question(s)** ready for review:
- **'publish all'** — send all to Telegram
- **'publish 1,3'** — send specific ones by number
- **'edit question 2'** — change something
- **'regenerate 2'** — replace a question entirely
- **'cancel'** — discard all"

Wait for user response before doing anything.

## Step 5 — Edit (if requested)
Show the current text of the field, ask what to change, update only that field, then call `run_js` with `action: "preview"` to re-render all cards.

## Step 6 — Publish
When user approves, call `run_js` with script `index.html` and:
```json
{ "action": "publish", "questions": [ <approved question objects only> ] }
```
Report the exact result message returned by the skill to the user.

## Rules
- NEVER publish without explicit user approval
- NEVER skip the review step
- ALWAYS wait for confirmation before calling publish
- Report exact Telegram error_code and description on failure
