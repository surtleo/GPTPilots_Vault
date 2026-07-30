# gptpilots-onboarding - Work Plan

## TL;DR (For humans)
<!-- Fill this LAST, after the detailed plan below is written, so it summarizes the REAL plan. -->
<!-- Plain English for a non-engineer: NO file paths, NO todo numbers, NO wave/agent/tool names. -->

**What you'll get:** A safe, repeatable way to prove that the existing AI backend and web screen work together, without touching teammates' model, goldenset, or feature work. You will leave a short redacted result that the team can act on.

**Why this approach:** The source RFP data belongs on the VM, but the browser can stay on your computer through a private connection. Starting with proof of operation is safer and more useful than changing code before you have seen the system work.

**What it will NOT do:** It will not change the LoRA model, web features, goldenset, raw data, or public deployment settings. It will not expose secrets or document contents.

**Effort:** Short
**Risk:** Medium - the VM is shared, so the plan isolates ports, processes, and evidence.
**Decisions to sanity-check:** Backend stays on the VM, the web client stays local, and the connection uses private SSH forwarding.

Your next move: start this plan only when you are ready to run the checks on your own VM account. Full execution detail follows below.

---

> TL;DR (machine): Short, medium-risk runtime onboarding with private VM API access, local web smoke test, and redacted evidence.

## Scope
### Must have
- Inventory the user-owned VM workspace before any setup command.
- Verify backend health, authenticated catalog access, and one bounded chat response.
- Connect the local web client through SSH local forwarding only.
- Record a redacted result and hand it to the team.
### Must NOT have (guardrails, anti-slop, scope boundaries)
- Do not modify raw RFP data, generated index data, source code, model configuration, web features, firewall rules, or goldenset files.
- Do not use `--force`, public port exposure, `credential.helper store`, `pkill`, or port-wide kill commands.
- Do not record credentials, business names, RFP excerpts, prompts, answers, screenshots, or raw logs in evidence.

## Verification strategy
> Zero human intervention - all verification is agent-executed.
- Test decision: tests-after + existing pytest, npm lint/build, curl, and browser developer tools.
- Evidence: `.omo/evidence/gptpilots-onboarding/task-<N>.md`; records only command outcome, HTTP status, count, route name, elapsed time, and redacted error class.

## Execution strategy
### Parallel execution waves
> Target 5-8 todos per wave. Fewer than 3 (except the final) means you under-split.
- Wave 1: inventory and prerequisites (1-2).
- Wave 2: backend and private connection (3-4).
- Wave 3: web smoke and handoff evidence (5).

### Dependency matrix
| Todo | Depends on | Blocks | Can parallelize with |
| --- | --- | --- | --- |
| 1 | none | 3 | 2 |
| 2 | none | 4 | 1 |
| 3 | 1 | 4 | none |
| 4 | 2, 3 | 5 | none |
| 5 | 4 | final wave | none |

## Todos
> Implementation + Test = ONE todo. Never separate.
<!-- APPEND TASK BATCHES BELOW THIS LINE WITH edit/apply_patch - never rewrite the headers above. -->
- [ ] 1. Inventory the assigned VM workspace without mutation
  What to do / Must NOT do: SSH using the user's existing alias; record only whether the personal project directory, `.venv`, `data/raw`, `data/extracted`, and `data/chroma` exist. Check an unused personal API port and log directory. Do not print `.env`, list RFP filenames, alter symlinks, delete files, or run setup/index commands.
  Parallelization: Wave 1 | Blocked by: none | Blocks: 3
  References (executor has NO interview context - be exhaustive): `GPTPilots_Project/README.md` section 10-0 and 10-1; `.omo/drafts/gptpilots-onboarding.md` Decisions.
  Acceptance criteria (agent-executable): shell checks return the existence state of the five paths; selected API port is not listening; output is summarized without identifiers.
  QA scenarios (name the exact tool + invocation): happy: `test -d` and `ss -ltn`; failure: if personal project or required shared inputs are absent, stop and report the missing category instead of creating/copying anything. Evidence `.omo/evidence/gptpilots-onboarding/task-1.md`.
  Commit: N | operational verification only
- [ ] 2. Verify local web prerequisites and reserve local ports
  What to do / Must NOT do: In `GPTPilots_Webs`, confirm Node 20+, npm, working tree, and port availability for Vite and the SSH-forwarded API. Do not edit `.env` yet and do not install packages if `node_modules` already satisfies the lockfile state.
  Parallelization: Wave 1 | Blocked by: none | Blocks: 4
  References (executor has NO interview context - be exhaustive): `GPTPilots_Webs/RUNBOOK.md` sections 2 and 4; `GPTPilots_Webs/package.json` scripts.
  Acceptance criteria (agent-executable): Node major version is at least 20; npm is available; local ports selected for Vite and forwarding are unused; the frontend working tree state is recorded without changing it.
  QA scenarios (name the exact tool + invocation): happy: `node --version`, `npm --version`, and port checks; failure: missing Node/npm or occupied port stops the flow with the failing prerequisite recorded. Evidence `.omo/evidence/gptpilots-onboarding/task-2.md`.
  Commit: N | operational verification only
- [ ] 3. Start and verify a user-owned VM backend process
  What to do / Must NOT do: Follow the established-account path when data/index already exist; otherwise stop for team guidance rather than running the fresh-account sequence automatically. Start FastAPI on the selected user-owned VM port with a user-owned log file, capture its PID, then verify health and authenticated catalog access. Do not use port 8000 by default if it is occupied, `--force`, broad kill commands, or public firewall changes.
  Parallelization: Wave 2 | Blocked by: 1 | Blocks: 4
  References (executor has NO interview context - be exhaustive): `GPTPilots_Project/README.md` sections 10-1 and 11; `.env.example` for server-only variables.
  Acceptance criteria (agent-executable): `/health` returns HTTP 200 with status `ok`; authenticated `/rfps` returns HTTP 200 and a count of 100; the recorded PID belongs to the executor's command and log path.
  QA scenarios (name the exact tool + invocation): happy: `curl` health and authenticated catalog request; failure: startup timeout, 401, or non-200 stops the flow and records status/error class only. Evidence `.omo/evidence/gptpilots-onboarding/task-3.md`.
  Commit: N | operational verification only
- [ ] 4. Create private VM-to-local API forwarding and configure the local client
  What to do / Must NOT do: Create an SSH local port forward from a free local port to the executor-owned remote API port. Set the web `.env` to the local forwarded URL and the matching shared token; do not put an OpenAI key in the web environment or expose the remote port publicly. Start the web client only after the forwarded local health endpoint is successful.
  Parallelization: Wave 2 | Blocked by: 2, 3 | Blocks: 5
  References (executor has NO interview context - be exhaustive): `GPTPilots_Webs/RUNBOOK.md` sections 3-4; `GPTPilots_Webs/.env.example`; `GPTPilots_Project/README.md` section 11.
  Acceptance criteria (agent-executable): local forwarded `/health` is HTTP 200; Vite starts on its chosen port; browser network requests use only localhost and show no CORS or 401 failure.
  QA scenarios (name the exact tool + invocation): happy: local `curl` health then `npm run dev`; failure: failed forward, mismatched token, or CORS error stops the browser test and records the redacted category. Evidence `.omo/evidence/gptpilots-onboarding/task-4.md`.
  Commit: N | local operational configuration only
- [ ] 5. Perform one bounded browser Q&A smoke test and hand off redacted evidence
  What to do / Must NOT do: Open recommendations, one RFP detail route, and ask one question from that detail page. Allow at least 30 seconds for the documented response time. Verify citations, active document behavior, token/cost display, and absence of 401/CORS errors. Make no further paid requests and do not save answer text or screenshots.
  Parallelization: Wave 3 | Blocked by: 4 | Blocks: final wave
  References (executor has NO interview context - be exhaustive): `GPTPilots_Webs/RUNBOOK.md` sections 5 and 7; `GPTPilots_Project/README.md` section 11.
  Acceptance criteria (agent-executable): recommendations and one detail page load; one detail-context `/ask` is HTTP 200 and includes `answer`, `active_doc_id`, `citations`, and `cost`; browser renders citations/token/cost; evidence contains only pass/fail, route class, count, and elapsed time.
  QA scenarios (name the exact tool + invocation): happy: browser DevTools Network plus visible UI; failure: a 401, CORS error, timeout, missing citation, or clarify response from the detail context is recorded and handed off without code changes. Evidence `.omo/evidence/gptpilots-onboarding/task-5.md`.
  Commit: N | operational verification only

## Final verification wave
> Runs in parallel after ALL todos. ALL must APPROVE. Surface results and wait for the user's explicit okay before declaring complete.
- [ ] F1. Plan compliance audit
- [ ] F2. Code quality review
- [ ] F3. Real manual QA
- [ ] F4. Scope fidelity

## Commit strategy
- No commit is expected. This plan deliberately avoids tracked source, data, and configuration changes.

## Success criteria
- The user can independently distinguish the VM backend from the local frontend and safely start/stop only their own processes.
- The private forwarded web flow shows one complete, cited Q&A response without credential or NDA leakage.
- The team receives a concise redacted result and any blocker, with no duplicate goldenset, LoRA, or UI feature work.
