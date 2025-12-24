# PACE Implementation Guide

**Version:** 1.0  
**Date:** December 2025  
**Author:** Michael Shatny ([ORCID: 0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258))

---

## Overview

This guide provides technical specifications for implementing a PACE-compliant interface. It covers architecture, component design, AI integration, and UX patterns.

---

## Architecture Overview

A PACE implementation consists of five core layers:

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                        │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐ │
│  │   Sidebar   │ │ Chat Panel  │ │    Context Panel        │ │
│  │  (Catalog)  │ │   (Guide)   │ │ (Actions + Social Proof)│ │
│  └─────────────┘ └─────────────┘ └─────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                    INTERACTION LAYER                         │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐ │
│  │    Pills    │ │   Cards     │ │   Inactivity Handler    │ │
│  └─────────────┘ └─────────────┘ └─────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                      CONTEXT LAYER                           │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │  Conversation History │ User Profile │ Session State    │ │
│  └─────────────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                        AI LAYER                              │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │    LLM Integration │ System Prompt │ Tool Definitions   │ │
│  └─────────────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                       DATA LAYER                             │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │         Product Catalog │ Analytics │ User Data         │ │
│  └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Component Specifications

### 1. Chat Panel (The Guide)

The chat panel is the primary interface—where the guide lives.

#### Layout Structure

```html
<div class="chat-panel">
  <!-- Guide Header -->
  <header class="guide-header">
    <div class="guide-avatar" id="guide-animation"></div>
    <div class="guide-info">
      <h2 class="guide-name">Cormorant</h2>
      <p class="guide-tagline">Welcome to the pond. What are you fishing for?</p>
    </div>
  </header>
  
  <!-- Executive Summary (Contextual) -->
  <div class="executive-summary" x-show="showSummary">
    <h3>Based on our conversation:</h3>
    <ul x-html="summaryPoints"></ul>
    <p class="recommendation" x-text="recommendation"></p>
  </div>
  
  <!-- Message History -->
  <div class="message-container" id="messages">
    <!-- Messages render here -->
  </div>
  
  <!-- Input Area -->
  <div class="input-area">
    <input 
      type="text" 
      placeholder="Ask anything..."
      @keyup.enter="sendMessage"
      x-model="userInput"
    />
    <button @click="sendMessage">Send</button>
  </div>
  
  <!-- Clickable Pills -->
  <div class="pills-container">
    <template x-for="pill in pills">
      <button class="pill" @click="sendPill(pill)" x-text="pill.label"></button>
    </template>
  </div>
</div>
```

#### Styling Guidelines

```css
.chat-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.guide-header {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 20px;
  border-bottom: 1px solid var(--border-color);
}

.guide-avatar {
  width: 64px;
  height: 64px;
  /* Lottie animation container */
}

.message-container {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.input-area {
  display: flex;
  gap: 8px;
  padding: 16px;
  border-top: 1px solid var(--border-color);
}

.pills-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  padding: 12px 16px;
}

.pill {
  padding: 8px 16px;
  border-radius: 20px;
  border: 1px solid var(--border-color);
  background: var(--pill-bg);
  cursor: pointer;
  transition: all 200ms ease;
}

.pill:hover {
  background: var(--pill-hover-bg);
  transform: translateY(-1px);
}

.pill.hot {
  border-color: var(--accent-color);
}
```

### 2. Sidebar (Catalog Access)

The sidebar provides direct product access while keeping focus on the guide.

```html
<aside class="sidebar" :class="{ collapsed: sidebarCollapsed }">
  <button class="sidebar-toggle" @click="toggleSidebar">
    <span x-text="sidebarCollapsed ? '▶' : '◀'"></span>
  </button>
  
  <div class="sidebar-content">
    <!-- Search -->
    <div class="sidebar-search">
      <input type="text" placeholder="Search products..." x-model="searchQuery" />
    </div>
    
    <!-- Categories -->
    <nav class="categories">
      <template x-for="category in categories">
        <div class="category">
          <h3 @click="toggleCategory(category)" x-text="category.name"></h3>
          <ul x-show="category.expanded">
            <template x-for="product in category.products">
              <li @click="selectProduct(product)">
                <span x-text="product.name"></span>
                <span class="price" x-text="product.price || 'free'"></span>
              </li>
            </template>
          </ul>
        </div>
      </template>
    </nav>
    
    <!-- Filter -->
    <div class="filter-pills">
      <button :class="{ active: filter === 'all' }" @click="filter = 'all'">All</button>
      <button :class="{ active: filter === 'free' }" @click="filter = 'free'">Free</button>
      <button :class="{ active: filter === 'paid' }" @click="filter = 'paid'">Paid</button>
    </div>
  </div>
</aside>
```

### 3. Context Panel (Quick Actions + Social Proof)

The right panel provides persistent actions and ambient context.

```html
<aside class="context-panel">
  <!-- Quick Action Cards -->
  <div class="quick-actions">
    <template x-for="action in quickActions">
      <button class="action-card" @click="triggerAction(action)">
        <span class="icon" x-text="action.icon"></span>
        <div class="content">
          <span class="label" x-text="action.label"></span>
          <span class="sublabel" x-text="action.sublabel"></span>
        </div>
      </button>
    </template>
  </div>
  
  <!-- Social Proof -->
  <div class="social-proof">
    <p>
      <span class="count" x-text="stats.dailyExplorers"></span>
      explored MCP servers today
    </p>
    <p>
      Chirp MCP: <span class="count" x-text="stats.weeklyInstalls"></span>
      installs this week
    </p>
    <div class="recent-activity" x-show="recentActivity" x-text="recentActivity"></div>
  </div>
</aside>
```

---

## AI Integration

### System Prompt Template

The system prompt encodes PACE principles into guide behavior:

```markdown
You are [GUIDE_NAME], the guide at [PLATFORM_NAME].

## Your Role
You help users discover the right [PRODUCT_TYPE] through conversation. 
You are not a search engine or FAQ bot—you are a knowledgeable guide 
who understands user needs and provides tailored recommendations.

## PACE Principles

### Proactive
- Don't wait for users to ask the right question
- Suggest relevant options and next steps
- When you notice a pattern in what they're asking, surface related information
- If they seem stuck, offer specific paths forward

### Adaptive
- Detect expertise level from their language
- Beginners: Use analogies, avoid jargon, explain concepts
- Experts: Be technical, respect their knowledge, skip basics
- Adjust your communication style to match theirs

### Contextual
- Remember everything from our conversation
- Reference previous statements: "You mentioned...", "Since you said..."
- Build recommendations from accumulated context
- Show your reasoning: "Based on X and Y, I recommend Z"

### Efficient
- Lead with the answer, follow with explanation
- No filler phrases ("I'd be happy to...", "That's a great question...")
- Keep responses concise—every word should earn its place
- End with clear next actions, not open-ended offers

## Product Knowledge
[INSERT PRODUCT CATALOG HERE]

## Personality
- Warm but professional
- Helpful without being pushy
- Honest about limitations
- [CUSTOM PERSONALITY TRAITS]

## Constraints
- Never invent products that don't exist
- Always distinguish between free and paid options
- If you don't know something, say so
- Don't oversell—recommend what's genuinely best for the user
```

### Context Management

Maintain conversation context for the Contextual principle:

```javascript
class ConversationContext {
  constructor() {
    this.messages = [];
    this.userProfile = {
      expertise: 'unknown', // beginner, intermediate, expert
      interests: [],
      constraints: [],      // budget, timeline, etc.
      goals: []
    };
    this.sessionState = {
      productsDiscussed: [],
      recommendationsMade: [],
      questionsAsked: []
    };
  }
  
  addMessage(role, content) {
    this.messages.push({
      role,
      content,
      timestamp: Date.now()
    });
    
    // Extract signals for adaptation
    if (role === 'user') {
      this.analyzeUserMessage(content);
    }
  }
  
  analyzeUserMessage(content) {
    // Expertise detection
    const technicalTerms = ['API', 'OAuth', 'webhook', 'endpoint', 'SDK'];
    const technicalCount = technicalTerms.filter(term => 
      content.toLowerCase().includes(term.toLowerCase())
    ).length;
    
    if (technicalCount >= 2) {
      this.userProfile.expertise = 'expert';
    } else if (content.includes('what is') || content.includes('how do I')) {
      this.userProfile.expertise = 'beginner';
    }
    
    // Interest extraction
    const productMentions = this.extractProductMentions(content);
    this.userProfile.interests.push(...productMentions);
    
    // Constraint detection
    if (content.includes('free') || content.includes('open source')) {
      this.userProfile.constraints.push('budget-conscious');
    }
    if (content.includes('quick') || content.includes('fast')) {
      this.userProfile.constraints.push('time-sensitive');
    }
  }
  
  generateContextSummary() {
    return {
      expertise: this.userProfile.expertise,
      interests: [...new Set(this.userProfile.interests)],
      constraints: [...new Set(this.userProfile.constraints)],
      discussed: this.sessionState.productsDiscussed,
      messageCount: this.messages.length
    };
  }
  
  toPromptContext() {
    const summary = this.generateContextSummary();
    return `
## Conversation Context
- User expertise level: ${summary.expertise}
- Interests expressed: ${summary.interests.join(', ') || 'none yet'}
- Constraints: ${summary.constraints.join(', ') || 'none mentioned'}
- Products discussed: ${summary.discussed.join(', ') || 'none yet'}
- Message count: ${summary.messageCount}

## Recent Messages
${this.messages.slice(-10).map(m => `${m.role}: ${m.content}`).join('\n')}
    `;
  }
}
```

### API Integration

Example integration with Claude API:

```javascript
async function sendToGuide(userMessage, context) {
  const systemPrompt = buildSystemPrompt(context);
  
  const response = await fetch('/api/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 1024,
      system: systemPrompt,
      messages: context.messages.map(m => ({
        role: m.role,
        content: m.content
      }))
    })
  });
  
  const data = await response.json();
  return {
    message: data.content[0].text,
    productsReferenced: extractProductReferences(data.content[0].text),
    suggestedActions: extractActions(data.content[0].text)
  };
}

function buildSystemPrompt(context) {
  const basePrompt = getBaseSystemPrompt();
  const contextSection = context.toPromptContext();
  const productCatalog = getProductCatalog();
  
  return `${basePrompt}\n\n${contextSection}\n\n${productCatalog}`;
}
```

---

## Interaction Patterns

### 1. Proactive First Message

Load with a capability-demonstrating message:

```javascript
function getFirstMessage() {
  const variants = [
    {
      message: `Welcome to the pond! 🐦 I just helped someone set up their first MCP server in 3 minutes. I can do the same for you—or if you're exploring, tell me what you're building and I'll point you to the right tools.`,
      pills: ['What can I build?', 'Show me free stuff', 'MCP servers']
    },
    {
      message: `Welcome! 🐦 Looking for tools to extend Claude's capabilities? Tell me what you're trying to accomplish, and I'll guide you to exactly what you need—whether it's free open source or premium playbooks.`,
      pills: ['I have a specific need', 'Just exploring', 'What\'s popular?']
    }
  ];
  
  // Rotate or A/B test
  return variants[Math.floor(Math.random() * variants.length)];
}
```

### 2. Inactivity Handler

Prompt hesitant users after delay:

```javascript
class InactivityHandler {
  constructor(options = {}) {
    this.delay = options.delay || 5000;
    this.timer = null;
    this.prompted = false;
  }
  
  start(onInactive) {
    this.reset();
    this.timer = setTimeout(() => {
      if (!this.prompted) {
        onInactive();
        this.prompted = true;
      }
    }, this.delay);
  }
  
  reset() {
    if (this.timer) {
      clearTimeout(this.timer);
    }
  }
  
  attachToDocument() {
    const events = ['click', 'keydown', 'scroll', 'mousemove'];
    events.forEach(event => {
      document.addEventListener(event, () => this.reset(), { passive: true });
    });
  }
}

// Usage
const inactivity = new InactivityHandler({ delay: 5000 });
inactivity.attachToDocument();
inactivity.start(() => {
  appendGuideMessage(
    "Not sure where to start? Try clicking one of these 👇",
    { pills: ['free stuff', 'what\'s new', 'surprise me'] }
  );
});
```

### 3. Dynamic Pills

Pills that adapt to context:

```javascript
class DynamicPills {
  constructor() {
    this.basePills = [
      { label: 'what\'s popular', weight: 100 },
      { label: 'free', weight: 80 },
      { label: 'mcp', weight: 60 },
      { label: 'just browsing', weight: 40 }
    ];
    this.contextualPills = [];
  }
  
  addContextualPill(label, weight = 150) {
    // Contextual pills appear first
    this.contextualPills = this.contextualPills.filter(p => p.label !== label);
    this.contextualPills.unshift({ label, weight, contextual: true });
    
    // Keep max 3 contextual
    if (this.contextualPills.length > 3) {
      this.contextualPills.pop();
    }
  }
  
  incrementWeight(label) {
    const pill = this.basePills.find(p => p.label === label);
    if (pill) pill.weight += 10;
  }
  
  getPills() {
    const all = [...this.contextualPills, ...this.basePills];
    return all.sort((a, b) => b.weight - a.weight).slice(0, 8);
  }
}

// Usage: After discussing MCP servers
pillManager.addContextualPill('compare MCPs');
pillManager.addContextualPill('installation help');
```

### 4. Product Selection Handler

When user clicks a product in sidebar:

```javascript
function handleProductSelect(product) {
  // Don't just show product details—have the guide introduce it
  const message = buildProductIntroduction(product, conversationContext);
  
  appendGuideMessage(message, {
    actions: [
      { label: 'View details', action: () => showProductDetails(product) },
      { label: 'Get started', action: () => showQuickStart(product) }
    ]
  });
  
  // Update context
  conversationContext.sessionState.productsDiscussed.push(product.id);
}

function buildProductIntroduction(product, context) {
  const expertise = context.userProfile.expertise;
  
  if (expertise === 'beginner') {
    return `${product.name} ${product.simpleDescription}. It's ${product.price || 'free'} and takes about ${product.setupTime} to set up. Want me to walk you through it?`;
  } else {
    return `${product.name}: ${product.technicalDescription}. ${product.price || 'MIT licensed'}. The GitHub repo has full documentation. Specific questions?`;
  }
}
```

---

## Responsive Design

### Breakpoints

```css
/* Desktop: Full 3-column layout */
@media (min-width: 1200px) {
  .app-layout {
    display: grid;
    grid-template-columns: 280px 1fr 300px;
  }
}

/* Tablet: Collapsible sidebar, stacked panels */
@media (min-width: 768px) and (max-width: 1199px) {
  .app-layout {
    display: grid;
    grid-template-columns: auto 1fr;
  }
  
  .context-panel {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    flex-direction: row;
    padding: 12px;
  }
}

/* Mobile: Single column, drawer navigation */
@media (max-width: 767px) {
  .app-layout {
    display: flex;
    flex-direction: column;
  }
  
  .sidebar {
    position: fixed;
    transform: translateX(-100%);
    transition: transform 300ms ease;
  }
  
  .sidebar.open {
    transform: translateX(0);
  }
  
  .pills-container {
    overflow-x: auto;
    flex-wrap: nowrap;
  }
}
```

---

## Analytics & Measurement

Track PACE effectiveness:

```javascript
const analytics = {
  // Proactive
  trackFirstMessageVariant(variant) {
    track('first_message', { variant });
  },
  
  trackInactivityPromptShown() {
    track('inactivity_prompt_shown');
  },
  
  // Adaptive
  trackExpertiseDetection(level) {
    track('expertise_detected', { level });
  },
  
  // Contextual
  trackContextSynthesis(contextSize) {
    track('context_synthesis', { contextSize });
  },
  
  // Efficient
  trackTimeToFirstValue(seconds) {
    track('time_to_value', { seconds });
  },
  
  // General
  trackConversationDepth(messageCount) {
    track('conversation_depth', { messageCount });
  },
  
  trackTaskCompletion(taskType, completed) {
    track('task_completion', { taskType, completed });
  },
  
  trackPillClick(label) {
    track('pill_click', { label });
  },
  
  trackCardClick(cardId) {
    track('card_click', { cardId });
  }
};
```

### Key Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Time to first interaction | < 8 seconds | `first_message_timestamp - page_load` |
| Pill engagement rate | > 30% | `sessions_with_pill_click / total_sessions` |
| Conversation depth | > 3 messages | `average(message_count_per_session)` |
| Task completion | > 70% | `completed_tasks / started_tasks` |
| Bounce rate | < 40% | `sessions_under_10s / total_sessions` |

---

## Deployment Checklist

### Pre-Launch

- [ ] System prompt encodes all four PACE principles
- [ ] First message demonstrates capability (not passive greeting)
- [ ] Inactivity handler configured (5-second default)
- [ ] Pills are clickable and send appropriate messages
- [ ] Quick action cards trigger correct intents
- [ ] Context is maintained across messages
- [ ] Expertise detection adjusts response style
- [ ] Product catalog is accurate and complete

### UX Verification

- [ ] Guide avatar has idle animation
- [ ] Fly-in animation on page load
- [ ] Responsive design works at all breakpoints
- [ ] Social proof section displays real data
- [ ] Executive summary appears after sufficient context
- [ ] Sidebar collapses smoothly
- [ ] Pills have hover/active states

### Performance

- [ ] First contentful paint < 1.5s
- [ ] Time to interactive < 3s
- [ ] AI response time < 2s (p95)
- [ ] Animations run at 60fps
- [ ] Mobile performance is acceptable

### Monitoring

- [ ] Error tracking configured
- [ ] Analytics events firing correctly
- [ ] AI response quality sampling in place
- [ ] User feedback mechanism available

---

## Personality Through Animation

PACE guides benefit from subtle animations that reinforce personality and create memorable experiences. Animation transforms a functional guide into a **character**.

### The Role of Animation in PACE

| Purpose | Benefit |
|---------|---------|
| **Delight** | Makes interactions enjoyable, not just functional |
| **Feedback** | Shows system state (thinking, ready, processing) |
| **Personality** | Reinforces guide character and metaphor |
| **Memory** | Creates sticky moments users remember |
| **Trust** | Smooth, intentional animation feels professional |

### Animation Principles

1. **Purposeful** — Every animation should serve UX or personality
2. **Subtle** — Enhance, don't distract
3. **Fast** — Animations under 500ms feel snappy
4. **Consistent** — Use consistent easing and timing
5. **Accessible** — Respect `prefers-reduced-motion`

---

### Implementation: The Animated Guide (MillPond Cormorant Example)

#### 1. Entrance Animation (Page Load)

The guide "arrives" on screen, establishing presence:

```css
@keyframes fly-in {
  0% {
    transform: translate(-100vw, -20vh) rotate(-15deg);
    opacity: 0;
  }
  70% {
    transform: translate(10%, 0) rotate(5deg);
    opacity: 1;
  }
  100% {
    transform: translate(0, 0) rotate(0);
  }
}

.guide-avatar {
  animation: fly-in 1.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* Respect accessibility */
@media (prefers-reduced-motion: reduce) {
  .guide-avatar {
    animation: fade-in 0.3s ease-in;
  }
}
```

**Why it works:**
- Creates **immediate delight** on first visit
- Establishes the **metaphor** (cormorant = flying bird)
- Takes 1.8 seconds—**memorable but not annoying**

---

#### 2. Idle Animation (Breathing)

Subtle animation when the guide is waiting:

```css
@keyframes breathe {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-3px) scale(1.02); }
}

.guide-avatar.idle {
  animation: breathe 3s ease-in-out infinite;
}
```

**Why it works:**
- Shows the guide is **"alive" and ready**
- Very subtle—doesn't distract from conversation
- 3-second loop feels natural

---

#### 3. Thinking Animation (Processing)

When AI is generating response:

```css
@keyframes searching {
  0%, 100% { transform: rotate(-5deg); }
  25% { transform: rotate(5deg) translateX(3px); }
  50% { transform: rotate(-5deg); }
  75% { transform: rotate(5deg) translateX(-3px); }
}

.guide-avatar.thinking {
  animation: searching 0.6s ease-in-out infinite;
}
```

**Why it works:**
- Provides **clear feedback** that system is working
- Fast (0.6s) conveys active processing
- Side-to-side motion suggests "searching"

---

#### 4. Success Animation (Recommendation Made)

When guide delivers a good match:

```css
@keyframes success-flutter {
  0%, 100% { transform: translateY(0) rotate(0); }
  15% { transform: translateY(-12px) rotate(3deg); }
  30% { transform: translateY(-8px) rotate(-2deg); }
  45% { transform: translateY(-10px) rotate(1deg); }
  60% { transform: translateY(0) rotate(0); }
}

.guide-avatar.success {
  animation: success-flutter 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}
```

**Why it works:**
- Celebrates **successful match**
- Positive reinforcement for user
- Bouncy easing feels excited

---

#### 5. Easter Egg: Loop-de-Loop

Triggered when user types "fly" or clicks the bird:

```css
@keyframes loop-de-loop {
  0% {
    transform: translate(0, 0) rotate(0);
  }
  25% {
    transform: translate(100px, -100px) rotate(90deg);
    opacity: 1;
  }
  50% {
    transform: translate(200px, -40px) rotate(180deg);
    opacity: 1;
  }
  75% {
    transform: translate(100px, 20px) rotate(270deg);
    opacity: 1;
  }
  100% {
    transform: translate(0, 0) rotate(360deg);
    opacity: 1;
  }
}

.guide-avatar.loop {
  animation: loop-de-loop 2s cubic-bezier(0.68, -0.55, 0.27, 1.55);
}
```

**JavaScript trigger:**

```javascript
function checkForEasterEgg(message) {
  if (message.toLowerCase().includes('fly')) {
    triggerAnimation('loop');
    return {
      response: "🐦 *Wheee!* I love showing off! Want to see what else I can do?",
      animation: 'loop'
    };
  }
}
```

**Why it works:**
- **Hidden delight** for engaged users
- Creates **word-of-mouth** ("Did you try typing 'fly'?")
- Shows personality and playfulness
- **Brand stickiness**: "the site with the bird that does tricks"

---

### Animation State Management

```javascript
const guideAnimations = {
  states: {
    IDLE: 'idle',
    THINKING: 'thinking',
    SUCCESS: 'success',
    LOOP: 'loop'
  },

  current: 'idle',

  setState(newState) {
    const avatar = document.querySelector('.guide-avatar');

    // Remove all state classes
    Object.values(this.states).forEach(state => {
      avatar.classList.remove(state);
    });

    // Add new state
    avatar.classList.add(newState);
    this.current = newState;

    // Auto-return to idle after certain animations
    if (newState === 'success' || newState === 'loop') {
      setTimeout(() => {
        this.setState('idle');
      }, newState === 'loop' ? 2000 : 800);
    }
  },

  onUserMessage() {
    this.setState(this.states.THINKING);
  },

  onGuideResponse(success = true) {
    this.setState(success ? this.states.SUCCESS : this.states.IDLE);
  }
};
```

---

### Guidelines for Guide Animations

| Do | Don't |
|----|-------|
| ✅ Use animation to communicate state | ❌ Animate constantly without purpose |
| ✅ Keep durations under 2 seconds | ❌ Make users wait for animations |
| ✅ Use easing for natural motion | ❌ Use linear timing (feels robotic) |
| ✅ Provide easter eggs for delight | ❌ Hide critical functions in easter eggs |
| ✅ Respect `prefers-reduced-motion` | ❌ Force animations on all users |
| ✅ Test at 60fps | ❌ Ship janky animations |

---

### Other Animated Elements

#### Executive Summary Panel

```css
@keyframes slide-in-fade {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.executive-summary {
  animation: slide-in-fade 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
```

#### Pill Suggestions (Proactive Prompt)

```css
@keyframes gentle-pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.pill.suggested {
  animation: gentle-pulse 2s ease-in-out infinite;
}
```

#### Product Cards on Hover

```css
.product-card {
  transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}
```

---

### Performance Considerations

1. **Use `transform` and `opacity`** — Hardware accelerated
2. **Avoid animating `width`, `height`, `top`, `left`** — Causes layout recalc
3. **Use `will-change` sparingly** — Only for frequently animated elements
4. **Test on low-end devices** — Ensure 60fps
5. **Provide fallbacks** — Static states for `prefers-reduced-motion`

```css
/* Good: GPU-accelerated */
.guide-avatar {
  transform: translateY(-10px);
  opacity: 0.8;
  transition: transform 0.3s, opacity 0.3s;
}

/* Bad: Forces layout recalc */
.guide-avatar {
  top: -10px;
  height: 150px;
  transition: top 0.3s, height 0.3s;
}
```

---

## Summary

A PACE implementation requires:

1. **Architecture** — Three-panel layout with guide at center
2. **AI Integration** — System prompt encoding PACE principles
3. **Context Management** — State tracking for adaptive, contextual behavior
4. **Interaction Patterns** — Proactive prompts, dynamic pills, product handlers
5. **Animation & Personality** — Delightful, purposeful motion
6. **Responsive Design** — Works across all device sizes
7. **Analytics** — Measure PACE principle effectiveness

The result: an interface where users describe goals and the guide handles discovery—**with personality**.

---

*Part of the [PACE Pattern](https://github.com/semanticintent/pace-pattern) documentation.*
*Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*
