# API Keys & Environment Variables Quick Reference

| Variable | Required | Used By | Example | Notes |
|----------|----------|---------|---------|-------|
| **GROQ_API_KEY** | ✅ YES | Backend, Python ML | `gsk_xxxxx...` | Get from [console.groq.com](https://console.groq.com) |
| **GROQ_MODEL** | ✅ YES | Backend, Python ML | `openai/gpt-oss-20b` | LLM model for AI responses |
| **PYTHON_BACKEND_URL** | ✅ YES | Backend | `http://localhost:8000` | Genomic analysis service URL |
| **PYTHON_VALIDATOR_URL** | ⚠️ OPTIONAL | Backend | `http://localhost:8001` | VCF validation service |
| **VITE_API_URL** | ⚠️ OPTIONAL | Frontend | `https://backend.onrender.com` | Leave empty for local dev |
| **CORS_ORIGIN** | ✅ YES | Backend | `http://localhost:5173,https://app.com` | Frontend URLs allowed |
| **PORT** | ⚠️ OPTIONAL | Backend | `5000` | Default: 5000 |
| **FORWARD_TIMEOUT_MS** | ⚠️ OPTIONAL | Backend | `10000` | Default: 10000ms |
| **FORWARD_RETRIES** | ⚠️ OPTIONAL | Backend | `2` | Default: 2 |
| **MAX_CONTENT_LENGTH** | ⚠️ OPTIONAL | Python ML | `5242880` | File upload limit (bytes) |
| **BACKEND_URL** | ⚠️ OPTIONAL | Frontend Dev | `http://localhost:5000` | Vite proxy target |

---

## 🎯 Minimum Required for Render Deployment

| Variable | Value |
|----------|-------|
| **GROQ_API_KEY** | Your Groq API key |
| **GROQ_MODEL** | `openai/gpt-oss-20b` |
| **PYTHON_BACKEND_URL** | `https://your-python-backend.onrender.com` |
| **CORS_ORIGIN** | `https://your-frontend.vercel.app` |

---

## 📍 Where to Set Each Variable

### Local Development (.env files)

**Backend (`backend/.env`):**
```bash
GROQ_API_KEY=your-key-here
GROQ_MODEL=openai/gpt-oss-20b
PYTHON_BACKEND_URL=http://localhost:8000
PYTHON_VALIDATOR_URL=http://localhost:8001
CORS_ORIGIN=http://localhost:5173,http://localhost:3000
PORT=5000
FORWARD_TIMEOUT_MS=10000
FORWARD_RETRIES=2
```

**Frontend (`frontend/.env.local`):**
```bash
VITE_API_URL=
BACKEND_URL=http://localhost:5000
```

### Render Deployment

**Web Service Environment Variables:**
1. Go to Render dashboard
2. Select your service
3. Go to Environment tab
4. Add all variables listed above

### Vercel Frontend Deployment

**Project Settings → Environment Variables:**
```
VITE_API_URL=https://pharmaguard-backend.onrender.com
```

---

## 🔄 Service URLs Mapping

```
┌─────────────────────────────────────┐
│         Browser (Frontend)           │
│      (deploy on Vercel)              │
└──────────────┬──────────────────────┘
               │ (VITE_API_URL)
               ▼
┌─────────────────────────────────────┐
│      Express Backend                 │
│   (pharmaGuard-backend on Render)    │
│   Port: 5000                         │
└──────────────┬──────────────────────┘
               │ (PYTHON_BACKEND_URL)
               ▼
┌─────────────────────────────────────┐
│      Python ML Service               │
│   (pharmaGuard-ml on Render/Heroku)  │
│   Port: 8000                         │
└─────────────────────────────────────┘
               │
               │ (GROQ_API_KEY)
               ▼
       ┌───────────────┐
       │  Groq API     │
       │  LLM Service  │
       └───────────────┘
```

---

## ✅ Setup Checklist

### Before Local Development
- [ ] Create `.env` file in `backend/` from `.env.example`
- [ ] Create `.env.local` in `frontend/` (optional)
- [ ] Get GROQ_API_KEY from Groq Console
- [ ] Set PYTHON_BACKEND_URL to your local Python service

### Before Render Deployment
- [ ] Get GROQ_API_KEY from Groq Console
- [ ] Deploy Python ML service, note its URL
- [ ] Add all environment variables to Render service
- [ ] Test endpoints after deployment

### Before Vercel Deployment
- [ ] Update VITE_API_URL to Render backend URL
- [ ] Deploy frontend
- [ ] Verify API calls work

---

## ⚡ Quick Start

```bash
# 1. Clone repo
git clone https://github.com/Deepika14145/Pharma.git
cd Pharma

# 2. Setup backend
cd backend
npm install
cp ../.env.example .env
# Edit .env with your GROQ_API_KEY and service URLs

# 3. Setup frontend
cd ../frontend
npm install
# Optional: create .env.local if deploying to different backend

# 4. Run services
# Terminal 1: Python ML
cd pharma_ml
python app.py

# Terminal 2: Backend
cd backend
npm start

# Terminal 3: Frontend
cd frontend
npm run dev
```

---

## 🚨 Troubleshooting

**API keys not working?**
1. Verify key format: `gsk_...` for Groq
2. Check key is not expired
3. Verify key has correct permissions
4. Try regenerating key

**Services can't communicate?**
1. Check all URLs are correct and accessible
2. Verify CORS_ORIGIN includes all frontend URLs
3. Check firewall/network permissions
4. Verify service is running on expected port

**Frontend can't reach backend?**
1. Check VITE_API_URL is set correctly for production
2. Verify backend is deployed and running
3. Check CORS_ORIGIN includes frontend domain
4. Open browser console for CORS errors
