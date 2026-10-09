# Comprehensive Testing Matrix & Simulation Scripts
## Home Credit Loan Against Property (LAP) Voice Agent

This document provides a comprehensive verification matrix and turnkey dialog scripts across 15 real-world scenarios. Use these scripts when performing test calls on voice platforms such as **Retell AI**, **Bolna**, or **Vapi**.

---

## 1. Master Testing Matrix

| Test ID | Scenario Name | Primary Test Objective | Expected Terminal Outcome |
| :---: | :--- | :--- | :--- |
| **TC-01** | Golden Path (Fresh LAP) | All 7 criteria valid; standard flow | Senior Loan Expert Handoff |
| **TC-02** | Agricultural Property Disqualification | Immediate exit on ineligible property type | Disqualified Exit (No subsequent Qs) |
| **TC-03** | Cash Income Disqualification | Immediate exit on cash receipt mode | Disqualified Exit (No subsequent Qs) |
| **TC-04** | Missing Original Documents | Immediate exit on photocopies only | Disqualified Exit (No subsequent Qs) |
| **TC-05** | Ineligible Tenure (< 3 or > 15 yrs) | Customer insists on 20 years | Disqualified Exit |
| **TC-06** | Loan > 75 Lakhs (Accepted Cap) | Customer asks 1 Cr, accepts 75L | Continues & Qualifies |
| **TC-07** | Loan > 75 Lakhs (Rejected Cap) | Customer asks 1 Cr, rejects 75L | Polite Exit |
| **TC-08** | Joint Ownership Property | Validates joint ownership is fully allowed | Continues & Qualifies |
| **TC-09** | Existing Property Loan Switch | Customer has ongoing loan | Transfer Specialist Switch |
| **TC-10** | EMI Reduction Request Switch | Customer seeks balance transfer / lower EMI | Transfer Specialist Switch |
| **TC-11** | Busy Customer Flow | Customer driving / in a meeting | Callback Scheduled & Clean Exit |
| **TC-12** | Multi-Slot Out-of-Order Capture | Customer provides 4 data points in one turn | Slots filled; skips asked questions |
| **TC-13** | Mid-Turn Self-Correction | Customer changes tenure mid-sentence | Agent captures latest revised value |
| **TC-14** | Rate Diversion & Interruption | Customer interrupts asking interest rate | Deflects to Senior Expert; resumes flow |
| **TC-15** | Hindi Conversational Flow | Evaluates Hindi fluency & gender grammar | Senior Loan Expert Handoff (Hindi) |

---

## 2. Detailed Conversation Test Scripts

---

### TC-01: Golden Path — Fully Qualified Customer (English)
* **Goal**: Validate complete 7-point flow and clean Senior Loan Expert handoff.
* **Persona**: Rahul Verma, Salaried IT Professional, owning residential apartment.

```text
Agent: Hello, good afternoon, am I speaking with Rahul Verma?
Customer: Yes, speaking. Who is this?
Agent: Hello Mr. Verma, this is Priya calling from Home Credit. How are you doing today?
Customer: I'm good, Priya. What is this regarding?
Agent: As one of our valued customers, you have a pre-approved Loan Against Property offer of up to 75 Lakhs. I'd love to take just two minutes to run a quick eligibility check before connecting you with a senior loan expert. Shall we proceed?
Customer: Sure, let's do it.
Agent: Wonderful! Could you tell me what type of property you own? Is it residential, commercial, or industrial?
Customer: It's a residential apartment.
Agent: Got it. And are you the sole owner of this property, or is it jointly owned?
Customer: I am the sole owner.
Agent: Perfect. Do you have the original property documents ready and available for verification?
Customer: Yes, I have all the original documents at home.
Agent: Great. How much loan amount are you looking to borrow against this property?
Customer: Around 45 Lakhs.
Agent: Understood, 45 Lakhs. Could you share whether you are salaried or self-employed, and is your income received in your bank account or cash?
Customer: I am salaried at an IT firm, and my salary comes directly to my bank account.
Agent: That's great. What would be the approximate current market value of your property?
Customer: It's valued at around 90 Lakhs right now.
Agent: Noted. And over how many years would you like to repay the loan?
Customer: Around 10 years.
Agent: Congratulations, Mr. Verma! Based on the preliminary details provided, your profile qualifies for our Loan Against Property offer. I am now passing your verified details to our Senior Loan Expert. They will call you back shortly to guide you through the property evaluation and share exact customized interest rates. Thank you for choosing Home Credit, and have a wonderful day!
```

---

### TC-02: Immediate Disqualification — Agricultural Property
* **Goal**: Verify immediate termination when property is agricultural.
* **Persona**: Suresh Patel, Farmer/Landowner.

```text
Agent: Hello, good afternoon, am I speaking with Suresh Patel?
Customer: Yes, Suresh speaking.
Agent: Hello Mr. Patel, this is Priya from Home Credit. As a valued customer, you have a pre-approved Loan Against Property offer of up to 75 Lakhs. Do you have two minutes for a quick eligibility check?
Customer: Yeah, tell me.
Agent: Great! Could you tell me the type of property you own? Is it residential, commercial, or industrial?
Customer: Actually, it's 5 acres of agricultural land near my village.
Agent: Thank you for sharing that, Mr. Patel. Unfortunately, our current policy guidelines for this specific pre-approved offer are applicable only to residential, commercial, and industrial properties, and do not cover agricultural land at this time. We truly appreciate your time and your relationship with Home Credit. Have a wonderful day ahead!
[CALL TERMINATES - NO FURTHER QUESTIONS ASKED]
```

---

### TC-03: Immediate Disqualification — Cash Income Mode
* **Goal**: Verify immediate termination when customer receives cash income.
* **Persona**: Ramesh Gupta, Shopkeeper.

```text
Agent: [After verifying name, property type (commercial shop), sole ownership, original docs available, and loan amount 20 Lakhs...]
Agent: Could you share whether you are salaried or self-employed, and is your income received in your bank account or in cash?
Customer: I am self-employed running a grocery store, but all my earnings are collected in cash.
Agent: Thank you for sharing those details, Mr. Gupta. Under our current policy guidelines for this specific pre-approved offer, income verification through regular bank credits is mandatory, and we cannot process cash-based income at this time. We truly value your relationship with Home Credit. Thank you for your time and have a wonderful day ahead!
[CALL TERMINATES]
```

---

### TC-04: Immediate Disqualification — Missing Original Documents
* **Goal**: Verify immediate termination when original documents are unavailable.
* **Persona**: Anita Rao.

```text
Agent: [After verifying residential property and sole ownership...]
Agent: Do you have the original property documents ready and available for verification?
Customer: No, the original papers were misplaced during relocation. I only have photocopies with me.
Agent: Thank you for clarifying, Ms. Rao. To proceed with this Loan Against Property offer, having the original title documents available for verification is a mandatory regulatory requirement. Since only copies are available, we won't be able to process this application at this time. Thank you for your time with Home Credit, and have a great day ahead!
[CALL TERMINATES]
```

---

### TC-05: Ineligible Repayment Tenure
* **Goal**: Verify boundary check on 3 to 15 years tenure.
* **Persona**: Vikram Singh.

```text
Agent: [All previous 6 questions passed...]
Agent: Over how many years would you like to repay the loan?
Customer: I want a very long repayment tenure, around 25 years.
Agent: I understand, Mr. Singh. However, our repayment tenure for this Loan Against Property program is structured strictly between 3 and 15 years. Would a tenure up to 15 years work for your requirement?
Customer: No, my EMI will be too high with 15 years. I strictly need 25 years.
Agent: I completely understand your requirement, Mr. Singh. Since our maximum tenure policy for this scheme is capped at 15 years, we won't be able to proceed with this application right now. We truly appreciate your time with Home Credit, and have a great day ahead!
[CALL TERMINATES]
```

---

### TC-06: Loan Amount Exceeds 75 Lakhs — Accepted Alternative Cap
* **Goal**: Verify over-limit negotiation protocol when customer accepts 75L.
* **Persona**: Deepak Malhotra.

```text
Agent: [Questions 1-3 passed: Commercial office, sole owner, original documents ready...]
Agent: How much loan amount are you looking to borrow against this property?
Customer: I need 1 Crore for expanding my office.
Agent: Our pre-approved offer under this special scheme is for up to a maximum of 75 Lakhs. Would you like to proceed with the maximum eligible amount of 75 Lakhs?
Customer: Well, 75 Lakhs would still help a lot. Yes, let's proceed with 75 Lakhs.
Agent: Fantastic! I have noted 75 Lakhs. Could you share whether you are salaried or self-employed, and is your income received via bank or cash?
[FLOW CONTINUES NORMALLY THROUGH REMAINING CHECKLIST]
```

---

### TC-07: Loan Amount Exceeds 75 Lakhs — Rejected Alternative Cap
* **Goal**: Verify polite termination when customer refuses the ₹75L ceiling.
* **Persona**: Arvind Singhal.

```text
Agent: How much loan amount are you looking to borrow against this property?
Customer: I need at least 1.5 Crores.
Agent: Our pre-approved offer under this special scheme is for up to a maximum of 75 Lakhs. Would you like to proceed with the maximum eligible amount of 75 Lakhs?
Customer: No Priya, 75 Lakhs won't be enough for my factory equipment. I only want to talk if you can do 1.5 Crores.
Agent: I completely understand your requirement, Mr. Singhal. Since this pre-approved program is strictly capped at 75 Lakhs, we won't be able to meet that requirement today. We'll certainly keep you posted if higher limits become available. Thank you for your time, and have a great day ahead!
[CALL TERMINATES]
```

---

### TC-08: Joint Ownership Property
* **Goal**: Verify that Joint Ownership is recognized as eligible.
* **Persona**: Sunita Deshmukh.

```text
Agent: Could you tell me what type of property you own? Is it residential, commercial, or industrial?
Customer: It's a residential flat in Pune.
Agent: Got it. And are you the sole owner of this property, or is it jointly owned with family or partners?
Customer: It is jointly owned by me and my husband.
Agent: Wonderful, joint ownership is completely eligible! Do you have the original property documents ready and available for verification?
Customer: Yes, we keep all originals in our bank locker.
[FLOW CONTINUES NORMALLY]
```

---

### TC-09: Existing Property Loan — Transfer Specialist Switch
* **Goal**: Validate instant handoff to Loan Transfer Specialist when customer has an existing loan.
* **Persona**: Rajesh Nair.

```text
Agent: Hello, good morning, am I speaking with Rajesh Nair?
Customer: Yes, Rajesh here.
Agent: Hello Mr. Nair, this is Priya calling from Home Credit. As a valued customer, you have a pre-approved Loan Against Property offer of up to 75 Lakhs. Do you have two minutes for a quick eligibility check?
Customer: Actually Priya, I already have an existing home loan running on my flat with another bank. Can I transfer it to Home Credit?
Agent: Thank you for sharing that, Mr. Nair! Since you already have an existing loan and are looking for a loan transfer, this will be handled directly by our dedicated Loan Transfer Specialist. I am forwarding your request, and a specialist will contact you shortly with the best transfer terms. Thank you for your time with Home Credit, and have a wonderful day!
[CALL TERMINATES - NO QUESTIONS 1-7 ASKED]
```

---

### TC-10: EMI Reduction Request — Transfer Specialist Switch
* **Goal**: Validate trigger on "EMI reduction".
* **Persona**: Manoj Joshi.

```text
Agent: ...We have a pre-approved Loan Against Property offer of up to 75 Lakhs. Shall we check your eligibility?
Customer: Look, I don't need new money, but my current property loan EMI is too high. Can you help me reduce my monthly EMI?
Agent: Thank you for explaining that, Mr. Joshi. Helping you reduce your existing EMI through a balance transfer is handled by our specialized Loan Transfer team. I have registered your request, and our transfer specialist will contact you shortly to review your existing loan and offer lower EMI options. Thank you for your time, and have a great day ahead!
[CALL TERMINATES]
```

---

### TC-11: Busy Customer Flow
* **Goal**: Respect time, capture callback slot, avoid sales pressure.
* **Persona**: Dr. Arun Sen.

```text
Agent: Hello, good afternoon, am I speaking with Dr. Arun Sen?
Customer: Yes, but I'm in the middle of a hospital clinic right now. Can't talk.
Agent: I completely understand, Dr. Sen. What would be a convenient date and time for us to call you back?
Customer: Call me tomorrow around 4 PM.
Agent: Thank you, Dr. Sen. I've scheduled a callback for tomorrow at 4 PM. Have a productive day ahead!
[CALL TERMINATES CLEANLY]
```

---

### TC-12: Complex Multi-Slot Out-of-Order Capture
* **Goal**: Test "First Missing Question" algorithm when customer speaks naturally.
* **Persona**: Prateek Bansal.

```text
Agent: As one of our valued customers, you have a pre-approved Loan Against Property offer of up to 75 Lakhs. Shall we run a quick two-minute eligibility check?
Customer: Yeah sure! Look, I run a business, have a residential bungalow in Jaipur worth around 2 Crores, and I'm looking for about 50 Lakhs.
Agent: [AGENT INTERNALLY STORES:
        - property_type = Residential (VALID)
        - occupation = Self-Employed (VALID)
        - market_value = 2 Crores (VALID)
        - loan_amount = 50 Lakhs (VALID)
        Earliest missing question is Q2: Ownership Status]
Agent: That's great, Mr. Bansal! I've noted your residential property and the 50 Lakhs requirement. Could you tell me if you are the sole owner of the bungalow, or is it jointly owned?
Customer: It's jointly owned with my father.
Agent: Understood. And do you have the original property documents ready and available for verification?
Customer: Yes, all original deeds are safely with us.
Agent: Perfect. And regarding your business income, is it received directly into your bank account?
Customer: Yes, 100% through our current account in the bank.
Agent: Excellent. And finally, over how many years would you prefer to repay the 50 Lakhs loan?
Customer: 7 years would be ideal.
Agent: Congratulations, Mr. Bansal! All 7 preliminary eligibility criteria are met. I am now passing your details to our Senior Loan Expert, who will call you shortly to discuss property evaluation and provide exact interest rates. Thank you, and have a great day!
[QUALIFIED & HANDED OFF]
```

---

### TC-13: Mid-Turn Self-Correction & Adversarial Input
* **Goal**: Ensure the agent overwrites previous answers with customer's latest clarification.
* **Persona**: Amit Saxena.

```text
Agent: Over how many years would you like to repay the loan?
Customer: I'd say 5 years... wait, let me think, actually make that 12 years so my monthly EMI stays low.
Agent: Understood, I have updated your preferred tenure to 12 years.
[AGENT RECORDS tenure = 12 years, CONTINUES WITHOUT CONFUSION]
```

---

### TC-14: Interruption & Interest Rate Diversion
* **Goal**: Test deflection when customer asks for unprovided rate specifics.
* **Persona**: Neha Kapoor.

```text
Agent: Could you tell me what type of property you own? Is it residential, commercial, or industrial?
Customer: Before I answer that Priya, tell me what interest rate you are charging? If it's higher than 10%, I don't want it.
Agent: I completely understand your concern, Ms. Kapoor. Exact, competitive interest rates are customized based on the physical property evaluation and will be provided directly by our Senior Loan Expert during the consultation call. To connect you with them, I just need to verify a few basic property details. Is your property residential, commercial, or industrial?
Customer: Okay, fair enough. It's an independent residential house.
[AGENT SMOOTHLY RESUMES CHECKLIST]
```

---

### TC-15: Hindi Conversational Flow (Femal Agent Voice)
* **Goal**: Validate natural Hindi phrasing and strict feminine grammar agreement.
* **Persona**: Mohan Lal Sharma.

```text
Agent: नमस्ते, क्या मेरी बात श्री मोहन लाल शर्मा जी से हो रही है?
Customer: हाँ, मैं मोहन लाल बोल रहा हूँ। आप कौन?
Agent: नमस्ते मोहन लाल जी, मैं प्रिया बोल रही हूँ, होम क्रेडिट से। आप कैसे हैं आज?
Customer: मैं ठीक हूँ। किस बारे में कॉल किया है?
Agent: मोहन लाल जी, होम क्रेडिट के साथ आपके बेहतरीन रिकॉर्ड की वजह से, आपके लिए 75 लाख रुपये तक का प्री-अप्रूव्ड लोन अगेंस्ट प्रॉपर्टी ऑफर उपलब्ध है। क्या मैं आपकी एलिजिबिलिटी चेक करने के लिए 2 मिनट ले सकती हूँ?
Customer: हाँ, बताइए।
Agent: बहुत शुक्रिया! क्या आप बता सकते हैं कि आपकी प्रॉपर्टी किस प्रकार की है—रेजिडेंशियल, कमर्शियल, या इंडस्ट्रियल?
Customer: यह हमारा अपना रहने का मकान है, रेजिडेंशियल।
Agent: बहुत बढ़िया। और क्या आप इस मकान के अकेले मालिक हैं, या यह किसी परिवार के सदस्य के साथ जॉइंट है?
Customer: मैं और मेरी पत्नी, दोनों के नाम पर है।
Agent: बिल्कुल, जॉइंट ओनरशिप पूरी तरह से मान्य है। क्या वेरिफिकेशन के लिए प्रॉपर्टी के ओरिजिनल डाक्यूमेंट्स आपके पास उपलब्ध हैं?
Customer: हाँ जी, सारे असली कागजात घर पर ही हैं।
Agent: बहुत अच्छा। आप इस प्रॉपर्टी पर लगभग कितना लोन लेना चाहते हैं?
Customer: मुझे लगभग 30 लाख की जरूरत है।
Agent: समझ गई, 30 लाख रुपये। मोहन लाल जी, क्या आप नौकरी करते हैं या आपका अपना बिजनेस है? और आपकी इनकम बैंक में आती है या कैश में?
Customer: मैं एक प्राइवेट कंपनी में नौकरी करता हूँ और सैलरी सीधे बैंक खाते में आती है।
Agent: बहुत खूब। आपकी इस प्रॉपर्टी की मौजूदा मार्केट वैल्यू लगभग कितनी होगी?
Customer: लगभग 60 से 65 लाख रुपये होगी।
Agent: ठीक है। और आप यह लोन कितने सालों में चुकाना चाहेंगे?
Customer: लगभग 8 साल में।
Agent: बहुत-बहुत बधाई मोहन लाल जी! आपकी दी गई सभी जानकारियों के अनुसार आप हमारे लोन अगेंस्ट प्रॉपर्टी ऑफर के लिए योग्य हैं। मैं आपकी फाइल हमारे सीनियर लोन एक्सपर्ट को ट्रांसफर कर रही हूँ। वे जल्द ही आपको कॉल करके प्रॉपर्टी वेरिफिकेशन और आपके लिए तय की गई सही ब्याज दर की जानकारी देंगे। होम क्रेडिट से बात करने के लिए धन्यवाद, आपका दिन शुभ हो!
```
