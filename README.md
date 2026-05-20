# Create New Agent — Empty/No-KB Agent Flow

PXO Workflow Analysis and design specification for the V3 "No Knowledge Base" agent creation path on CustomGPT.ai.

## Problem

V3 agents don't require a knowledge base. The old "Pick data source" page had no first-class path for creating an empty agent — only a buried API icon at the bottom. Downstream states (Ask page blocked, preview erroring, deployments redirecting) actively punished anyone who landed there.

## Solution

Surface "No Knowledge Base" as a named, explained, first-class card in the **Most Popular** row of the data source picker — with an inline explainer that sets accurate expectations before the user commits.

---

## PXO Analysis

### Step 00 — Problem Hypothesis

**Refined Problem Statement**
V3 introduces a new agent paradigm where the knowledge base is optional — agents can rely entirely on model intelligence, tool calls, and system prompts. The current "Pick data source" page has no first-class path for this. The only escape hatch (API icon, buried at the bottom) is invisible to most users, and the downstream experience (Ask page blocked, preview erroring, deployments redirecting) actively punishes anyone who lands there accidentally. We need to surface "no KB" as a legitimate, intentional first-class choice — with copy and flow that sets accurate expectations before the user commits.

**Impacted User Segment**
- Developers and technical builders who want a pure LLM agent (tool-calling, reasoning, API-backed, no corpus)
- Power users building workflow agents, orchestrators, or multi-step agentic pipelines
- Early V3 adopters who don't yet have content to upload but want to start configuring

**Business Risk**
Invisible path → missed activation for a growing segment. If the only users who discover "empty agent" are those who already know to look for the API icon, CustomGPT loses the developer/agentic wedge to competitors who make tool-only agents a primary surface.

**Metric at Risk**
Agent creation conversion rate (specifically, drop-off between "Pick data source" page and first agent configuration step). Secondary: 7-day retention of newly created agents with no KB.

**Current User Alternatives**
- Accidentally create an agent with a dummy URL and delete it later
- Use the API icon without understanding what it does
- Abandon CustomGPT for a tool where "no KB" is the default

**Why Now**
V3 shipped an agentic model that decouples agents from corpora. The old gating logic (no KB = blocked UX) is now a product mismatch that will actively confuse V3 users who arrive expecting agent-first behavior.

**Validated Hypothesis**
If we surface "Start without a knowledge base" as a named, explained first-class option in the Most Popular row, for developers and agentic builders, then agent creation conversion will increase and 7-day retention of empty agents will improve, because users will have accurate expectations about what they're creating and won't be surprised by a blocked Ask page.

**Risk Level: Medium**
Low engineering cost (the path already exists). High copy/framing risk — if the card is misread as "broken" or "incomplete," naive users will avoid it or feel confused. The UX burden is almost entirely on clarity of language and post-selection onboarding.

---

### Step 01 — Empathy Engine

**Think**
- "Do I need to upload files before I can use this? What if I don't have any?"
- "What does 'API' mean in this context? Is that for developers only?"
- "Is this an agent I can actually deploy, or is it a placeholder until I add content?"
- "Why is 'no knowledge base' not listed here — is it not supported?"

**Feel**
- **Confusion**: The page is framed as "Pick a data source" — the entire mental model is "I need data." An option to have no data feels like opting out of the product.
- **Anxiety (naive users)**: "Am I doing this wrong if I don't pick something?"
- **Excitement (power users)**: "I just want to write a system prompt and tool config — stop making me pretend I have a KB."
- **Distrust**: Discovering the API icon by accident and not knowing if it will give them a broken agent.

**Say**
- "Why do I have to pick a data source? I just want a chatbot."
- "Is there a way to skip this step?"
- "What's the API option? Is that for connecting via API or is it the agent type?"
- "I created an agent without content and now it won't let me chat — what did I break?"

**Do**
- Scroll to the bottom looking for a "skip" or "none" option
- Accidentally click "File Upload" and then close the modal
- Search "none" or "skip" in the search bar
- Close the tab and try again later

**Hidden Frictions**
- The page title "Pick data source" presupposes data is required — it primes users to feel like they're doing something wrong if they don't select one
- No visible "Skip" or "Continue without data" affordance
- The API icon at the bottom is unlabeled contextually — users don't know if selecting it creates a functional agent

**Emotional Risks**
- A naive user who selects "no KB" and hits the old blocked Ask page will feel the product is broken — high churn risk at day 1
- A developer who finds the API icon but can't verify it works before committing will hesitate

**Trust Barriers**
- "Is this a real agent or a sandbox?"
- "Will my users (on deployments) get a good experience if there's no KB?"
- "Can I add a KB later, or is this permanent?"

---

### Step 02 — JTBD Extraction

**Core Functional Job**
Create an agent that responds intelligently without requiring a static knowledge base — using model capabilities, tool calls, a system prompt, and/or external APIs as the primary intelligence layer.

**Emotional Job**
Feel like a capable builder who's in control of the agent's design from the start — not someone who has to upload a dummy file just to get past a gating screen.

**Social Job**
Ship a V3 agentic product to stakeholders or end-users and have it described as "AI-powered" — not "a chatbot trained on our docs." The distinction signals technical sophistication.

**Job Steps (chronological)**
1. Decide to build an agent with no static content (deliberate choice, not ignorance)
2. Navigate to Create page
3. Recognize that "no knowledge base" is a supported, valid option
4. Understand what they're getting: model-only, tool-callable, system-prompt-driven
5. Confirm the choice and proceed to agent configuration
6. Configure system prompt, tools, model settings
7. Test the agent via preview
8. Deploy or share

**Success Definition**
User selects the empty-agent path, understands the tradeoff before clicking, lands in the config screen with the right mental model, and has a working preview within 10 minutes.

**Failure Definition**
User selects empty-agent path without understanding it → hits a broken-looking state downstream → loses trust in the product → churns or opens a support ticket.

---

### Step 03 — Constraint Mapping

**Technical Constraints**
- The empty-agent path already exists (API icon triggers it) — this is a UX surfacing problem, not an engineering build
- The downstream states (Ask page, preview, deployments) still carry the old "no KB" blocking logic and must be updated in parallel — this design cannot ship without those fixes
- The card must integrate into the existing data source picker grid without breaking the layout

**Data Constraints**
- No usage analytics on how many users currently discover the API icon path — hard to baseline conversion improvement
- Unknown split between users who want "no KB permanently" vs. "start empty, add later"

**Legal/Compliance**
- None specific to this flow. Standard data handling applies.

**Organizational Constraints**
- Copy must not position "no KB" as a downgrade — it needs to feel like a deliberate V3 feature, not a workaround
- Must not confuse existing users who rely on the KB-gated behavior
- Label and description must be agreed across product, marketing, and support

**Edge Cases**
- User selects "no KB," configures tools, then adds a KB later — the flow must not dead-end this path
- User selects "no KB" by accident and wants to go back — back navigation must be obvious
- Existing agents with no KB (created via API icon) — do they inherit any new UI treatment?

---

### Step 04 — Persona Roles

**Naive — "The Curious Starter"**
First-time CustomGPT user, arrived from a ProductHunt post about V3 agents. Doesn't know what a knowledge base is. Will read every card label carefully. If the card feels like it requires expertise or has jargon ("API," "agentic"), they'll skip it and pick "File Upload" instead just to feel safe. They need the card to sound approachable and give them a one-sentence promise of what they'll be able to do.

**Medium — "The Iterative Builder"**
Has created 3-5 agents before, typically uploads a website or PDF. Is exploring V3 features, saw the changelog about agentic capabilities. Will quickly scan the Most Popular row looking for something new. If the card has a clear badge ("New" or "V3") and a short descriptor like "System prompt + tools only," they'll click it to explore. They'll accept a short explainer modal before proceeding.

**Expert — "The Agentic Developer"**
Integrating CustomGPT into a production workflow. Wants to define tool schemas, write a detailed system prompt, and test via API. Has been frustrated by the KB requirement. Will search for "none" or scroll to the bottom looking for the API icon. If they find a properly labeled "No Knowledge Base" card in Most Popular they'll feel relieved and click immediately. Does not need hand-holding — just confirmation that this is a real, supported path.

---

### Step 05 — Behavioral Trigger Design

**Activation Trigger**
The card must be visually distinct from data-source cards without looking "broken" or lesser. A subtle but clear visual cue — a different icon type (spark/agent icon vs. a data connector icon), and a label that is a verb phrase, not a noun — signals "this is a different kind of thing."

**Aha Moment**
When the user reads the card descriptor and thinks: "Oh — I can just write a system prompt and deploy. I don't need to prepare content first." The copy does the work here. The aha moment is copy-driven, not interaction-driven.

**Competence Moment**
First successful preview response in the test window after configuring only a system prompt. The agent responds intelligently with no KB. The user thinks: "This actually works. It's not broken."

**Control Moment**
In agent config, a clear indication that they can add a knowledge base later — a non-blocking callout ("You can connect a data source anytime from Settings"). This removes the fear of being locked in and makes the choice feel reversible.

---

### Step 06 — Information Architecture

**IA Tree**
```
Create Agent
└── Pick data source
    ├── Most Popular (8 cards)
    │   ├── Website
    │   ├── Website + Images
    │   ├── WordPress
    │   ├── File Upload
    │   ├── YouTube
    │   ├── AI Vision
    │   ├── Multi-Agent
    │   └── ★ No Knowledge Base [NEW]
    ├── Drives
    │   └── Google Drive / SharePoint / OneDrive
    └── [other categories...]
        └── API (existing — de-emphasize or remove)

No Knowledge Base → selected
└── Explainer overlay (one-click dismiss)
    └── Agent Config screen
        ├── System Prompt (primary field, autofocused)
        ├── Model & Tools settings
        └── "Add knowledge base later" callout (non-blocking)
```

**Naive Task Flow**
1. Arrives on "Pick data source" page
2. Reads "Most Popular" row
3. Sees "No Knowledge Base" card — reads subtext "Chat using AI only — no files needed"
4. Hesitates → reads explainer before proceeding
5. Clicks through → lands on config with system prompt field focused
6. Types something in system prompt or leaves default
7. Tests in preview → it works → proceeds to deploy

**Medium Task Flow**
1. Scans Most Popular row — notices "No Knowledge Base" card with a V3/New badge
2. Clicks directly — reads the brief explainer
3. Dismisses, configures system prompt and tools
4. Tests preview, iterates on prompt
5. Publishes

**Expert Task Flow**
1. Searches "none" or scrolls quickly — spots "No Knowledge Base" card in Most Popular
2. Clicks without reading the modal (closes it immediately)
3. Goes straight to system prompt + tool schema configuration
4. Uses API to test, then deploys

---

### Step 07 — Failure Mode Design

| Failure Mode | Scenario | Recovery Design |
|---|---|---|
| **Accidental selection** | User clicks the card without intending to | Explainer before commit has "← Back to data sources" link |
| **Expectation mismatch post-selection** | User configures nothing, tries to chat in preview → generic responses | Empty state: "Your agent is using base AI only. Add a system prompt to define its behavior." Not an error — a guidance prompt. |
| **Old blocking logic still active** | Design ships before downstream fixes | Gate card behind feature flag until downstream fixes confirmed live |
| **User thinks "No KB" is temporary** | Expects to add KB later, doesn't know how | "Add knowledge base later" callout in config with link to Settings → Sources |
| **Confusion with "API" icon** | Two paths to same place | Remove or relabel bottom API icon once Most Popular card is live |
| **Naive user deploys before testing** | Skips preview, deploys empty-prompt agent | Pre-deploy check: "Your agent has no system prompt. Add one for better responses." Non-blocking warning. |

**Graceful Degradation**
If downstream fixes (Ask page, preview, deployments) are not ready, the card should not be shown. The existing API icon path remains as the escape hatch for technical users. Ship the card only when the full flow is coherent end-to-end.

---

### Step 08 — Metric Definition

**Primary Metric**
Empty-agent creation rate: % of "Create agent" sessions that complete via the "No Knowledge Base" path (reaches agent config screen).

**Secondary Metrics**
1. Empty-agent 7-day preview usage rate — did the user open preview at least once in the first 7 days?
2. Empty-agent → deployment rate — % of empty agents deployed within 14 days
3. KB add-on rate — % of empty agents that subsequently connect a data source

**Early Signal (7-day)**
- Card click rate (impressions → clicks on "No Knowledge Base" card)
- Overlay → "Create agent" CTA conversion rate
- % of users writing a system prompt within 10 minutes of creation

**Long-Term Signal (30-day)**
- Retention of empty agents vs. KB-backed agents
- Support ticket volume mentioning "no knowledge base" or "empty agent" — should decrease
- Developer API usage correlation: are empty-agent users more likely to integrate via API?

**Leading Indicators**
- High modal dismiss rate (close without reading) → experts are adopting fast
- System prompt completion rate at first session → activation quality signal
- Time-to-first-preview-response for empty agents

---

### Step 09 — Low-Fi Specification

**Layout Structure**

```
┌─────────────────────────────────────────────────────────┐
│  Pick data source for your agent                        │
│  [🔍 Find the source you need quickly]    [⚙ Settings] │
├─────────────────────────────────────────────────────────┤
│  Most Popular                                           │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐          │
│  │Website │ │Web+Img │ │WP      │ │File Up │          │
│  └────────┘ └────────┘ └────────┘ └────────┘          │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────────────┐  │
│  │YouTube │ │AI Vis. │ │Multi-  │ │✦ No Knowledge  │  │
│  │        │ │        │ │Agent   │ │  Base          │  │
│  │        │ │        │ │        │ │  AI-only       │  │
│  └────────┘ └────────┘ └────────┘ └────────────────┘  │
├─────────────────────────────────────────────────────────┤
│  Drives                                                 │
│  ...                                                    │
└─────────────────────────────────────────────────────────┘
```

**On card click — Inline explainer overlay:**

```
┌─────────────────────────────────────────────┐
│  ✦ No Knowledge Base                        │
│  ─────────────────────────────────────────  │
│  Your agent uses AI reasoning and tools     │
│  only — no uploaded content needed.         │
│                                             │
│  Best for:                                  │
│  • Workflow agents & orchestration          │
│  • Tool-calling and API integrations        │
│  • Prompt-only assistants                   │
│                                             │
│  You can add a knowledge base later         │
│  from agent settings.                       │
│                                             │
│  [Create agent →]    [← Back to sources]   │
└─────────────────────────────────────────────┘
```

**Priority Hierarchy**
1. Label ("No Knowledge Base") — must be unambiguous
2. One-line descriptor on the card ("AI reasoning only — no files needed")
3. Explainer content (best-for list, reversibility note)
4. CTA ("Create agent →")

**State Logic**

| State | Behavior |
|---|---|
| Default | Card visible in Most Popular row, same size as peers |
| Hover | Subtle highlight |
| Click | Explainer overlay appears, page dimmed |
| Explainer dismissed (Back) | Returns to grid, no state change |
| Explainer confirmed (Create) | Navigates to agent config, system prompt autofocused |

**Empty States**
- Agent config system prompt field: placeholder "Describe your agent's role, tone, and capabilities…"

---

### Step 10 — Heuristic & Cognitive Audit

| Heuristic | Current Violation | Improvement |
|---|---|---|
| **Visibility of system status** | User doesn't know if an empty agent is functional | Card descriptor + explainer confirm this is a supported, working path before they click |
| **Match between system and mental model** | Page title "Pick data source" implies data is required | Retitle to "Set up your agent" or add subtitle "Choose a data source, or start without one" |
| **User control & freedom** | API icon path has no "go back" | Explainer overlay has explicit "← Back to sources" |
| **Consistency & standards** | "API" icon label is ambiguous | Replace bottom API icon with consistently named card; retire the ambiguous label |
| **Error prevention** | Users can deploy an empty-prompt agent | Pre-deploy warning if system prompt is empty (non-blocking) |
| **Recognition over recall** | Users must already know "no KB" is possible | Surfacing the card in Most Popular makes it discoverable |
| **Flexibility & efficiency** | Expert users forced to read explainer | Explainer is dismissible in one click |
| **Aesthetic & minimalist design** | Card must not look "lesser" | Equal card dimensions, purposeful icon, same visual weight |
| **Help & documentation** | No help text on picker page | Tooltip on card's info icon links to help article |

**Progressive Disclosure**
- Card → one-line descriptor (level 1)
- Click → explainer with best-for list (level 2)
- Config screen → system prompt + tools + "add KB later" callout (level 3)
- Settings → full KB/source management (level 4)

**Accessibility (WCAG 2.1)**
- Card must be keyboard-navigable (Tab focus → Enter to open explainer)
- Explainer overlay must trap focus while open, return focus to card on close
- Icon must have aria-label: "Create an agent without a knowledge base"
- Color differentiation must not be the only visual distinguisher (also use icon + label)

---

### Step 11 — High-Fi Execution

**Microcopy — Complete Inventory**

| Location | Copy |
|---|---|
| Card label | **No Knowledge Base** |
| Card descriptor (one line) | AI reasoning only — no files needed |
| Card badge | **New** |
| Card icon aria-label | Create a knowledge-base-free agent |
| Overlay heading | Start without a knowledge base |
| Overlay body | Your agent uses AI reasoning and tools — no uploaded content required. |
| Overlay "Best for" items | Workflow agents & orchestration · Tool-calling and API integrations · Prompt-only assistants |
| Overlay reversibility note | You can connect a data source anytime from agent settings. |
| Overlay primary CTA | Create agent |
| Overlay secondary link | ← Back to data sources |
| Config — system prompt placeholder | Describe your agent's role, tone, and capabilities… |
| Config — KB callout | No knowledge base connected. You can add one anytime in Settings → Sources. |
| Pre-deploy warning | Your agent has no system prompt. Add one so users get useful responses. |
| Pre-deploy warning CTA | Add system prompt |
| Pre-deploy warning dismiss | Deploy anyway |

**Visual Hierarchy**
- Card icon: spark or neural-network-style (not file, globe, or database metaphor — those signal data)
- Card badge: small corner badge in brand purple — label **New** (not "Beta")
- Card descriptor: 12px, muted color, appears below label

**Interaction Details**
- Card click → 150ms fade-in on overlay
- "Back to sources" → overlay fades out in 100ms, no page reload
- System prompt field on config → autofocused on arrival

**Responsive Behavior**
- Mobile: cards in Most Popular stack to 2-column grid
- Explainer becomes a bottom sheet (full-width, 60vh) instead of centered overlay

---

### Step 12 — Validation Loop

**Before Development**

| Test | Method | Pass Threshold |
|---|---|---|
| Cognitive load test | Show updated picker to 5 users — ask "what would you click if you wanted to build an agent without uploading any files?" | ≥4/5 click "No Knowledge Base" without prompting |
| Explainer comprehension test | After reading overlay, ask "what will your agent be able to do? what won't it?" | User accurately identifies: yes to AI reasoning/tools, no to content retrieval — in ≤60s |
| Job completion speed | Time from landing on picker to "Create agent" CTA click | Median ≤45 seconds for medium/expert persona |
| Risk perception validation | Ask: "Does choosing this option feel permanent?" | ≥4/5 say "no, I can add content later" |
| Prototype first | Build overlay → config → preview prototype and test copy before engineering | — |

**After Release**

| Signal | Threshold | Action |
|---|---|---|
| Card click-through rate | >8% of picker page visits | If <5%, move to position 1-3 or increase visual weight |
| Overlay → "Create agent" conversion | >60% | If <40%, A/B test body copy |
| Empty agent 7-day preview usage | >50% | If <30%, add post-creation onboarding email/tooltip |
| Support tickets "no KB broken" | Trending to zero within 30 days | If still receiving tickets, downstream blocking logic has not cleared |
| Unintended-selection recovery rate | <30% of overlay views end in "Back" | If >30%, card label or placement may be confusing |

**Iteration Triggers**
- If empty-agent 30-day retention is lower than KB-backed agents by >20%: add in-app prompt at day 3/7 suggesting they add a KB or tool integration
- If card CTR is consistently below 5%: A/B test "Start from scratch" or "Prompt-only agent" as label

---

## Executive Summary

The V3 empty-agent creation problem is primarily a **discoverability and expectation-setting** problem, not an engineering one — the path exists, but it's invisible and the downstream states actively undermine trust. The solution is a **named, visually distinct card in the Most Popular row** with a one-click explainer overlay that communicates three things before the user commits: what they're getting (AI reasoning, no corpus), what it's best for (tools, orchestration, prompt-only), and that it's reversible (add KB later). The highest-risk element is copy — specifically, whether the card label and descriptor prevent both false positives (naive users clicking by accident) and false negatives (expert users doubting this is a supported path). **The card must not ship until the downstream blocking logic (Ask page, preview errors, deployment redirects) is cleared**, because first impressions of the empty-agent flow are entirely determined by what happens after the user creates the agent, not by the picker screen itself. Success is measured by overlay-to-creation conversion above 60%, 7-day preview usage above 50%, and a reduction in support tickets about "empty agent broken" to near zero within 30 days.

---

## Related Work

- [V3 Release](https://github.com/miovrag/V3-Release) — V3 feature release documentation
- [CustomGPT V3 UI](https://github.com/miovrag/customgpt-v3-ui) — V3 UI work
