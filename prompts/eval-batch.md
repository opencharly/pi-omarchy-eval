# /eval-batch — omarchy PR-eval batch wave

Run a batch wave of omarchy PR evaluations through the full pi-agent lane.

1. Assemble the PR list (numbers, or "all open omacom/omarchy PRs not already
evaluated at this head").
2. For each PR: omarchy-config-oracle (TRIAGE + PLAN) → if triage passes,
omarchy-eval-runner (RED-PROBE → EVAL → EVIDENCE) → omarchy-cold-reader.
3. Or orchestrate via omarchy-eval-supervisor (16 lean lanes concurrent; GPU-class
lanes SERIAL; bed names unique session-wide; the lane board, evals/min ledger, and
REDO state machine are the supervisor's).
4. PUBLICATION GATE: nothing posts to omacom/omarchy without explicit operator
approval. Rendering the comment (template + disclaimer verbatim + Assisted-by
footer) to a local file is NOT posting.

Standing rules, tier semantics, oracle rules, media contract: opencharly/eval-omarchy
eval/PR-EVAL-LANE.md (the single source) + eval/references/*.md.
