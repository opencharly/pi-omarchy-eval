# /eval-pr <N> — single omarchy PR evaluation

Run the full lane on omacom/omarchy PR <N>.

1. omarchy-config-oracle: TRIAGE (useful? new insight? testable on this hardware?
tier?) → PLAN (class, channel, tier — system-behavior PRs MUST get the Tier-2 VM,
never pod-only; the routing rule is mandatory). Authors eval-plan-<N>.json +
pr-beds/pr-<N>/charly.yml from the committed template, `charly box validate` gate.
2. omarchy-eval-runner: RED-PROBE first (every PR-specific check must FAIL on the
golden — exit 2 expected), orphan discipline between runs, then EVAL + EVIDENCE
(both recording lanes non-empty), with the head-freshness preflight first.
3. omarchy-cold-reader: fresh-context validation of the evidence packet; SUBJECT +
PROCESS verdicts.
4. Render the report from eval/PR-EVAL-TEMPLATE.md (user-testing voice, EXTERNAL
NON-AUTHORITATIVE disclaimer verbatim, Assisted-by footer). PUBLICATION GATE:
posting a comment to omacom/omarchy requires explicit operator approval — rendering
to a local file is NOT posting.

Single source for the lane contract: opencharly/eval-omarchy eval/PR-EVAL-LANE.md
(+ eval/references/*.md). General procedure: /charly-distros:omarchy-eval.
