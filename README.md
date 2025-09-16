# Task 1: Managing Conversation History with Summarization

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Colab](https://img.shields.io/badge/Platform-Google%20Colab-orange)
![Groq API](https://img.shields.io/badge/API-Groq%20OpenAI%20Client-green)

---

## 📝 Objective
The goal of Task 1 is to implement a **conversation management system** capable of:  

- Maintaining a running **user–assistant chat history**
- Performing **summarization** to keep the conversation concise
- Allowing **customizable truncation** by turns, characters, or words
- Implementing **periodic summarization** after every `k` messages using Groq API  

This project demonstrates handling conversational data efficiently while adhering strictly to standard Python and Groq/OpenAI client usage — **no frameworks are used**.

---

## 🔹 Features and Code Explanation

1. **Conversation History**
```bash
conversation_history = []

def add_message(role, content):
    """
    Add a message to conversation history.

    Args:
        role (str): 'user' or 'assistant'
        content (str): Text content
    """
    conversation_history.append({"role": role, "content": content})
```
**Explanation:**
   - Each message is stored as a dictionary with role and content.
   - All messages are kept in the conversation_history list for later processing, truncation, or summarization.
2. **Truncation Options**
```bash
truncated_history = truncate_history(conversation_history, max_turns=4)
```
**Explanation**
   - Limit conversation by:
     - Number of turns (`max_turns`)
     - Number of characters (`max_chars`)
     - Number of words (`max_words`)
   - Ensures concise conversation history for downstream processing.

3. **Periodic Summarization**
```bashsummary = summarize_history(conversation_history)
conversation_history = [{"role": "system", "content": f"Summary so far: {summary}"}]
```
**Explanation**
   - Summarizes conversation after every `k` messages.
   - Uses Groq API model `groq/compound-mini` for generating summaries.
   - Replaces the conversation history with a summarized system message.

5. **Demonstration**
```bash
# Example outputs
Truncated to last 4 messages:
system: Summary so far: We've just started...

Truncated to last 50 characters:
system: ...next about neural networks or AI?

```
**Explanation**
   - Multiple sample conversations are included.
   - Outputs show:
     - Summarized history after periodic triggers
     - Truncated conversation for different settings

---
# Task 2: JSON Schema Classification & Information Extraction
---

## 📝 Objective

The goal of Task 2 is to implement structured information extraction from conversational chats using a JSON Schema and Groq API function calling.

This ensures that free-text chats can be transformed into structured JSON outputs containing:

- Name
- Email
- Phone
- Location
- Age

The extracted JSON is validated against a schema for correctness.

---

## 🔹 Features

### 1. JSON Schema Definition
- Specifies required fields: `name`, `email`, `phone`, `location`, `age`
```python
REQUIRED_FIELDS = ["name", "email", "phone", "location", "age"]
```

### 2. Function Calling with Groq API
- Uses Groq’s OpenAI-compatible client for structured outputs
- Extracts structured data from free-form chats
```python
def extract_info(chat_text):
    """
    Extract structured information from chat using Groq API.
    """
    response = client.chat.completions.create(
        model="llama-3.1-8b-instant",
        messages=[
            {"role": "system", "content": "You are an information extractor. Only reply in valid JSON."},
            {"role": "user", "content": f"Extract fields {REQUIRED_FIELDS} from chat:\n{chat_text}"}
        ],
        temperature=0,          # deterministic output
        max_completion_tokens=300,
        top_p=1,
        stream=False
    )
    try:
        extracted = json.loads(response.choices[0].message.content)
    except json.JSONDecodeError:
        extracted = {"error": "Failed to parse JSON"}
    return extracted
```

### 3. Validation
- Each output is checked against the JSON schema using `jsonschema`
- Returns True/False for schema compliance
```python
from jsonschema import validate, ValidationError

def validate_schema(data):
    schema = {
        "type": "object",
        "properties": {field: {"type": "string"} for field in REQUIRED_FIELDS},
        "required": REQUIRED_FIELDS
    }
    try:
        validate(instance=data, schema=schema)
        return True
    except ValidationError:
        return False
```
### 4. Demonstration
- At least 3 sample chats are processed
- Shows extracted JSON and validation results
```python
Extracted Info: {'name': 'Alice Johnson', 'email': 'alice@example.com', 'phone': '555-1234', 'location': 'New York', 'age': '29'}
Valid Schema: True
```

## ⚙️ Requirements

- **Python 3.x**
- **OpenAI-compatible Groq client** (`openai` package)
- **Requests** (optional, for API testing)
- **Google Colab** (recommended for demonstration)

**Note:** No frameworks used — only standard Python + Groq/OpenAI client.

---

## 🛠 Setup Instructions

1. **Install dependencies in Colab:**

```bash
!pip install openai requests
```
2. **Set up your Groq API key:**
```bash
import os
os.environ["GROQ_API_KEY"] = "YOUR_GROQ_API_KEY_HERE"
```
3. **Initialize the Groq/OpenAI client:**
```bash
import openai

client = openai.OpenAI(
    base_url="https://api.groq.com/openai/v1",
    api_key=os.environ.get("GROQ_API_KEY")
)
```
