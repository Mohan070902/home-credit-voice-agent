# SalesAgents AI — AI Voice Agent Design Assignment
### Home Credit Loan Against Property (LAP) Qualification System

**Candidate Role**: AI Intern / Prompt Engineer  
**Usecase**: Home Credit Loan Against Property (LAP) Voice Agent Qualification  
**Target Platform**: Retell AI / Bolna / Vapi

---

## 📂 Submission Repository Structure

| File | Description | Link |
| :--- | :--- | :--- |
| **`SYSTEM_PROMPT.md`** | Master System Prompt formatted with markdown & runtime variables | [View System Prompt](SYSTEM_PROMPT.md) |
| **`SYSTEM_PROMPT_PLAIN.txt`** | Pure plain-text version ready for direct copy-paste into Voice Platforms (Retell, Bolna, etc.) | [View Plain Text](SYSTEM_PROMPT_PLAIN.txt) |
| **`ARCHITECTURE_AND_APPROACH.md`** | Complete 5-layer system architecture, state machine, First Missing Question algorithm & business logic | [View Architecture](ARCHITECTURE_AND_APPROACH.md) |
| **`TESTING_MATRIX_AND_SCRIPTS.md`** | 15 real-world dialog scripts & test scenarios (Golden path, Agri, Cash, Joint, >75L, Diversions, Hindi) | [View Testing Matrix](TESTING_MATRIX_AND_SCRIPTS.md) |
| **`VOICE_PLATFORM_SETUP_GUIDE.md`** | Step-by-step setup guide for Retell AI & Bolna to test live and generate public call links | [View Platform Guide](VOICE_PLATFORM_SETUP_GUIDE.md) |

---

## 🎯 Quick Highlights of Solution

1. **5-Layer Architecture**: Conversation Layer $\rightarrow$ Business Rules $\rightarrow$ State/Memory $\rightarrow$ Decision Routing $\rightarrow$ Action Handoff.
2. **First Missing Question Algorithm**: Parses multi-entity responses out-of-order without re-asking questions or breaking conversation flow.
3. **Immediate Disqualification Engine**: Instantly exits without asking further questions on Agricultural land, Cash income, Missing original documents, or Inflexible tenure.
4. **Loan Transfer Switch**: Automatically routes existing mortgage loans and EMI-reduction requests to dedicated transfer specialists.
5. **Over-Limit Handling**: Intelligently offers ₹75 Lakhs cap if customer asks for more, cleanly branching based on their acceptance.
6. **Strict Handoff Gating**: Zero premature qualifications — senior human loan experts receive only 100% compliant leads.
7. **Voice-First Design**: Tuned for short, natural, conversational turns with interruption tolerance and strict gender agreement in English & Hindi.
