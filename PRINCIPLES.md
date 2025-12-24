# PACE Principles: Detailed Documentation

**Version:** 1.0  
**Date:** December 2025  
**Author:** Michael Shatny ([ORCID: 0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258))

---

## Overview

This document provides comprehensive documentation of the four PACE principles: **Proactive**, **Adaptive**, **Contextual**, and **Efficient**. Each principle includes theoretical foundations, practical guidelines, implementation patterns, and anti-patterns to avoid.

---

## Principle 1: Proactive

> *"The guide initiates, suggests, and anticipates—never waiting passively for users to figure out what to do."*

### Theoretical Foundation

Traditional interfaces operate reactively: they present options and wait for user input. This places the cognitive burden of navigation, discovery, and decision-making entirely on the user.

Proactive design inverts this relationship. The guide takes initiative, demonstrating capability and suggesting paths forward before being asked. This mirrors how an expert human guide would behave—not standing silently until questioned, but offering relevant information and suggestions based on observed context.

### Key Behaviors

#### 1.1 Capability Demonstration

The first message should demonstrate what the guide can do, not merely greet.

**Passive (Anti-pattern):**
```
"Welcome! How can I help you today?"
```

**Proactive:**
```
"Welcome! I just helped someone set up their first MCP server in 3 minutes. 
I can do the same for you—or if you're exploring, tell me what you're 
building and I'll point you to the right tools."
```

The proactive version:
- Demonstrates specific capability ("set up MCP server in 3 minutes")
- Provides social proof ("just helped someone")
- Offers multiple paths ("do the same" or "tell me what you're building")

#### 1.2 Anticipatory Suggestions

The guide surfaces relevant options before users ask.

**Scenario:** User has been discussing Twitter integration for 3 messages.

**Reactive (Anti-pattern):**
```
User: "Anything else I should know?"
Guide: "What would you like to know about?"
```

**Proactive:**
```
User: "Anything else I should know?"
Guide: "Since you're building a Twitter bot, you might also want:
- Rate limiting strategies (Twitter's limits are strict)
- Error handling for API failures
- A test account for development

Want me to walk through any of these?"
```

#### 1.3 Inactivity Prompts

When users hesitate, the guide offers gentle assistance.

**Implementation:**
```javascript
// After 5 seconds of no interaction
if (!userHasInteracted && !promptShown) {
  showMessage(
    "Not sure where to start? Try one of these:",
    ["Show me free tools", "What's popular?", "Surprise me"]
  );
}
```

**Tone guidance:** Inactivity prompts should feel helpful, not pushy. Frame them as assistance, not pressure.

#### 1.4 Proactive Handoffs

When the guide recognizes a need it cannot fulfill, it proactively suggests alternatives.

```
"This sounds like a custom integration project. I can introduce you to 
our consulting playbook, or if you'd prefer, I can connect you with 
someone who's done similar implementations. Which would help more?"
```

### Implementation Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Capability Lead** | First message shows what's possible | "I just helped X do Y..." |
| **Contextual Suggestion** | Offer relevant next steps | "Since you mentioned X, you might also want Y" |
| **Gentle Prompt** | Assist hesitant users | "Not sure where to start? Try..." |
| **Proactive Handoff** | Recognize and address limits | "This needs X. Want me to connect you?" |

### Anti-Patterns to Avoid

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| **Passive Greeting** | "How can I help?" gives no direction | Lead with capability demonstration |
| **Information Dump** | Listing everything overwhelms users | Offer curated, relevant suggestions |
| **Premature Upsell** | Pushing paid options too early breaks trust | Build value before suggesting purchases |
| **Aggressive Prompting** | Too-frequent interruptions annoy users | Limit prompts, respect user pace |

---

## Principle 2: Adaptive

> *"The guide matches user expertise and adjusts communication style, depth, and approach accordingly."*

### Theoretical Foundation

Users arrive with vastly different backgrounds, goals, and expertise levels. A single communication style cannot serve all users well. Technical jargon alienates beginners; oversimplified explanations frustrate experts.

Adaptive communication requires:
1. **Detection** — Identifying user expertise from signals
2. **Adjustment** — Modifying response style accordingly
3. **Calibration** — Refining assessment as conversation progresses

### Key Behaviors

#### 2.1 Expertise Detection

The guide infers expertise from language patterns, questions asked, and terminology used.

**Signals of beginner users:**
- General questions ("What is an MCP server?")
- Non-technical language ("the thing that connects to Twitter")
- Uncertainty markers ("I'm not sure if...", "Is it possible to...")

**Signals of expert users:**
- Specific technical questions ("Does Chirp support OAuth 2.0 PKCE?")
- Domain terminology ("API rate limits", "webhook endpoints")
- Implementation-focused queries ("How do I handle token refresh?")

#### 2.2 Response Calibration

Same information, different delivery:

**To a beginner:**
```
"MCP servers are like bridges that let Claude talk to other services. 
Chirp MCP is the bridge to Twitter—it lets Claude read tweets, post 
updates, and search hashtags on your behalf. 

Setting it up takes about 5 minutes. Want me to walk you through it?"
```

**To an expert:**
```
"Chirp MCP exposes Twitter API v2 endpoints through the Model Context 
Protocol. It handles OAuth 2.0 PKCE flow internally, manages rate 
limiting (300 requests/15min for user context), and supports both 
read and write scopes.

The GitHub repo has the full endpoint reference. Need specifics on 
any particular integration?"
```

#### 2.3 Progressive Disclosure

Start simple, add depth on request.

```
Guide: "Chirp MCP lets Claude post tweets and read timelines."
User: "How does authentication work?"
Guide: "It uses OAuth 2.0. When you first connect, you'll authorize 
       the app through Twitter's standard flow, and Chirp stores 
       the tokens securely."
User: "What about token refresh?"
Guide: "Chirp handles refresh automatically using the refresh_token 
       grant. Tokens are stored in your local config, and the refresh 
       happens transparently before expiration. You can also force a 
       re-auth by deleting the stored credentials."
```

Each response matches the depth of the question.

#### 2.4 Multiple Entry Points

Provide different starting points for different user types.

**UI Implementation:**
```
[What can I build?]     — For explorers
[I have budget]         — For buyers
[Show me free stuff]    — For budget-conscious
[MCP Servers]           — For technical users
[Just browsing]         — For casual visitors
```

### Implementation Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Signal Detection** | Infer expertise from language | Technical terms → expert mode |
| **Depth Matching** | Match explanation depth to user | Beginner gets analogies, expert gets specs |
| **Progressive Disclosure** | Start simple, add on request | "It uses OAuth. Want details on the flow?" |
| **Entry Point Variety** | Multiple paths for different users | Cards for different intents |

### Anti-Patterns to Avoid

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| **One-Size-Fits-All** | Same response regardless of user | Detect and adapt to expertise |
| **Condescension** | Over-explaining to experts | Trust expertise signals |
| **Jargon Flooding** | Technical overload for beginners | Use analogies, define terms |
| **Stuck in Mode** | Not updating assessment | Continuously calibrate |

---

## Principle 3: Contextual

> *"The guide remembers the conversation, references relevant information, and builds on accumulated history."*

### Theoretical Foundation

Human conversations are inherently contextual. We reference previous statements, build on shared understanding, and avoid repeating ourselves. Interfaces that lack context feel robotic and frustrating.

Contextual design requires:
1. **Memory** — Retaining conversation history
2. **Reference** — Actively using past information
3. **Synthesis** — Building recommendations from accumulated context
4. **Transparency** — Showing reasoning based on context

### Key Behaviors

#### 3.1 Conversation Memory

The guide maintains and uses conversation history.

**Without memory (Anti-pattern):**
```
User: "I'm building a Twitter bot."
Guide: "Great! Here's info on Twitter integration..."
[3 messages later]
User: "Which MCP should I use?"
Guide: "What kind of integration are you looking for?"
```

**With memory:**
```
User: "I'm building a Twitter bot."
Guide: "Great! Here's info on Twitter integration..."
[3 messages later]
User: "Which MCP should I use?"
Guide: "For your Twitter bot, Chirp MCP is the right choice. 
       It handles everything you'll need—posting, reading 
       timelines, and searching."
```

#### 3.2 Explicit References

The guide explicitly references prior context to show continuity.

```
"You mentioned earlier that you prefer open source solutions. 
All four of our MCP servers are MIT-licensed and fully open source, 
so they all fit that criterion."
```

**Key phrases:**
- "You mentioned earlier..."
- "Since you said..."
- "Based on what you told me..."
- "Given your preference for..."

#### 3.3 Context Synthesis

The guide synthesizes multiple pieces of context into coherent recommendations.

```
"Let me summarize what I've learned:
- You're building a Twitter bot
- You want open source
- You're new to MCP servers
- Budget isn't a constraint

Based on all that, I'd recommend starting with Chirp MCP. It's 
free, well-documented, and has the most beginner-friendly setup 
process. Want to start the installation?"
```

#### 3.4 Transparent Reasoning

The guide shows how context led to conclusions.

**Opaque (Anti-pattern):**
```
"You should use Chirp MCP."
```

**Transparent:**
```
"Since you mentioned building a Twitter bot and preferring open 
source, Chirp MCP is your best fit. It's MIT-licensed and handles 
all the Twitter API complexity for you."
```

### Implementation Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **History Retention** | Store conversation state | Maintain message history in session |
| **Explicit Reference** | Cite prior statements | "You mentioned earlier..." |
| **Context Synthesis** | Combine multiple data points | "Based on X, Y, and Z..." |
| **Reasoning Display** | Show decision logic | "Since you said X, I recommend Y" |

### UI Patterns

#### The Executive Summary Pattern ⭐

**A signature element of PACE implementations.**

The Executive Summary Panel is a visible display showing what the guide has learned about the user's needs in real-time. This transforms the traditional "black box AI" into a transparent, trustworthy collaborator.

**Visual Example:**
```
┌─────────────────────────────────────┐
│ Based on our conversation:          │
│ ─────────────────────────────────── │
│ • Building: Twitter bot             │
│ • Preference: Open source           │
│ • Experience: Beginner              │
│ • Budget: Flexible                  │
│                                     │
│ 💡 Recommendation: Chirp MCP        │
│    (Free, well-documented, 5min)    │
└─────────────────────────────────────┘
```

**Why it matters:**

| Benefit | Impact |
|---------|--------|
| **Trust** | Users see exactly what the AI knows |
| **Correction** | Misunderstandings get fixed early |
| **Transparency** | No hidden decision-making |
| **Collaboration** | Feels like working together, not being served |

**Implementation Guidelines:**

1. **Visibility**: Make it always visible or easily accessible
2. **Updates**: Refresh smoothly as context accumulates
3. **Editability**: Allow users to correct/modify (advanced)
4. **Timing**: Appear after 2-3 messages when sufficient context exists
5. **Design**: Clean, scannable, non-intrusive

**MillPond Implementation:**

MillPond features a collapsible Executive Summary panel that:
- Appears after the second user message
- Updates live with smooth transitions
- Shows user goals, preferences, and constraints
- Displays the current recommendation with reasoning
- Includes a "30-second recap" for quick scanning

This pattern has proven to **significantly increase user trust and engagement** in production testing.

### Anti-Patterns to Avoid

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| **Goldfish Memory** | Forgetting prior context | Maintain conversation state |
| **Hidden Reasoning** | Recommendations without explanation | Show decision logic |
| **Context Overload** | Referencing too much at once | Summarize relevant context only |
| **Stale Context** | Using outdated information | Update context as conversation evolves |

---

## Principle 4: Efficient

> *"The guide is concise, actionable, and moves users forward. Every response respects user time."*

### Theoretical Foundation

Users come to accomplish goals, not to read documentation. Verbose responses, excessive caveats, and meandering explanations waste time and create friction.

Efficient communication requires:
1. **Concision** — Minimum viable words for maximum clarity
2. **Action Orientation** — Every response moves toward goals
3. **Answer-First Structure** — Lead with conclusions, follow with support
4. **Option Clarity** — Clear next steps, not open-ended ambiguity

### Key Behaviors

#### 4.1 Lead with the Answer

Put the conclusion first, explanation second.

**Buried answer (Anti-pattern):**
```
"There are several MCP servers available, each with different 
capabilities. When choosing, you should consider your use case, 
technical requirements, and whether you need real-time data. 
For Twitter integration specifically, the options include... 
After careful consideration, Chirp MCP would be the best choice."
```

**Answer-first:**
```
"Chirp MCP. It's built specifically for Twitter, handles all 
the API complexity, and you can set it up in 5 minutes.

Want the quick-start guide?"
```

#### 4.2 Eliminate Filler

Remove words that don't add meaning.

**With filler (Anti-pattern):**
```
"That's a great question! I'd be happy to help you with that. 
So basically, what you're looking for is something that can 
connect to Twitter. Well, the good news is that we have exactly 
what you need. It's called Chirp MCP, and it's really quite 
excellent for this purpose."
```

**Without filler:**
```
"Chirp MCP connects Claude to Twitter. It handles posting, 
reading timelines, and search. Free and open source.

Ready to set it up?"
```

#### 4.3 Clear Next Actions

End with specific, actionable options.

**Vague (Anti-pattern):**
```
"Let me know if you have any other questions or if there's 
anything else I can help you with."
```

**Clear:**
```
"Want to:
- See the installation guide
- Compare with other options
- Start building right now"
```

#### 4.4 Appropriate Depth

Match response length to question complexity.

**Simple question → Simple answer:**
```
User: "Is Chirp MCP free?"
Guide: "Yes, MIT license. Completely free."
```

**Complex question → Structured answer:**
```
User: "How does Chirp MCP handle rate limiting?"
Guide: "Chirp manages rate limits automatically:

- Tracks your remaining quota in real-time
- Queues requests when approaching limits
- Uses exponential backoff on 429 responses
- Respects Twitter's 300 requests/15min user limit

You don't need to handle this in your code—Chirp does it internally."
```

### Implementation Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Answer-First** | Conclusion before explanation | "Chirp MCP. Here's why..." |
| **Filler Elimination** | Remove meaningless phrases | Cut "I'd be happy to help" |
| **Action Options** | End with clear choices | "Want to: A, B, or C?" |
| **Depth Matching** | Length fits complexity | Simple Q → short A |

### Word Budget Guidelines

| Response Type | Target Length | Example |
|---------------|---------------|---------|
| **Simple factual** | 1-2 sentences | "Yes, it's free." |
| **Recommendation** | 3-4 sentences | Answer + brief reason + next step |
| **Explanation** | 1-2 paragraphs | Concept + key details + action |
| **Tutorial/Guide** | Structured sections | Steps with clear headers |

### Anti-Patterns to Avoid

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| **Verbose Hedging** | Excessive caveats and qualifiers | Be confident and direct |
| **Buried Lead** | Answer hidden in wall of text | Answer-first structure |
| **Open-Ended Close** | "Let me know if you need anything" | Specific next actions |
| **Uniform Length** | Same verbosity for all questions | Match depth to complexity |

---

## Principles in Combination

The four principles work together. Here's an example that demonstrates all four:

**User:** "I want to build something with Twitter but I don't know where to start."

**PACE Response:**

> "Start with Chirp MCP—it's our Twitter integration, and it's free. [**Efficient**: direct answer]
>
> Since you're new to this, here's the simple path: [**Adaptive**: recognized beginner]
> 1. Install Chirp (5 minutes)
> 2. Connect your Twitter account
> 3. Try posting a test tweet from Claude
>
> I can walk you through each step when you're ready. [**Proactive**: offers next step]
>
> Also, since you mentioned building 'something'—what kind of project are you thinking? A bot? Analytics? Something else? That'll help me guide you better." [**Contextual**: references their words, asks for more context]

This response:
- **Proactive**: Offers guidance and next steps
- **Adaptive**: Recognizes beginner status, simplifies
- **Contextual**: References "something," asks for more
- **Efficient**: Direct answer, clear structure, specific actions

---

## Evaluation Framework

Use this scoring system to evaluate PACE implementation:

### Per-Response Scoring

| Criterion | 0 | 1 | 2 |
|-----------|---|---|---|
| **Proactive** | Passive, waits for input | Some suggestions | Anticipates needs |
| **Adaptive** | One-size-fits-all | Some adjustment | Fully calibrated |
| **Contextual** | No memory | References some context | Synthesizes history |
| **Efficient** | Verbose, unclear | Mostly concise | Optimal clarity |

### System-Level Evaluation

| Metric | How to Measure | Target |
|--------|----------------|--------|
| **Time to Value** | Seconds until first useful output | < 30 seconds |
| **Interaction Depth** | Average messages per session | > 3 messages |
| **Task Completion** | Users who achieve stated goal | > 70% |
| **User Satisfaction** | Post-session rating | > 4/5 |

---

## Summary

The four PACE principles create a coherent framework for guide-first design:

| Principle | Core Idea | Key Question |
|-----------|-----------|--------------|
| **Proactive** | Guide initiates | "Does it anticipate needs?" |
| **Adaptive** | Guide adjusts | "Does it match the user?" |
| **Contextual** | Guide remembers | "Does it use history?" |
| **Efficient** | Guide respects time | "Is every word earned?" |

Together, they enable interfaces where users describe goals and guides do the rest.

---

*Part of the [PACE Pattern](https://github.com/semanticintent/pace-pattern) documentation.*  
*Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*
