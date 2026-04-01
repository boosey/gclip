# Installing gclip into an Existing Paperclip Instance

This guide walks you through installing gclip's 9 agents and 37 skills into a running Paperclip instance that already has an organization in place.

---

## Prerequisites

- A running Paperclip instance with board access
- An existing company with agents already deployed
- Git installed locally

```bash
git clone https://github.com/boosey/gclip.git
cd gclip
```

---

## 1. Understand the Mapping

gclip defines 9 engineering-focused agents. Your existing org likely has some overlap. Here's how to think about the mapping:

### Your Current Org

```
CEO
├── CIO
│   └── CTO
│       ├── Senior Developer & Tech Lead
│       │   └── Frontend Engineer
│       └── UX/UI Designer
├── General Counsel (Legal Research)
├── CFO
├── COO
└── CMO
```

### gclip Agents → Your Org

| gclip Agent | Action | Maps To | Notes |
|---|---|---|---|
| **CEO** | ENHANCE | Your existing CEO | Add gclip skills + cognitive config to existing agent |
| **CTO** | ENHANCE | Your existing CTO | Add gclip skills + cognitive config to existing agent |
| **Staff Engineer** | ENHANCE | Your existing Senior Developer | Rename optional — add review/investigate/guard skills |
| **UX Designer** | ENHANCE | Your existing UX/UI Designer | Add full gclip design skill suite (11 skills) |
| **Release Engineer** | CREATE | New agent under CTO | No existing equivalent — handles ship/deploy pipeline |
| **QA Engineer** | CREATE | New agent under CTO | No existing equivalent — handles testing/browser automation |
| **CSO** | CREATE | New agent under CTO | No existing equivalent — handles security audits |
| **Evaluator** | CREATE | New agent under CEO | Novel — the self-improvement engine |
| **Learning Curator** | CREATE | New agent under Evaluator | Novel — institutional memory manager |

### Agents You Keep As-Is (Outside gclip Scope)

These agents are not part of gclip's engineering focus. They stay untouched:

- **CIO** — Your CIO manages the CTO. gclip's CTO reports to the CEO in the default config, but in your org the CTO reports to the CIO. Keep this hierarchy.
- **General Counsel** — Legal research. No gclip equivalent.
- **CFO** — Finance. No gclip equivalent.
- **COO** — Operations. No gclip equivalent.
- **CMO** — Marketing. No gclip equivalent.
- **Frontend Engineer** — Reports to Senior Developer. gclip doesn't have a separate frontend role — your Senior Developer (Staff Engineer equivalent) delegates to them. Keep as-is.

### Your Resulting Org After Installation

```
CEO
├── CIO
│   └── CTO (enhanced with gclip skills)
│       ├── Senior Developer (enhanced → Staff Engineer skills)
│       │   └── Frontend Engineer (unchanged)
│       ├── UX/UI Designer (enhanced with 11 gclip skills)
│       ├── Release Engineer (NEW)
│       ├── QA Engineer (NEW)
│       └── CSO (NEW)
├── General Counsel (unchanged)
├── CFO (unchanged)
├── COO (unchanged)
├── CMO (unchanged)
├── Evaluator (NEW)
│   └── Learning Curator (NEW)
```

---

## 2. Enhance Existing Agents

For each existing agent that maps to a gclip role, you need to update their system prompt (AGENTS.md content) and add skills.

### 2a. Enhance Your CEO

Open your CEO agent in the Paperclip UI (Settings → Agent Configuration).

**Add skills:** `autoplan`, `office-hours`, `plan-ceo-review`, `workflow-gate`

**Update system prompt** — append the content from `agents/ceo/AGENTS.md` (everything below the `---` frontmatter) to your CEO's existing system prompt. The key additions are:

- Cognitive gearing (user-sovereignty principle)
- Decision triage (MECHANICAL / TASTE / USER_CHALLENGE)
- 10-stage strategic review capability
- Scope challenge modes (EXPANSION / SELECTIVE_EXPANSION / HOLD / REDUCTION)

**Set cognitive config** (if your Paperclip version supports custom metadata):
```yaml
cognitive:
  gearing: user-sovereignty
  decision_default: USER_CHALLENGE
  workflow_phases: [think, plan]
  anti_sycophancy: true
```

### 2b. Enhance Your CTO

**Add skills:** `plan-eng-review`, `plan-design-review`, `retro`, `codex`, `workflow-gate`

**Update system prompt** — append content from `agents/cto/AGENTS.md`. Key additions:

- Search-first cognitive gearing
- Engineering and design plan review capabilities
- Weekly retro production
- Issue status transition authority (todo → in_progress)

**Note:** In your org, CTO reports to CIO (not CEO). This is fine. gclip's `reportsTo: ceo` is the default — adapt to your hierarchy. The CTO's skills and cognitive config work regardless of where they sit in the tree.

### 2c. Enhance Your Senior Developer → Staff Engineer

**Add skills:** `review`, `investigate`, `guard`, `learn`

**Update system prompt** — append content from `agents/staff-engineer/AGENTS.md`. Key additions:

- Boil-the-lake cognitive gearing
- Two-pass adversarial code review
- Root-cause investigation with 3-strike rule
- Destructive command guard
- Operational learning logging

**Operational limits to configure:**
- `max_fixes_per_heartbeat: 50`
- `strike_rule: 3`
- `wtf_likelihood_threshold: 0.7`

**Optional:** Rename from "Senior Developer" to "Staff Engineer" for consistency with gclip terminology.

### 2d. Enhance Your UX/UI Designer

**Add skills:** `design-consultation`, `design-review`, `design-html`, `design-shotgun`, `user-research`, `wireframe`, `information-architecture`, `interaction-design`, `ab-test-design`, `browse`, `learn`

**Update system prompt** — append content from `agents/ux-designer/AGENTS.md`. This is the biggest enhancement — your UX/UI Designer goes from a general design role to a full UX lifecycle agent covering research, wireframing, IA, design systems, visual auditing, implementation, interaction design, and experimentation.

---

## 3. Create New Agents

For each new agent, create them through the Paperclip UI or API.

### 3a. Create Release Engineer

**Via Paperclip UI:** Agents → Create Agent

| Field | Value |
|---|---|
| Name | Release Engineer |
| Title | Release Engineer |
| Reports To | CTO |
| Adapter | claude_local |
| Budget | $300/mo (30000 cents) |
| Context Mode | thin |
| Heartbeat | Event-triggered |

**Skills:** `ship`, `land-and-deploy`, `document-release`, `setup-deploy`

**System prompt:** Copy the full markdown body from `agents/release-engineer/AGENTS.md`

### 3b. Create QA Engineer

| Field | Value |
|---|---|
| Name | QA Engineer |
| Title | QA Engineer |
| Reports To | CTO |
| Adapter | claude_local |
| Budget | $400/mo (40000 cents) |
| Context Mode | fat |
| Heartbeat | Event-triggered |

**Skills:** `browse`, `qa`, `qa-only`, `benchmark`, `canary`, `setup-browser-cookies`

**System prompt:** Copy from `agents/qa-engineer/AGENTS.md`

### 3c. Create CSO (Chief Security Officer)

| Field | Value |
|---|---|
| Name | CSO |
| Title | Chief Security Officer |
| Reports To | CTO |
| Adapter | claude_local |
| Budget | $300/mo (30000 cents) |
| Context Mode | fat |
| Heartbeat | Event-triggered |

**Skills:** `cso-audit`, `investigate`, `careful`, `freeze`, `unfreeze`, `guard`

**System prompt:** Copy from `agents/cso/AGENTS.md`

### 3d. Create Evaluator

| Field | Value |
|---|---|
| Name | Evaluator |
| Title | Agent Performance Evaluator |
| Reports To | CEO |
| Adapter | claude_local |
| Budget | $600/mo (60000 cents) |
| Context Mode | fat |
| Heartbeat | Weekly (Monday 02:00 UTC) + event (on issue close) |

**Skills:** `evaluate`, `propose-improvement`, `retro`, `learn`

**System prompt:** Copy from `agents/evaluator/AGENTS.md`

**Important:** The Evaluator reports directly to the CEO, not to the CIO/CTO chain. This is intentional — it observes all agents independently and cannot be influenced by the engineering hierarchy it evaluates.

### 3e. Create Learning Curator

| Field | Value |
|---|---|
| Name | Learning Curator |
| Title | Institutional Memory Manager |
| Reports To | Evaluator |
| Adapter | claude_local |
| Budget | $200/mo (20000 cents) |
| Context Mode | thin |
| Heartbeat | Weekly (Monday 06:00 UTC) |

**Skills:** `learn`, `retro`

**System prompt:** Copy from `agents/learning-curator/AGENTS.md`

**Environment variables:** Set `GH_TOKEN` (optional) if you want the Learning Curator to create PRs for approved agent definition changes.

---

## 4. Install Skills

Each gclip skill has a `SKILL.md` file in `skills/{skill-name}/SKILL.md`. Skills need to be registered in your Paperclip instance so agents can invoke them.

### How to Install a Skill

For each skill directory in `skills/`:

1. Open Paperclip UI → Skills → Create Skill
2. **Name:** Use the directory name (e.g., `review`, `qa`, `cso-audit`)
3. **Description:** Copy the `description` field from the SKILL.md frontmatter
4. **System prompt:** Copy the full markdown body from the SKILL.md file
5. **Assign to agents:** Check the agent table below

### Skill → Agent Assignment Reference

**Strategic (CEO)**
- `autoplan` → CEO
- `office-hours` → CEO
- `plan-ceo-review` → CEO

**Engineering (CTO)**
- `plan-eng-review` → CTO
- `plan-design-review` → CTO
- `codex` → CTO

**Code Quality (Staff Engineer / Senior Developer)**
- `review` → Staff Engineer
- `investigate` → Staff Engineer, CSO
- `guard` → Staff Engineer, CSO

**Design (UX/UI Designer)**
- `design-consultation` → UX Designer
- `design-review` → UX Designer
- `design-html` → UX Designer
- `design-shotgun` → UX Designer
- `user-research` → UX Designer
- `wireframe` → UX Designer
- `information-architecture` → UX Designer
- `interaction-design` → UX Designer
- `ab-test-design` → UX Designer

**Testing (QA Engineer)**
- `qa` → QA Engineer
- `qa-only` → QA Engineer
- `benchmark` → QA Engineer
- `canary` → QA Engineer
- `browse` → QA Engineer, UX Designer
- `setup-browser-cookies` → QA Engineer

**Security (CSO)**
- `cso-audit` → CSO
- `careful` → CSO
- `freeze` → CSO
- `unfreeze` → CSO

**Shipping (Release Engineer)**
- `ship` → Release Engineer
- `land-and-deploy` → Release Engineer
- `document-release` → Release Engineer
- `setup-deploy` → Release Engineer

**Workflow (Shared)**
- `workflow-gate` → CEO, CTO

**Self-Improvement (Evaluator + Learning Curator)**
- `evaluate` → Evaluator
- `propose-improvement` → Evaluator
- `retro` → CTO, Evaluator, Learning Curator
- `learn` → Staff Engineer, UX Designer, Evaluator, Learning Curator

---

## 5. Configure the Self-Improvement Loop

The self-improvement loop requires scheduled heartbeats for three agents:

### Heartbeat Schedule

| Agent | Schedule | Purpose |
|---|---|---|
| CTO | Sunday 23:00 UTC | Weekly retro (metrics gathering) |
| Evaluator | Monday 02:00 UTC | Performance evaluation + proposal generation |
| Learning Curator | Monday 06:00 UTC | Apply approved changes + prune learnings |

Set these in Paperclip UI → Agent → Heartbeat Configuration.

The 3-hour gaps between heartbeats ensure each agent's output is available as input for the next.

### Board Approval Workflow

When the Evaluator generates improvement proposals, they appear in:

**Paperclip UI → Board → Pending Approvals**

Each proposal includes:
- **Summary** — what change is proposed
- **Evidence** — metrics and data points supporting the proposal
- **Proposed change** — specific diff to agent definitions or configuration
- **Rollback plan** — how to undo if the change doesn't work

You (the board) approve or reject each proposal. Approved changes are applied by the Learning Curator on its next heartbeat.

### Initialize the Learning Store

Create the institutional memory file in your Paperclip workspace:

```bash
# In your Paperclip company workspace
mkdir -p .context/retros
echo '[]' > .context/learnings.json
```

This gives agents a place to write and read operational learnings.

---

## 6. Configure the Ethos

Copy `ETHOS.md` into your Paperclip company's shared documents. This is the cognitive philosophy all gclip agents reference. There are two ways to make it available:

**Option A — Shared document (recommended):**
Upload `ETHOS.md` as a company-wide document in Paperclip UI → Documents → Upload. All agents with `fat` context mode will receive it automatically.

**Option B — Prepend to each agent prompt:**
Add a reference line to each enhanced/new agent's system prompt:
```
You operate under the gclip ethos. See ETHOS.md for the full cognitive philosophy.
```

---

## 7. Verify the Installation

After all agents are created and skills installed, verify with this checklist:

### Agent Verification

- [ ] CEO has 4 gclip skills (autoplan, office-hours, plan-ceo-review, workflow-gate)
- [ ] CTO has 5 gclip skills (plan-eng-review, plan-design-review, retro, codex, workflow-gate)
- [ ] Senior Developer has 4 gclip skills (review, investigate, guard, learn)
- [ ] UX/UI Designer has 11 gclip skills (all design + browse + learn)
- [ ] Release Engineer exists with 4 skills (ship, land-and-deploy, document-release, setup-deploy)
- [ ] QA Engineer exists with 6 skills (browse, qa, qa-only, benchmark, canary, setup-browser-cookies)
- [ ] CSO exists with 6 skills (cso-audit, investigate, careful, freeze, unfreeze, guard)
- [ ] Evaluator exists with 4 skills (evaluate, propose-improvement, retro, learn)
- [ ] Learning Curator exists with 2 skills (learn, retro)

### Hierarchy Verification

- [ ] Release Engineer, QA Engineer, CSO report to CTO
- [ ] Evaluator reports to CEO (not CIO or CTO)
- [ ] Learning Curator reports to Evaluator
- [ ] Existing agents (CIO, General Counsel, CFO, COO, CMO, Frontend Engineer) are unchanged

### Heartbeat Verification

- [ ] CTO has weekly heartbeat (Sunday 23:00 UTC)
- [ ] Evaluator has weekly heartbeat (Monday 02:00 UTC)
- [ ] Learning Curator has weekly heartbeat (Monday 06:00 UTC)
- [ ] All other new agents are event-triggered

### Self-Improvement Verification

- [ ] `.context/learnings.json` exists and is readable by all agents
- [ ] `.context/retros/` directory exists
- [ ] ETHOS.md is accessible as a shared document
- [ ] Board approval queue is accessible at Board → Pending Approvals

---

## 8. Recommended Rollout Order

Don't install everything at once. Roll out in phases:

### Week 1: Core Engineering

1. Enhance CTO with gclip skills
2. Enhance Senior Developer with Staff Engineer skills
3. Create QA Engineer
4. Create Release Engineer

This gives you the basic Think → Plan → Build → Review → Test → Ship pipeline.

### Week 2: Design + Security

5. Enhance UX/UI Designer with full skill suite
6. Create CSO

This adds design rigor and security auditing to the pipeline.

### Week 3: Self-Improvement

7. Enhance CEO with gclip skills
8. Create Evaluator
9. Create Learning Curator
10. Configure heartbeat schedules
11. Initialize learning store

This activates the self-improvement loop. Give it 2-3 weeks of data before expecting meaningful proposals from the Evaluator.

---

## Troubleshooting

### Agent can't find skills
Skills must be registered in Paperclip before agents can invoke them. Check Skills → list to verify all 37 skills are registered.

### Evaluator has no data
The Evaluator needs heartbeat history, cost events, and closed issues to analyze. It won't produce useful proposals until agents have been running for at least 1-2 weeks.

### Workflow gate blocks everything
The workflow-gate skill enforces phase ordering. If an agent is blocked, check the issue's current status — it may be in the wrong phase for that agent's role. Fix by updating the issue status in Paperclip.

### Budget alerts
Default budgets in gclip are conservative. If agents are hitting 80% warnings frequently, the Evaluator will eventually propose budget adjustments. Or adjust manually in Agent → Settings → Budget.

### Learning store permissions
All agents with the `learn` skill need read/write access to `.context/learnings.json`. Ensure the Paperclip workspace has appropriate file permissions.
