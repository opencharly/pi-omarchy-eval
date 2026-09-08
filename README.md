# pi-omarchy-eval

Pi package for the **omarchy PR-evaluation lane**: the skills + prompts that drive the
committed pi agents end to end, on any system.

- **Agents** do NOT live here — their single home is the umbrella repo
  (`opencharly/opencharly` `.pi/agents/omarchy-{config-oracle,eval-runner,supervisor,cold-reader}.md`),
  so the definitions are never duplicated (R3). This package ships the skills and the
  prompt surface.
- **Skills**: `skills/omarchy-eval-lane` (the pi-agent lane + state machine),
  `skills/omarchy-eval-golden` (the golden VM chain runbook).
- **Prompts**: `/eval-pr <N>`, `/eval-batch`, `/eval-cold-read <pr>`.
- The binding lane contract lives in `opencharly/eval-omarchy`
  (`skills/omarchy-eval-lane/SKILL.md` + `eval/references/*.md`); general eval procedure in the
  charly marketplace corpus (`/charly-distros:omarchy-eval`, `/charly-check:check`,
  `/charly-vm:vm`).

## Install

```sh
pi install git:github.com/opencharly/pi-omarchy-eval@v<calver>
```

Requires the umbrella project (for the agents + the eval-omarchy checkout) and a
charly binary (v2026.244+ schema) for the bed lanes.
