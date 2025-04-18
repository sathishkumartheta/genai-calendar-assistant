
# 🧠 Building a GenAI Calendar Assistant with Multi-Agent Planning

> “What meetings do I have today?”  
> “Schedule a sync with the team tomorrow at 2pm.”  
> “Cancel my 4pm demo call.”

These aren’t futuristic dreams — they’re real instructions a GenAI assistant can understand, plan, and execute using nothing but natural language and a connection to your Google Calendar.

In this blog post, I’ll walk you through how I built a **multi-agent GenAI calendar assistant** using Gemini, LangChain, and Google Calendar API — as part of the Google GenAI Intensive capstone.

---

## 🎯 The Problem: Calendars Are Smart, But Not Helpful

While Google Calendar is powerful, the typical user interface still assumes you know **how** to use it:

- You need to click, select, set time, timezone, event type
- You need to remember syntax (especially on mobile)
- You have to think in software terms, not in human terms

That’s where **Generative AI** steps in.

---

## 🚀 The Solution: A Conversational AI Assistant

This assistant acts like your **AI-powered executive assistant**, capable of:
- 📅 Listing upcoming events
- ➕ Creating meetings with time, title, and duration
- ❌ Deleting events by name
- 🤖 Understanding flexible user inputs like:
  - “Move all my meetings tomorrow”
  - “What do I have today?”

This was achieved by combining:
- **Gemini 2.0 Flash**: for fast, structured generation
- **LangChain Agents**: to route queries to the right tools
- **Google Calendar API**: for real-time event manipulation
- **Multi-agent planning**: separating thought (planner) from execution (scheduler)

---

## 🔁 Architecture: How the AI Thinks

```mermaid
graph TD
    A[User Query] --> B[Planner Agent (Gemini)]
    B --> C{Action Type?}
    C -- create_event --> D[Scheduler Agent]
    C -- list_events --> E[Event Lister Tool]
    C -- delete_event --> F[Event Deleter Tool]
    D --> G[Google Calendar API]
    E --> G
    F --> G
```

---

## 💻 Sample Implementation (Code Snippets)

### 🔷 Structured Output from Planner
```json
{
  "action_type": "create_event",
  "summary": "Team Sync",
  "start_time": "2025-04-20T09:00:00",
  "end_time": "2025-04-20T10:00:00",
  "timezone": "Asia/Kolkata"
}
```

### 🔧 Creating an Event with Function Calling
```python
def create_event(summary, start_time, end_time, timezone="Asia/Kolkata"):
    event = {
        'summary': summary,
        'start': {'dateTime': start_time, 'timeZone': timezone},
        'end': {'dateTime': end_time, 'timeZone': timezone}
    }
    return calendar_service.events().insert(calendarId='primary', body=event).execute()
```

---

## 🧪 Output Evaluation for Reliability

To ensure reliable function calling and agent routing, we validate the planner agent's output:

```python
def validate_planner_json(json_text):
    try:
        data = json.loads(json_text)
        return "action_type" in data
    except:
        return False
```

This helps avoid LLM hallucinations or malformed actions.

---

## ⚠️ Limitations and Future Possibilities

### ❌ Current Limitations
- Doesn’t handle **recurring events** yet
- Struggles with **ambiguous phrases** like "after lunch"
- Can’t yet **reschedule** or **rename** events in-place

### 🌅 Future Art of the Possible
- 🧠 Use embeddings to reference past conversations
- 🧭 Add a voice interface or mobile integration
- 🧑‍🤝‍🧑 Collaborate across multiple users' calendars
- 🤖 Dynamically learn user preferences (meeting lengths, time blocks)

---

## 🎓 Final Thoughts

This project shows how GenAI isn't just about chat — it's about **actionable intelligence**. By combining LLMs with structured outputs, API access, and LangChain’s agent framework, we’ve created something **useful, scalable, and extensible**.

🔗 [Notebook on Kaggle or GitHub (link)]  
🔧 [Code repository (optional)]

---

## 🙌 Thanks to the Google GenAI team for the opportunity to explore what’s possible with Gemini, LangChain, and real-world AI applications.
