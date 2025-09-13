# Task 1: Conversation Summarization using Groq API

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

## 🔹 Features

1. **Conversation History**
   - Maintains messages from both user and assistant.
   - Each message stores the `role` (`user` or `assistant`) and the `content`.

2. **Truncation Options**
   - Limit conversation by:
     - Number of turns (`max_turns`)
     - Number of characters (`max_chars`)
     - Number of words (`max_words`)
   - Ensures concise conversation history for downstream processing.

3. **Periodic Summarization**
   - Summarizes conversation after every `k` messages.
   - Uses Groq API model `groq/compound-mini` for generating summaries.
   - Replaces the conversation history with a summarized system message.

4. **Demonstration**
   - Multiple sample conversations are included.
   - Outputs show:
     - Summarized history after periodic triggers
     - Truncated conversation for different settings

---
# Task 2: JSON Schema Classification & Information Extraction using Groq API
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

### 2. Function Calling with Groq API
- Uses Groq’s OpenAI-compatible client for structured outputs
- Extracts structured data from free-form chats

### 3. Validation
- Each output is checked against the JSON schema using `jsonschema`
- Returns True/False for schema compliance

### 4. Demonstration
- At least 3 sample chats are processed
- Shows extracted JSON and validation results


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
