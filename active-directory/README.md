# Active Directory Simulation Trials

**Date:** 2025‑04‑30

## Overview

This project tests how well the OpenAI Codex CLI can automate the build‑out of a university‑scale Active Directory (about 2 000 staff and 15 000 learners). Each *trial* sets Codex at a different autonomy level. Inputs, instructions, and outputs stay inside the folder for that trial so results are clean and comparable.

---

## Planned trials

1. **trial1‑suggest**   Manual use of *suggest* mode. You approve every patch.
2. **trial2‑auto‑edit**  Semi‑automated *auto‑edit* mode. Approve chunks with the `a` key when ready.
3. **trial3‑full‑auto** Fully automated *full‑auto* mode. No intervention after launch.
4. **trial4‑instructional** Prompt‑driven run that also loads background context from `codex.md`. Can pair with *auto‑edit* or *full‑auto*.

---

## Folder layout (inside `active‑directory/`)

* `trialX/staging.md` Short instructions specific to that trial
* `trialX/codex.md` Long‑form context that you will point Codex to at start
* `trialX/data/` CSV files for users, groups, and OUs
* `trialX/output/` PowerShell scripts that Codex writes
* `trialX/README.md` Plain‑English summary of the trial

---

## How to start a trial

At the Codex shell, open with a prompt like:

> *Work only in* `active‑directory/trial2‑auto‑edit`. *Read* `staging.md` *and* `codex.md` *from that folder, ignore everything else. Begin when ready.*

---

## Common goals

* Build a realistic AD: OUs, users, groups, and nested groups.
* Measure Codex speed, accuracy, and how much human effort it needs.
* Record prompts, decisions, and fixes so findings are reproducible.

---

## Logging template (copy into each trial’s notes)

| Metric | What to record |
|--------|----------------|
| Prompt text | Exact words you gave Codex |
| Mode | suggest · auto‑edit · full‑auto |
| Wall‑clock time | Start → finish |
| Approvals | Count or attach session log |
| Errors | Syntax bugs, logic errors, hallucinations |
| Output quality | Did the scripts run as intended? |
| Session narrative | What you fixed and why |

---

## Contact

Repo: `codex‑cli‑sysadmin‑edu`
Maintainer: Steve

---

*End of file*

---

*Local time (New Waterford, NS):* 2025‑04‑30 18:32 ADT  
*UTC:* 2025‑04‑30T21:32Z
