---
layout: post
title: "🧠 Building a Multi-Agent AI Calendar Assistant with GenAI + Google Calendar"
date: 2025-04-19
categories: genai langchain googlecalendar productivity agents
---

> "What’s on my calendar today?"  
> "Can you schedule a sync tomorrow at 2pm?"  
> "Cancel the 4 PM AI meeting."

These aren't commands typed into a complex UI — they're natural phrases spoken or typed to a **multi-agent AI assistant** built using **Gemini LLM, LangChain, and Google Calendar API**. This blog is the story of how I built such an assistant for the Google GenAI Capstone, and the technical decisions that shaped it.

---

## 🎯 The Problem

Managing a calendar is surprisingly tedious — hopping between tabs, scanning through dates, clicking on time slots, and manually updating events. It's a cognitive tax we’ve all accepted for years.

With GenAI tools at our fingertips, I asked:  
**“Why can’t I just talk to my calendar like I would to a human assistant?”**

---

## 💡 The Vision

I imagined an assistant that could understand natural queries like:
- "List my events today"
- "Schedule a project sync on Sunday at 11am"
- "Delete the AI demo"
- "Reschedule the design review to next Friday"

But to truly pull this off, it needed more than just an LLM. It needed orchestration, understanding, memory, and integration.

---

## 🧠 Architecture Overview

<p align="center">
  <img src="/assets/calendar_architecture.png" alt="AI Calendar Assistant Architecture" width="600"/>
</p>

At the heart of the assistant lies a **planner → executor** pipeline, built using LangChain’s agents and Google Calendar APIs.

---

## 🧱 Core Components

Each piece of the pipeline was built with purpose. Here’s why they exist:

### 1. 🕵️‍♂️ Intent Classifier (`intent_chain`)
Before doing anything, the assistant checks **if your message is even calendar-related**.

**Why?**  
Users often say “hello” or “what’s up”. Rather than returning a confusing error or trying to parse it as a command, we route it to a **small talk model**.

```python
intent = intent_chain.invoke({"user_input": user_input})
if "no" in intent.content.lower():
    reply = small_talk_chain.invoke({"user_input": user_input})
    return reply.content.strip()
```

...

```python
if action_type == "create_event":
    return scheduler_agent(parsed)
elif action_type == "list_events":
    return get_upcoming_events()
elif action_type == "delete_event":
    return delete_event(parsed.get("summary", ""))
```

This keeps the assistant friendly, and allows casual conversation.

---

### 2. 🗺️ Planner Agent (`planner_chain`)
This is the brains of the operation.

It takes user input like:
> “Schedule a team sync tomorrow at 2 for one hour”

And returns structured JSON like:
```json
{
  "action_type": "create_event",
  "summary": "Team Sync",
  "start_time": "2025-04-20T14:00:00",
  "end_time": "2025-04-20T15:00:00",
  "timezone": "Asia/Kolkata"
}
```

This approach uses **structured output generation** with Gemini Flash, allowing the assistant to plan what needs to be done — not just answer textually.

---

### 3. ⚙️ Executor Tools

Depending on the `"action_type"`, the planner’s output is routed to the right handler:

- `list_events` → returns a list of upcoming events
- `create_event` → calls a scheduler agent to create a new event
- `delete_event` → deletes matching events
- `update_event` → placeholder for future support

This separation of **planning and acting** is what makes the agent robust.

---

## 💬 Real Conversation Flow

Here’s an actual interaction from the assistant:

```
📝 Ask me anything: list events
🤖 You have 3 events today:
- 12:00 PM - Design Sync
- 4:00 PM - AI Demo
- 6:00 PM - Retro Review

📝 Ask me anything: cancel the AI Demo
🤖 Deleted event: AI Demo
```

---

## 🔬 Few-Shot Prompting & Custom Instructions

To improve reliability, I fine-tuned the planner prompt using **few-shot examples** of how to respond to vague instructions, non-calendar prompts, and ambiguous language like “move that meeting.”

---

## ⚠️ Limitations

- The LLM sometimes hallucinates event names
- Doesn’t yet handle recurring events
- No fuzzy matching on typos (e.g., “AI deemo” won't match “AI demo”)
- Deletion requires exact title match
- Needs manual auth step for credentials

---

## 🔮 Future Possibilities

- Add memory for context-aware replies
- Use fuzzy search for smarter deletion
- Voice input via browser or mobile
- Calendar visualization
- Slack or WhatsApp integration

---

## 🧾 Final Thoughts

This assistant was built entirely in a Kaggle notebook, using:
- 🧠 `Gemini Flash` (via `langchain-google-genai`)
- 🔗 `LangChain agents`
- 📆 `Google Calendar API`
- 🤖 Custom planner/scheduler architecture

I hope this project inspires more natural, assistive workflows — where AI not only generates text, but takes action.

Thanks to Google GenAI Intensive for the opportunity!

---

## 📂 Code Notebook

📎 [View the full Kaggle notebook](https://kaggle.com/)  
📸 Includes: code, prompts, architecture, and working examples.
