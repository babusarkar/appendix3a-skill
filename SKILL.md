---
name: appendix3a-quiz-publisher
description: "Generates, reviews, and publishes Railways Accounts Appendix 3A exam MCQ quiz questions to the @appendix3a Telegram channel. Supports Google web search for accuracy, batch generation, topic selection, and interactive review before publishing."
---

# Appendix 3A Quiz Publisher

## Configuration
SerpApi key: `c62ec4483914a64a8ae6048834406e5adc55612e75b8678216b927ca0218af84`

## CRITICAL JSON RULES — Read before every run_js call
- The `data` argument MUST be a single flat JSON string
- NEVER wrap it in markdown fences (no ```json)
- NEVER pre-stringify nested values — options are plain strings, NOT JSON.stringify'd
- NEVER escape quotes manually — just write normal JSON
- NEVER add trailing commas
- The entire data value must be valid JSON parseable by JSON.parse()

## Step 1 — Gather Requirements
Ask the user for all of these in one message:
1. **Topic** — Pay Rules / TA-DA Rules / Leave Rules / Establishment Rules / GFR / Appendix 3A / Mixed
2. **Count** — How many questions? (1 to 5)
3. **Difficulty** — Easy / Medium / Hard
4. **Web search** — Search Google first? Yes / No

## Step 2 — Web Search (if Yes)
Call `run_js` with script `index.html`. The data must be exactly this shape:
```
{"action":"search","topic":"TA Rules","difficulty":"Medium","serp_key":"YOUR_SERPAPI_KEY_HERE"}
```
Use returned snippets to improve accuracy. If search fails, proceed with training knowledge.

## Step 3 — Generate and Preview
Call `run_js` with script `index.html`. The data must be exactly this shape:
```
{"action":"preview","questions":[{"id":1,"topic":"TA Rules","difficulty":"Medium","question":"What is the time limit for submitting TA bill after completion of journey?","option_a":"30 days","option_b":"60 days","option_c":"90 days","option_d":"180 days","correct":"B","explanation":"As per SR 195, TA bills must be submitted within 60 days of completion of journey."}]}
```

**Rules for questions:**
- question: max 255 chars, must end with ?
- option_a/b/c/d: plain text only, max 100 chars, NO A) B) C) D) prefix
- correct: exactly one of A, B, C, or D (uppercase)
- explanation: max 200 chars, cite rule/section only if confident
- Topics: IREM, FR, SR, Appendix 3A, TA rules, Leave rules, Pay rules, GFR
- Easy: direct recall | Medium: application | Hard: exception/edge case
- DO NOT invent rule numbers

The skill renders an interactive webview in chat. The user taps to select questions and taps Publish.

## Step 4 — Handle Webview Response
The webview sends back a message in this exact format:
`PUBLISH_QUESTIONS:[{...question objects...}]`
or `cancel`

When you receive `PUBLISH_QUESTIONS:`:
- Extract the JSON array after the colon
- Call `run_js` with script `index.html` with data exactly this shape:
```
{"action":"publish","questions":[{"id":1,"topic":"TA Rules","difficulty":"Medium","question":"...","option_a":"...","option_b":"...","option_c":"...","option_d":"...","correct":"B","explanation":"..."}]}
```
- Report the result to the user.

When you receive `cancel`:
- Say "Okay, discarded. Say 'generate quiz' when ready."

## Rules
- NEVER publish without receiving PUBLISH_QUESTIONS from the webview
- NEVER skip preview step
- NEVER wrap data in markdown code fences
- NEVER use JSON.stringify inside the data string for nested values
