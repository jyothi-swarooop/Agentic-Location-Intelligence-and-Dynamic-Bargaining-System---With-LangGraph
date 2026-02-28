# 🔍 PlaceDiscover and Negotiation Agentic Platform

An intelligent AI-powered platform that discovers local businesses, analyzes their reviews, and automates SMS-based deal negotiations using LangGraph agentic workflows.

---

## 🎯 What This Project Does

This platform combines advanced AI orchestration with real-world business discovery and negotiation capabilities:

1. **Natural Language Business Search** - Ask in plain English: *"Find the best gym in Koramangala, Bangalore"*
2. **Intelligent Review Analysis** - AI scrapes and analyzes Google Maps reviews to identify the best options
3. **Automated SMS Negotiation** - The agent drafts and sends negotiation messages to businesses
4. **Human-in-the-Loop Control** - You approve every message before it's sent

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend Dashboard                        │
│            (Real-time streaming, Chat interface)                 │
└─────────────────────────────────────────────────────────────────┘
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                     FastAPI Backend Server                       │
│           (REST API, Server-Sent Events, State Management)       │
└─────────────────────────────────────────────────────────────────┘
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                        LangGraph Agent                          │
│     ┌──────────┐    ┌──────────┐    ┌───────────────┐          │
│     │  Router  │ →  │ Responder│ →  │ Human Review  │          │
│     │  (LLM)   │    │  (LLM)   │    │  (Interrupt)  │          │
│     └──────────┘    └──────────┘    └───────────────┘          │
└─────────────────────────────────────────────────────────────────┘
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                      External Services                           │
│   ┌──────────┐  ┌──────────────┐  ┌──────────────────┐         │
│   │ SerpStack│  │WebScraping.AI│  │   SMSMobileAPI   │         │
│   │ (Search) │  │  (Reviews)   │  │ (SMS Gateway)    │         │
│   └──────────┘  └──────────────┘  └──────────────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| **🔎 Smart Business Discovery** | Uses SerpStack API to find local businesses with ratings, phone numbers, and addresses |
| **📊 Review Intelligence** | Scrapes and analyzes Google Maps reviews using AI for sentiment and insights |
| **💬 Automated Negotiation** | LangGraph-powered agent drafts persuasive negotiation messages |
| **👤 Human Approval** | Every SMS requires your approval before sending |
| **📡 Real-time Updates** | Server-Sent Events stream agent reasoning to the dashboard |
| **🔄 Reflection Pattern** | Agent drafts, critiques, and revises messages for better outcomes |

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **LLM** | Groq (Llama 3.3 70B) | Intent parsing, message generation, sentiment analysis |
| **Orchestration** | LangGraph | State machine with conditional routing and interrupts |
| **Backend** | FastAPI | REST API with SSE for streaming |
| **Search** | SerpStack API | Local business discovery |
| **Scraping** | WebScraping.AI | Google Maps review extraction |
| **SMS** | SMSMobileAPI | Send/receive SMS via Android app |
| **Frontend** | HTML/CSS/JavaScript | Interactive dashboard with chat |

---

## 📁 Project Structure

```
PlaceDiscover_and_Negotiation_Agentic_Platform/
├── app/
│   ├── main.py                 # FastAPI routes and SSE endpoints
│   ├── config.py               # Environment configuration
│   ├── models.py               # Pydantic schemas
│   ├── agent/
│   │   ├── graph.py            # LangGraph workflow definition
│   │   ├── nodes.py            # Workflow nodes (router, responder, etc.)
│   │   ├── state.py            # Agent state schema
│   │   └── tools.py            # SerpStack, WebScraping integrations
│   ├── messaging/
│   │   ├── base.py             # Messaging interface
│   │   ├── service.py          # Provider factory
│   │   └── smsmobileapi.py     # SMS implementation
│   └── static/
│       └── style.css           # Dashboard styles
├── frontend/
│   └── dashboard.html          # Main UI
├── backend/
│   └── requirements.txt        # Python dependencies
├── .env.example                # Environment template
├── netlify.toml                # Netlify deployment config
├── render.yaml                 # Render deployment config
└── README.md                   # This file
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- API Keys (all have free tiers):
  - [Groq](https://console.groq.com/) - LLM inference
  - [SerpStack](https://serpstack.com/) - Business search
  - [WebScraping.AI](https://webscraping.ai/) - Review scraping
  - [SMSMobileAPI](https://smsmobileapi.com/) - SMS gateway (requires Android phone)

### Installation

```bash
# Clone the repository
git clone https://github.com/Mokesh2005/PlaceDiscover_and_Negotiation_Agentic_Platform.git
cd PlaceDiscover_and_Negotiation_Agentic_Platform

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.\.venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate

# Install dependencies
pip install -r backend/requirements.txt

# Copy and configure environment variables
cp .env.example .env
# Edit .env with your API keys
```

### Environment Variables

Create a `.env` file with your API keys:

```bash
# LLM Configuration
GROQ_API_KEY=gsk_your_groq_key_here
GROQ_API_KEY_2=gsk_backup_key_optional

# Search & Scraping
SERPSTACK_API_KEY=your_serpstack_key
WEBSCRAPING_AI_API_KEY=your_webscraping_key

# SMS Provider
messaging_provider=smsmobileapi
smsmobileapi_key=your_smsmobileapi_key
```

### Run the Server

```bash
python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Access the Dashboard

Open your browser to: `http://localhost:8000/frontend/dashboard.html`

---

## 📖 How It Works

### The Reflection Pattern

The agent uses a **reflection pattern** for better negotiation outcomes:

```
Generate → Critique → Revise → Get Approval → Send → Analyze Response → Loop
```

1. **Responder** drafts a message using context (reviews, pricing, user goals)
2. **Revisor** critiques the draft for tone, leverage, and strategy
3. **Human approval** required before sending
4. **Loop continues** based on shop replies until deal reached

### LangGraph Workflow

```
START
  ↓
ROUTER (Intent Detection)
  ├──→ PATH A: Simple Search
  │      ├─ Fetch businesses from SerpStack
  │      ├─ Select top 3 by rating
  │      ├─ Scrape and analyze reviews
  │      └─ Return best recommendation → END
  │
  └──→ PATH B: Negotiation
         ├─ Initialize negotiation state
         ├─ Formulate strategy
         ├─ HUMAN REVIEW (⏸️ Interrupt)
         ├─ Send SMS after approval
         ├─ Poll for reply
         ├─ Analyze response
         └─ Loop or END
```

---

## 🧪 Testing

### Quick Test (No SMS Required)

1. Enter query: `"Find the best cafe in Indiranagar, Bangalore"`
2. Click **Start Scouting**
3. Watch the agent:
   - Parse your intent
   - Search for businesses
   - Analyze reviews
   - Recommend the best option

### Full Test (With SMS)

1. Set up SMSMobileAPI on your Android phone
2. Add API key to `.env`
3. Search for a business
4. Click **Negotiate** on a result
5. Enter your goal (e.g., "Get 20% discount")
6. Review and approve the draft message
7. Continue the negotiation loop

---

## 🌐 Deployment

### Deploy to Render (Backend)

1. Create a new Web Service on [Render](https://render.com)
2. Connect your GitHub repository
3. Set build command: `pip install -r backend/requirements.txt`
4. Set start command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
5. Add environment variables from `.env`

### Deploy to Netlify (Frontend)

1. Connect your GitHub repository to [Netlify](https://netlify.com)
2. Set publish directory to `frontend`
3. Update `API_BASE_URL` in the frontend to point to your Render backend

See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed instructions.

---

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Server won't start** | Ensure virtual environment is activated and dependencies installed |
| **No API key error** | Check `.env` file exists and contains valid keys |
| **No search results** | Verify SerpStack API key and quota |
| **SMS not sending** | Ensure SMSMobileAPI app is running on your phone |
| **Frontend blank** | Use correct URL with `.html` extension |

---

## 📄 License

MIT License - Feel free to use and modify for your projects.

---

## 🙏 Acknowledgments

- [LangGraph](https://github.com/langchain-ai/langgraph) - State graph framework
- [Groq](https://groq.com) - Fast LLM inference
- [SerpStack](https://serpstack.com) - Business search API
- [WebScraping.AI](https://webscraping.ai) - Web scraping service
- [SMSMobileAPI](https://smsmobileapi.com) - SMS gateway

---

## 📬 Contact

**Repository:** [github.com/Mokesh2005/PlaceDiscover_and_Negotiation_Agentic_Platform](https://github.com/Mokesh2005/PlaceDiscover_and_Negotiation_Agentic_Platform)

**Issues:** Use GitHub Issues for bugs and feature requests

---

**Built with ❤️ by Mokesh**
