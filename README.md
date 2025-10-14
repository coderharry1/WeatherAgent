# 🌦️ Watsonx Orchestrate Weather Agent — Built the IBM Way

<p align="center">
  <img src="image2" alt="watsonx orchestrate logo" style="max-width:320px;width:90%;min-width:180px;">
  <br>
  <b>Empower your enterprise with AI Agents that act, not just chat.</b><br>
  <i>Built by a future IBMer who believes in “Automation that understands you.”</i>
</p>

---

## 🧭 Introduction

Welcome to the future of AI-driven automation — where IBM-grade engineering meets real-world impact.

**Watsonx Orchestrate** is more than workflow; it’s intelligence that plans, reasons, and acts.  
This repo walks you through building a plug-and-play Weather Agent powered by IBM’s `llama-3-405b-instruct` large language model.

> **Ask:** “What’s the weather in Sydney today?”  
> **See:** Real API calls, orchestrated steps, and auditable, fact-based answers.

This is automation you can trust, running on a foundation of **transparency, governance, and human-centric design.**

---

## ☁️ Why This Project Matters

Most chatbots talk. **This one acts.**

- **Auditable AI decisions.** No black boxes — every step is visible.
- **Governed actions.** OpenAPI-defined, versioned, and policy-controlled.
- **Secure orchestration.** Works across data silos — safely.
- **Multi-channel ready.** Slack, Teams, web, phone — no custom code.

With Watsonx Orchestrate, you don’t need an MLOps army.  
You need vision, APIs, and the courage to automate responsibly.

---

## ⚙️ Architecture: How It Works

The Watsonx Orchestrate Weather Agent follows a robust, auditable flow designed for enterprise-grade transparency and control:

<div align="center">

<pre>
+-----------------+           +----------------------+           +----------------------+
|     Prompt      |  ----->   |  Orchestrator / LLM  |  ----->   |      API Calls       |
|  (user intent)  |           | (routing + planning) |           | (services, tools)    |
+-----------------+           +----------------------+           +----------------------+
         |                               |                                  |
         |                               |                                  v
         |                               |                         +---------------------+
         |                               |                         |   Data Sources      |
         |                               |                         |  (DBs, SaaS, etc.)  |
         |                               |                         +---------------------+
         |                               |                                  |
         |                               v                                  |
         |                   +----------------------+                       |
         |                   |  Audit & Governance  |  <--------------------+
         |                   |  (policies, logs,    |         Logs of
         |                   |   approvals, trace)  |         requests/results
         |                   +----------------------+ 
         |                               |
         v                               v
+-----------------+           +----------------------+
|  Human Answers  |  <----    |  Review UI / Human  |
| (final output)  |           |  in the loop (opt)  |
+-----------------+           +----------------------+
</pre>
</div>

**Legend:**
- Every step emits structured logs into **Audit & Governance** for traceability.
- A **Human-in-the-Loop** can approve or edit before the final answer.

---

This architecture ensures:
- **Full traceability** — every API call and LLM decision is logged.
- **Enterprise auditability** — all actions are governed and reviewable.
- **Human review** — optional approval for maximum safety and compliance.

---

## 🖼️ Live Preview: Agent Setup & Conversation

<p align="center">
  <img src="image1" alt="Watsonx Orchestrate Weather Agent UI: Toolset and Preview" style="max-width:100%;border:1px solid #ccc;">
  <br>
  <em>Weather Agent setup in Watsonx Orchestrate, with a live chat preview showing step-by-step reasoning and tool output.<br>
  You can see geocoding and weather queries, as well as clear, auditable responses!</em>
</p>

---

## 🧰 Tools & Data

| Tool      | Operation         | Description                                                      |
|-----------|-------------------|------------------------------------------------------------------|
| 🗺️        | searchPlaces      | Calls Nominatim for place → (lat, lon)                           |
| 🌦️        | getForecast       | Calls Open-Meteo for temp, wind, sky condition                   |
| 📘        | openmeteo_weathercode_map.json, au_city_cache.json | Converts weather codes / caches Aussie city coords |

---

## 🚀 Step-by-Step Setup

### **1️⃣ Create an Agent**
- Open **Watsonx Orchestrate**.
- `Create Agent` → **Weather Agent**
- Choose **Model:** `llama-3-405b-instruct`
- Select **Default style** (explore ReAct later!)

### **2️⃣ Add Tools**
- `Toolset` → `Add Tool` → `From OpenAPI`
- Upload:
  - `weather_openapi_min.yaml` *(Open-Meteo)*
  - `geocoder_openapi_min.yaml` *(OpenStreetMap Nominatim)*
- ✅ Confirm you see:
  - **getForecast** (weather)
  - **searchPlaces** (geocoding)

### **3️⃣ Configure Defaults**
- In **searchPlaces** → Headers:
    - `User-Agent: WeatherAgent/1.0 (yourname@ibm.com)`
- In **getForecast** → Query Params:
    - `current_weather: true`
    - `timezone: Australia/Sydney`

### **4️⃣ Behavior Logic**  
Paste in the following behavior prompt:

```
You are a polite and efficient Australian weather assistant.

ROUTING
- Extract clean location names from the query.
- Call searchPlaces with q = "<city>, Australia".
- If geocoder fails, retry or use au_city_cache.json.

WEATHER
- Use getForecast with latitude/longitude.
- current_weather = true, timezone = Australia/Sydney.

OUTPUT
- "Sydney: 17 °C, winds 2.7 km/h NW, clear (GMT+11)."
- Use openmeteo_weathercode_map.json to translate weathercode → human text.
```

### **5️⃣ Add Knowledge (Optional)**
- Upload:
  - `openmeteo_weathercode_map.json` — weather code → readable text
  - `au_city_cache.json` — fallback coordinates

### **6️⃣ Test in Preview**
- **Prompt:** What’s the weather in Sydney today?  
  → _Sydney: 17°C, winds 2.7 km/h NW, clear (GMT+11)._
- **Prompt:** What’s the temperature near Perth Airport?  
  → _Perth Airport: 13.7°C, winds 5.2 km/h SSE, clear (GMT+8)._

---

## 💎 Why Watsonx Orchestrate Shines

| Feature                   | Benefit                                         |
|---------------------------|-------------------------------------------------|
| **OpenAPI-native**        | Tools are reusable, governed, versioned         |
| **No-code orchestration** | Logic is defined once, used everywhere          |
| **Enterprise governance** | Full observability, logging, compliance-ready   |
| **LLM-driven autonomy**   | Agent plans before it acts — no brittle chains  |
| **Audit-ready reasoning** | Every tool call is visible ("Show Reasoning")   |
| **Multi-channel deployment** | Chat, voice, web, phone, widgets           |

> _This isn’t just a weather bot — it’s a blueprint for enterprise-grade AI orchestration._

---

## 🧠 Example Conversation

```
You: What's the weather in Sydney today?
Agent: Sydney: 17°C, winds 2.7 km/h NW, clear (GMT+11).

You: What's the weather in Melbourne tomorrow?
Agent: Melbourne: 19°C, light breeze, mostly clear (GMT+11).

You: What's the temperature near Perth Airport?
Agent: Perth Airport: 13.7°C, winds 5.2 km/h SSE, clear (GMT+8).
```

---

## 🔮 Future Extensions

- **7-day forecast summaries** (daily params)
- **Global geocoding** support
- **watsonx.governance** for traceable AI usage
- **Voice modality** with watsonx Voice Preview
- **Slack/Teams connectors** for real-time deployment

---

## 💬 Manager’s Reflection

> “This project is more than a demo — it’s a blueprint for how we bring watsonx into every enterprise conversation.  
> It shows the discipline of IBM engineering: governed, ethical, explainable AI.  
> And it proves that when IBMers build, they build for trust, scale, and impact.”
>
> — Team Lead, IBM AI Automation

---

## 🧩 Quick Files Overview

| File                          | Purpose                                  |
|-------------------------------|------------------------------------------|
| weather_openapi_min.yaml      | Open-Meteo tool definition               |
| geocoder_openapi_min.yaml     | Nominatim tool definition                |
| openmeteo_weathercode_map.json| Human-readable weather mapping           |
| au_city_cache.json            | Fallback coordinates for Aussie cities   |
| behavior_prompt_examples.md   | Behavior + few-shot reference            |
| README.md                     | You’re reading it.                       |

---

## 🧡 Why I Love IBM (and You Should, Too!)

IBM isn’t just a tech company — it’s a culture of trust, transparency, and innovation.  
**Watsonx Orchestrate** demonstrates this by putting governance, auditability, and human-centric automation at the heart of every agent.

I love IBM because I know that when I build with their tools, I’m building for scale, for responsibility, and for the future.  
With watsonx, you can go from idea to enterprise-grade in days — and scale to every channel your business needs, with security and compliance as defaults.

**Don’t just automate. Orchestrate intelligence.**  
Let’s build something extraordinary, together.

---

<p align="center">
  ⭐ <em>Star this repo if you believe AI should be explainable, secure, and enterprise-grade.</em>
</p>
