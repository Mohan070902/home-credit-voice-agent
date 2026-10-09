The assignment asks you to build an AI Voice Agent for Home Credit that informs existing customers about a pre-approved Loan Against Property (LAP) offer, collects 7 eligibility data points sequentially, handles interruptions/diversions, immediately disqualifies ineligible customers, detects loan-transfer cases, and finally hands qualified leads to a senior loan expert.

# 1. Difficulty Rating


### Overall Difficulty: 7.5/10 — Intermediate to Advanced


### Table

Area | Difficulty
Understanding requirements | ⭐⭐☆☆☆
Basic prompt writing | ⭐⭐☆☆☆
Conversation-flow design | ⭐⭐⭐⭐☆
Business-rule implementation | ⭐⭐⭐⭐⭐
Handling interruptions/diversions | ⭐⭐⭐⭐⭐
Voice-agent behavior | ⭐⭐⭐⭐☆
Edge-case handling | ⭐⭐⭐⭐⭐
Testing | ⭐⭐⭐⭐☆
Overall | 7/10

### Why it is challenging

The difficult part is not writing something like:
> "Ask the customer their property type, income, loan amount, etc."
The real challenge is making the agent reliably handle situations such as:
> Customer: "Yes, I'm interested. I earn ₹80,000 a month and my property is worth around ₹1 crore."
The agent must remember those details, avoid asking them again later, but still continue through the required checklist.
The assignment explicitly requires sequential capture while remembering information provided out of order.
It also requires immediate termination for disqualifying answers and a separate flow for existing-property-loan/EMI-reduction cases.

---


# 2. What Exactly Are You Being Asked to Build?

Think of your solution as 5 layers:
```Plain Text
                    AI VOICE AGENT
                          │
          ┌───────────────┴───────────────┐
          │                               │
   Conversation Layer               Business Logic
          │                               │
 Greeting → Offer → Questions       Eligibility Rules
          │                               │
          └───────────────┬───────────────┘
                          │
                    State / Memory
                          │
            ┌─────────────┴─────────────┐
            │                           │
     Customer Answers             Diversions
            │                           │
            └─────────────┬─────────────┘
                          │
                     Final Decision
                          │
             ┌────────────┴────────────┐
             │                         │
          Eligible                 Not Eligible
             │                         │
        Human Handoff               End Call
```
Your System Prompt should essentially implement this architecture in natural language.

---


# 3. First Understand the Business Flow

The complete expected conversation is:
```Plain Text
START
  ↓
Greeting
  ↓
Verify Customer
  ↓
Customer Busy?
  ├── YES → Ask callback time → End
  └── NO
       ↓
Explain LAP Offer
       ↓
Check Existing Loan / EMI Transfer
  ├── YES → Transfer specialist message → End
  └── NO
       ↓
Eligibility Questions
       ↓
1. Property Type
       ↓
2. Ownership
       ↓
3. Original Documents
       ↓
4. Loan Amount
       ↓
5. Occupation + Income Mode
       ↓
6. Property Market Value
       ↓
7. Tenure
       ↓
Eligibility Decision
       ├── FAIL → Explain → End
       └── PASS
             ↓
       Senior Loan Expert Handoff
             ↓
           END
```
The assignment specifically says that the final handoff can happen only after all eligibility questions have been answered.

---


# 4. Understand the 7 Eligibility Variables

This is the heart of your prompt.
Create an internal state like:
```Plain Text
property_type = UNKNOWN
ownership_status = UNKNOWN
documents_available = UNKNOWN
loan_amount = UNKNOWN
occupation = UNKNOWN
income_mode = UNKNOWN
market_value = UNKNOWN
tenure = UNKNOWN
```
Although there are 7 questions, question #5 contains two pieces of information:
```Plain Text
Occupation
+
Income Mode
```
The assignment treats these together as one checklist item.

---


# 5. Define the Eligibility Rules Explicitly

This is where many candidates will lose marks.
Don't write vague instructions like:
> "Check whether the customer is eligible."
Define every rule explicitly.

## Rule 1 — Property Type

Allowed:
```Plain Text
Residential
Commercial
Industrial
```
Not allowed:
```Plain Text
Agricultural
```
If:
> "It's agricultural land."
Then:
```Plain Text
Politely inform customer that they don't meet
the criteria for this specific offer.

END CALL.
```
The assignment explicitly identifies agricultural property as an immediate disqualification.

---


# 6. Ownership

Allowed:
```Plain Text
Sole ownership
Joint ownership
```
Both are eligible.
Therefore:
> "It's jointly owned by me and my brother."
→ Eligible.
Don't accidentally disqualify joint ownership.

---


# 7. Original Documents

Customer must have:
```Plain Text
Original property documents
```
If:
> "I only have photocopies."
→ Disqualify.
If:
> "The originals are available at home."
→ Eligible.
The key is availability, not necessarily physically holding them during the call. The requirement says original documents must be available for verification.

---


# 8. Loan Amount

Maximum:
₹75,00,000
If customer says:
> "I need ₹50 lakh."
→ Continue.
If:
> "I need ₹75 lakh."
→ Continue.
If:
> "I need ₹90 lakh."
Do not immediately reject.
The assignment specifically says:
1. Explain that the maximum is ₹75 lakh.
1. Ask whether they want to proceed with ₹75 lakh.
So:
```Plain Text
Customer: I need 90 lakhs.

Agent:
"The current offer is available for up to ₹75 lakhs.
Would you like to proceed with the maximum eligible
amount of ₹75 lakhs?"
```
If YES:
```Plain Text
loan_amount = 7500000
continue
```
If NO:
```Plain Text
end call
```

---


# 9. Occupation + Income Mode

Allowed occupations:
```Plain Text
Salaried
Self-employed
```
Both are eligible.
But income must be:
```Plain Text
Bank
```
Not:
```Plain Text
Cash
```
So:

### Table

Occupation | Income | Result
Salaried | Bank | ✅
Salaried | Cash | ❌
Self-employed | Bank | ✅
Self-employed | Cash | ❌
This is one of the most important business rules in the assignment.

---


# 10. Market Value

Ask:
> "Approximately what is the current market value of the property?"
Important:
The assignment does not provide a minimum market-value eligibility threshold.
Therefore, do not invent one.
For example, don't write:
> "Property value must be at least ₹1 crore."
That requirement isn't in the assignment.
Simply capture:
```Plain Text
market_value = customer's estimate
```

---


# 11. Tenure

Allowed:
3–15 years
Therefore:
```Plain Text
2 years → ❌
3 years → ✅
10 years → ✅
15 years → ✅
16 years → ❌
```
The assignment explicitly specifies this range.

---


# 12. The Most Important Design: State Management

This is where you can make your submission significantly better than a basic candidate submission.
Your prompt should instruct the AI to maintain a qualification state.
For example:
```Plain Text
QUALIFICATION_STATE

customer_verified: UNKNOWN

property_type: UNKNOWN
ownership_status: UNKNOWN
documents_available: UNKNOWN
loan_amount: UNKNOWN
occupation: UNKNOWN
income_mode: UNKNOWN
market_value: UNKNOWN
tenure: UNKNOWN

existing_property_loan: UNKNOWN
emi_reduction_request: UNKNOWN

disqualified: FALSE
transfer_required: FALSE
```
You don't necessarily need to expose this to the customer.
It's internal reasoning/state.

---


# 13. Handling Out-of-Order Information

This is specifically required by the assignment.
Suppose the agent asks:
> "What type of property do you have?"
Customer says:
> "It's residential, jointly owned, and worth about ₹80 lakh."
You should capture:
```Plain Text
property_type = Residential
ownership_status = Joint
market_value = ₹80 lakh
```
Then:
```Plain Text
DO NOT ASK:

"Is it jointly owned?"

DO NOT ASK:

"What is the market value?"
```
Instead continue with the first missing checklist item.
For example:
> "Thank you. Do you have the original property documents available for verification?"
This demonstrates that your agent actually understands conversational context rather than behaving like a rigid questionnaire.

---


# 14. The "First Missing Question" Algorithm

This is an excellent rule to put directly into your System Prompt.
After every customer response:
```Plain Text
1. Extract every eligibility fact from the response.
2. Store all valid information.
3. Check for disqualifying information.
4. Check for loan-transfer intent.
5. Identify the earliest unanswered eligibility item.
6. Ask only that question.
```
In pseudocode:
```Plain Text
ON CUSTOMER RESPONSE:

extract_information()

if disqualifying_condition:
    disqualify()
    end_call()

else if transfer_condition:
    transfer_message()
    end_call()

else:
    update_state()

    next_question =
        first unanswered checklist item

    ask(next_question)
```
This is probably the single most important piece of logic in your assignment.

---


# 15. Handle Existing Loan / EMI Reduction

The assignment says the standard flow is a Fresh Loan.
But if the customer says:
> "I already have a loan on this property."
or:
> "I want to reduce my EMI."
You must NOT continue the normal eligibility flow.
Instead:
```Plain Text
Acknowledge
↓
Explain that a specialist for loan transfer
will contact them
↓
End call
```
This is a separate branch.
Example:
> "Thank you for clarifying. Since you already have an existing loan on the property, this would need to be handled by our loan-transfer specialist. A specialist will contact you shortly regarding this. Thank you for your time."
Then end.
Don't ask questions 1–7.

---


# 16. Busy Customer Flow

At the beginning:
> "Hello, may I speak with Mr. Sharma?"
Customer:
> "I'm actually busy right now."
Your agent should NOT continue the pitch.
Instead:
> "Certainly, I understand. What would be a convenient time for us to call you back?"
Capture preferred callback time.
Then end.
The assignment explicitly requires this behavior.

---


# 17. Customer Verification

You should make verification an explicit gate.
Example:
```Plain Text
Agent:
"Hello, may I speak with Rahul?"

Customer:
"Yes, this is Rahul."

→ verified = TRUE
```
But:
> "Who is calling?"
The agent should explain who it is and why it's calling without revealing unnecessary sensitive information.

---


# 18. Offer Presentation

After successful verification:
Explain:
- customer loyalty
- special LAP offer
- maximum amount ₹75 lakh
- preliminary qualification purpose
The assignment says the offer should be presented as a reward for the customer's loyalty.
Avoid sounding like a robotic advertisement.
Bad:
> "YOU HAVE BEEN SELECTED FOR A LOAN. ARE YOU INTERESTED? YES OR NO?"
Better:
> "We're reaching out because, as one of our valued customers, you may be eligible for a special Loan Against Property offer of up to ₹75 lakh. I'd just like to ask a few questions to check whether the offer is suitable for you."

---


# 19. Don't Build a Yes/No Bot

This is explicitly mentioned as mandatory.
Your agent needs to understand:
> "Yeah, it's my house."
> "Actually, it's jointly owned with my wife."
> "I've got the original papers somewhere at home."
> "I run a small business and most of my income comes directly into my bank account."
> "Around seventy-five lakhs, maybe a little less."
All of these should be interpreted correctly.

---


# 20. Handle Natural Speech

Your prompt should explicitly tell the agent to understand:

### Fillers

> "Umm... yeah... it's residential."

### Corrections

> "It's 10 years—actually, make that 12."
Latest valid answer should overwrite the previous value.

### Interruptions

> "Yes, it's residential—sorry, can I ask something?"
The agent should answer the question and then return to the checklist.

### Multiple answers

> "It's my own house, original papers are available, and I need around 40 lakhs."
Capture all three.

---


# 21. Diversion Handling

This is another major evaluation area.
Customer:
> "Before that, what interest rate do you offer?"
The agent doesn't have the exact interest rate.
Do NOT invent it.
Instead:
> "The exact interest rate will be provided by our senior loan expert after the preliminary qualification. For now, I'd like to complete a few basic eligibility questions."
Then return to the next missing question.
The assignment says the senior loan expert will provide exact interest rates after qualification.

---


# 22. Customer Asks Unrelated Question

Example:
> "Where is your nearest branch?"
Your prompt should instruct:
```Plain Text
Answer briefly if the information is available
through additional_context_from_rag.

Otherwise do not fabricate an answer.

Return to qualification flow.
```
This connects directly to the supplied:
```Plain Text
additional_context_from_rag
```
variable.

---


# 23. Use the Prompt Variables Properly

Your System Prompt must account for:
```Plain Text
company_name
customer_name
agent_name
agent_gender
current_date
current_day
current_time
additional_context_from_rag
language_to_speak
conversation_history
customer_utterance
```
These are explicitly required by the assignment.
A strong prompt shouldn't hardcode:
> "You are Priya."
Instead:
```Plain Text
You are {{agent_name}}, an AI voice assistant representing
{{company_name}}.

Your gender is {{agent_gender}} and you must consistently
use language appropriate to that gender where applicable.

The customer is {{customer_name}}.

Current date: {{current_date}}
Current day: {{current_day}}
Current time: {{current_time}}

Speak only in {{language_to_speak}} unless the platform's
language behavior explicitly requires otherwise.
```

---


# 24. Hindi/English Handling

Because:
```Plain Text
language_to_speak
```
is supplied dynamically, your prompt should not randomly switch languages.
If:
```Plain Text
language_to_speak = Hindi
```
the agent should conduct the conversation in Hindi.
If:
```Plain Text
language_to_speak = English
```
conduct it in English.
If the platform supports natural bilingual handling, you can instruct it to follow the configured language unless the customer clearly requests another supported language.

---


# 25. Agent Gender

This is an easy requirement to overlook.
The assignment says:
> agent_gender must be adhered to strictly.
Therefore your prompt should explicitly say:
```Plain Text
Maintain the configured agent gender consistently.
Do not describe yourself using a different gender.
Use appropriate gendered linguistic forms when speaking
languages where gender affects grammar.
```

---


# 26. RAG Variable

The assignment gives:
```Plain Text
additional_context_from_rag
```
This means you should design the prompt so product information can be injected dynamically.
Important rule:
```Plain Text
Never invent product information.

If a product-specific question is answered by
additional_context_from_rag, use that information.

If the information is unavailable, say that the senior
loan expert will provide the exact details.
```
This is a good Prompt Engineer-level design decision.

---


# 27. Recommended System Prompt Structure

I would structure your final prompt like this:
```Plain Text
ROLE & OBJECTIVE

IDENTITY

LANGUAGE & VOICE

CALL OBJECTIVE

CONVERSATION PRINCIPLES

CALL FLOW

CUSTOMER VERIFICATION

BUSY CUSTOMER FLOW

OFFER PRESENTATION

TRANSFER/EXISTING LOAN DETECTION

ELIGIBILITY STATE

7 ELIGIBILITY QUESTIONS

ELIGIBILITY RULES

OUT-OF-ORDER INFORMATION HANDLING

DISQUALIFICATION LOGIC

LOAN AMOUNT > ₹75 LAKH LOGIC

DIVERSION HANDLING

INTERRUPTION HANDLING

RAG KNOWLEDGE RULES

HANDOFF RULES

CALL TERMINATION RULES

EDGE CASES

INTERNAL STATE MANAGEMENT

FINAL RESPONSE/HANDOFF
```
This structure is much better than one giant paragraph.

---


# 28. Build the Prompt Step-by-Step


## Step 1 — Define role

```Plain Text
You are an AI Voice Agent representing {{company_name}}.

Your role is to make preliminary qualification calls to
existing customers regarding a pre-approved Loan Against
Property offer.

You are NOT the final loan decision-maker.
Your purpose is to collect preliminary eligibility
information and qualify the customer for a senior loan expert.
```

---


## Step 2 — Define objective

```Plain Text
Your primary objective is to:

1. Verify the intended customer.
2. Determine whether they are available to talk.
3. Explain the LAP offer.
4. Detect existing-loan/EMI-transfer cases.
5. Collect all seven eligibility data points.
6. Apply the eligibility rules accurately.
7. Disqualify immediately when a mandatory criterion fails.
8. Hand off qualified customers to a senior loan expert.
```

---


# 29. Define the Checklist

Use something like:
```Plain Text
CHECKLIST:

1. Property Type
2. Ownership Status
3. Original Documents Availability
4. Desired Loan Amount
5. Occupation + Income Mode
6. Estimated Property Market Value
7. Desired Tenure
```
Then:
```Plain Text
Always ask the earliest unanswered item.

Never ask a question again if the answer has already
been clearly provided by the customer.
```

---


# 30. Eligibility Decision Table

Your internal prompt can contain a table-like structure:
```Plain Text
PROPERTY TYPE
Residential → Eligible
Commercial → Eligible
Industrial → Eligible
Agricultural → Disqualify

OWNERSHIP
Sole → Eligible
Joint → Eligible

DOCUMENTS
Original documents available → Eligible
Original documents unavailable → Disqualify

LOAN AMOUNT
≤ ₹75 lakh → Eligible
> ₹75 lakh → Offer ₹75 lakh alternative

OCCUPATION
Salaried → Eligible
Self-employed → Eligible

INCOME MODE
Bank → Eligible
Cash → Disqualify

MARKET VALUE
Capture value.
No minimum threshold specified in assignment.

TENURE
3–15 years → Eligible
<3 years → Disqualify
>15 years → Disqualify
```

---


# 31. Immediate Disqualification Rule

This must be extremely explicit.
```Plain Text
IMPORTANT:

If at ANY point the customer provides an answer that
clearly violates an eligibility criterion:

1. Stop the normal qualification flow.
2. Do not ask additional eligibility questions.
3. Politely explain that the customer does not meet
   the criteria for this specific offer at this time.
4. End the call.

Do not attempt to persuade the customer or override
the eligibility rule.
```
That directly addresses the assignment requirement.

---


# 32. Handoff Gate

Another critical rule:
```Plain Text
NEVER say that the customer is fully qualified until
all seven eligibility items have been answered and
all applicable criteria have passed.
```
Then:
```Plain Text
IF:

all 7 = answered
AND
no disqualifying condition
AND
no transfer condition

THEN:

tell customer that a senior loan expert will call shortly
to discuss the next steps and exact interest rate.
```

---


# 33. Testing Strategy

This is where you can really impress the evaluator.
Don't make just one successful test call.
The assignment specifically asks candidates to conduct multiple calls with different scenarios.
I recommend at least 12 test scenarios.

### Test 1 — Perfect Eligible Customer

```Plain Text
Residential
Sole
Original documents available
₹50 lakh
Salaried
Bank income
₹1 crore property
10-year tenure
```
Expected:
QUALIFIED → HANDOFF

---


### Test 2 — Agricultural Property

```Plain Text
Property = Agricultural
```
Expected:
Immediate disqualification

---


### Test 3 — Cash Income

```Plain Text
Self-employed
Income = Cash
```
Expected:
Immediate disqualification

---


### Test 4 — Missing Original Documents

```Plain Text
Original documents unavailable
```
Expected:
Immediate disqualification

---


### Test 5 — Tenure Too Short

```Plain Text
Tenure = 2 years
```
Expected:
Disqualification

---


### Test 6 — Tenure Too Long

```Plain Text
Tenure = 20 years
```
Expected:
Disqualification

---


### Test 7 — Loan Amount ₹90 Lakh

Expected:
```Plain Text
Explain ₹75 lakh limit
Ask whether customer wants ₹75 lakh
```

---


### Test 8 — Joint Ownership

Expected:
Continue

---


### Test 9 — Existing Loan

Customer:
> "I already have a loan against this property."
Expected:
Transfer specialist flow → End

---


### Test 10 — EMI Reduction

Customer:
> "Actually, I want to reduce my existing EMI."
Expected:
Transfer specialist → End

---


### Test 11 — Out-of-Order Answers

Customer:
> "It's a residential house worth about ₹1.2 crore, jointly owned with my wife."
Expected internal state:
```Plain Text
property_type = Residential
market_value = ₹1.2 crore
ownership = Joint
```
Then agent asks only the next missing question.

---


### Test 12 — Busy Customer

Customer:
> "I'm in a meeting."
Expected:
> "Certainly. What would be a convenient time for us to call you back?"
Then end.

---


# 34. Add Adversarial Testing

For an AI Prompt Engineer position, go beyond normal testing.
Try:

### Contradiction

> "It's residential... actually no, it's agricultural land."
Expected:
latest clear answer = Agricultural → disqualify

---


### Multiple answers

> "It's my own residential property, original documents are available, I need 40 lakhs and I'd prefer 10 years."
Agent should capture:
```Plain Text
property = residential
ownership = sole
documents = available
loan = ₹40 lakh
tenure = 10 years
```

---


### Interrupting the agent

Agent:
> "Could you tell me whether—"
Customer:
> "Wait, what documents do you need?"
Agent should answer briefly and then return to the checklist.

---


### Ambiguous answer

> "I think the papers are probably somewhere."
Don't assume:
```Plain Text
documents = TRUE
```
Instead ask a clarification:
> "Just to confirm, do you have the original property documents available for verification?"

---


# 35. What the Evaluator Is Probably Looking For

Even though the assignment doesn't provide a formal scoring rubric, the requirements strongly suggest these evaluation dimensions:

### 1. Prompt clarity

Can the AI consistently understand its role?

### 2. Business-rule accuracy

Does it correctly identify:
```Plain Text
Agricultural → reject
Cash income → reject
Tenure outside 3–15 → reject
```

### 3. State management

Does it remember answers?

### 4. Conversation flexibility

Can it handle natural speech?

### 5. Edge cases

Does it behave correctly when customers interrupt, change answers, or provide information out of order?

### 6. Handoff discipline

Does it avoid prematurely qualifying someone?

### 7. Voice experience

Does it sound like an advisor rather than a questionnaire?

### 8. Testing quality

Did you demonstrate multiple scenarios?

---


# 36. Common Mistakes to Avoid


## ❌ Mistake 1 — Asking only yes/no questions

The assignment explicitly says not to design the bot that way.

---


## ❌ Mistake 2 — Asking every question in rigid order

This fails the out-of-order requirement.

---


## ❌ Mistake 3 — Forgetting previously supplied information

This creates an annoying customer experience.

---


## ❌ Mistake 4 — Continuing after disqualification

Example:
> Agricultural property → "Okay, what is your income?"
Wrong.
Once disqualified:
```Plain Text
POLITE MESSAGE → END
```

---


## ❌ Mistake 5 — Continuing fresh-loan questions for existing loans

Existing loan / EMI reduction → specialist transfer.

---


## ❌ Mistake 6 — Inventing interest rates

The assignment says the senior expert will provide exact interest rates.

---


## ❌ Mistake 7 — Inventing market-value requirements

There is no minimum market value specified.

---


## ❌ Mistake 8 — Premature handoff

Even if the customer looks obviously eligible, all seven data points must be captured before final handoff.

---


## ❌ Mistake 9 — Overlong robotic responses

Voice agents should generally communicate in short conversational turns.

---


## ❌ Mistake 10 — No test evidence

Don't submit only the prompt.
Demonstrate that you tested edge cases.

---


# 37. Recommended Submission Package

The assignment ultimately asks for:
1. System Prompt
1. Public link to call recording & call logs
1. Audio
1. Transcript
I recommend going slightly beyond the minimum and organizing your submission as:
```Plain Text
AI Voice Agent Assignment
│
├── 1. System Prompt
│
├── 2. Voice Agent Platform
│
├── 3. Test Scenarios
│     ├── Eligible
│     ├── Agricultural
│     ├── Cash Income
│     ├── Existing Loan
│     ├── >75L
│     ├── Busy Customer
│     └── Out-of-order
│
├── 4. Call Recordings
│
├── 5. Transcripts
│
└── 6. Brief Test Results
```

---


# 38. How I Would Approach the Assignment

If I were submitting this for an AI Intern / Prompt Engineer position, I would use this development process:
```Plain Text
PHASE 1
Understand requirements
        ↓
PHASE 2
Convert requirements into business rules
        ↓
PHASE 3
Design conversation state machine
        ↓
PHASE 4
Write System Prompt
        ↓
PHASE 5
Deploy on Retell/Bolna
        ↓
PHASE 6
Run happy-path test
        ↓
PHASE 7
Run failure-path tests
        ↓
PHASE 8
Run adversarial tests
        ↓
PHASE 9
Fix prompt
        ↓
PHASE 10
Capture recordings/transcripts
        ↓
PHASE 11
Prepare submission
```
The assignment permits platforms such as Retell AI, Bolna, or similar for implementation/testing.

---


# 39. Difficulty From an Interviewer's Perspective

I'd classify it as:

### 7.5/10 for a fresher


### 6/10 for someone with prompt-engineering experience


### 4–5/10 for an experienced conversational-AI engineer

It tests more than prompt syntax.
The evaluator is likely testing whether you understand:
Prompt Engineering + State Management + Conversational AI + Business Logic + Edge Cases + Testing
rather than simply whether you can write a nice-looking prompt.

---


# 40. Your Biggest Opportunity to Stand Out

The strongest differentiator would be to explicitly demonstrate this principle:
> "Extract first, validate second, update state third, ask the next missing question fourth."
For example:
```Plain Text
Customer:
"I have a residential property worth about 1.1 crore.
It's jointly owned with my wife and I need 60 lakhs."

Agent internally:

property_type = Residential
market_value = ₹1.1 Cr
ownership = Joint
loan_amount = ₹60 L

Next missing item:
documents_available

Agent:
"Thank you. Do you have the original property documents
available for verification?"
```
That's much closer to how a production conversational AI system should behave.

---


## Final Recommendation

Don't start by writing the final prompt immediately.
First create:
```Plain Text
Requirements
     ↓
Eligibility Rules
     ↓
State Variables
     ↓
Decision Tree
     ↓
Conversation Rules
     ↓
Edge Cases
     ↓
System Prompt
     ↓
Testing Matrix
```
That approach will produce a much stronger submission than simply writing a 3–4 page prompt.
Submission Deliverables:
1. Complete System Prompt Text
2. Test Audios (for different case scenarios)
3. Audio Transcripts (for each audio)
4. Documents that explain your overall approach to the problem and how you solved it (OPTIONAL - but Recommended) - Not asked in Assignment Instructions