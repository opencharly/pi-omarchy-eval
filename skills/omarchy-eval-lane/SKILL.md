---
name: omarchy-eval-lane
description: |
  The omarchy PR-eval lane as a pi-agent pipeline — the four committed agents
  (config-oracle, eval-runner, eval-supervisor, cold-reader), the state machine, and
  how to invoke the lane from any system. Use before driving an omacom/omarchy PR
  evaluation.
---

# omarchy-eval-lane — the pi-agent lane

The lane runs ENTIRELY on pi agents — no GitHub Actions in the eval path. The four
agents are committed in the umbrella repo (`opencharly/opencharly`
`.pi/agents/omarchy-{config-oracle,eval-runner,eval-supervisor,cold-reader}.md`); any
system with the umbrella project in pi can run the lane.

## The state machine

    TRIAGE → PLAN → RED-PROBE → EVAL → EVIDENCE → COLD-READ → ACCEPT/REPORT

- **omarchy-config-oracle** (Stage 1): TRIAGE + PLAN — class, channel, tier, clone
  entity, check plan with known-red justification, recording plan, expected-phase
  budget. Authors `pr-beds/pr-<N>/charly.yml` DIRECTLY (the plan IS the charly.yml)
  from the committed template, gated by `charly box validate`. NEVER runs a bed.
- **omarchy-eval-runner** (Stage 2): RED-PROBE (must FAIL exit 2 on the golden) →
  EVAL → FULL EVIDENCE EVALUATION (code/diff, check results, .cast screencast, media
  frames/video → the complete evidence packet, both recording lanes non-empty) →
  CLEANUP: NEVER leaves a VM running (destroy + domstate-gone verified; zero residual
  domains/locks at handoff). Runs beds as persistent background tasks; enforces orphan
  discipline, head-freshness preflight, and golden re-provision duties. NEVER edits
  source.
- **omarchy-cold-reader** (Stage 3): FRESH-context, ARTIFACTS-ONLY validation of the
  evidence packet against the rubric + the 10 standing rules, using pi vision
  (vision_ask / pi.read) AND deterministic evidence (.cast text, verbatim output) —
  NEVER from a running VM (the runner has torn everything down). Emits SUBJECT +
  PROCESS verdicts with findings.
- **omarchy-eval-supervisor**: ORCHESTRATION (lane board, slot arbitration,
  ONE EVAL LANE PER CPU CORE default — nproc-derived, RAM-capped), MEASUREMENT
  (evals/min), the REDO state machine (disputes → council),
  and the publication gate (a comment posts ONLY behind operator approval; rendered
  from the template with the disclaimer verbatim + Assisted-by footer).

## The FULL LOOP (binding)
Every stage fully GRADES the work of the stage before it and can TRIGGER A CHANGE:
redo-plan (the oracle re-authors the charly.yml when the config/checks do not set the PR
up for a proper eval — the runner CONFIG AUDIT: pr-apply seam, known-red diff-ADDED
markers at proven-landing paths, the FULL record:/spice: loop, only the two plugin
provider candies, the PR's channel golden at 2G/1cpu, box validate green),
redo-run (infra/lock/media-pull on a sound config), redo-read (media incomplete before
grading), escalate (loop guard ≥3 redo entrances → council, never a silent re-run).
A finding closes only with a setup change + re-run evidence. Contract:
`opencharly/eval-omarchy` `eval/references/full-loop.md`.

## Lane rules (the binding single source)

The 10 standing rules, tier semantics, oracle marker/path rules, media contract and
cold-reader rubric live in `opencharly/eval-omarchy` `eval/PR-EVAL-LANE.md` (+ its
`eval/references/*.md`) — THIS SKILL ONLY POINTS THERE (R3). General eval procedure,
beds and guardrails: `/charly-distros:omarchy-eval`, `/charly-check:check`.

## Running the lane

- Single PR: `/eval-pr <N>` (also: invoke omarchy-eval-supervisor with the PR number).
- Batch wave: `/eval-batch` — operator approval required before any posting.
- Standalone cold read: `/eval-cold-read <pr>`.
- From anything non-interactive: `pi -p` with the agent selected (see agents' own
  `description:` frontmatter for the delivery audit probe).

## Host requirements (the any-system contract)

- config-oracle / eval-runner: a charly binary (v2026.244+ schema) + libvirt session;
  GPU classes need the host's GPU in vfio mode (`requires_exclusive: [nvidia-gpu]`,
  SERIAL lane) — absent hardware degrades to PARTIAL/NOT-EVALUABLE, never a faked bed.
- cold-reader: pi vision (vision_ask) + the media artifacts.
- supervisor: subagent orchestration (runs.lanes/runs.all, slot arbitration).
