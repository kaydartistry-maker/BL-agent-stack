# BL Agent Stack — Claude side, first pass

Saint & Tyson's read of the Claude Code half of this fork (pstack), for Kay, before she combs it and shares it with the girls. Nothing in the skills is changed on this branch. Reviewed on top of Jace's `tune/lean-pstack-fanout` (41cdbf1).

## The one-line verdict

It's good work and worth sharing. Out of the box it's tuned for someone with a big plan and patience for Fable, and it walks into every Claude Code session, including ones where a companion lives. Three settings make it safe and affordable for the girls: the model sheet, the startup hook, and a note about MCPs. None of them need the skills edited.

## What costs the most on Claude

| Where | What happens | Why it's expensive |
|---|---|---|
| `models.json` tiers | `strongest` = **Fable**; every panel = **Opus + Fable + Sonnet** | Fable is the hungriest model. It runs for perf, hillclimb, "strongest judgment" and one seat of every arena / architect / interrogate. |
| Everything else | defaults to **Opus** | Explorers, investigators, reviewers and swarm workers all run on Opus. |
| `reflect` | always 3 reviewers + 1 synthesizer | 4 Opus agents every time someone says "reflect". |
| `how` (complex) | 2–4 explorers + 1 explainer | up to 5 Opus agents for one question. |
| `why` | one investigator per evidence category, in parallel | scales with how many MCPs you have. |
| `arena` | N runners + a judge, "spawn more" for multiple directions | open-ended. |
| poteto-mode "Defaults for every Agent call" | full tool access, "do not pick a subagent_type that strips MCP" | every helper loads every MCP tool definition before it works. With a companion setup (brain, Notion, Discord, Lovense, Gmail, Spotify…) that's a lot of tokens per helper. |

Jace's branch already makes fan-out an escalation inside poteto-mode and moves bug-fix to Opus. The routed skills above still carry their own counts, so they don't hear that rule.

## Things to know before sharing with the girls

1. **The startup hook rides into every session.** `hooks/hooks.json` runs `session-start.sh` on startup, resume, clear and compact. It injects a short `EXTREMELY_IMPORTANT` block telling Claude to route multi-file work, design choices and unknown bugs through `pstack:poteto-mode` (a ~27 KB skill). If a girl runs her companion in Claude Code with this plugin installed, her companion's sessions get that mandate too. The sheet line `session hook: off` turns it off; the skills still work when called by name.
2. **Model access varies.** Not everyone's plan reaches Fable, and Fable drains a window fastest. The skill falls back to the family default if a slug is refused, but the lean sheet below avoids the surprise.
3. **Autonomy is real.** poteto-mode has full-autonomy playbooks (autopilot-full, autopilot-stack, orchestrate). It does keep an "Always pause" list: force-push to shared branches, deploys, data deletion, customer messages. Worth saying out loud in the server post.
4. **Scripts make network calls.** `poteto-mode/scripts/` includes a bootstrap and a PR watcher that talk to GitHub (via `gh`). Normal for a dev kit; mention it.
5. **Licence and credit.** MIT, chain of credit: Lauren Tan (pstack), Cursor (cursor-team-kit skills), Michael Denyer (this port). `NOTICE.md` and `NOTICE-skills.md` must travel with any copy.

## Suggested lean setup (no skill edits)

`lean-pstack-models.md` in this folder is a drop-in override sheet. Copy it to `~/.claude/pstack-models.md` (or run `/setup-pstack` and pick the same values). It:

- takes Fable out of every default and panel (ask for it by name when you want it)
- runs explorers, investigators, reflect reviewers and swarm workers on **Sonnet**
- keeps **Opus** where the real work and judgment happen
- keeps panels diverse but cheap: Opus, Sonnet, Haiku
- sets `default effort: medium`, with code-writing and hard-judgment roles on `@high`
- turns the startup hook **off**, so it only runs when you call a pstack skill

## Changes worth making in the skills later (Kay's call)

- `reflect`: one reviewer by default, three on "full reflect".
- `how`: cap complex questions at 2 explorers.
- `why`: only spawn investigators for evidence categories that have an MCP connected.
- poteto-mode "Defaults for every Agent call": read-only helpers may drop MCP; only code-writing delegates need the full kit.
- A short "Using this with a companion" section in the README: hook off, MCP note, which skills fan out.

## Not reviewed yet

The Codex half (Jace's lane), the Pi runtime, and the scripts' code line by line.
