# 🚀 Privacy-First Personal Assistant

**Track:** Concierge Agents
**Subtitle:** A Capstone Project for the Kaggle 5-Day AI Agents Intensive Course. This agent protects user privacy by asking for explicit permission before saving any long-term memories.

[![Kaggle 5-Day AI Agents Course](https://img.shields.io/badge/Kaggle-5--Day%20AI%20Agents%20Course-blue)](https://www.kaggle.com/competitions/ai-agents-capstone-project-nov-2025)

---

## 📺 Project Video 
//upcoming

## 1. 🎯 The Pitch (Problem, Solution, Value)

This project maps directly to the "Category 1" evaluation criteria.

* **The Problem:** Standard personal assistants have a major flaw: they save everything you say by default. This creates a privacy nightmare, as users have no control over whether sensitive information (like health data, family birthdays, or private thoughts) is stored permanently.

* **The Solution:** This agent inverts the model. It's a "Privacy-First" assistant that assumes all information is temporary. It is built to detect sensitive information and **must ask for explicit user permission** before saving anything to its long-term memory.

* **The Value (Core Concept):** This project's core idea is building user trust through explicit consent. It gives the user genuine control over their data by using a Human-in-the-Loop (HITL) workflow. The agent can still be a helpful, stateful assistant, but only with facts the user has personally approved.

## 2. 🛠️ The Implementation (Architecture & Key Concepts)

This agent's architecture and code are designed to satisfy the "Category 2: The Implementation" criteria by using 3+ key concepts from the course.

### 🏛️ Architecture

The agent's workflow is centered around a "pause and resume" logic. The full process flow and system architecture are visualized below:
![Untitled](https://github.com/user-attachments/assets/d9787e2b-6ed6-4ddd-9690-f55031fa45de)

### 🔑 Key Concepts Used

This project implements **three core concepts** from the course notebooks:

1.  **Long-Running Operations (LRO):** This is the core of the privacy feature. The agent uses a custom tool, `process_user_fact`, which detects sensitive keywords (e.g., "allergic," "birthday"). When a sensitive fact is found, the tool calls `tool_context.request_confirmation()` to **pause** the entire agent workflow and wait for the user's "yes/no" response. This is the "Human-in-the-Loop" (HITL) pattern.

2.  **Long-Term Memory:** This concept is split into two parts:
    * **Saving:** If (and only if) the user approves the LRO, the agent resumes, and the tool saves the conversation to the `InMemoryMemoryService` using `await memory_service.add_session_to_memory()`.
    * **Retrieving:** The agent uses the `preload_memory` tool. This *proactively* loads all *approved* facts from long-term memory into the agent's context at the start of every new conversation.

3.  **Context Engineering (Compaction):** This is the "forget" mechanism. If the user *rejects* a save, the fact is not saved to long-term memory. We also enable `EventsCompactionConfig` on the `App` with an aggressive interval. This ensures the rejected fact is quickly and automatically removed from the *short-term* session history, making the agent truly "forget" it.

### ✨ Bonus Concepts Used

* **Effective Use of Gemini:** The agent's reasoning is powered by `Gemini(model="gemini-2.5-flash")`, satisfying the bonus criteria.
* **Custom Tools:** The entire project is orchestrated by a custom `FunctionTool` (`process_user_fact`) that handles the complex LRO and memory logic.

## 3. 💻 Technology Stack

* **Google Agent Development Kit (ADK)**
* **Google Gemini API** (gemini-2.5-flash)
* **Python 3** & `asyncio`
* **Google Colab** (for development and demonstration)
* **Deployment:** Agent Engine (ADK) on Google Cloud (Vertex AI)

## 4. ⚙️ Setup & Usage

This project is designed to run in a Google Colab or Kaggle Notebook.

### Setup

1.  **Get API Key:** Create a Google Gemini API key at [Google AI Studio](https://aistudio.google.com/app/api-keys).
2.  **Add Secret:** Open the notebook in Google Colab and click the "Secrets" (🔑) tab. Create a new secret named `GOOGLE_API_KEY` and paste your key as the value.
3.  **Run Setup Cells:** Run cells 1-6 in the notebook. This will:
    * Install `google-adk`.
    * Import all libraries.
    * Configure the API key.
    * Define the LRO tool, the agent, the app, and the helper functions.

### How to Run the Interactive Demo

Run the final cell (Cell 8: `FINAL DEMO: Interactive Chat Loop`) to start the agent.

> **Note:** The demo code (`cell 7`) is a pre-scripted run to show all features working. The interactive chat (cell 8) is for live demonstration.

You can test the full privacy workflow:

1.  **Test 1: Save a Fact**
    * **You:** `I am allergic to strawberries.`
    * **Agent:** `(PAUSES)` ... `Assistant requires approval. Approve? (y/n):`
    * **You:** `y`
    * **Agent:** `Got it. I've saved this to your long-term profile.`

2.  **Test 2: Reject a Fact**
    * **You:** `My birthday is August 10th.`
    * **Agent:** `(PAUSES)` ... `Assistant requires approval. Approve? (y/n):`
    * **You:** `n`
    * **Agent:** `Okay, I will not save this fact.`

3.  **Test 3: Verify Memory**
    * *Restart the interactive chat (cell 8) to simulate a new session.*
    * **You:** `What do you know about me?`
    * **Agent:** `I know that you are allergic to strawberries.`
    * *(Notice: It correctly remembers the allergy but not the birthday).*

---
*This project was built for the Kaggle 5-Day AI Agents Intensive Course (Nov 2025).*
