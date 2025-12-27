# PACE: Pattern for Agentic Conversational Experience

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18049371.svg)](https://doi.org/10.5281/zenodo.18049371)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Demo](https://img.shields.io/badge/demo-PACE%201.0.1-blue)](https://demo.millpond.dev)
[![Production](https://img.shields.io/badge/production-MillPond-green)](https://millpond.dev)

**A framework for guide-first interfaces where AI leads users through conversation rather than traditional navigation.**

> *"Don't make users hunt. Let the guide fish for them."*

---

## Abstract

PACE (Pattern for Agentic Conversational Experience) is a design framework that inverts the traditional relationship between users and interfaces. Instead of presenting catalogs, menus, or navigation hierarchies for users to browse, PACE implementations use an AI guide to lead users through conversation toward their goals.

The framework defines four behavioral principles that form a recursive acronym:

- **P**roactive — The guide initiates, suggests, and anticipates
- **A**daptive — The guide matches user expertise and adjusts communication
- **C**ontextual — The guide remembers, references, and builds on history
- **E**fficient — The guide is concise, actionable, and moves users forward

**Author:** Michael Shatny  
**ORCID:** [0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258)  
**Date:** December 2025  
**License:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

---

## The Problem PACE Solves

Traditional digital interfaces—especially in commerce, documentation, and enterprise software—operate on a **browse-and-hunt** paradigm:

```
Traditional Interface:
┌─────────────────────────────────────┐
│  Here's everything we have.        │
│  Good luck finding what you need.  │
│                                     │
│  [Grid of 47 products]              │
│  [12 filter options]                │
│  [Search box]                       │
│                                     │
│  User must: Learn taxonomy          │
│             Know what to search     │
│             Browse exhaustively     │
└─────────────────────────────────────┘
```

This places cognitive burden on users who may not know what they're looking for, don't understand the taxonomy, or simply want guidance rather than options.

PACE inverts this relationship:

```
PACE Interface:
┌─────────────────────────────────────┐
│  🐦 Guide                           │
│                                     │
│  "Welcome! I just helped someone    │
│   solve a similar problem in 3     │
│   minutes. Tell me what you're     │
│   building, and I'll point you     │
│   to exactly what you need."       │
│                                     │
│  [Ask anything...]                  │
│                                     │
│  User must: Describe their goal    │
│  Guide does: Everything else       │
└─────────────────────────────────────┘
```

---

## The Recursive Acronym

PACE works on two complementary levels:

### Layer 1: The Framework (What)

> **P**attern for **A**gentic **C**onversational **E**xperience

This defines what PACE is: a design pattern for building AI-guided interfaces.

### Layer 2: The Principles (How)

> **P**roactive, **A**daptive, **C**ontextual, **E**fficient

This defines how PACE implementations should behave.

The same four letters. Two complementary meanings. Self-documenting.

---

## The Four Principles

### Proactive

The guide doesn't wait for users to figure out what to do.

**Traditional:** "How can I help you?"  
**PACE:** "I just helped someone set up their first integration in 3 minutes. I can do the same for you—or if you're exploring, tell me what you're building."

**Implementation:**
- First message demonstrates capability
- Suggests next steps before being asked
- Auto-prompts after inactivity
- Surfaces relevant options proactively

### Adaptive

The guide matches the user's expertise level and adjusts communication accordingly.

**To a beginner:** "MCP servers let Claude connect to external services. Think of them as bridges between Claude and the tools you already use."

**To an expert:** "Perch MCP exposes D1 database introspection APIs. Supports semantic anchoring, schema analysis, and query optimization—ideal for performance debugging."

**Implementation:**
- Detects expertise from language patterns
- Adjusts technical depth dynamically
- Offers appropriate entry points
- Never condescends or overwhelms

### Contextual

The guide remembers the conversation and references relevant information.

**Without context:** "Here are all our products..."
**With context:** "Since you mentioned slow database queries and using D1, Perch MCP is your best option. It analyzes your schema and suggests index optimizations automatically."

**Implementation:**
- Maintains conversation history
- References previous statements
- Builds recommendations on accumulated context
- Shows reasoning ("Based on what you told me...")

### Efficient

The guide is concise and actionable. Every response moves the user forward.

**Inefficient:** "There are many considerations when choosing an MCP server. First, you should think about your use case. Then, consider the technical requirements. After that..."

**Efficient:** "For database performance: Perch MCP. Analyzes your D1 schema, suggests indexes, free. Want to scan your database now?"

**Implementation:**
- No filler, no excessive caveats
- Leads with the answer
- Offers clear next actions
- Respects user's time

---

## Design Philosophy

### Zero-Friction Onboarding

Users should never feel like they're learning an interface. The interaction should feel natural—like talking to a knowledgeable friend.

### Discovery Disguised as Play

Exploration should be enjoyable, not exhausting. The guide makes finding things feel like a conversation, not a chore.

### Honest Guidance

PACE implementations guide users to what's genuinely best for them—including free options when appropriate. The goal is trust, not conversion.

### Permission Slips

Interactive elements (pills, cards, suggestions) give users permission to engage. They lower the barrier to first interaction by signaling "you can click this safely."

---

## Reference Implementation: MillPond

MillPond is the first implementation of the PACE pattern—a conversational storefront where an AI guide named **Cormorant** helps users discover tools and products.

- **Interactive Demo:** [demo.millpond.dev](https://demo.millpond.dev) - PACE 1.0.1 pattern showcase
- **Production Site:** [millpond.dev](https://millpond.dev) - Live storefront with AI chat

### Latest Features (December 2025)

| Feature | Description | Key Benefit |
|---------|-------------|-------------|
| **Executive Summary Panel** ⭐ | Live display of what the guide knows | Transparency & trust |
| **Animated Cormorant** 🐦 | Flying entrance + thinking states + easter eggs | Memorable brand experience |
| **Dynamic Pills** | Context-aware interaction shortcuts | Low-friction engagement |
| **Social Proof** | Real-time activity signals | Validation & confidence |
| **Inactivity Prompts** | Gentle nudges after 5s of hesitation | Proactive assistance |

*Note: MillPond is in early deployment. See [METRICS.md](METRICS.md) for projected impact data.*

### The Metaphor

MillPond uses a consistent foraging metaphor inspired by watching cormorants fish at Mill Pond Park, Richmond Hill, Ontario:

- **Cormorant** — The guide (a diving bird that hunts with purpose)
- **Pond** — The storefront environment
- **Fishing** — Discovering products through conversation
- **Dive** — Deep exploration of user needs
- **Surface** — Delivering the right solution

**The greeting:** *"Welcome to the pond. What are you fishing for?"*

This biological metaphor is [documented in detail](INSPIRATION.md) with research citations.

---

## PACE vs. Existing Patterns

| Pattern | Focus | PACE Difference |
|---------|-------|-----------------|
| **Conversational Commerce** | Adding chat to shopping | PACE: Conversation IS the interface |
| **Chatbot UX** | Bot interaction design | PACE: Guide-first, not support-first |
| **Agentic UX** | AI autonomy patterns | PACE: Specific principles for commerce/discovery |
| **Virtual Assistants** | General AI helpers | PACE: Named persona, honest guidance |
| **Guided Selling** | Sales-driven flows | PACE: User-goal-driven, includes free options |

---

## Implementation Checklist

Use this checklist to evaluate whether an interface implements PACE:

### Proactive
- [ ] First message demonstrates capability (not just greets)
- [ ] Guide suggests next steps without being asked
- [ ] Inactive users receive gentle prompts
- [ ] Relevant options surface proactively

### Adaptive
- [ ] Technical depth adjusts to user expertise
- [ ] Multiple entry points for different user types
- [ ] Language matches user's communication style
- [ ] Never condescends or overwhelms

### Contextual
- [ ] Conversation history is maintained
- [ ] Previous statements are referenced
- [ ] Recommendations build on accumulated context
- [ ] Reasoning is transparent ("Based on...")

### Efficient
- [ ] Responses lead with answers
- [ ] No filler or excessive caveats
- [ ] Clear next actions are offered
- [ ] User time is respected

---

## Connection to Semantic Intent

PACE is part of the broader **Semantic Intent** philosophy, which emphasizes clarity before code and intent before implementation.

The relationship:

```
Semantic Intent (Philosophy)
├── "Clarity before code"
├── "Intent before implementation"
└── Natural language as source of truth
          ↓
    PACE Pattern (Framework)
    ├── Pattern for Agentic Conversational Experience
    ├── Proactive, Adaptive, Contextual, Efficient
    └── Guide-first interaction design
              ↓
        MillPond (Implementation)
        ├── Cormorant as guide
        ├── Conversation over catalog
        └── "Ask, don't browse"
```

For more on Semantic Intent, see: [Semantic Intent as Single Source of Truth](https://semanticintent.dev/papers/semantic-intent-ssot) (DOI: [10.5281/zenodo.17114972](https://doi.org/10.5281/zenodo.17114972))

---

## Repository Structure

```
pace-pattern/
├── README.md              # This file - pattern overview
├── PRINCIPLES.md          # Deep dive on P-A-C-E principles
├── IMPLEMENTATION.md      # Technical implementation guide with code
├── INSPIRATION.md         # Biological foundations (cormorant foraging)
├── METRICS.md             # Framework for measuring PACE effectiveness
├── CONTRIBUTING.md        # How to participate in PACE development
├── CITATION.cff           # Machine-readable citation metadata
├── LICENSE                # CC BY 4.0 license
├── CHANGELOG.md           # Version history
└── examples/
    └── millpond/          # Reference implementation case study
```

---

## Citation

If you use or reference PACE in your work, please cite:

### APA Style

Shatny, M. (2025). PACE: Pattern for Agentic Conversational Experience. GitHub. https://github.com/semanticintent/pace-pattern. ORCID: 0009-0006-2011-3258

### BibTeX

```bibtex
@misc{shatny2025pace,
  author = {Shatny, Michael},
  title = {PACE: Pattern for Agentic Conversational Experience},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/semanticintent/pace-pattern},
  note = {ORCID: 0009-0006-2011-3258}
}
```

---

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to:
- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material for any purpose

Under the following terms:
- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made.

---

## Author

**Michael Shatny**  
Ontario, Canada  
ORCID: [0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258)

Part of the [Semantic Intent](https://semanticintent.dev) ecosystem.

---

*"Welcome to the pond. What are you fishing for?"* 🐦
