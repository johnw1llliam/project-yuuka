# Project YUUKA
YUUKA – Your Useful Utility for Keeping Allowances

<a href="https://static.wikia.nocookie.net/blue-archive/images/e/e1/Yuuka_Live2D.gif/revision/latest?cb=20211128050418" class="mw-file-description image"><img src="https://static.wikia.nocookie.net/blue-archive/images/e/e1/Yuuka_Live2D.gif/revision/latest/scale-to-width-down/300?cb=20211128050418" decoding="async" loading="lazy" width="300" height="213" class="mw-file-element lazyloaded" data-image-name="Yuuka Live2D.gif" data-image-key="Yuuka_Live2D.gif" data-relevant="1" data-src="https://static.wikia.nocookie.net/blue-archive/images/e/e1/Yuuka_Live2D.gif/revision/latest/scale-to-width-down/300?cb=20211128050418"></a>

Project YUUKA is a personal finance assistant built on n8n, designed to help you manage your allowances and spending habits. The assistant is powered by an AI persona named "Yuuka," who acts as a strict but caring ~~girlfriend~~ personal accountant. The project is comprised of two core n8n workflows that you can set to work together: a versatile chat interface and a proactive daily spending analyzer. Both workflows are stateful, sharing a common MongoDB chat memory to ensure "Yuuka" remembers all your interactions.

Core Technologies
Workflow Automation: n8n

- LLM: OpenAI (gpt-4.1-mini)
- AI Orchestration: LangChain (via n8n's LangChain nodes)
- Finance Database: MySQL
- Chat Memory: MongoDB
- Web Search: Tavily AI

## Workflows:
**1. Yuuka (Chat)**<br>
This is the main conversational interface, triggered via a webhook. It uses an AI-powered router to understand the user's intent and direct the query to one of three specialized agents.
It features:<br>

- Agent Router: An initial OpenAI agent classifies the user's message into one of three categories: chat, finance, or google.
- Chat Branch: Handles casual, non-financial conversation.
- Finance Branch: A specialized agent that can answer financial questions. It is equipped with a MySQL Tool to query the finance database (e.g., income_total table) and provide real-time budget information.
- Google Search Branch: A two-step agent for web searches.
- All agents share the same "Yuuka" persona and are connected to a MongoDB Chat Memory (keyed by a uuid), allowing for continuous, stateful conversations.

**2. Yuuka (Daily Spending)**<br>
This is a proactive workflow designed to run automatically (e.g., via a daily schedule). It gathers a complete financial summary and uses the "Yuuka" AI to analyze it for the user.
The workflow has following features:<br>

- Fetch 3-Day History: Queries the MySQL database for the total spending from the last three days (daily_total table).
- Fetch Today's Spending: Queries the MySQL database for all itemized expenses from the current day (daily_spending table).
- Fetch Remaining Budget: Retrieves the user's total remaining budget (income_total table).
- Format Data: Custom Code nodes format this data into a clean, human-readable list for the AI.
- AI Analysis: All the financial data is passed to the "Yuuka" agent, who is prompted to "analyze ~~her boyfriend's~~ daily expenses" and provide a (likely annoyed) summary.
- This workflow shares the same MongoDB Chat Memory, so the daily analysis becomes part of the ongoing conversation history.
