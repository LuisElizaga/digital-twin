# AI Digital Twin

An AI-powered digital twin chatbot that simulates your personality using OpenAI's GPT-4o-mini with persistent conversation memory.

![Next.js](https://img.shields.io/badge/Next.js-16.0-black?logo=next.js)
![React](https://img.shields.io/badge/React-19.2-blue?logo=react)
![FastAPI](https://img.shields.io/badge/FastAPI-Python-green?logo=fastapi)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript)

## 🎯 Features

- 💬 **Real-time AI Chat** - Conversational interface powered by GPT-4o-mini
- 🧠 **Persistent Memory** - Conversations are saved and can be resumed
- 🎭 **Customizable Personality** - Define your twin's personality via `me.txt`
- 🎨 **Modern UI** - Built with Next.js 16, React 19, and Tailwind CSS 4
- 📦 **Session Management** - Multiple conversation sessions support
- ⚡ **Fast & Responsive** - Optimized with async operations

## 🏗️ Architecture

```
TWIN/
├── frontend/          # Next.js 16 + React 19 + TypeScript + Tailwind 4
├── backend/           # FastAPI (Python) + OpenAI API
├── memory/            # Persistent conversation storage (JSON)
└── week2/             # Course documentation
```

## 🛠️ Tech Stack

### Frontend
- **Next.js 16.0.0** (App Router)
- **React 19.2.0**
- **TypeScript 5**
- **Tailwind CSS 4**
- **Lucide React** (Icons)

### Backend
- **FastAPI** (Python web framework)
- **OpenAI API** (GPT-4o-mini)
- **Python 3.10+**
- **uvicorn** (ASGI server)

## 📋 Prerequisites

- **Node.js** >= 20.9.0 (for Next.js 16)
- **Python** >= 3.10
- **OpenAI API Key** ([Get one here](https://platform.openai.com/api-keys))
- **npm** or **yarn** or **pnpm**

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/LuisElizaga/TWIN.git
cd TWIN
```

### 2. Backend Setup

#### Install Dependencies

```bash
cd backend

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# On Linux/Mac:
source .venv/bin/activate
# On Windows:
# .venv\Scripts\activate

# Install packages
pip install -r requirements.txt
```

#### Configure Environment Variables

Create a `.env` file in the `backend/` directory:

```bash
# backend/.env
OPENAI_API_KEY=sk-your-api-key-here
CORS_ORIGINS=http://localhost:3000
```

#### Create Your Twin's Personality

Create a `me.txt` file in the `backend/` directory with your twin's personality description:

```bash
# backend/me.txt
You are a helpful AI assistant with expertise in software development...
(Add your personality description here)
```

#### Run the Backend Server

```bash
# From backend/ directory
python server.py

# Or with uvicorn directly:
uvicorn server:app --reload --host 0.0.0.0 --port 8000
```

The backend API will be available at: `http://localhost:8000`

**API Documentation:** `http://localhost:8000/docs`

---

### 3. Frontend Setup

#### Install Dependencies

```bash
cd frontend
npm install
```

#### Run the Development Server

```bash
npm run dev
```

The frontend will be available at: `http://localhost:3000`

---

## 📡 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | API information |
| `/health` | GET | Health check |
| `/chat` | POST | Send message to the AI twin |
| `/sessions` | GET | List all conversation sessions |

### Example Chat Request

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Hello, who are you?",
    "session_id": null
  }'
```

### Response

```json
{
  "response": "I'm your AI Digital Twin...",
  "session_id": "uuid-session-id"
}
```

## 🎨 Frontend Scripts

```bash
npm run dev      # Start development server
npm run build    # Build for production
npm run start    # Start production server
npm run lint     # Run ESLint
```

## 🐍 Backend Scripts

```bash
python server.py                      # Run server
uvicorn server:app --reload           # Run with hot reload
uvicorn server:app --host 0.0.0.0    # Expose to network
```

## 📁 Project Structure

```
TWIN/
├── backend/
│   ├── server.py              # FastAPI application
│   ├── me.txt                 # Personality definition (not in repo)
│   ├── .env                   # Environment variables (not in repo)
│   ├── requirements.txt       # Python dependencies
│   └── pyproject.toml         # Python project config
│
├── frontend/
│   ├── app/
│   │   ├── page.tsx           # Main page
│   │   ├── layout.tsx         # Root layout
│   │   └── globals.css        # Global styles
│   ├── components/
│   │   └── twin.tsx           # Chat component
│   ├── public/                # Static assets
│   └── package.json           # Node dependencies
│
├── memory/                    # Conversation storage (not in repo)
├── .gitignore                 # Git ignore rules
└── README.md                  # This file
```

## 🔒 Security Notes

**Files excluded from Git (sensitive data):**
- `backend/.env` - API keys and secrets
- `backend/me.txt` - Personal personality description
- `memory/*.json` - Conversation history
- `backend/__pycache__/` - Python cache
- `backend/.venv/` - Virtual environment
- `frontend/node_modules/` - Node dependencies
- `frontend/.next/` - Next.js build cache

**Important:** Never commit your OpenAI API key or personal data to version control.

## 🎓 Use Cases

- **AI Course Companion** - Learning assistant for AI deployment
- **Personal AI Assistant** - Customized chatbot with your personality
- **Customer Support** - Template for AI-powered support systems
- **Conversational AI Demo** - Showcase of modern AI chat implementation

## 🧪 Testing

### Test Backend

```bash
# Check health
curl http://localhost:8000/health

# Test chat endpoint
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello", "session_id": null}'
```

### Test Frontend

1. Open `http://localhost:3000`
2. Type a message in the chat input
3. Verify the AI responds
4. Check that conversation persists (same session)

## 🐛 Troubleshooting

### Backend Issues

**Error: `openai.OpenAIError: The api_key client option must be set`**
- Solution: Set `OPENAI_API_KEY` in `backend/.env`

**Error: `ModuleNotFoundError: No module named 'fastapi'`**
- Solution: Install dependencies with `pip install -r requirements.txt`

### Frontend Issues

**Error: `Node.js version ">=20.9.0" is required`**
- Solution: Update Node.js to version 20 or higher

**Error: `Cannot connect to backend`**
- Solution: Ensure backend is running on `http://localhost:8000`

## 📚 Learning Resources

This project was built as part of an AI in Production course. Key concepts covered:

- Building production-ready AI applications
- FastAPI backend development
- Next.js frontend with Server Components
- OpenAI API integration
- Conversation memory management
- Deployment strategies

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Luis Elízaga**
- GitHub: [@LuisElizaga](https://github.com/LuisElizaga)
- Location: Bogotá, Colombia / Madrid, Spain

## 🙏 Acknowledgments

- Built as part of the "AI in Production" course
- Powered by OpenAI's GPT-4o-mini
- UI inspired by modern chat interfaces

---

**⭐ If you find this project useful, please give it a star!**
