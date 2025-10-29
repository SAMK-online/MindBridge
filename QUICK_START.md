# 🚀 Quick Start Guide

Get MindBridge running in 5 minutes!

---

## 🎯 Two Deployment Options

### **Option 1: Full Production Deployment** (Recommended for demos)
- **Frontend**: Vercel (Free)
- **Backend**: Railway (Free tier)
- **Total Time**: ~10 minutes
- **Total Cost**: $0

### **Option 2: Local Development**
- Run everything on your machine
- **Total Time**: ~5 minutes
- **Cost**: $0

---

## 🌐 Option 1: Production Deployment

### **Step 1: Deploy Backend to Railway** (3 minutes)

1. Go to [railway.app/new](https://railway.app/new)
2. Click "Deploy from GitHub repo"
3. Select `SAMK-online/MindBridge`
4. Add environment variables:
   ```
   OPENROUTER_API_KEY=your_key_here
   TAVILY_API_KEY=your_key_here
   ```
5. Wait 2-3 minutes for build
6. Copy your Railway URL: `https://your-project.up.railway.app`

**📖 Detailed guide:** [RAILWAY_DEPLOYMENT.md](./RAILWAY_DEPLOYMENT.md)

---

### **Step 2: Deploy Frontend to Vercel** (3 minutes)

1. Go to [vercel.com/new](https://vercel.com/new)
2. Import: `SAMK-online/MindBridge`
3. **IMPORTANT Settings:**
   - Framework Preset: **Vite**
   - Root Directory: **ui**
   - Build Command: `npm run build`
   - Output Directory: `dist`
4. Add environment variable:
   ```
   VITE_API_URL=https://your-project.up.railway.app
   ```
5. Click "Deploy"
6. Your app is live at: `https://mindbridge-xxx.vercel.app`

---

### **Step 3: Test It!** (1 minute)

1. Open your Vercel URL
2. Click "Talk to Nima AI"
3. Say: "I'm feeling stressed about work"
4. Watch Nima AI respond! 🎉

---

## 💻 Option 2: Local Development

### **Prerequisites**
- Python 3.10+
- Node.js 18+
- Git

### **Step 1: Clone & Setup** (2 minutes)

```bash
# Clone repository
git clone https://github.com/SAMK-online/MindBridge.git
cd MindBridge

# Create .env file
cp .env.example .env

# Edit .env and add your API keys:
# OPENROUTER_API_KEY=your_key_here
# TAVILY_API_KEY=your_key_here
```

---

### **Step 2: Backend Setup** (2 minutes)

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Start backend
python voice_api.py
```

Backend runs at: `http://localhost:8000`

---

### **Step 3: Frontend Setup** (1 minute)

**New terminal:**

```bash
# Navigate to UI
cd ui

# Install dependencies
npm install

# Start dev server
npm run dev
```

Frontend runs at: `http://localhost:5173`

---

### **Step 4: Test It!** (30 seconds)

1. Open `http://localhost:5173`
2. Click "Talk to Nima AI"
3. Say: "I need help with anxiety"
4. Watch the agents work! 🎉

---

## 🔑 Getting API Keys

### **OpenRouter (NVIDIA Nemotron)**
1. Visit: https://openrouter.ai
2. Sign up (free)
3. Go to "Keys" → Create new key
4. Copy key: `sk-or-v1-...`

**Cost:** ~$0.10-0.50 per 100 conversations

---

### **Tavily (Web Search)**
1. Visit: https://tavily.com
2. Sign up (free)
3. Copy API key from dashboard

**Free tier:** 1,000 searches/month

---

### **ElevenLabs (Voice - Optional)**
1. Visit: https://elevenlabs.io
2. Sign up (free)
3. Copy API key from settings

**Free tier:** 10,000 characters/month

---

## 🎨 Demo Mode (No Backend Needed)

Want to see the UI without setting up backend?

```bash
cd ui
npm install
npm run dev
```

**Note:** Voice features won't work, but you can explore the UI!

---

## 🆘 Troubleshooting

### **Backend won't start**
```bash
# Check Python version
python --version  # Should be 3.10+

# Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

---

### **Frontend can't connect to backend**
```bash
# Check backend is running
curl http://localhost:8000

# Check CORS settings in voice_api.py
# Should allow origin: http://localhost:5173
```

---

### **"Module not found" errors**
```bash
# Backend
pip install -r requirements.txt

# Frontend
cd ui && npm install
```

---

## 📚 Next Steps

**After getting it running:**

1. **Explore the Agents**
   - Try different scenarios (anxiety, burnout, crisis)
   - Watch agent reasoning in real-time
   - Test privacy tier selection

2. **Customize It**
   - Add more therapist specializations
   - Modify conversation stages
   - Adjust crisis detection thresholds

3. **Deploy for Real**
   - Follow [RAILWAY_DEPLOYMENT.md](./RAILWAY_DEPLOYMENT.md)
   - Share your live demo!

---

## 🎯 Common Use Cases

### **Demo for Hackathon Judges**
```bash
# Run locally for control
python voice_api.py
cd ui && npm run dev

# Open http://localhost:5173
# Full agent visualization available
```

### **Share with Others**
```bash
# Deploy to Railway + Vercel
# Share your Vercel URL
# Everyone can try Nima AI!
```

### **Development**
```bash
# Run with auto-reload
uvicorn voice_api:app --reload
cd ui && npm run dev

# Make changes, see results instantly
```

---

## 💡 Tips

**Performance:**
- First response takes ~3-5 seconds (LLM initialization)
- Subsequent responses: ~1-2 seconds

**Best Practices:**
- Test with OpenRouter credits before going live
- Monitor Railway usage (free tier: $5/month)
- Use Vercel analytics to track users

**Security:**
- Never commit `.env` file
- Rotate API keys regularly
- Use environment variables in production

---

## 🎉 You're Ready!

Choose your path:
- 🌐 **Production:** Deploy to Railway + Vercel
- 💻 **Local:** Run on your machine
- 🎨 **Demo:** UI-only mode

**Questions?** Open an issue: [GitHub Issues](https://github.com/SAMK-online/MindBridge/issues)

---

**Built with ❤️ using NVIDIA Nemotron**
