# MindBridge - Nima AI

> **Democratizing mental health support through AI-powered therapist matching**

[![Built with NVIDIA Nemotron](https://img.shields.io/badge/NVIDIA-Nemotron-76B900?style=for-the-badge&logo=nvidia)](https://developer.nvidia.com/nemotron)
[![Powered by LangGraph](https://img.shields.io/badge/LangGraph-Multi--Agent-blue?style=for-the-badge)](https://github.com/langchain-ai/langgraph)

## 🎯 Mission

Every year, millions struggle with mental health challenges but can't access professional support due to cost, availability, or stigma. **MindBridge** bridges this gap by connecting people who can't afford therapy with volunteer therapists—guided by **Nima AI**, our autonomous support assistant powered by NVIDIA Nemotron.

## ✨ What Makes Nima AI Different

Unlike traditional chatbots, Nima AI demonstrates **true autonomous agency**:

- 🤝 **Intelligent Therapist Matching** - Autonomously searches, vets, and recruits volunteer therapists when needed
- 🚨 **Instant Crisis Detection** - Advanced Nemotron-powered reasoning detects risk indicators in real-time
- 💬 **Empathetic Intake** - Six-stage conversational flow that builds trust without rushing users
- 📈 **Adaptive Habit Tracking** - Personalized micro-habits with streak tracking between therapy sessions
- 🔒 **Privacy-First Design** - User-controlled privacy tiers (No Records → Full Support)

## 🏗️ Multi-Agent Architecture

```
┌─────────────────────────────────────────────┐
│     Coordinator Agent (Nemotron 49B)        │
└────────────┬────────────────────────────────┘
             │
    ┌────────┼────────┬───────────┬──────────┐
    ▼        ▼        ▼           ▼          ▼
┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐
│Intake  ││Crisis  ││Resource││Habit   ││Quality │
│Agent   ││Agent   ││Agent   ││Agent   ││Monitor │
│(9B)    ││(9B)    ││(49B)   ││(9B)    ││(49B)   │
└────────┘└────────┘└────────┘└────────┘└────────┘
```

### Agent Responsibilities

| Agent | Model | Purpose |
|-------|-------|---------|
| **Coordinator** | Nemotron 49B | Orchestrates workflow and agent coordination |
| **Intake Agent** | Nemotron 9B | Conducts empathetic, stage-aware conversations |
| **Crisis Agent** | Nemotron 9B | ReAct-based risk assessment and intervention |
| **Resource Agent** | Nemotron 49B | Autonomous therapist search and matching |
| **Habit Agent** | Nemotron 9B | Adaptive habit recommendations and tracking |

## 🚀 Tech Stack

### Frontend
- **React 19** + **TypeScript** - Modern UI with type safety
- **Vite** - Lightning-fast build tool
- **Tailwind CSS** - Utility-first styling
- **React Router** - Client-side routing

### Backend
- **Python 3.13** - Core backend language
- **FastAPI** - High-performance async API framework
- **LangGraph** - Multi-agent orchestration framework
- **NVIDIA Nemotron** (via OpenRouter) - LLM reasoning engine
- **Tavily API** - Web search for therapist resources

### Data & State
- **Pydantic** - Data validation and settings management
- **JSON** - Therapist database (demo)
- **Supabase** (optional) - Production database
- **Redis** (optional) - State caching

## 📦 Project Structure

```
MindBridge/
├── agents/              # AI agent implementations
│   ├── base_agent.py    # Abstract base class
│   ├── intake_agent.py  # Conversational intake
│   ├── crisis_agent.py  # Crisis detection (ReAct)
│   ├── resource_agent.py # Therapist matching
│   ├── habit_agent.py   # Habit tracking
│   └── coordinator_agent.py
├── workflows/           # LangGraph orchestration
│   ├── intake_to_crisis_workflow.py
│   └── crisis_to_resource.py
├── models/              # Data models
│   ├── user.py
│   ├── therapist.py
│   ├── habit.py
│   └── mock_data.py
├── ui/                  # React frontend
│   ├── src/
│   │   ├── App.tsx
│   │   ├── App.css
│   │   └── components/
│   ├── package.json
│   └── vite.config.ts
├── voice_api.py         # Voice integration backend
├── streamlit_demo.py    # Demo interface
├── requirements.txt     # Python dependencies
├── .env.example         # Environment template
└── README.md
```

## 🛠️ Installation & Setup

### Prerequisites
- **Python 3.10+**
- **Node.js 18+**
- **OpenRouter API Key** (for NVIDIA Nemotron access)
- **Tavily API Key** (for web search)

### Backend Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/mindbridge.git
cd mindbridge

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your API keys:
# OPENROUTER_API_KEY=your_key_here
# TAVILY_API_KEY=your_key_here
```

### Frontend Setup

```bash
# Navigate to UI directory
cd ui

# Install dependencies
npm install

# Start development server
npm run dev
```

### Run the Application

**Option 1: Voice API (Full Experience)**
```bash
# Terminal 1: Start backend API
python voice_api.py

# Terminal 2: Start frontend
cd ui && npm run dev

# Open http://localhost:5173
```

**Option 2: Streamlit Demo**
```bash
streamlit run streamlit_demo.py
```

## 🌍 Environment Variables

Create a `.env` file in the root directory:

```env
# Required
OPENROUTER_API_KEY=your_openrouter_api_key
TAVILY_API_KEY=your_tavily_api_key

# Optional (for production)
SUPABASE_URL=your_supabase_url
SUPABASE_KEY=your_supabase_key
REDIS_URL=redis://localhost:6379
```

## 🎨 Key Features Explained

### 1. Privacy Tiers
Users control how much AI assistance they receive:
- **No Records** - Complete anonymity
- **Your Private Notes** - User-encrypted only (default)
- **Assisted Handoff** - Platform helps transitions
- **Full Support** - Complete AI assistance

### 2. Autonomous Crisis Detection
Uses ReAct (Reason + Act) pattern:
```python
THOUGHT → ACTION → OBSERVATION → DECISION
```
- Detects 5 risk levels: NONE → IMMEDIATE
- Auto-escalates to emergency resources
- Continuous monitoring during sessions

### 3. Intelligent Therapist Matching
When therapists are scarce, the system:
1. Searches internal database
2. Web searches via Tavily
3. Autonomously reaches out to new volunteers
4. Matches based on specialization + availability

### 4. Habit Tracking System
- Deterministic recommendations based on user context
- Streak tracking and progress monitoring
- Adaptive difficulty adjustment
- Therapist feedback integration

## 🚢 Deployment

### Vercel (Frontend)
```bash
cd ui
vercel --prod
```

### Railway/Render (Backend)
```bash
# Procfile
web: uvicorn voice_api:app --host 0.0.0.0 --port $PORT
```

### Docker (Full Stack)
```bash
docker-compose up -d
```

## 📊 Demo Scenarios

### Scenario 1: Career Burnout
User describes work/school overload → Intake Agent collects context → Crisis Agent assesses (LOW risk) → Matched with Career Counselor → Habit Agent suggests "End-of-day decompress"

### Scenario 2: Crisis Intervention
User mentions self-harm → Immediate escalation → Emergency privacy tier → Crisis resources surfaced → Expedited therapist match → Continuous monitoring

## 🤝 Contributing

This project was built for the NVIDIA Nemotron Hackathon. While primarily a demonstration of agentic AI capabilities, contributions are welcome:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

MIT License - See [LICENSE](LICENSE) for details

## 🙏 Acknowledgments

- **NVIDIA** for open-sourcing Nemotron models
- **LangChain** for the LangGraph framework
- **OpenRouter** for unified LLM API access
- **Volunteer therapists** making mental health accessible

## 📧 Contact

**MindBridge Team**
Project built for NVIDIA Nemotron Hackathon

---

**Built with ❤️ using NVIDIA Nemotron**
*Making mental health support accessible to everyone*
