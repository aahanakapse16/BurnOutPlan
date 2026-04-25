# 🌿 Sanctuary — Personalized Burnout Recovery Planner

> A multi-agent AI system that generates a science-backed, personalized 21-day burnout recovery plan in under 60 seconds.

**Built by:** Mohit Yeole & Aahana Kapse  
**Institution:** Ramdeobaba University, Nagpur — Electronics and Communication Engineering  
**Semester:** 4th Semester B.Tech, Session 2025–26

---

## 📌 Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [How It Works](#how-it-works)
- [Agent Architecture](#agent-architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
- [Environment Variables](#environment-variables)
- [Running the App](#running-the-app)
- [Fallback System](#fallback-system)
- [Output Tabs](#output-tabs)
- [Quality Scoring Rubric](#quality-scoring-rubric)
- [Contributors](#contributors)

---

## Overview

**Sanctuary** is a Streamlit web application that uses a sequential multi-agent AI pipeline to create a fully personalized burnout recovery plan. The user describes their situation — work stress, lifestyle, triggers — and within 60 seconds, the system:

1. Searches the web for **current, evidence-based** recovery techniques
2. **Matches** those techniques to the user's specific constraints and lifestyle
3. **Writes** a structured 21-day recovery plan across 3 phases
4. **Independently scores** the plan on 5 quality criteria

The result is a plan that is more personalized, more current, and more verifiable than anything a single-prompt chatbot can produce.

---

## The Problem

Generic burnout advice fails for four reasons:

| Problem | Why It Matters |
|---|---|
| **Trigger Blindness** | Someone burnt out from isolation has a completely different problem than someone drowning in Slack notifications. Generic advice ignores this. |
| **Constraint Blindness** | A parent of two young children cannot do 30 minutes of morning meditation. A student with no income cannot take a week off. Generic advice makes people feel guilty. |
| **Static Knowledge** | Wellness articles are frozen in time. Research on sleep science, CBT, and occupational health keeps evolving. |
| **No Quality Check** | A blog post or wellness app has no feedback loop. It produces output and moves on — no scoring, no verification. |

This project addresses all four directly using a multi-agent pipeline.

---

## How It Works

The system runs as a sequential pipeline inside a Streamlit app. Each agent receives the verified output of the previous agent, so no agent can invent something that wasn't already established upstream.

```
User Profile Input
      │
      ▼
┌─────────────────┐
│  Agent 1        │  ──► Tavily Live Web Search (up to 8 turns)
│  Researcher     │
└────────┬────────┘
         │ research_brief
         ▼
┌─────────────────┐
│  Agent 2        │
│  Habit Analyzer │
└────────┬────────┘
         │ habit_analysis
         ▼
┌─────────────────┐
│  Agent 3        │
│  Plan Writer    │
└────────┬────────┘
         │ recovery_plan (21 days)
         ▼
┌─────────────────┐
│  Agent 4        │
│  Judge          │  ──► JSON scores on 5 criteria
└────────┬────────┘
         │
         ▼
   Streamlit Output Tabs
```

---

## Agent Architecture

### Agent 1 — Researcher
Uses a **function-calling loop** (up to 8 turns) with Tavily Search to find current, evidence-based burnout recovery techniques specific to the user's triggers. This is not a single-shot prompt — the agent actively decides what to search for across multiple turns.

### Agent 2 — Habit Analyzer
Takes the research brief and the user's profile and **maps techniques to the user's actual lifestyle and constraints**. Its only job is reasoning about fit — it does not write a plan. This separation of concerns ensures the final plan is based on explicit matching, not intuition.

### Agent 3 — Plan Writer
Writes the **21-day recovery plan** constrained to work only from Agent 1's research and Agent 2's analysis. It cannot invent techniques that were not already established upstream. The plan is split into three phases:
- **Phase 1 (Days 1–7):** Rest and stabilization — very gentle habits
- **Phase 2 (Days 8–14):** Rebuilding — slightly more active habits
- **Phase 3 (Days 15–21):** Reintegration — sustainable routines

Each day includes a morning micro-habit, a main focus for the day, and an evening check-in reflection prompt.

### Agent 4 — Judge
Independently evaluates the recovery plan on **5 criteria** (see Quality Scoring Rubric below). The Judge was not involved in writing the plan, so its scores are not biased toward approving its own work. Returns structured JSON.

---

## Tech Stack

| Component | Technology |
|---|---|
| Frontend / UI | Streamlit |
| LLM | Google Gemini 2.0 Flash (`gemini-2.0-flash`) |
| Web Search | Tavily Search API |
| Language | Python 3.11+ |
| LLM Client | `google-genai` |

---

## Project Structure

```
sanctuary/
├── app.py              # Streamlit UI, orchestration, output rendering
├── agents.py           # All 4 agent functions + fallback equivalents
├── prompts.py          # System prompts for each agent
├── requirements.txt    # Python dependencies
├── .env.example        # Template for required API keys
├── .gitignore
└── docs/
    └── architecture_diagram.excalidraw
```

### File Responsibilities

| File | Purpose |
|---|---|
| `app.py` | Handles form input, calls agents in sequence, renders output tabs and score card with ring chart |
| `agents.py` | Contains `burnout_researcher_agent`, `habit_analyzer_agent`, `plan_writer_agent`, `judge_agent` and all their fallback equivalents |
| `prompts.py` | Stores `RESEARCHER_SYSTEM_PROMPT`, `HABIT_ANALYZER_SYSTEM_PROMPT`, `PLAN_WRITER_SYSTEM_PROMPT`, `JUDGE_SYSTEM_PROMPT`, and `JUDGE_EVAL_PROMPT` |

---

## Setup & Installation

### Prerequisites
- Python 3.11 or higher
- A [Google Gemini API key](https://aistudio.google.com/app/apikey)
- A [Tavily API key](https://tavily.com/)

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/your-username/sanctuary-burnout-planner.git
cd sanctuary-burnout-planner

# 2. Create a virtual environment
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up environment variables
cp .env.example .env
# Then edit .env and add your API keys
```

---

## Environment Variables

Create a `.env` file in the root directory with the following:

```env
GEMINI_API_KEY=your_gemini_api_key_here
TAVILY_API_KEY=your_tavily_api_key_here
```

> ⚠️ Never commit your `.env` file. It is already in `.gitignore`.

---

## Running the App

```bash
streamlit run app.py
```

The app will open at `http://localhost:8501` in your browser.

### How to use it
1. Fill in your **Work Situation** — your role, hours, and workload
2. Fill in your **Stress Triggers** — what specifically is draining your energy
3. Fill in your **Lifestyle** — sleep, diet, exercise, screen time
4. Click **Build My Recovery Plan**
5. The progress bar will update as each agent finishes. Your plan appears in 4 tabs within ~60 seconds.

---

## Fallback System

The system is designed to **never crash** and always produce a usable output — even if every LLM call fails.

| Failure | System Response |
|---|---|
| `GEMINI_API_KEY` missing | All agents switch to fallback mode automatically. UI shows a warning. |
| Gemini quota exceeded (429) | `AgentExecutionError` is raised, relevant fallback function runs. |
| Tavily returns no results | Researcher continues with built-in recovery guidance. |
| Judge JSON cannot be parsed | Zero-scored fallback dict is returned. App does not crash. |
| Any unhandled exception | Streamlit catches it and shows an error message. |

---

## Output Tabs

Once the pipeline finishes, the app renders four tabs:

| Tab | Content | Produced By |
|---|---|---|
| **Research Brief** | Markdown report of current evidence-based recovery techniques | Agent 1 — Researcher |
| **Habit Analysis** | Which techniques fit the user and why, and which were excluded | Agent 2 — Habit Analyzer |
| **21-Day Plan** | Full day-by-day plan split into 3 phases | Agent 3 — Plan Writer |
| **Quality Review** | Scores on 5 criteria, ring chart, and metric bars | Agent 4 — Judge |

### Score Labels

| Score Range | Label |
|---|---|
| 4.25 and above | ✨ Exceptional Match |
| 3.50 – 4.24 | ✅ Strong Match |
| Below 3.50 | 🌱 Emerging Match |

---

## Quality Scoring Rubric

The Judge agent scores the recovery plan on these 5 criteria (each scored 1–5):

| Criterion | What It Checks |
|---|---|
| **Scientific Grounding** | Can all techniques be traced back to the research brief? |
| **Personalization** | Does the plan reflect the user's specific triggers, or is it generic? |
| **Practicality** | Are the daily habits realistic given the user's lifestyle? |
| **Progression** | Does difficulty build sensibly across the three phases? |
| **Compassionate Tone** | Is the language kind and supportive, not prescriptive or guilt-inducing? |

---

## Why Multi-Agent Instead of a Single Chatbot?

A single-prompt chatbot can generate a burnout recovery plan in one shot. The problem is:
- It has no grounding — it relies entirely on training data
- It cannot verify its own output
- It cannot check whether the advice actually fits the person

The sequential multi-agent pipeline solves each of these:
- **Researcher** uses live web search across up to 8 turns — not possible in a single prompt
- **Habit Analyzer** explicitly matches techniques to the user — separating analysis from writing
- **Plan Writer** is constrained to only use what was established upstream — no hallucination
- **Judge** evaluates the plan independently — unbiased, separate from writing

---

## Contributors

| Name | Role |
|---|---|
| **Mohit Yeole** | Role B — Builder & Deployer: Leads agent implementation, LLM-as-Judge integration, UI development, and deployment. Owns the working application and Loom video.|
| **Aahana Kapse** | Role A — Architect & Integrato: Leads problem definition, task decomposition, architecture design, and API/tool integrations. Owns the system design document.|

**Institution:** Ramdeobaba University, Nagpur  
**Department:** Electronics and Communication Engineering  
**Academic Year:** 2025–26

---

> *"The goal is steadiness, not perfect completion."*
