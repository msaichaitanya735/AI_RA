# 📘 AI_RA – Complete Usage Guide

Welcome to **AI_RA (AI Resume Automation)** – a complete system that automates resume and cover letter generation using GPT, Google Sheets, LaTeX, and n8n.

---

## 🗂️ 1. Setup Google Sheet

### Required Columns
You must create a Google Sheet with the following **columns in exact order**:

```
RowID | COMPANY | JOB TITLE | JOB DESCRIPTION | Link Of Application | GENERATED COVER LETTER LINK | GENERATED RESUME LINK | STATUS | APPLICATION STATUS | Mission and Values | Skills (Keywords Extracted) | Refined Keywords | General Cover Letter | Personalised Cover letter | Final Cover Letter | Summary | EducationSection | Skills | Experience | Project | Resume
```

This sheet will act as the source of truth and input for the automation pipeline.

---

## 🧠 2. Setup Google Apps Script

### Step-by-Step Instructions:

1. Go to your Google Sheet
2. Click on **Extensions > Apps Script**
3. Paste the code from the `script.gs` file (available in this repo)
4. Save and deploy the project

### Add Triggers (Important!)
You need to add **two triggers** to the script:

- 🔹 **Trigger 1**: OnEdit of `JOB DESCRIPTION`
  - Purpose: Sends updated row data to the **n8n webhook** to start automation

- 🔹 **Trigger 2**: OnEdit of **Final Column** (e.g., `Resume`)
  - Purpose: Triggers the function to compile the final LaTeX code from all returned automation data

To add these:
- Go to Apps Script → Triggers → Add Trigger
- Choose function, set event type to **On Edit**, and save

---

## ⚙️ 3. Setup n8n Automation

### Option A: Use Free Cloud (Trial)
- Sign up for n8n cloud (15-day free trial)

### Option B: Run Locally with Docker
You can self-host n8n using a Docker image that comes with pre-configured setup.

```bash
docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

- Access it via `http://localhost:5678`

Use **Ngrok** to expose a public URL for webhook connection:
```bash
ngrok http 5678
```

Replace the generated `https://xxxx.ngrok.io` in your Apps Script as the webhook URL.

---

## 🔄 4. Import n8n Workflow

- Import `n8n_workflow.json` from this repo into your n8n instance
- This workflow handles:
  - Reading Google Sheet row
  - Sending job description to **OpenAI**
  - Extracting keywords
  - Generating cover letter and LaTeX resume string

---

## 🔐 5. Authentication and LLM Setup in n8n

- Add Google authentication to access your Sheet securely from n8n.
- Add OpenAI or **OpenRouter** credentials.

### About OpenRouter:
[OpenRouter.ai](https://openrouter.ai) is a unified API gateway that lets you choose between many LLM providers. It allows:
- Model switching (e.g., GPT-3.5, Claude, Mixtral, etc.)
- Cost optimization
- Configurable sampling parameters like **temperature** per request

### My Setup:
- Model: **GPT-3.5 Turbo**
- Provider: OpenRouter (via API key)
- Temperature: 0.7 (adjustable)

---

## 🚀 6. Using the Automation

Once everything is setup:

- Edit a row with:
  - `RowID`, `Company`, `Role`, and `Job Description`
- Within **30 seconds**, your:
  - Tailored **resume**
  - ATS-friendly **cover letter** (optional)
  will be generated!

⚠️ Cover letter is kept separate to avoid token cost in jobs where it's not required.

---

## 🤝 Need Help?

Feel free to reach out to me — I’m always happy to help if you get stuck!

— **SaiChaitanya Muthyala**
