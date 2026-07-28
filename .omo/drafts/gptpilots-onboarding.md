---
slug: gptpilots-onboarding
status: plan-created
intent: clear
review_required: false
pending-action: offer execution handoff
approach: Verify the existing VM-backed RAG system safely, tunnel it privately to the local web client, and record redacted runtime evidence.
---

# Draft: gptpilots-onboarding

## Components (topology ledger)

| id | outcome | status | evidence path |
| --- | --- | --- | --- |
| vm-inventory | User identifies their own VM workspace, existing data/index state, and free ports without mutation. | active | GPTPilots_Project/README.md section 10 |
| backend | User verifies the existing FastAPI backend with bounded API calls and an owned PID. | active | GPTPilots_Project/README.md sections 10-11 |
| web-client | User accesses the VM backend from the local React client through private SSH forwarding. | active | GPTPilots_Webs/RUNBOOK.md sections 1-5 |
| evidence | User records redacted pass/fail evidence without duplicate goldenset, LoRA, or UI work. | active | GPTPilots_Project/README.md; GPTPilots_Webs/RUNBOOK.md |

## Open assumptions (announced defaults)

| assumption | adopted default | rationale | reversible |
| --- | --- | --- | --- |
| First contribution | Runtime verification evidence, not goldenset work, model fine-tuning, or web feature changes. | Goldenset evaluation is complete and belongs to another teammate. | Yes |
| Runtime topology | Backend runs on the VM; frontend runs locally and reaches it only through SSH local forwarding. | Keeps data private and avoids public port/firewall work. | Yes |
| Account-specific values | VM user, host alias, paths, and ports stay as executor-supplied placeholders. | Do not place credentials or private infrastructure identifiers in the plan. | Yes |
| Cost boundary | Zero-cost checks plus no more than one charged /ask smoke call. | Enough to prove the integration while limiting spend. | Yes |

## Findings (cited)

- Core repository is a Python RAG backend with documented fresh-account and existing-account VM paths, FastAPI health and ask endpoints, and a known re-index timeout risk. `GPTPilots_Project/README.md` sections 10-11.
- Web repository is a React/Vite frontend that expects the API at localhost and requires matching API token configuration. `GPTPilots_Webs/RUNBOOK.md` sections 1-5.
- VM access and a backend `.env` with the team OpenAI key are already available to the user.
- Goldenset evaluation is complete and owned by a teammate; it is explicitly excluded.

## Decisions (with rationale)

- Inventory before setup or index work. Existing paths use the established-account procedure; fresh setup is only for a missing personal workspace.
- Use a user-owned VM port, log, and PID; never use broad process termination commands.
- Use SSH local forwarding rather than exposing the backend on a public VM port.
- Redact all evidence: do not retain or share tokens, RFP text, prompts, answers, screenshots, or raw logs.

## Scope IN

- Safe first-run inventory and backend health checks.
- Private VM-to-local web connection and one bounded chat smoke test.
- Redacted runtime evidence and teammate handoff.

## Scope OUT (Must NOT have)

- No raw-data, index, firewall, source-code, model, or web-feature modification.
- No goldenset authoring, evaluation, or analysis.
- No public deployment, public API port, broad process termination, or credential persistence changes.

## Open questions

- None blocking. The executor supplies their existing VM SSH alias/account and selects unused personal ports at execution time.

## Approval gate

status: approved
The user confirmed VM access and ready backend credentials. This draft records the approved scope; plan generation is authorized, but implementation remains a separate worker action.
