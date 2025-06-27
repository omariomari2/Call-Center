# Building Up Call Center Using Amazon Connect & Lambda in Node.js Environment with JavaScript
---

## ☎️ Lab 2 – Building a Call Center with Amazon Connect

### 🧭 Goal

Deploy a basic cloud-based call center using Amazon Connect, hook it up with our `PersonalBanker` Lex bot, and route calls either to the bot or a live agent. Sprinkle in a few enhancements like DTMF options (press 1 / press 2) and personalized greetings.

---

### 📞 Setting Up Amazon Connect

> I launched Amazon Connect in **North Virginia** (same region as Lex).

**Key setup points:**
- Skipped admin creation (used default)
- Disabled outbound calling
- Took the default S3 bucket settings
- Created the instance and claimed a UK Direct Dial Number (for this lab)
- Noted down the number for testing later

---

### 🌀 Creating a Simple Contact Flow

Instead of editing the huge sample flow, I imported a lightweight version:

- Downloaded `Simple Workflow` file from the repo
- Opened **Routing → Contact Flows**
- Clicked **Create contact flow**, then used the **Import Flow** option
- Loaded the `Simple Workflow` file
- This flow: greets the caller → plays a prompt → transfers them to a queue

I published it with **Save & Publish**, then:

- Went to **Routing → Phone Numbers**
- Edited my claimed number to link it to this new `Simple Workflow`

Now when I called in, it said the welcome message and sent me to a queue. Neat, but we’re just getting started.

---

### 🤖 Integrating the Lex Bot

Now came the fun part—injecting my `PersonalBanker` Lex bot from Lab 1 into this call flow.

Steps:
1. Back in **Amazon Connect Admin**, I added the Lex bot under **Contact Flows → Amazon Lex → + Add Lex Bot**
2. Then I edited the `Simple Workflow` flow:
   - From the left panel, dragged **Get Customer Input** into the flow (from the *Interact* category)
   - Added a prompt:  
     ```
     How can we help you today?
     ```
   - Selected **Amazon Lex**, picked `PersonalBanker`
   - Added intent: `GetBalanceCheck`
3. Unlinked the Play Prompt → Queue connection
4. Linked Play Prompt → Get Customer Input → then its Default branch → Transfer to Queue  
   (Other branches like "Error" or "No Input" went to Disconnect)

Saved and Published again.

Tested it: when I called the number, I got prompted by Lex. Said “check my bank balance” → got routed into the bot → entered my account type and PIN → got back a mock balance. Mission accomplished.

---

### 🎛️ Enhancing the Experience with DTMF + Additional Intents

I realized there was no way to speak to an agent from the bot, so I:

1. Created a new intent in Lex: `Advisor`
   - Sample utterances:
     ```
     advisor  
     talk  
     talk to someone  
     press one  
     ```
2. Added utterance `press two` to `GetBalanceCheck` as well (for symmetry)

Built the bot again in Lex.

Then back in the **Get Customer Input** box in Connect, I added both intents:
- `GetBalanceCheck`
- `Advisor`

Wired the **Advisor** branch to **Transfer to Queue**, so it now allowed users to say:
- “talk to someone”  
- or literally press 1 on the phone pad

This added both voice and keypad navigation options. Tested again—pressing 1 got me to an agent; pressing 2 triggered the balance check flow. Felt legit!

---

### 🧑‍💻 Bonus Round: Personalized Greetings via Caller Number

I took it one step further—made Connect greet callers **by name** using their phone number.

Steps:
1. Created a new Lambda: `myPersonalResponder`
2. Pasted in the code from `myPersonalResponder_v1.js`
   - It checks the incoming number and returns a name if matched

3. In Contact Flow editor:
   - From the left panel, dragged in **Invoke AWS Lambda Function**
   - Wired it right after the “Entry point” of the flow

4. Used the returned value to set a **Contact Attribute** like `CustomerName`
5. In the Play Prompt, used dynamic speech like:  
   ```
   Hi {CustomerName}, thanks for calling!
   ```

Now the system greeted known callers by name. Otherwise, it defaulted to a generic message.

---

### 🧾 Takeaways

- Connected Lex bots into real call flows
- Gave users voice + keypad options for interaction
- Handled branching logic with intent mapping
- Personalized flows using Lambda + caller number detection

---
