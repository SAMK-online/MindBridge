# 🚂 Railway Deployment Guide

Complete guide to deploy MindBridge backend API to Railway.

---

## 🎯 Overview

**What we're deploying:**
- FastAPI backend (`voice_api.py`)
- All agents (Intake, Crisis, Resource, Habit)
- LangGraph workflows
- NVIDIA Nemotron integration

**What's NOT included (deployed separately):**
- Frontend UI → Deploy to Vercel

---

## 📋 Prerequisites

Before deploying to Railway, ensure you have:

✅ **GitHub Account** (repository already set up)
✅ **Railway Account** - Sign up at [railway.app](https://railway.app)
✅ **API Keys:**
   - OpenRouter API Key (for NVIDIA Nemotron)
   - Tavily API Key (for web search)
   - ElevenLabs API Key (optional, for voice)

---

## 🚀 Deployment Steps

### **Step 1: Create New Railway Project**

1. **Visit Railway Dashboard**
   - Go to: https://railway.app/new
   - Sign in with GitHub

2. **Deploy from GitHub**
   - Click "Deploy from GitHub repo"
   - Select: `SAMK-online/MindBridge`
   - Railway will auto-detect Python project

3. **Configure Root Directory**
   - Railway will use the repository root (correct!)
   - No need to change directory

---

### **Step 2: Add Environment Variables**

In Railway dashboard, go to **Variables** tab and add:

```env
# Required - NVIDIA Nemotron Access
OPENROUTER_API_KEY=your_openrouter_key_here

# Required - Web Search
TAVILY_API_KEY=your_tavily_key_here

# Optional - Text-to-Speech
ELEVENLABS_API_KEY=your_elevenlabs_key_here

# Optional - Port (Railway sets this automatically)
PORT=8000
```

**Important Notes:**
- Railway automatically sets `$PORT` - don't hardcode it
- ElevenLabs is optional (voice features won't work without it)
- Never commit these keys to GitHub

---

### **Step 3: Deploy Configuration (Already Done! ✅)**

Your repository already includes:

✅ **Procfile**
```
web: uvicorn voice_api:app --host 0.0.0.0 --port $PORT
```

✅ **railway.toml**
```toml
[build]
builder = "NIXPACKS"
buildCommand = "pip install -r requirements.txt"

[deploy]
startCommand = "uvicorn voice_api:app --host 0.0.0.0 --port $PORT"
```

✅ **requirements.txt** - All dependencies included

---

### **Step 4: Deploy!**

1. **Trigger Deployment**
   - Railway auto-deploys on push to `main`
   - Or click "Deploy" in Railway dashboard

2. **Monitor Build**
   - Watch logs in real-time
   - Build takes ~2-3 minutes
   - Look for: ✅ "Build successful"

3. **Wait for Deployment**
   - Deployment takes ~1 minute
   - Look for: ✅ "Deployment successful"

4. **Get Your URL**
   - Railway provides: `https://your-project.up.railway.app`
   - Copy this URL - you'll need it for frontend!

---

### **Step 5: Test Your Deployment**

1. **Health Check**
   ```bash
   curl https://your-project.up.railway.app/
   ```
   Should return: `{"message": "MindBridge Voice API"}`

2. **Test Intake Endpoint**
   ```bash
   curl -X POST https://your-project.up.railway.app/voice/intake \
     -H "Content-Type: application/json" \
     -d '{
       "user_id": "test123",
       "message": "I need help with anxiety",
       "session_id": "test_session"
     }'
   ```

3. **Check Logs**
   - In Railway dashboard → Deployments → Logs
   - Should see: Agent processing messages

---

## 🔗 Connect Frontend to Backend

### **Update Vercel Environment Variables**

1. **Go to Vercel Dashboard**
   - Project: MindBridge
   - Settings → Environment Variables

2. **Add Production Variable**
   ```
   Name: VITE_API_URL
   Value: https://your-project.up.railway.app
   Environment: Production
   ```

3. **Redeploy Frontend**
   - Vercel will auto-redeploy
   - Or manually trigger redeploy

### **Test Local Development with Railway Backend**

Create `ui/.env.local`:
```env
VITE_API_URL=https://your-project.up.railway.app
```

Then:
```bash
cd ui
npm run dev
```

Your local frontend will now use the Railway backend!

---

## 🛠️ Common Issues & Solutions

### **Issue: Build Fails - "ModuleNotFoundError"**

**Cause:** Missing dependency in `requirements.txt`

**Solution:**
```bash
# Add missing package to requirements.txt
pip freeze | grep package-name >> requirements.txt
git add requirements.txt
git commit -m "Add missing dependency"
git push
```

---

### **Issue: "Port 8000 already in use"**

**Cause:** Railway sets `$PORT` dynamically

**Solution:** Already fixed in Procfile!
```
web: uvicorn voice_api:app --host 0.0.0.0 --port $PORT
```

---

### **Issue: CORS Errors**

**Cause:** Frontend origin not allowed

**Solution:** Update `voice_api.py` CORS settings:
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:5173",           # Local dev
        "https://mindbridge.vercel.app",    # Production
        "https://*.vercel.app"              # Vercel preview
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

### **Issue: API Returns 500 Error**

**Cause:** Missing environment variables

**Solution:**
1. Check Railway Variables tab
2. Ensure `OPENROUTER_API_KEY` and `TAVILY_API_KEY` are set
3. Redeploy after adding variables

---

## 📊 Monitoring & Logs

### **View Real-time Logs**
```
Railway Dashboard → Your Project → Deployments → Logs
```

You'll see:
- Agent processing: `[INTAKE AGENT] Processing message...`
- Crisis detection: `[CRISIS AGENT] Risk level: LOW`
- Therapist matching: `[RESOURCE AGENT] Found 3 matches`

### **Check Metrics**
```
Railway Dashboard → Metrics
```

Monitor:
- CPU usage
- Memory usage
- Request count
- Response time

---

## 💰 Cost Optimization

Railway offers:
- **Free Tier**: $5 credit/month (hobby projects)
- **Pro Tier**: $5/month + usage

**Tips to stay in free tier:**
1. Use Railway sleep mode (auto-sleep after 30min inactivity)
2. Deploy only when needed (demo/hackathon)
3. Monitor usage in dashboard

---

## 🔄 Update Deployment

### **Automatic Updates (Recommended)**

Railway auto-deploys when you push to `main`:

```bash
# Make changes locally
git add .
git commit -m "Update agents"
git push origin main

# Railway deploys automatically!
```

### **Manual Redeploy**

In Railway dashboard:
- Click "Redeploy" button
- Deploys latest commit from GitHub

---

## 🎯 Production Checklist

Before going live:

- [ ] All environment variables set in Railway
- [ ] API endpoint tested with curl/Postman
- [ ] CORS configured for production domain
- [ ] Frontend connected to Railway URL
- [ ] Logs show successful agent processing
- [ ] Error monitoring set up (optional: Sentry)
- [ ] Rate limiting implemented (optional)

---

## 🆘 Support Resources

**Railway Documentation:**
- Docs: https://docs.railway.app
- Discord: https://discord.gg/railway
- Status: https://status.railway.app

**MindBridge Issues:**
- GitHub Issues: https://github.com/SAMK-online/MindBridge/issues

---

## 🎉 Success!

Once deployed, your backend will be available at:
```
https://your-project.up.railway.app
```

**Test the full stack:**
1. Frontend: `https://mindbridge.vercel.app`
2. Backend: `https://your-project.up.railway.app`
3. Nima AI is now live! 🎊

---

**Built with ❤️ for NVIDIA Nemotron Hackathon**
