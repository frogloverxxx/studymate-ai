# StudyMate AI — Deployment Guide

**Global Certification #4 | AI Chatbot with Bring-Your-Own Model**

This project uses the same Azure OpenAI setup as a standard chatbot, but is architectured so the AI model backend can be swapped (the "Bring-Your-Own Model" concept).

---

## Setup Steps (Same Azure OpenAI process)

### Step 1 — Create Azure OpenAI Resource
1. Go to [portal.azure.com](https://portal.azure.com), sign in with your student email.
2. Search **"Azure OpenAI"** → click **Create**.
3. Fill in: Subscription = Azure for Students, Resource group = `gc-chatbot-rg`, Name = something unique like `studymate-ai`, Pricing = Standard S0.
4. Click **Review + Create** → **Create**. Wait 1–2 min.

### Step 2 — Deploy a Model
1. Go to your OpenAI resource → click **"Go to Azure OpenAI Studio"**.
2. Go to **Deployments** → **+ Create new deployment**.
3. Choose `gpt-35-turbo` (or `gpt-4o-mini`), name it `gpt-35-turbo`.
4. Click **Create**.

### Step 3 — Get Your Keys
1. Back in Azure Portal → your OpenAI resource → **Keys and Endpoint**.
2. Copy the **Endpoint** and **KEY 1**.

### Step 4 — Configure the Chatbot
1. Open `index.html` in a text editor.
2. Find the config section and paste your values:
   ```javascript
   const AZURE_ENDPOINT  = "https://studymate-ai.openai.azure.com";
   const API_KEY         = "your-key-here";
   const DEPLOYMENT_NAME = "gpt-35-turbo";
   ```
3. Save and open in browser to test.

### Step 5 — Deploy to Azure Static Web Apps
1. Push `index.html` to a GitHub repo called `studymate-ai`.
2. In Azure Portal → search **Static Web Apps** → **Create**.
3. Connect your GitHub repo, set App location to `/`, leave others blank.
4. Click **Create**. Your live URL appears in 2–3 minutes.

---

## "Bring-Your-Own Model" Architecture

This chatbot separates the AI call into a `callModel()` function that can swap between backends:

| Model Option | Status | How It Works |
|-------------|--------|--------------|
| **Azure OpenAI** | Active | Calls Azure OpenAI REST API (current) |
| **Local Model** | Placeholder | Would connect to Ollama/LM Studio on localhost |
| **OpenAI Direct** | Placeholder | Would call OpenAI API directly with a different key |

This design means after Azure credits expire, the bot can switch to a free local model without changing the UI or chat logic — only the `callModel()` function changes.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| 401 error | Wrong API key |
| 404 error | Wrong endpoint or deployment name |
| 429 error | Rate limited, wait a minute |
| Model selector disabled options | Those are placeholders showing the BYOM architecture |
