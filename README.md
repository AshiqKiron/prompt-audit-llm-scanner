# 🛡️ Prompt Audit Scanner

**An automated security tool to analyze and red-team LLM system prompts for vulnerabilities.**

[🚀 **Try the Live Demo on Hugging Face Spaces**](https://huggingface.co/spaces/ashiquzzaman/prompt-audit-scanner)

---

## 📖 Overview

The **Prompt Audit Scanner** is a specialized web application designed for AI developers, security researchers, and red teamers. It allows users to input a target LLM system prompt and automatically audits it for common vulnerabilities, including:

*   **Prompt Injection:** Susceptibility to user instructions overriding system directives.
*   **Jailbreaking:** Potential to bypass safety filters and alignment guardrails.
*   **Data Leakage:** Risks of the model accidentally revealing backend logic, API keys, or hidden instructions.
*   **Boundary Enforcement:** Evaluation of how strictly the prompt enforces its defined persona and rules.

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend** | Streamlit | Rapid, interactive web UI for prompt input and report visualization. |
| **Backend** | Python | Core logic, API handling, and meta-prompt construction. |
| **LLM Provider** | Groq API | Ultra-fast inference engine for the language model. |
| **Language Model** | Llama 3.3 70B (Versatile) | The core reasoning engine acting as the "Security Auditor". |
| **Deployment** | Hugging Face Spaces | Cloud hosting and continuous deployment. |

---

## 🏗️ Architecture

The application follows a streamlined, three-tier architecture designed for low-latency security auditing:

1.  **Presentation Layer (Streamlit):** 
    *   Captures the target system prompt via a text area.
    *   Handles secure API key injection (via Hugging Face Secrets or manual entry).
    *   Renders the final vulnerability report in real-time.
2.  **Logic Layer (Python Backend):** 
    *   Takes the user's input and injects it into a carefully engineered **Meta-Prompt**. 
    *   This meta-prompt forces the LLM to adopt the persona of an "Expert AI Security Researcher" and defines the exact evaluation criteria (injection, jailbreaks, leakage).
3.  **Inference Layer (Groq API):** 
    *   Receives the meta-prompt and processes it using the Llama 3.3 70B model.
    *   Returns a structured, analytical critique of the target prompt, which is then passed back to the frontend.

---

## 🧠 Why Llama 3.3 70B (via Groq)?

Selecting the right model is critical for an auditing tool. We chose **Llama 3.3 70B** hosted on **Groq** for several strategic reasons:

*   **Superior Reasoning Capabilities:** Prompt injection and jailbreak analysis require deep logical reasoning to understand how a malicious user might twist words. The 70B parameter size provides the nuance required to catch subtle logical flaws that smaller models (like 8B) often miss.
*   **Ultra-Low Latency:** Groq's LPU (Language Processing Unit) inference engine processes the 70B model at lightning speed. What used to take 15-20 seconds on standard cloud GPUs now takes less than 3 seconds, providing a seamless user experience.
*   **Instruction Following:** Llama 3.3 excels at following complex, multi-layered system instructions, ensuring the "Auditor Persona" never breaks character or forgets to evaluate all security criteria.
*   **Cost-Effectiveness:** Compared to proprietary alternatives like GPT-4 or Claude 3.5, Llama 3.3 via Groq offers a highly competitive price-to-performance ratio for high-volume scanning.

---

## ⚖️ Trade-offs Made

Building an LLM-based security tool requires balancing accuracy, speed, and cost. Here are the primary trade-offs made in this project:

1.  **LLM-as-a-Judge vs. Deterministic Rules:** 
    *   *Trade-off:* We rely entirely on LLM reasoning rather than hardcoded regex or rule-based checks. 
    *   *Impact:* While this allows the tool to catch novel, zero-day style logical jailbreaks, it introduces a small risk of "false positives" or hallucinated vulnerabilities compared to deterministic scanners.
2.  **Model Size vs. Speed/Cost:** 
    *   *Trade-off:* We chose the 70B model over the 8B model. 
    *   *Impact:* The 70B model is significantly more accurate at finding edge-case vulnerabilities, but it costs slightly more per token and uses more compute than an 8B model. We prioritized *accuracy* over *maximum speed*.
3.  **Cloud API vs. Local Inference:** 
    *   *Trade-off:* We use the Groq Cloud API instead of hosting the model locally via vLLM or Ollama. 
    *   *Impact:* This removes the need for users to have massive GPU hardware to run the tool, making it highly accessible. However, it introduces a dependency on Groq's uptime, rate limits, and internet connectivity.

---

## 🚀 How to Fork and Use This Project

Want to run it locally or modify the code? Follow these steps:

### 1. Fork and Clone
1. Click the **Fork** button at the top right of this GitHub repository.
2. Clone your forked repository to your local machine:
```
git clone https://github.com/YOUR-USERNAME/prompt-audit-scanner.git
cd prompt-audit-scanner
```

### 2. Set Up the Environment
Create a virtual environment and install the required dependencies:
```
# Create virtual environment
python -m venv venv

# Activate it (Windows)
venv\Scripts\activate

# Activate it (Mac/Linux)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Configure API Keys
You need a Groq API key. You can get one for free at [console.groq.com](https://console.groq.com/).

*   **Option A (Environment Variable):** Set it in your terminal before running:
```
export GROQ_API_KEY="gsk_YOUR_ACTUAL_KEY_HERE"
```
*   **Option B (UI Prompt):** Just run the app, and it will ask you to paste the key securely in the sidebar.

### 4. Run Locally
Launch the Streamlit app:
```
streamlit run app.py
```
Open your browser and navigate to `http://localhost:8501`.

### 5. Deploy to Hugging Face Spaces
1. Create a new **Space** on Hugging Face and select **Streamlit** as the SDK.
2. Connect it to your forked GitHub repository.
3. Go to your Space **Settings** -> **Variables and Secrets**.
4. Add a new secret: Name = `GROQ_API_KEY`, Value = `your_groq_key`.
5. The Space will automatically build and deploy!

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](../../issues) or submit a Pull Request to improve the auditing meta-prompt or UI.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
