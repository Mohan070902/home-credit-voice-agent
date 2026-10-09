# Step-by-Step Voice Platform Setup Guide
## Implementing Your Home Credit Voice Agent on Retell AI or Bolna

This guide walks you through deploying your System Prompt onto a live voice platform (**Retell AI** or **Bolna**), performing test calls, and generating the public shareable link for your assignment submission.

---

## Option 1: Retell AI (Recommended — Fastest & Best Audio Quality)

Retell AI offers a free trial with complimentary call minutes, web-call testing directly from your browser, and built-in public share links.

### Step 1: Create a Free Account
1. Visit [https://www.retellai.com](https://www.retellai.com) and click **Start for Free** / **Sign Up**.
2. Sign in with Google or your email.

### Step 2: Create a New Voice Agent
1. From the Retell dashboard, navigate to **Agents** in the left sidebar.
2. Click **Create New Agent** (or "+ New Agent").
3. Choose **Single-Prompt Agent** (or LLM-powered agent).
4. Set the Model: Select **GPT-4o**, **GPT-4o-mini**, or **Claude 3.5 Sonnet**. (GPT-4o or GPT-4o-mini is ideal for fast voice response).

### Step 3: Configure Agent Settings & Voice
1. **Agent Name**: `Home Credit LAP Qualifier`
2. **Voice Selection**:
   - For English: Choose an Indian English or warm professional voice (e.g., ElevenLabs / Cartesia voices with neutral or Indian accents like *Kavita*, *Aditi*, or *Priya*).
   - For Hindi: Select a Hindi-supported voice (e.g., ElevenLabs Hindi or Cartesia multi-lingual).
3. **Ambient Sound / Responsiveness**:
   - Turn-taking sensitivity: Default (or 0.6–0.7).
   - Enable Backchanneling: Optional (adds natural "mhm", "I see").

### Step 4: Paste Your System Prompt
1. In the **System Prompt / Instructions** field, paste the full prompt from:
   `SYSTEM_PROMPT_PLAIN.txt` (or copy from `SYSTEM_PROMPT.md`).
2. Populate the default variables in the Retell variable manager or at the top of the prompt:
   - `company_name`: Home Credit
   - `agent_name`: Priya
   - `agent_gender`: Female
   - `customer_name`: Rahul Verma
   - `language_to_speak`: English
   - `additional_context_from_rag`: "Home Credit is a leading non-banking financial company (NBFC) in India offering flexible personal and property loans. Loan Against Property allows customers to mortgage residential, commercial, or industrial properties at competitive rates with transparent processing."
3. **First Utterance / Greeting**:
   Set the opening greeting:
   > *"Hello, good day! Am I speaking with Rahul Verma?"*

### Step 5: Test the Agent via Web Call
1. Click **Test Agent** / **Web Call** (microphone button at the top right of Retell dashboard).
2. Allow browser microphone access.
3. Speak with the agent acting as different customer personas using the scripts in `TESTING_MATRIX_AND_SCRIPTS.md`:
   - **Call 1**: Golden path (Residential, Sole owner, Original docs ready, ₹45L, Salaried bank credit, 10 years) $\rightarrow$ Verify Senior Expert handoff.
   - **Call 2**: Disqualification (Mention Agricultural land or Cash income) $\rightarrow$ Verify immediate polite termination.
   - **Call 3**: Existing Loan (Say "I already have a loan on this property") $\rightarrow$ Verify Transfer Specialist routing.
   - **Call 4**: Over-limit amount (Say "I need 1.2 Crores") $\rightarrow$ Verify the 75 Lakhs alternative proposal.

### Step 6: Obtain Public Links for Submission
1. Go to **Call History** / **Call Logs** in the Retell dashboard.
2. Select your completed test call.
3. You will see:
   - Audio player with the call recording
   - Full transcript with timestamps
   - Latency and turn analysis
4. Click **Share** or **Copy Public Link** (ensure link visibility is set to Public / Anyone with link).
5. Copy this URL — this is the **Public Link to Call Recording & Call Logs** required for your submission!

---

## Option 2: Bolna (Great for Indian Accents & Hindi/Hinglish)

Bolna ([https://www.bolna.dev](https://www.bolna.dev)) is designed specifically for conversational voice AI in India and offers multi-lingual support (Hindi, Tamil, English).

### Step 1: Sign Up on Bolna
1. Visit [https://www.bolna.dev](https://www.bolna.dev).
2. Create your account and open the Bolna Studio dashboard.

### Step 2: Create Agent
1. Click **Create Agent**.
2. Select **Inbound / Outbound Voice Agent**.
3. Set LLM provider: **OpenAI (GPT-4o-mini)** or **Groq / DeepSeek**.
4. Set Transcriber (STT): **Deepgram (Nova-2 Indian English/Hindi)**.
5. Set Synthesizer (TTS): **Cartesia** or **ElevenLabs** (Indian English or Hindi voice).

### Step 3: Insert Prompt & Variables
1. Paste the prompt from `SYSTEM_PROMPT_PLAIN.txt`.
2. Configure parameters:
   - Initial message: *"Hello, good day! Am I speaking with {{customer_name}}?"*
   - Context variables: `customer_name: Rahul Verma`, `company_name: Home Credit`, `agent_name: Priya`.

### Step 4: Run Test Calls & Get Logs
1. Click **Try Agent** (Web Call).
2. Conduct the required test calls.
3. Open **Executions / Call Logs**, click your call, and grab the shareable recording & transcript URL.

---

## 3. Checklist for Final Submission

- [x] Master System Prompt text (`SYSTEM_PROMPT.md` or `SYSTEM_PROMPT_PLAIN.txt`)
- [x] Architectural & Technical Approach (`ARCHITECTURE_AND_APPROACH.md`)
- [x] Multi-scenario Testing Matrix & Dialogs (`TESTING_MATRIX_AND_SCRIPTS.md`)
- [ ] Active voice agent deployed on Retell AI or Bolna
- [ ] At least 1–3 completed test calls recorded
- [ ] Public shareable links to Call Recording & Call Logs copied
