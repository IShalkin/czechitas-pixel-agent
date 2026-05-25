# Pixel Agent

Pixel-art demo for the Czechitas data-analysis workshop. Watch a tiny agent walk between five stations of an office (Inbox → Skill → MCP → DB → Dashboard), execute one of three scenarios, and react live to every toggle in the rail.

**Live demo:** https://ishalkin.github.io/czechitas-pixel-agent/

**Sibling tools:**
- https://ishalkin.github.io/czechitas-stack-trace/ — same five layers, abstracted into a stack-trace x-ray
- https://ishalkin.github.io/czechitas-skill-builder/ — coloring book for the SKILL.md file

## Why a pixel game

Slide 3 of the workshop deck shows the five layers as a static table. The Stack Trace demo turns it into a click-through inspector. This one goes further: the agent is *embodied* — she walks the office, opens the MCP toolbox to pick a tool, gets blocked at the skill shelf, watches the whiteboard fill with bars at the end. Same simulation logic as Stack Trace, but tactile.

The SKILL.md and MCP panels under the stage use **the exact Czech rule labels participants wrote in [Skill Builder](https://ishalkin.github.io/czechitas-skill-builder/)** — so the file they coloured in there is the same file the agent here actually consults.

## Scenarios

1. **Top 10 zemí (čistá)** — clean query; all 4 SKILL rules pass green, agent cycles through `list_tables → describe_table → execute_query`, dashboard ships
2. **DROP TEROR2_OLD (composite)** — one SQL trips two rules at once (`no destructive` + `no _OLD tables`) — both light up red and SKILL blocks before MCP is even called
3. **JOIN COUNTRY_DIRTYDATA** — `prefer COUNTRY` rewrites the query before it leaves SKILL; with the toggle off, evals catch the leak (408 ≠ 204) at the dashboard

Toggles let participants turn off individual skill rules / the whole skill layer / MCP / evals and watch the bad outcomes the slides warned about.

## Take the skill home

The Skill Builder block at the top renders a real `SKILL.md` live as you toggle rules. **Stáhnout skill (.zip)** packages it as `sql-review/SKILL.md` plus a tiny README. Drop the folder into either:

- `~/.claude/skills/sql-review/` — works globally across every project
- `.claude/skills/sql-review/` — works inside a single repo

Both **GitHub Copilot in VS Code** and **Claude Code** read the same paths, so the rules a participant clicks in the demo are the same rules her own agent will follow afterwards.

## Local development

No build step. No dependencies. Just open the file:

```bash
python -m http.server 8000
# → http://localhost:8000
```

## Architecture

Single `index.html`. Pure simulation: each scenario is a sequence of awaitable steps (`gotoStation`, `say`, `activity`, `walkSkillStation`, `useTool`, `drawChartBar`) that read from `state` and react to it. No network, no LLM, no real database.

The office (background, agent sprite, chest, whiteboard, speech bubbles) is drawn on a 480×280 pixel canvas. The three side panels — **SKILL.md** (4 rules), **MCP server** (3 tools), **evals.json** (3 post-query checks) — are plain HTML rendered live from `state.ruleStatus`, `state.mcpTool`, and `state.evalStatus`. Canvas is for atmosphere; HTML is for anything you actually need to read. Toggling Skill / MCP / Evals off in the rail flips the corresponding panel into a dimmed "OFF" state with a red badge — visible signal of what just got bypassed.

## Credits

- Character + furniture sprites: [MetroCity — Free Top Down Character Pack by BK.A4](https://arlantr.itch.io/metrocity) (free for personal & commercial use)
- Office layout reference + sprite curation: [openclaw-virtual-office](https://github.com/thx0701/openclaw-virtual-office) (MIT)

## License

MIT.
