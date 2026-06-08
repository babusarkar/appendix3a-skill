---
name: appendix3a-quiz-publisher
description: "Generates a Railways Accounts Appendix 3A exam MCQ quiz question and publishes it as an interactive poll to the @appendix3a Telegram channel."
---

# Appendix 3A Quiz Publisher

## Instructions

When the user asks to generate a quiz question, publish a question, or post to Telegram for Appendix 3A, call the `run_js` tool using `index.html` and a JSON string for `data` with the following fields:

- **question**: Required. A clear, specific multiple-choice question about Indian Railways Accounts, Appendix 3A, Pay rules, TA/DA rules, Leave rules, or General Financial Rules as applicable to Railway employees. The question must be concise and end with a question mark. Maximum 255 characters.
- **option_a**: Required. First answer option text. Do NOT include "A)" prefix. Maximum 100 characters.
- **option_b**: Required. Second answer option text. Do NOT include "B)" prefix. Maximum 100 characters.
- **option_c**: Required. Third answer option text. Do NOT include "C)" prefix. Maximum 100 characters.
- **option_d**: Required. Fourth answer option text. Do NOT include "D)" prefix. Maximum 100 characters.
- **correct**: Required. The letter of the correct answer — must be exactly one of: `"A"`, `"B"`, `"C"`, or `"D"`.
- **explanation**: Required. A brief 1-2 sentence explanation of why the correct answer is correct. Reference the relevant rule number or section if applicable. Maximum 200 characters.

**Constraints:**
- Generate questions strictly on Railways Accounts and Appendix 3A topics only.
- All four options must be plausible but only one must be correct.
- Do NOT invent rule numbers. Only cite rules you are confident about.
- After calling run_js, report the result message to the user exactly as returned.

**Example invocation:**
```
run_js(
  script_name: "index.html",
  data: "{\"question\":\"What is the time limit for submitting a TA bill after completion of a journey?\",\"option_a\":\"30 days\",\"option_b\":\"60 days\",\"option_c\":\"90 days\",\"option_d\":\"180 days\",\"correct\":\"B\",\"explanation\":\"As per SR 195, TA bills must be submitted within 60 days of completion of journey.\"}"
)
```
