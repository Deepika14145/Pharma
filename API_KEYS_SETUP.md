# PharmaGuard API Keys & Environment Setup Guide

This document lists all API keys and environment variables required to run PharmaGuard in development and production.

## 📋 API Keys Required

### 1. **GROQ_API_KEY** (Required for AI Chat)
**Used By:** Backend (`backend/routes/chat.js`), Python ML (`pharma_ml/pipeline.py`)

**What It Is:** API key for Groq's LLM API - provides AI-powered explanations and clinical recommendations

**How to Get:**
1. Go to [Groq Console](https://console.groq.com)
2. Sign up or log in with your account
3. Navigate to API Keys section
4. Create a new API key
5. Copy the key (it won't be shown again)

**Configuration:**
```bash
GROQ_API_KEY=your-groq-api-key-here
```

**Models Available:**
- `openai/gpt-oss-20b` (recommended, fastest)
- `gemma-7b-it`
- `llama2-70b`
- `mixtral-8x7b-32768`

---

## 🔗 Service URLs Required

### 2. **PYTHON_BACKEND_URL** (Required for VCF Analysis)
**Used By:** Backend (`backend/routes/analyze.js`)

**What It Is:** URL endpoint of the Python ML service that performs genomic analysis

**Local Development:**
```bash
PYTHON_BACKEND_URL=http://localhost:8000
```

**Production Options:**
- **Heroku:** `https://your-app-name.herokuapp.com`
- **Render:** `https://your-app-name.onrender.com`
- **Railway:** `https://your-app-name.up.railway.app`

**Expected Endpoint:** POST `/analyze` - accepts VCF file and returns drug-gene interaction analysis

---

### 3. **PYTHON_VALIDATOR_URL** (Optional for VCF Validation)
**Used By:** Backend (`backend/routes/analyze.js`)

**What It Is:** URL endpoint of the Python validator service for pre-validation of VCF files

**Local Development:**
```bash
PYTHON_VALIDATOR_URL=http://localhost:8001
```

**Note:** If not set, validation is performed by main Python backend

---

### 4. **VITE_API_URL** (Frontend API URL)
**Used By:** Frontend (`frontend/src/services/api.js`)

**What It Is:** Backend API endpoint that the frontend connects to

**Local Development (leave empty):**
```bash
VITE_API_URL=
```
Uses Vite proxy to `http://localhost:5000`

**Production:**
```bash
VITE_API_URL=https://pharmaguard-backend.onrender.com
```

---

## ⚙️ Server Configuration Variables

### 5. **CORS_ORIGIN** (CORS Configuration)
**Used By:** Backend (`backend/server.js`)

**What It Is:** Comma-separated list of allowed frontend origins

**Format:**
```bash
CORS_ORIGIN=http://localhost:5173,http://localhost:3000,https://myapp.vercel.app
```

---

### 6. **GROQ_MODEL** (LLM Model Selection)
**Used By:** Backend, Python ML

**Available Models:**
```bash
GROQ_MODEL=openai/gpt-oss-20b
```

Other options:
- `gemma-7b-it` - Fast, good for summaries
- `llama2-70b` - High quality responses
- `mixtral-8x7b-32768` - Balanced quality/speed

---

### 7. **PORT** (Backend Server Port)
**Used By:** Backend

**Default:**
```bash
PORT=5000
```

---

### 8. **FORWARD_TIMEOUT_MS** (Request Timeout)
**Used By:** Backend (`backend/routes/analyze.js`)

**Default:** 10000ms (10 seconds)

```bash
FORWARD_TIMEOUT_MS=10000
```

---

### 9. **FORWARD_RETRIES** (Request Retries)
**Used By:** Backend

**Default:** 2 retries

```bash
FORWARD_RETRIES=2
```

---

### 10. **MAX_CONTENT_LENGTH** (File Upload Size)
**Used By:** Python ML (`pharma_ml/app.py`)

**Default:** 5242880 bytes (5 MB)

```bash
MAX_CONTENT_LENGTH=5242880
```

---

## 📝 Environment File Examples

### Development (.env or .env.local)

**Frontend:**
```bash
VITE_API_URL=
BACKEND_URL=http://localhost:5000
```

**Backend:**
```bash
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
GROQ_MODEL=openai/gpt-oss-20b
PYTHON_BACKEND_URL=http://localhost:8000
PYTHON_VALIDATOR_URL=http://localhost:8001
CORS_ORIGIN=http://localhost:5173,http://localhost:3000
PORT=5000
FORWARD_TIMEOUT_MS=10000
FORWARD_RETRIES=2
```

**Python ML (.env in pharma_ml/):**
```bash
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
MAX_CONTENT_LENGTH=5242880
FLASK_ENV=development
```

---

### Production (Render Deployment)

**Set in Render Dashboard → Environment Variables:**

```
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
GROQ_MODEL=openai/gpt-oss-20b
PYTHON_BACKEND_URL=https://your-python-backend.onrender.com
PYTHON_VALIDATOR_URL=https://your-python-validator.onrender.com
CORS_ORIGIN=https://my-frontend.vercel.app,https://www.myapp.com
PORT=5000
FORWARD_TIMEOUT_MS=10000
FORWARD_RETRIES=2
VITE_API_URL=https://pharmaguard-backend.onrender.com
```

---

## 🚀 Deployment Steps

### 1. Deploy Python ML Backend (Render)

1. Create new Web Service on Render
2. Connect your GitHub repository
3. Build Command: `pip install -r pharma_ml/requirements.txt`
4. Start Command: `python -m flask --app pharma_ml/app run --host=0.0.0.0`
5. Set Environment Variables (same as development)
6. Deploy

**Note the URL:** `https://your-python-backend.onrender.com`

---

### 2. Deploy Express Backend (Render)

1. Create new Web Service on Render
2. Build Command: `cd backend && npm install`
3. Start Command: `cd backend && npm start`
4. Set Environment Variables:
   - `GROQ_API_KEY=...`
   - `PYTHON_BACKEND_URL=https://your-python-backend.onrender.com`
   - `PYTHON_VALIDATOR_URL=...` (if needed)
   - `CORS_ORIGIN=...`

**Note the URL:** `https://pharmaguard-backend.onrender.com`

---

### 3. Deploy Frontend (Vercel)

1. Connect repository to Vercel
2. Framework: Vite
3. Build Command: `cd frontend && npm run build`
4. Output Directory: `frontend/dist`
5. Environment Variable:
   - `VITE_API_URL=https://pharmaguard-backend.onrender.com`

---

## 🔒 Security Best Practices

✅ **DO:**
- Use `.env.example` with placeholder values (committed to git)
- Store actual secrets in platform environment variables
- Rotate API keys regularly
- Use different keys for dev/staging/production

❌ **DON'T:**
- Commit `.env` file with real keys to git
- Use same API key across all environments
- Share API keys in chat/documentation
- Expose keys in client-side code

---

## ✅ Verification Checklist

Before deployment, verify:

- [ ] GROQ_API_KEY is valid and active
- [ ] PYTHON_BACKEND_URL is accessible and running
- [ ] CORS_ORIGIN includes your frontend URL
- [ ] GROQ_MODEL exists on Groq platform
- [ ] All required variables are set in deployment platform
- [ ] `.env` file is in `.gitignore` (don't commit real secrets)
- [ ] No API keys hardcoded in source code

---

## 🆘 Troubleshooting

**Chat not working?**
- Verify GROQ_API_KEY is set and valid
- Check Groq API status at [console.groq.com](https://console.groq.com)
- Verify GROQ_MODEL is correct

**Analysis not working?**
- Verify PYTHON_BACKEND_URL is correct and service is running
- Check backend logs: `npm logs` (Render) or `heroku logs` (Heroku)
- Test endpoint: `curl PYTHON_BACKEND_URL/analyze`

**Frontend can't connect to backend?**
- Verify VITE_API_URL in production or proxy in dev
- Check CORS_ORIGIN includes frontend domain
- Check browser console for CORS errors

---

## 📚 References

- **Groq Console:** https://console.groq.com/keys
- **Groq Models:** https://docs.groq.com/latest/models
- **Render Docs:** https://render.com/docs
- **Vercel Docs:** https://vercel.com/docs
