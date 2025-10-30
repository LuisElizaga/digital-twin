# AI Digital Twin

An AI-powered digital twin chatbot that simulates your personality using AWS Bedrock with persistent conversation memory, deployed on AWS infrastructure via Terraform.

![Next.js](https://img.shields.io/badge/Next.js-16.0-black?logo=next.js)
![React](https://img.shields.io/badge/React-19.2-blue?logo=react)
![FastAPI](https://img.shields.io/badge/FastAPI-Python-green?logo=fastapi)
![AWS](https://img.shields.io/badge/AWS-Bedrock-orange?logo=amazon-aws)
![Terraform](https://img.shields.io/badge/Terraform-IaC-purple?logo=terraform)

## 🎯 Features

- 💬 **Real-time AI Chat** - Conversational interface powered by AWS Bedrock (Nova Lite)
- 🧠 **Persistent Memory** - Conversations saved in S3 with session management
- 🎭 **Customizable Personality** - Define your twin's personality via data files
- 🎨 **Modern UI** - Built with Next.js 16, React 19, and Tailwind CSS 4
- ☁️ **AWS Serverless** - Lambda, API Gateway, S3, and CloudFront
- 🚀 **Infrastructure as Code** - Automated deployment with Terraform
- 🌐 **Production Ready** - Multi-environment support (dev, test, prod)

## 🏗️ Architecture

```
┌─────────────────┐
│   CloudFront    │ ← CDN Distribution
│   (Frontend)    │
└────────┬────────┘
         │
┌────────▼────────┐
│   S3 Bucket     │ ← Static Website (Next.js)
│   (Frontend)    │
└─────────────────┘
         │
         │ API Calls
         │
┌────────▼────────┐
│  API Gateway    │ ← REST API
└────────┬────────┘
         │
┌────────▼────────┐
│  Lambda         │ ← FastAPI + Mangum
│  (Python)       │
└────┬────────┬───┘
     │        │
     │        └────────────┐
┌────▼────────┐   ┌───────▼────────┐
│ AWS Bedrock │   │   S3 Bucket    │
│ (Nova Lite) │   │   (Memory)     │
└─────────────┘   └────────────────┘
```

## 🛠️ Tech Stack

### Frontend
- **Next.js 16.0.0** (App Router, Static Export)
- **React 19.2.0**
- **TypeScript 5**
- **Tailwind CSS 4**
- **Lucide React** (Icons)

### Backend
- **FastAPI** (Python web framework)
- **AWS Bedrock** (Nova Lite model)
- **Mangum** (ASGI adapter for Lambda)
- **Python 3.12**
- **boto3** (AWS SDK)

### Infrastructure
- **Terraform** (Infrastructure as Code)
- **AWS Lambda** (Serverless compute)
- **AWS API Gateway** (REST API)
- **AWS S3** (Static hosting + memory storage)
- **AWS CloudFront** (CDN)
- **AWS IAM** (Permissions)

## 📋 Prerequisites

### Local Development
- **Node.js** >= 20.9.0 (for Next.js 16)
- **Python** >= 3.12
- **uv** (Python package manager)
- **Docker** (for Lambda package building)

### AWS Deployment
- **AWS CLI** configured with credentials
- **Terraform** >= 1.0
- **AWS Account** with Bedrock access enabled
- **S3 bucket** for Terraform state (optional)

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/LuisElizaga/TWIN.git
cd TWIN
```

### 2. Local Development Setup

#### Backend Setup

```bash
cd backend

# Install uv if not already installed
# curl -LsSf https://astral.sh/uv/install.sh | sh

# Install dependencies
uv sync

# Configure data files (see Data Configuration section)
# Edit data/facts.json, data/style.txt, data/summary.txt
# Add data/linkedin.pdf

# Run the server
uv run uvicorn server:app --reload
```

Backend API: `http://localhost:8000`
API Docs: `http://localhost:8000/docs`

#### Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Run development server
npm run dev
```

Frontend: `http://localhost:3000`

---

## ☁️ AWS Deployment with Terraform

### Initial Setup

1. **Configure AWS CLI**
```bash
aws configure
# Enter your AWS Access Key ID, Secret Access Key, and region
```

2. **Enable AWS Bedrock Models**
- Go to AWS Console → Bedrock → Model access
- Request access to Amazon Nova models (especially `amazon.nova-lite-v1:0`)

3. **Review Terraform Variables**

Edit `terraform/terraform.tfvars`:
```hcl
project_name = "twin"
environment  = "dev"
aws_region   = "us-east-1"

# Optional: Custom domain
use_custom_domain = false
root_domain      = ""  # e.g., "example.com"
```

### Deploy to AWS

```bash
# Deploy to dev environment
./scripts/deploy.sh dev

# Deploy to production
./scripts/deploy.sh prod
```

The deployment script will:
1. 📦 Build Lambda deployment package with Docker
2. 🏗️ Initialize and apply Terraform infrastructure
3. 🚀 Build and deploy Next.js frontend to S3
4. 🌐 Output CloudFront and API Gateway URLs

### Destroy Infrastructure

```bash
# Destroy dev environment
./scripts/destroy.sh dev

# Destroy production (with confirmation)
./scripts/destroy.sh prod
```

---

## 📁 Project Structure

```
TWIN/
├── backend/
│   ├── data/                  # Personality configuration
│   │   ├── facts.json        # Personal information (JSON)
│   │   ├── linkedin.pdf      # LinkedIn profile export
│   │   ├── style.txt         # Communication style notes
│   │   └── summary.txt       # Summary notes
│   ├── server.py             # FastAPI application
│   ├── lambda_handler.py     # AWS Lambda entry point
│   ├── context.py            # Prompt context generation
│   ├── resources.py          # Data loading utilities
│   ├── deploy.py             # Lambda package builder
│   ├── requirements.txt      # Python dependencies
│   └── pyproject.toml        # UV project configuration
│
├── frontend/
│   ├── app/
│   │   ├── page.tsx          # Main page
│   │   ├── layout.tsx        # Root layout
│   │   └── globals.css       # Global styles
│   ├── components/
│   │   └── twin.tsx          # Chat component
│   ├── next.config.ts        # Next.js configuration
│   └── package.json          # Node dependencies
│
├── terraform/
│   ├── main.tf               # Main infrastructure definition
│   ├── variables.tf          # Input variables
│   ├── outputs.tf            # Output values
│   ├── versions.tf           # Provider versions
│   └── terraform.tfvars      # Variable values
│
├── scripts/
│   ├── deploy.sh             # Deployment automation
│   └── destroy.sh            # Cleanup automation
│
├── memory/                   # Local conversation storage
├── .gitignore                # Git ignore rules
└── README.md                 # This file
```

## 🎭 Data Configuration

The AI twin's personality is defined through data files in `backend/data/`:

### `facts.json` - Structured Personal Data
```json
{
  "full_name": "Your Name",
  "name": "Nick",
  "current_role": "Your Role",
  "location": "City/Country",
  "email": "your@email.com",
  "linkedin": "linkedin.com/in/yourprofile",
  "specialties": ["Skill1", "Skill2", "Skill3"],
  "years_experience": 15,
  "education": [
    {
      "degree": "Your Degree",
      "institution": "University Name",
      "year": "2020"
    }
  ]
}
```

### `linkedin.pdf` - Professional Profile
Export your LinkedIn profile as PDF and place it here. The system extracts text for context.

### `style.txt` - Communication Style
```
Describe your communication style:
- Tone (professional, casual, friendly)
- Preferred language patterns
- Common phrases or expressions
- Any specific mannerisms
```

### `summary.txt` - Additional Context
```
Additional notes about yourself:
- Career highlights
- Project experiences
- Technical expertise
- Personal interests
- Goals and aspirations
```

## 📡 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | API information and status |
| `/health` | GET | Health check |
| `/chat` | POST | Send message to AI twin |
| `/conversation/{session_id}` | GET | Retrieve conversation history |

### Example Chat Request

```bash
curl -X POST https://your-api.execute-api.us-east-1.amazonaws.com/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Hello, tell me about your experience",
    "session_id": null
  }'
```

### Response
```json
{
  "response": "Hello! I'm Luis, an AI and Cloud Platform Specialist...",
  "session_id": "uuid-session-id"
}
```

## ⚙️ Environment Variables

### Backend (Local Development)
Create `backend/.env`:
```bash
# AWS Configuration
DEFAULT_AWS_REGION=us-east-1
BEDROCK_MODEL_ID=amazon.nova-lite-v1:0

# Storage Configuration
USE_S3=false                # Use local storage for development
MEMORY_DIR=../memory        # Local memory directory

# CORS (optional)
CORS_ORIGINS=http://localhost:3000
```

### Backend (AWS Lambda)
Configured via Terraform, but can be overridden in AWS Console:
```bash
USE_S3=true
S3_BUCKET=twin-dev-memory-{account-id}
DEFAULT_AWS_REGION=us-east-1
BEDROCK_MODEL_ID=amazon.nova-lite-v1:0
CORS_ORIGINS=*
```

### Frontend
Create `frontend/.env.local` for development:
```bash
NEXT_PUBLIC_API_URL=http://localhost:8000
```

For production, this is set automatically during deployment.

## 🔧 AWS Bedrock Model Options

Available models (configure via `BEDROCK_MODEL_ID`):
- `amazon.nova-micro-v1:0` - Fastest, lowest cost
- `amazon.nova-lite-v1:0` - Balanced (default)
- `amazon.nova-pro-v1:0` - Most capable, higher cost

**Note:** Some regions require `us.` or `eu.` prefix (e.g., `us.amazon.nova-lite-v1:0`)

## 🔒 Security & Privacy

### Files Excluded from Git
- `backend/.env` - Environment variables
- `backend/data/*.pdf` - Personal documents
- `memory/*.json` - Conversation history
- `terraform/*.tfstate` - Terraform state files
- `terraform/.terraform/` - Terraform cache
- `backend/.venv/` - Python virtual environment
- `frontend/node_modules/` - Node dependencies

### AWS Security Best Practices
- ✅ S3 buckets have public access blocked (except frontend)
- ✅ Lambda has minimal IAM permissions
- ✅ API Gateway uses CORS configuration
- ✅ CloudFront provides HTTPS encryption
- ✅ Conversation data stored in private S3 bucket

## 🧪 Testing

### Local Backend Testing
```bash
# Health check
curl http://localhost:8000/health

# Test chat
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello", "session_id": null}'
```

### Production Testing
```bash
# Get API URL from Terraform output
cd terraform
terraform output api_gateway_url

# Test health endpoint
curl https://your-api-url.amazonaws.com/health

# Test chat endpoint
curl -X POST https://your-api-url.amazonaws.com/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello", "session_id": null}'
```

## 🐛 Troubleshooting

### Lambda Issues

**Error: "AccessDeniedException: User is not authorized to perform: bedrock:InvokeModel"**
- **Solution**: Enable Bedrock model access in AWS Console → Bedrock → Model access

**Error: "NoSuchBucket"**
- **Solution**: Ensure `USE_S3=true` and `S3_BUCKET` is set correctly in Lambda environment variables

**Error: "Module not found: mangum"**
- **Solution**: Rebuild Lambda package: `cd backend && uv run deploy.py`

### Terraform Issues

**Error: "Backend configuration changed"**
```bash
cd terraform
terraform init -reconfigure
```

**Error: "Resource already exists"**
```bash
# Import existing resource
terraform import aws_s3_bucket.memory bucket-name
```

### Frontend Issues

**Build Error: "Cannot find module"**
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```

## 💰 Cost Estimation

Approximate monthly AWS costs for low-medium usage:

| Service | Usage | Est. Cost |
|---------|-------|-----------|
| Lambda | 100K requests/month | $0.20 |
| API Gateway | 100K requests/month | $0.35 |
| S3 | 5 GB storage + requests | $0.15 |
| CloudFront | 10 GB data transfer | $0.85 |
| Bedrock (Nova Lite) | 1M input + 100K output tokens | $0.20 |
| **Total** | | **~$2/month** |

**Note:** Actual costs depend on usage. AWS Free Tier may cover initial usage.

## 🎓 Use Cases

- **Professional AI Assistant** - Represent yourself with an AI twin
- **Portfolio Project** - Showcase AI/Cloud engineering skills
- **Learning Platform** - Study serverless architecture and AI integration
- **Customer Support** - Template for AI-powered support bot
- **Interview Tool** - Let recruiters interact with your AI twin

## 📚 Learning Resources

This project demonstrates:
- ✅ AWS Serverless architecture (Lambda, API Gateway, S3)
- ✅ Infrastructure as Code with Terraform
- ✅ AWS Bedrock AI integration
- ✅ Modern React/Next.js development
- ✅ FastAPI backend design
- ✅ CI/CD automation with shell scripts
- ✅ Multi-environment deployment strategies

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Luis Elizaga**
- GitHub: [@LuisElizaga](https://github.com/LuisElizaga)
- Location: Bogotá, Colombia / Madrid, Spain
- Role: AI & Cloud Platform Specialist

## 🙏 Acknowledgments

- Built as part of an AI in Production course
- Powered by AWS Bedrock (Amazon Nova)
- UI inspired by modern chat interfaces
- Infrastructure automated with Terraform

---

**⭐ If you find this project useful, please give it a star!**

## 🔗 Related Projects

- [AWS Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest)
- [Next.js Documentation](https://nextjs.org/docs)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [AWS Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
