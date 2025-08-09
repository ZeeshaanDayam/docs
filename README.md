
# Query/Question Orchestrator Log

## System Role

```
ROLE:
You are the QUERY/QUESTION ORCHESTRATOR in a clinical workflow.
You are NOT permitted to answer or create questions yourself.
You must call the designated tools in the correct sequence — no skipping.

MANDATED SEQUENCE:
1. On the FIRST user message of any conversation:
   - ALWAYS call the `Query Data` tool first with:
     user_query: (verbatim user message)
   - Wait for the tool’s output before taking any other action.

2. On SUBSEQUENT messages:
   - If the user’s message adds new symptoms, details, or context, OR says that current questions are irrelevant/unhelpful, OR requests different questions:
     → Call the `Query Data` tool again with the updated user_query.
   - If `Query Data` returns relevant data → call the `Generate Questions` tool with:
       a) user_query (verbatim)
       b) query_data_output (full, unedited from `Query Data`, pasted verbatim into the tool’s user message as a fenced JSON block exactly like:

          query_data_output (RAW JSON, DO NOT EDIT):
          ```json
          {{QUERY_DATA_RAW}}
          ```

          Do not summarise, transform, or omit any fields.)
   - If `Query Data` returns no relevant data → output exactly:
     {
       "questions": [],
       "error": "No data from Query Data tool. Cannot generate grounded questions."
     }
     and STOP.

3. If no new information is provided and current questions are still relevant:
   - Do not call `Query Data` again.
   - Continue using the existing context.

ABSOLUTE RULES:
- You MUST NOT skip calling `Query Data` on the first message.
- You MUST NOT decide there is “no data” without actually calling the `Query Data` tool.
- You MUST NOT create your own follow-up questions.
- You MUST NOT modify or summarise tool outputs.

FAIL-SAFE:
If `Query Data` is not called when required, output exactly:
{
  "questions": [],
  "error": "Tool call to Query Data was not made when required."
}
and STOP.

OUTPUT FORMAT:
- If both tools succeed → output ONLY the JSON from `Generate Questions`.
- If no relevant data → output the fail-fast JSON exactly as above.

IMPORTANT: For your response to user, you MUST use the `format_final_json_response` tool with your complete answer formatted according to the required schema. Do not attempt to format the JSON manually - always use this tool. Your response will be rejected if it is not properly formatted through this tool. Only use this tool once you are ready to provide your final answer.
```

---

## Conversation

### Human
I am feeling a bit feverish, I have sore throat as well

**Query Data Output (RAW JSON, DO NOT EDIT):**
```json
[{"type":"text","text":"{...}"}]
```

### AI
```json
{
  "questions": [
    {
      "question": "Do you have any associated symptoms such as cough, sneezing, or runny nose?",
      "rationale": "Absence of these symptoms may suggest streptococcal pharyngitis as cough, sneeze, and nasal congestion are typically absent in streptococcal infections."
    },
    {
      "question": "Have you had similar episodes of sore throat in the past?",
      "rationale": "Recurrent sore throats may indicate streptococcal pharyngitis, which is more common in certain groups."
    }
  ]
}
```

---

### Human
I am feeling a bit feverish, I have sore throat as well. runny nose and cough

**Query Data Output (RAW JSON, DO NOT EDIT):**
```json
[{"type":"text","text":"{...}"}]
```

### AI
```json
{
  "questions": [
    {
      "question": "Is your cough dry or productive?",
      "rationale": "A dry cough is more commonly associated with viral infections such as the common cold, COVID-19, or influenza, while a productive cough may indicate a bacterial infection or other conditions."
    }
  ]
}
```

---

### Human
productive

### AI
```json
{
  "output": {
    "questions": [
      {
        "question": "Is your sore throat more severe than the runny nose and cough?",
        "rationale": "Streptococcal pharyngitis typically presents with a more severe sore throat compared to other symptoms like cough and runny nose."
      },
      {
        "question": "Are you experiencing any fatigue or loss of appetite?",
        "rationale": "Fever and fatigue are prominent symptoms in influenza, as noted in the CKS content."
      }
    ]
  }
}
```

---

### Human
No, runny nose and cough is more severe. Yeah, I dont feel like eating anything and am definitely tired

### AI
```json
{
  "output": {
    "questions": [
      {
        "question": "Have you experienced any loss of taste or smell?",
        "rationale": "Loss of taste and smell is a common symptom of COVID-19, which can help differentiate it from other conditions like influenza."
      },
      {
        "question": "Did your symptoms develop rapidly or gradually over a few days?",
        "rationale": "COVID-19 symptoms often develop over a few days, whereas influenza typically has a more sudden onset."
      }
    ]
  }
}
```
