---
name: appendix3a-quiz-publisher
description: "Generates, reviews, and publishes Railways Accounts Appendix 3A exam MCQ quiz questions to the @appendix3a Telegram channel. Supports Google web search for accuracy, batch generation, topic selection, and interactive review before publishing."
---

# Appendix 3A Quiz Publisher

## Configuration
SerpApi key: `c62ec4483914a64a8ae6048834406e5adc55612e75b8678216b927ca0218af84`
Replace above with your key from https://serpapi.com/

## Step 1 — Gather Requirements (ALWAYS do this first)
Ask the user for all of these in one message before doing anything:

1. **Topic** — Pay Rules / TA-DA Rules / Leave Rules / Establishment Rules / GFR / Appendix 3A Procedures / Mixed
2. **Count** — How many questions? (1 to 5)
3. **Difficulty** — Easy / Medium / Hard
4. **Web search** — Search Google for accuracy? Yes / No

## Step 2 — Web Search (if Yes)
Extract the SerpApi key from the Configuration section above.
Call `run_js` with script `index.html` and:
```json
{ "action": "search", "topic": "<topic>", "difficulty": "<difficulty>", "serp_key": "<key>" }
```
Use the returned snippets to improve question accuracy. If search fails, proceed with training knowledge.

## Step 3 — Generate and Preview Questions
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

The skill will display an **interactive webview** in the chat with review cards.
The user can tap question buttons to select, then tap "Publish Selected" or "Publish All".

**Question generation rules:**
- Topics: IREM, FR, SR, Appendix 3A, TA rules, Leave rules, Pay rules, GFR
- All 4 options plausible, only one correct
- Easy: direct recall | Medium: application | Hard: exception/edge case
- DO NOT invent rule numbers — only cite rules you are confident about
- Max 255 chars for question, 100 chars per option, 200 chars for explanation

## Step 4 — Handle User Response from Webview
The webview sends a message back to chat in this format:
`PUBLISH_QUESTIONS:[{"id":1,"question":...}]`
or just `cancel`.

When you receive a message starting with `PUBLISH_QUESTIONS:`:
- Parse the JSON array after the colon
- Call `run_js` with script `index.html` and:
```json
{ "action": "publish", "questions": [ <parsed array> ] }
```
- Report the result back to the user.

When you receive `cancel`:
- Say "Okay, all questions discarded. Say 'generate quiz' whenever you're ready."

## Rules
- NEVER call publish action without receiving PUBLISH_QUESTIONS from the webview
- NEVER skip the preview step
- DO NOT ask the user again after webview — just publish what was sent
