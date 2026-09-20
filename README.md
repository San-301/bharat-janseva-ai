# 🇮🇳 Bharat-JanSeva AI

**An Offline-First, Vernacular Government Scheme Navigator & Anti-Fraud Defender for Indian Citizens**

Built for the **WeMakeDevs Bharat Builds Tour — First Commit (Build It Track)** using the **AWS Strands Agents SDK**, **Ollama**, **Streamlit**, and **SQLite**.

---

## 📌 Problem Statement

Over 60% of rural Indian citizens miss out on eligible government welfare benefits due to administrative jargon, language barriers, complex eligibility criteria, and lack of document readiness. Furthermore, vulnerable citizens are frequently targeted by illegal middlemen demanding processing fees (₹500–₹1,000) or requesting OTPs for applications that are officially **100% Free (₹0)**.

## 💡 Solution

**Bharat-JanSeva AI** is an enterprise-grade, local-first welfare assistant that:
1. **Navigates Welfare Eligibility:** Matches citizens to verified government schemes based on land holding (acres/hectares/bigha), occupation, and category.
2. **Defends Against Middleman Scams:** Automatically detects unauthorized fee demands or OTP requests, issues high-contrast red-flag warnings, and logs audit trails into a local SQLite database.
3. **Provides Vernacular Audio Guidance:** Generates native voice audio in **Hindi, Telugu, Tamil, Kannada, and English** for low-literacy accessibility.
4. **Delivers High-Impact UI/UX:** Displays visual document readiness badges, clean status banners, and official `.gov.in` application links without text walls.
5. **Runs 100% Offline & Free:** Executes locally on hardware with zero cloud inference costs or external API dependencies.

## 🏗 System Architecture

```mermaid
graph TD
    UI[ Streamlit Dashboard UI/UX ] --> CACHE[ SQLite Response Cache <0.1s Load ]
    CACHE --> STRANDS[ AWS Strands Agents SDK Loop ]
    STRANDS --> OLLAMA[ Local Ollama Model qwen2.5:0.5b ]
    STRANDS --> TOOLS[ Local Custom Tools ]
    TOOLS --> SEARCH[ @tool search_schemes ]
    TOOLS --> FRAUD[ @tool evaluate_fraud_risk ]
    TOOLS --> DB[( SQLite Engine janseva_data.db )]
    DB --> Q_LOGS[ citizen_queries ]
    DB --> F_LOGS[ fraud_logs Audit Trail ]

## 🚀 Key Features

* **AWS Strands Agents SDK Orchestration:** Function calling and tool routing powered by `from strands import Agent, tool`.
* **Local LLM Inference:** Powered by Ollama running `qwen2.5:0.5b` (~390 MB) locally.
* **Dual SQLite Engine:**
  * `response_cache`: Stores generated responses for lightning-fast (<0.1s) query repeats.
  * `fraud_logs`: Real-time audit trail capturing reported middleman fees, location, and risk level.
* **Vernacular Voice Synthesis:** Offline and native audio generation via `gTTS` and `pyttsx3`.
* **Security & Guardrail System Prompt:** Built-in defenses against prompt injection, jailbreaks, PII leakage (Aadhaar/PIN redaction), and out-of-scope requests.

---

## 📁 Repository Structure


```

bharat-janseva-ai/
├── app.py                 # Core Streamlit application & AWS Strands Agent pipeline
├── schemes_data.json      # Knowledge base covering 7 official government schemes
├── janseva_data.db        # SQLite database for analytics, fraud logs, and response cache
├── requirements.txt       # Project dependencies
└── README.md              # Project documentation

```

---

## 📊 Verified Scheme Knowledge Base

The system includes pre-verified rules, eligibility thresholds, required document checklists, and official links for 7 core Indian schemes:

1. **PM-Kisan Samman Nidhi** (*Agriculture / DBT Cash Support*) — `pmkisan.gov.in`
2. **Pradhan Mantri MUDRA Yojana - Shishu** (*Collateral-Free Micro-Loans*) — `mudra.org.in`
3. **PM Vishwakarma Scheme** (*Artisan Toolkits & Credit*) — `pmvishwakarma.gov.in`
4. **Ayushman Bharat PM-JAY** (*Healthcare Coverage up to ₹5 Lakhs*) — `pmjay.gov.in`
5. **PM Awas Yojana - Gramin** (*Rural Housing Subsidy*) — `pmayg.nic.in`
6. **PM Surya Ghar: Muft Bijli Yojana** (*Solar Rooftop Subsidy*) — `pmsuryaghar.gov.in`
7. **National Social Assistance Programme** (*Pension Support*) — `nsap.nic.in`

---

## ⚙️ Local Setup & Installation

### Prerequisites
* Python 3.10+ installed
* [Ollama](https://ollama.com/) installed and running locally

### 1. Clone the Repository
```bash
git clone [https://github.com/](https://github.com/)<YOUR-USERNAME>/bharat-janseva-ai.git
cd bharat-janseva-ai

```

### 2. Set Up Virtual Environment

```bash
python -m venv venv
# On Windows:
.\venv\Scripts\Activate.ps1
# On macOS/Linux:
source venv/bin/activate

```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
pip install gTTS

```

### 4. Pull the Local Model via Ollama

```bash
ollama pull qwen2.5:0.5b

```

### 5. Launch the Application

```bash
streamlit run app.py

```

Open `http://localhost:8501` in your browser.

---

## 🧪 Test Scenarios

| Scenario | Input Query | Expected Output |
| --- | --- | --- |
| **Agriculture & Scam** | *"I am a small farmer with 2 acres in Nashik. What scheme applies? A local agent asked for ₹500 fee and OTP."* | 🚨 Red Flag Scam Warning<br>

<br>📌 Matched: **PM-Kisan Samman Nidhi** |
| **Micro-Loan** | *"I run a small shop and need a collateral-free business loan."* | ✅ Safe (Free Application)<br>

<br>📌 Matched: **PM MUDRA (Shishu)** |
| **Artisan / Craft** | *"I am a carpenter needing modern tools."* | ✅ Safe (Free Application)<br>

<br>📌 Matched: **PM Vishwakarma** |
| **Out-of-Scope** | *"Write a 500-word essay about cricket in Australia."* | ⚠️ Polite Scope Refusal (*"I am designed exclusively to assist Indian citizens..."*) |

---

## 📜 Hackathon Track Compliance

* **Track:** **Build It Track** (*WeMakeDevs Bharat Builds Tour — First Commit*)
* **AWS Integration:** Official open-source **AWS Strands Agents SDK** (`strands`).
* **Cloud Cost:** **₹0 / $0** (100% local execution using Ollama and SQLite).
* **AI Tool Disclosure:** Built using Cursor IDE, Claude, and ChatGPT.
