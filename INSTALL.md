# Installing PaperclipAI as a New Paperclip Company

This guide walks you through importing PaperclipAI as a brand-new Paperclip company package.

---

## Prerequisites

- Paperclip access with board permissions
- Git installed locally
- A fresh workspace or repository where you want the company to live

```bash
git clone <repo-url>
cd <repo-dir>
```

---

## 1. Import the Company

PaperclipAI is already structured as an importable Paperclip company package. The main entry points are:

- [`COMPANY.md`](COMPANY.md)
- [`.paperclip.yaml`](.paperclip.yaml)
- `agents/**/AGENTS.md`
- `skills/**/SKILL.md`

Import it into Paperclip:

```bash
paperclip company import --from .
```

If your Paperclip setup imports from a local folder through the UI, point it at the cloned repository directory instead.

---

## 2. Verify the Company

After import, confirm the company shape matches the package:

- `CEO` reports to the board
- `CIO` owns the engineering workflow
- `CTO` reports to `CIO`
- `CFO`, `CMO`, and `General Counsel` report to `CEO`
- `Evaluator` reports to `CEO`
- `Learning Curator` reports to `Evaluator`

The engineering spine should look like this:

- `CTO` owns `Staff Engineer`, `Release Engineer`, `QA Engineer`, `CSO`, and `UX Designer`

---

## 3. Load Shared Context

Copy or upload the shared company context into Paperclip:

- [`ETHOS.md`](ETHOS.md) for the company philosophy
- `.context/learnings.json` as the initial learning store
- `.context/retros/` as the retro archive folder

Initialize the learning store if needed:

```bash
mkdir -p .context/retros
echo '[]' > .context/learnings.json
```

---

## 4. Confirm Skills and Agents

Make sure the following are present after import:

- 13 agents total
- 37 skills total
- The full engineering workflow skills for planning, review, testing, shipping, and retrospectives
- The company roles for finance, marketing, legal, and self-improvement

If your Paperclip instance exposes an agent or skill count view, use it to confirm the import succeeded.

---

## 5. Recommended Next Step

Once imported, start by running a small company-level planning session through `CEO`, then a workflow planning pass through `CIO`. That gives the org a clean first heartbeat and confirms the gstack-style engineering spine is wired correctly.
