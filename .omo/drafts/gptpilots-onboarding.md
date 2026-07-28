---
slug: gptpilots-onboarding
status: awaiting-approval
intent: clear
review_required: false
pending-action: write .omo/plans/gptpilots-onboarding.md
approach: establish a safe first-run path on the existing GCP VM, then give the user an evaluation-focused contribution loop that preserves the team’s current parallel work.
---

# Draft: gptpilots-onboarding

## Components (topology ledger)
<!-- Lock the SHAPE before depth. One row per top-level component that can succeed or fail independently. -->
<!-- id | outcome (one line) | status: active|deferred | evidence path -->
| core-rag | User can safely verify the existing Python RAG backend on the VM without rebuilding data unnecessarily. | active | GPTPilots_Project/README.md §§1, 5, 10, 11 |
| web-client | User can connect the React web client to the verified backend locally or from the VM. | active | GPTPilots_Webs/RUNBOOK.md §§1-5 |
| golden-set | User can add and validate useful goldenset cases as a measurable, low-risk contribution. | active | GPTPilots_Project/README.md §7; src/eval/goldenset.py |
| collaboration | User can report reproducible evidence without overlapping teammates’ LoRA and UI work. | active | GPTPilots_Vault/회의 및 진행 상황/0725 (월).md |
| vm-inventory | User can inventory shared VM contents before changing or deleting anything. | active | GPTPilots_Vault/LCWModelLab/ProjectInit/GCP(Google Cloud Platform) 활용 방안.md |

## Open assumptions (announced defaults)
<!-- Record any default you adopt instead of asking, so the user can veto it at the gate. -->
<!-- assumption | adopted default | rationale | reversible? -->
| First contribution | Goldenset validation and evaluation analysis, not model fine-tuning or web feature changes. | Matches user experience and avoids concurrent ownership conflicts. | Yes |
| Runtime location | Run the data-bearing backend on the shared GCP VM; run the web UI locally first. | Preserves NDA data boundary and avoids public HTTP deployment complexity. | Yes |
| First-run scope | Reuse existing VM data/extracted artifacts and build a per-user Chroma index only if absent. | README documents shared raw/extracted inputs and per-user writable index. | Yes |
| Test strategy | Tests-after for environment or documentation changes; agent-executed smoke/evaluation checks for runtime. | No product feature is requested; runtime evidence is the goal. | Yes |

## Findings (cited - path:lines)
- `GPTPilots_Project` is the complete private Python RAG/backend: Python 3.12.13 + uv, local E5 embedding, Chroma, OpenAI generation, FastAPI. `README.md` §§1-11; `src/api/server.py`.
- `GPTPilots_Webs` is frontend-only: React/Vite/TypeScript. It calls the sibling backend on port 8000 and has no backend source. `README.md`; `RUNBOOK.md` §§1-4; `src/lib/api.ts`.
- Both repositories are currently clean on `main`; the core data, `.env`, virtual environment, and generated Chroma artifacts are ignored and must remain uncommitted.
- VM convention is shared read-only inputs under `/srv/GPTPilot` and per-user project/Chroma writes. Do not force-reextract the corpus; one large PDF is known to time out. `GPTPilots_Project/README.md` §10-0.
- Core test suite is `PYTHONPATH=. uv run pytest`; web CI runs lint, format-check, and build, but has no browser-test suite. `GPTPilots_Project/pyproject.toml`; `GPTPilots_Webs/package.json`; `.github/workflows/ci.yml`.
- Web recommendations/detail are live backend data, while onboarding/questionnaire/my-page remain mock or incomplete. `GPTPilots_Webs/RUNBOOK.md` §5; `src/pages/my-page.tsx`.
- Current project evaluation structure supports NDA-bound goldenset entries and smoke/full evaluation with analysis reports. `GPTPilots_Project/README.md` §7.

## Decisions (with rationale)
- Keep the user out of teammate-owned LoRA/fine-tuning and active web feature implementation until a specific handoff is agreed.
- Make first success an observable VM backend health check, one controlled web Q&A session, and one goldenset/evaluation contribution—not a full re-index or public deployment.
- Never expose port 8000 publicly in the onboarding phase; external exposure needs CORS, shared-token, firewall source restriction, and HTTPS consideration.

## Scope IN
- VM access and inventory checklist.
- Existing backend and local web-client first-run walkthrough.
- Goldenset authoring, validation, evaluation, and reporting workflow.
- Team coordination boundaries and a personal daily contribution loop.

## Scope OUT (Must NOT have)
- No source-code, environment, VM, firewall, or data mutations in this planning task.
- No deletion of VM files, raw RFP data, indexes, or other teammates’ work.
- No LoRA model changes, web feature implementation, or public production deployment.

## Open questions
- Which personal VM Linux account and SSH host alias should be used? This determines the exact safe workspace path and whether the documented shared `/srv/GPTPilot` layout is accessible.
- Does the user have authority to use the team OpenAI key and the existing VM `.env`, or should the plan stop at zero-cost health/retrieval checks?

## Approval gate
status: awaiting-approval
<!-- When exploration is exhausted and unknowns are answered, set status: awaiting-approval. -->
<!-- That durable record is the loop guard: on a later turn read it and resume at the gate instead of re-running exploration. -->
Proposed plan: first inventory and connect to the assigned VM account; confirm the existing backend health and one zero-cost retrieval; run the protected API and local web client using the same development token; then use the current goldenset/evaluation pipeline for measured contributions. Approval authorizes creating this plan only, not implementation.
