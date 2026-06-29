# AI-Augmented Detection Lab — Build Roadmap

A 12-week summer project to build a home security lab, generate real attack
telemetry, and build a Python tool that uses Claude (Anthropic API) to triage
the resulting alerts like a SOC analyst. The goal is a public, documented
GitHub project that demonstrates the ability to *build* security tooling — not
just follow tutorials.

---

## Before You Start

**Hardware check.** Running a SIEM + Windows victim + Kali attacker at once
wants ~16GB RAM on the host. With 8GB, run the victim and attacker
non-simultaneously, or put the SIEM on a cheap cloud VM. Confirm this on day one.

**Parallel daily habit.** On days without energy for the build, do **one CTF
challenge** (TryHackMe / HackTheBox) and write it up. This habit runs underneath
all 12 weeks and is its own portfolio signal. Writeups matter as much as solves.

**Two rules that disproportionately matter.**
1. Public GitHub from day one, with daily commits. The contribution graph itself
   signals discipline before anyone reads a line of code.
2. One flagship + the one daily habit. Resist sprawl. A finished, documented
   project beats five half-built ones.

**Where Claude fits**, beyond being the brain of the tool: use it as a
build-and-learn partner throughout — debugging the parser, explaining unfamiliar
log fields, drafting ATT&CK mappings, reviewing writeups for clarity.

---

## Phase 1 — Foundation & Lab (Weeks 1–2)

### Week 1 — Build the VMs
Stand up a hypervisor (VirtualBox), an Ubuntu victim, a Windows victim, and a
Kali attacker, all on an **isolated host-only network** so nothing touches the
real LAN. Snapshot everything clean. One box per day.
- **Artifact:** network diagram + a "how I built the lab" writeup.

### Week 2 — Stand up the SIEM
Deploy **Wazuh** (single-node; friendlier than Security Onion on a laptop).
Enroll agents on the victims, install **Sysmon** on Windows with a community
config (SwiftOnSecurity's is the standard), confirm logs flow to the dashboard.
- **Artifact:** screenshots of live telemetry.
- *Friction week — VM networking and agent enrollment always fight back. Normal.
  This is where Claude as a debugging partner earns its keep.*

---

## Phase 2 — Telemetry & Attacks (Weeks 3–4)

### Week 3 — Learn what an alert looks like
Generate benign noise, then run first attacks with **Atomic Red Team** — small
tests, each mapped to a MITRE ATT&CK technique, one technique a day. Watch the
logs light up; read the raw alert JSON.
- **Artifact:** first five detection writeups (technique → what I ran → what
  fired → the raw evidence).

### Week 4 — Broaden across tactics
Cover execution, persistence, credential access, lateral movement, exfiltration.
Keep a running table: technique → detected? → which rule caught it.
- **Artifact:** the start of a **detection coverage matrix** (exactly what
  detection engineers produce on the job — interviews well).

---

## Phase 3 — The Claude Triage Tool (Weeks 5–7) — the flagship

### Week 5 — Minimal loop
Get alerts out of the SIEM into Python (read `alerts.json` or hit the Wazuh API).
Minimal loop: one alert → Anthropic API → plain-English explanation to console.
- **Artifact:** working v0.1 on GitHub, README started.

### Week 6 — Structured triage
Prompt Claude to return JSON — summary, severity rating, ATT&CK technique ID,
recommended next investigative step — and parse it safely with real error
handling.
- **Artifact:** v0.2 producing structured triage records.
- *Prompt engineering becomes a visible skill here.*

### Week 7 — Make it demo-able
Add enrichment (feed the firing rule, related events, and host context into the
prompt — not just the bare alert), batch processing, and a simple **Streamlit**
dashboard so output is screenshot-friendly.
- **Artifact:** v0.3 with a UI and the first portfolio screenshots.

---

## Phase 4 — Depth & Differentiation (Weeks 8–9)

### Week 8 — Coverage and self-evaluation
Tag every triaged alert with its technique; render a coverage view (an **ATT&CK
Navigator** layer is a nice touch). Validate Claude's mapping against ground
truth — you know which atomic you ran, so measure how often the AI mapped it
correctly.
- **Artifact:** an accuracy analysis.
- *Critically evaluating your own AI tool's error rate stands out sharply from
  just bolting an LLM on and calling it done.*

### Week 9 — The AI-security extension
Prompt-injection-test your own pipeline: plant adversarial text in a filename or
command-line argument that tries to make the triage tool downgrade severity or
mislabel a real attack. Document what works, then add mitigations — delimit log
content, instruct the model to treat it as untrusted data, validate the output.
- **Artifact:** a "prompt injection in LLM SOC tooling" writeup.
- *A live, current concern and a genuine standout — and the piece most aligned
  with a cybersecurity concentration.*

---

## Phase 5 — Polish & Portfolio (Weeks 10–12)

### Week 10 — Harden and document
A real README — architecture diagram, setup steps, screenshots, honest
limitations — plus a code refactor and a few tests.
- **Artifact:** a repo that reads like an engineer wrote it, not a student.

### Week 11 — The capstone
Write the "I built an AI SOC analyst this summer" blog post and record a
three-minute demo video.
- **Artifact:** blog post + demo link, both linkable from a resume.
- *The writeup matters as much as the build — security hiring weights
  communication heavily.*

### Week 12 — Convert to job-search assets
Translate the project into resume bullets; prep to talk through architecture,
tradeoffs, and ATT&CK fluency in interviews; set the daily-CTF habit to carry
into the school year. Keep this week light — it's the buffer for spillover.

---

## Compressing to 10 weeks
Fold Weeks 8 and 9 together (do the injection testing, skip the deep accuracy
analysis) and merge Weeks 11–12.

---

## Tooling at a glance
- **Hypervisor:** VirtualBox
- **Victims:** Ubuntu + Windows (with Sysmon, SwiftOnSecurity config)
- **Attacker:** Kali
- **SIEM:** Wazuh (single-node)
- **Attack simulation:** Atomic Red Team (Caldera optional later)
- **Tool:** Python + Anthropic API + Streamlit
- **Framework fluency:** MITRE ATT&CK, ATT&CK Navigator
- **Daily habit:** TryHackMe / HackTheBox + writeups
