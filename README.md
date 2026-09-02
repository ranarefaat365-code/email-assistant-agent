# Email Assistant Agent — Human-in-the-Loop with Dynamic Middleware

A production-oriented AI email agent built with LangChain and LangGraph. It authenticates the user before granting inbox access, reads emails, and drafts replies that require explicit human approval before they are sent. Built as part of the LangChain Academy "Introduction to LangChain" course.

## Overview

Moving an agent from a prototype to something safe for real users requires control over what the agent can do and when a human must step in. This project demonstrates those production concepts through an email assistant with two safeguards:

- **Authentication gate**: the agent only exposes inbox tools after the user authenticates. Until then, the only tool available is the one that authenticates them.
- **Human-in-the-loop approval**: before any email is sent, the agent interrupts and waits for the user to approve, edit, or reject the draft.

## Architecture

The agent is built on LangChain's create_agent abstraction and uses middleware to customize its behavior at runtime.

- **Model**: gemini-3.6-flash via the langchain-google-genai provider.
- **Dynamic tools and prompts**: custom wrap-style middleware adjusts the tools and system prompt based on the authentication state held in the agent's custom state.
- **Human-in-the-loop**: prebuilt middleware interrupts the run on the send-email tool, surfacing the draft for approval, editing, or rejection.
- **Custom state**: tracks whether the user is authenticated, defaulting to unauthenticated.

The agent starts in an unauthenticated mode where only the authentication tool is available. Once the user provides valid credentials, the state flips to authenticated and the read and send tools are unlocked. Any send action is then paused for human review.

## Requirements

- Python 3.12 or later (below 3.14)
- A Google API key for Gemini
- Optional: a LangSmith API key for tracing and debugging

Email reading and sending are implemented as placeholder functions, so no real inbox or email provider is required to run the project.

## Setup

1. Clone the repository and enter the project directory.
2. Create a virtual environment and install dependencies with uv sync.
3. Copy .env.example to .env and add your actual API keys. The .env file is excluded from version control and must never be committed.
4. Launch the notebook with uv run python -m jupyter lab, then open 3.5_email_agent.ipynb and run the cells in order.

## Usage

Run the agent and provide credentials when prompted. Once authenticated, ask it to read an email and draft a reply. When you ask it to send, the run pauses and presents the draft. You can then approve it, edit the body before sending, or reject it with feedback. A standalone Python version is included in 3.5_email_agent.py for use with LangGraph Studio.

## Key Concepts Demonstrated

- Building production-oriented agents with middleware
- Dynamically adjusting tools and prompts based on runtime state
- Gating tool access behind an authentication step
- Implementing human-in-the-loop approval, editing, and rejection flows
- Managing custom agent state across a conversation

## Acknowledgments

Built following the LangChain Academy "Introduction to LangChain" course. Adapted to use Google's Gemini model in place of the original OpenAI model.

## Author

**Rana Refaat**

- GitHub: [ranarefaat365-code](https://github.com/ranarefaat365-code)
- LinkedIn: [rana-refaat](https://www.linkedin.com/in/rana-refaat-/)
