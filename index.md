---
layout: post
title: "🤖 Building a Multi-Agent AI Calendar Assistant with Gemini & LangChain"
date: 2025-04-18
categories: genai langchain googlecalendar agents productivity
---

> “Why click when you can just ask?”

This project is my submission for the **Google GenAI Intensive capstone**, where I built a fully functioning **Multi-Agent AI Calendar Assistant** that understands natural language and automates your Google Calendar using **LLMs**, **LangChain agents**, and **Google Calendar API**.

---

## 🧩 Problem Statement

Despite all the advancements in GenAI, most people still interact with their calendars through traditional GUIs. Managing your time — arguably the most precious resource — should be as seamless as talking to an assistant.

This assistant solves problems like:

- “What do I have today?”
- “Schedule a sync tomorrow at 3”
- “Cancel my meeting with HR”
- “Reschedule all Friday events to next Monday”

---

## 💡 Overview

This project brings together:

- A **Planner Agent** that parses natural language into structured JSON
- A **Scheduler Agent** that executes the intended calendar action
- An intelligent **multi-agent pipeline** that routes tasks and returns human-friendly responses

---

## 🛠️ Tech Stack

| Layer              | Technology                          |
|-------------------|--------------------------------------|
| LLM                | Gemini 2.0 Flash                     |
| Agent Framework    | LangChain AgentExecutor              |
| Planner Agent      | LangChain LLMChain with JSON output |
| Scheduler Agent    | LangChain LLM + Google Calendar API |
| Tooling            | Python, Google APIs, Retry logic    |
| Environment        | Kaggle Notebooks (Python 3.11)       |

---

## 📐 Architecture

![AI Calendar Assistant Architecture](A_flowchart_in_the_digital_illustration_depicts_an.png)

### 🔁 Workflow:

1. **User Input**: Natural language like “list events tomorrow”
2. **Planner Agent**: Interprets intent and generates structured JSON
3. **Router Logic**: Chooses correct tool or agent based on `action_type`
4. **Scheduler Agent**: Executes the action (create/list/delete)
5. **Response**: User gets friendly, readable confirmation or results

---

## 🤝 Key Capabilities

- 📅 List upcoming calendar events
- ➕ Schedule new meetings
- ✏️ Rename existing events
- ❌ Cancel/delete events
- ⏳ Reschedule by natural expressions (“move Friday’s call to Monday”)

All via text — no clicks required.

---

## 🧠 Sample Interaction

**User**: "List my events for today"  
**Planner**:
```json
{ "action_type": "list_events" }
```

**Scheduler**: Retrieves and formats calendar events.

**User**: "Schedule a demo sync at 2 PM tomorrow"  
**Planner**:
```json
{
  "action_type": "create_event",
  "summary": "Demo Sync",
  "start_time": "2025-04-19T14:00:00",
  "end_time": "2025-04-19T15:00:00",
  "timezone": "Asia/Kolkata"
}
```

**Scheduler**: Creates event and returns link.

---

## 📊 GenAI Evaluation Strategy

To ensure consistent structured outputs:
- Prompt tuned with clear JSON schema requirements
- Fallback logic to catch and retry if missing fields
- Separate planning and execution = modular debugging

```python
def is_valid_plan(json_blob):
    required_keys = {"action_type"}
    return all(k in json_blob for k in required_keys)
```

---

## ⚠️ Limitations

- Title-matching deletes are fuzzy
- No support for recurring events yet
- LLM inference errors under vague phrasing
- Timezone fallback is static (`Asia/Kolkata`)
- Not yet deployed — only runs inside Jupyter/Kaggle

---

## 🔮 Future Work

- Add embeddings for user preferences
- Enable rescheduling logic & conflict detection
- Integrate with Gmail/Slack
- Build a voice-based mobile agent
- Add LangGraph-based agent memory

---

## 📁 Notebook & Code

- [Kaggle Notebook](https://kaggle.com/)
- GitHub Repo (coming soon)
- Blog Source: [`_posts/2025-04-18-genai-calendar-assistant.md`](#)

---

## 🙌 Acknowledgements

Thanks to the Google GenAI team, LangChain developers, and the Kaggle kernel for powering this amazing learning and building journey.

---

## 🎯 Final Thoughts

This isn’t just about calendar management — it’s about showing how **conversational GenAI + APIs** can create real-world automation, today.

If this assistant saved you even 5 minutes, imagine what it could do in a week.

