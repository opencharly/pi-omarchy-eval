# /eval-cold-read <pr> — standalone cold read

Run the omarchy-cold-reader on an EXISTING eval evidence packet.

- Context: the eval plan (pr-plans/eval-plan-<N>.json), the report draft, the
evidence packet (summary.yml, verdict, timings), .check logs, media (.cast, .gif,
screen-*.png, screen.mp4).
- Read ALL verified lanes: PR code + diff (does the eval exercise the real behavior,
not token presence?), check results (deterministic truth), the .cast text (terminal
lane), and vision on the recorded media ON DEMAND (cross-check every visual claim
against deterministic sources — the GNOME-mislabel trap).
- Emit dual verdicts: SUBJECT (PASS/FAIL/NO VALIDATION) and PROCESS (config fit,
known-red, tier compliance, media quality, timing in budget), each finding tied to an
evidence reference. Write eval/evidence/<pr>-<calver>/verdict.yml. Findings are
ledger entries — never suppressed.
- A report that fails the cold read is FIXED, not posted.
