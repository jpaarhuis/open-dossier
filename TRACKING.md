# Tracking — open-dossier

Living project-tracking file (formerly HANDOFF.md). Stable sections on top, append-only log at the bottom; event detail lives in commit messages.

## Status

Open-source Claude skill for source-driven knowledge dossiers, published at https://github.com/jpaarhuis/open-dossier (public, MIT). Current release **v0.1.2**: skill body hardened via blind smoke tests, behavior validated with with/without-skill evals (+17pt over baseline, cold-start is where the skill earns its keep), trigger description optimized to 100/100/100 on a 20-query eval set. Installed in Jeffrey's desktop app via the marketplace; next up is real-world use on an actual project.

Origin: distilled from the GVB repo workflow (`C:\repos\gvb\gvb_documenten_en_plannen`, see its `CLAUDE.md`) combined with Karpathy's LLM-wiki pattern. GitHub account `jpaarhuis`, gh CLI authenticated.

## Decisions (settled, don't relitigate)

- Name **open-dossier** — Jeffrey's choice (alternatives dossier/paper-trail/source-wiki rejected).
- Scope: knowledge + execution layer (plans/actions/decisions), not Karpathy's pure wiki. The execution layer is the differentiator.
- English default; per-dossier label mapping allowed (Dutch `[BRON]/[INTERPRETATIE]/[WERKHYPOTHESE]/[BESLUIT]`), one vocabulary per dossier, set in DOSSIER.md.
- Zero dependencies, pure markdown instructions — works in Claude Code, claude.ai, Desktop.
- Ship first, iterate later — explicitly chosen over eval-first.
- Wiki-page creation threshold and priority assignment stay model judgment — deliberately not codified after the smoke tests.
- This file is tracked in git (Jeffrey's call, 2026-07-18; it was untracked as HANDOFF.md before).

## Backlog

Candidates, in rough order of value; none agreed yet:

1. `examples/` mini dossier in the repo for adoption. The Atlas fixture at `skills/open-dossier-workspace/fixtures/atlas-dossier` is a ready-made seed (2 sources, contradictions, decisions vs hypotheses) — it is gitignored, so copy it out rather than un-ignoring the workspace.
2. Release a packaged `.skill` file via GitHub release (`python -m scripts.package_skill` in skill-creator).
3. Feed findings from Jeffrey's real-project test back into the skill (watch: wiki-page threshold, priority assignment — the two judgment calls left open).
4. Deeper eval rounds if the skill body changes again — harness and both eval sets in the workspace are reusable.

## Lessons & gotchas

- **skill-creator's trigger runner (`scripts/run_eval.py`) measures nothing here**: it fakes the skill as a slash command in `.claude/commands/` (the `Skill` tool never fires) and returns "not triggered" on the first non-Skill tool call, which every exploring model does — recall structurally 0%. Replacement harness: `skills/open-dossier-workspace/trigger-eval/trigger_eval.py` (installs the real skill in a throwaway project, watches the whole stream).
- **Empty test projects fake trigger failures**: any query naming a file or dossier dies on "file not found" before the model considers the skill. Use `--seed-fixtures` to plant the referenced artifacts. Cost a full measurement round before diagnosed.
- **`claude -p` needs its own CLI login** (`/login` in a terminal): the desktop app injects host auth into interactive sessions, but headless subprocesses read `~/.claude/.credentials.json`, which can be expired while interactive "works".
- **Windows + skill-creator scripts**: `subprocess.run(["claude", ...])` can't resolve the npm `.cmd` shim (fix: `shutil.which`); `select.select()` on pipes is Unix-only (fix: reader thread + queue). Patched in the local plugin copy, not upstream.
- **Desktop-app marketplace clones never auto-fetch**: `~/.claude/plugins/marketplaces/<name>` stays on the install-time commit, so the Update button stays grey. `git fetch && git merge --ff-only origin/main` in that clone fixes it.
- **A dossier is self-documenting**: DOSSIER.md + exemplar files teach a bare model the whole workflow — evals against an existing dossier measure the fixture, not the skill. Only cold-start evals discriminate. (Product strength; now a README selling point.)

## Conventions

- Jeffrey communicates tersely; caveman mode may be active in his sessions (drop filler, keep technical substance).
- Commit style: sentence-case imperative subject, body explains why. Co-Authored-By Claude trailer.
- Bump `version` in `plugin.json` + git tag on every release-worthy change.
- Eval workspace: `skills/open-dossier-workspace/` (gitignored), fixtures + harnesses live there.

## Log

Append-only, one line per event, newest at the bottom.

- 2026-07-17 Initial release v0.1.0, published to GitHub, repo is plugin + own marketplace (`4ba7c1e`).
- 2026-07-17 README: desktop-app install flow documented (`5db45d0`).
- 2026-07-17 Smoke test: 2 blind runs, 17 frictions → 19 ambiguity fixes total, v0.1.1 (`bf7ab5f`).
- 2026-07-17 Eval loop: 3 evals × with/without skill, 100% vs 83%, analyst notes in `iteration-1/benchmark.json`; Jeffrey approved, no skill changes.
- 2026-07-17 Trigger-eval: built own harness (skill-creator's broken on Windows + by design), description 85%→100% accuracy, v0.1.2 (`b98dd2b`).
- 2026-07-17 Desktop app stuck on v0.1.0: marketplace clone never fetches; fast-forwarded manually.
- 2026-07-18 README rewritten with ChatGPT copy + hero image (`docs/hero.png`); HANDOFF.md restructured into this tracked TRACKING.md.
