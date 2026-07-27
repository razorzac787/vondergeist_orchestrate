# Vondergeist

**A multi-agent AI system that scans codebases and cloud configs for data breach risks, scores them by real-world exploitability, and lets you chat with an AI to understand and fix each one — like Copilot, but for security posture.**

---

## Why This Exists

A single leaked credential, an exposed API key, or one misconfigured storage bucket is often all it takes to compromise millions of records. History — and the present — keep proving this.

### Onavo
Facebook shut down its unpaid market research programs and pulled its Onavo VPN app from the Google Play Store after a TechCrunch investigation revealed that Onavo's code was repurposed inside a "Facebook Research" app that harvested data from teens without adequate disclosure. The incident showed how quietly a data-collection mechanism can cross ethical and legal lines once it's buried inside production code.

### Ashley Madison
In July 2015, a group calling itself "The Impact Team" breached Ashley Madison, a commercial site built around facilitating extramarital affairs, and stole its entire user database. When the company refused to shut the site down, the attackers released personal information on more than 2,500 users to prove their threat was real — and eventually leaked the full dataset. The company had publicly claimed its systems were secure right up until the breach.

### ShinyHunters — Kodak (2026)
In June 2026, Kodak confirmed a data breach after the ShinyHunters group claimed to have stolen 2.2 million customer and corporate records. The group listed Kodak on its dark web leak site and gave the company a three-day deadline before threatening to publish the stolen data.

These three incidents span two decades, three completely different industries, and three different attack motives — yet the underlying story is the same: a small oversight in how data is handled or secured becomes a large-scale violation of the trust people place in these companies. And the problem hasn't gone away with time. If anything, breaches in 2026 are more frequent than ever — most of them not from sophisticated zero-day exploits, but from leaked credentials sitting in a repository or a cloud resource left publicly accessible. Attackers increasingly don't need to "hack" in; they just log in.

**That's the gap Vondergeist fills** — catching these preventable failure modes (leaked secrets, misconfigured cloud resources, risky code patterns) before they ship, and making it easy for a developer to understand and fix them without needing to be a security expert.

---

## What Vondergeist Does

Vondergeist is a **multi-agent orchestration system**, not a single script. A Master Agent coordinates three specialized Worker Agents to turn raw scan output into a ranked, explainable, fixable set of alerts.

### Agents

| Agent | Purpose | Feature it Powers |
|---|---|---|
| **Master / Orchestrator Agent** | Coordinates the full pipeline — triggers scans, decides what's noise vs. a real alert, and routes findings to the right worker agent | The end-to-end "Scan → Score → Chat" workflow; the system's decision-making core |
| **Scanner Agent** | Runs detection tools against a repo or cloud config and normalizes raw findings (secrets, static analysis issues, misconfigurations) into a common schema | "Scan Repository" action and the raw findings feed |
| **Risk-Scoring Agent** | Uses the LLM to assess real-world exploitability and business impact of each finding, not just "flag exists" | The risk-ranked alert dashboard (critical → low) |
| **Remediation Advisor Agent** | Conversational agent (the "Copilot" of the system) that explains a specific finding in plain language and suggests or generates a concrete fix | The chat interface tied to each alert |

### Demo Scenarios
To ground the demo in reality rather than hypotheticals, Vondergeist is tested against two failure modes modeled directly on real 2026 incidents:
1. **Leaked developer credentials in a repository** — mirroring incidents where a single exposed token was the entire entry point for attackers.
2. **A misconfigured cloud storage/config file** — mirroring the pattern behind several of 2026's largest exposures, where publicly accessible cloud resources leaked millions of records.

---

## Tech Stack

- **Backend:** Python + FastAPI
- **AI / Agent Orchestration:** Claude API (Sonnet), using native tool-calling for inter-agent coordination
- **Detection Engines:**
  - `gitleaks` / `trufflehog` — secret scanning
  - `semgrep` / `bandit` — static analysis for security-relevant code patterns
  - Custom checks — cloud config parsing (e.g. Terraform/YAML) for public buckets, open security groups
- **Frontend:** React + Tailwind CSS
- **Database:** SQLite
- **Deployment:** Local + ngrok for a live demo link

---


