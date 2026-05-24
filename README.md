# Pixel Agent

Pixel-art demo for the Czechitas data-analysis workshop. Watch a tiny agent walk between five stations of an office (Inbox → Skill → MCP → DB → Dashboard), execute one of three scenarios, and react live to every toggle in the rail.

**Live demo:** https://ishalkin.github.io/czechitas-pixel-agent/

**Sibling tools:**
- https://ishalkin.github.io/czechitas-stack-trace/ — same five layers, abstracted into a stack-trace x-ray
- https://ishalkin.github.io/czechitas-skill-builder/ — coloring book for the SKILL.md file

## Why a pixel game

Slide 3 of the workshop deck shows the five layers as a static table. The Stack Trace demo turns it into a click-through inspector. This one goes further: the agent is *embodied* — she walks, picks up tools, gets blocked at the skill shelf, watches the database refuse her DROP. Same simulation logic as Stack Trace, but tactile.

## Scenarios

1. **Top 10 zemí** — query succeeds, but evals catch a leak when `preferCountry` is OFF (`COUNTRY_DIRTYDATA` 408 ≠ 204)
2. **Zkontroluj SQL** — broken query (`TEROR2_OLD`, `YEAR`, `victims`) gets blocked at the skill before MCP is called
3. **Smaž TEROR2_OLD** — `DROP TABLE` blocked by the `refuseDrop` rule

Toggles let participants turn off skill rules / MCP / evals and see the bad outcomes the slides warned about.

## Local development

No build step. No dependencies. Just open the file:

```bash
python -m http.server 8000
# → http://localhost:8000
```

## Architecture

Single `index.html`. Pure simulation: each scenario is a sequence of awaitable steps (`gotoStation`, `say`, `activity`, `logLine`) that read from `state` and react to it. No network, no LLM, no real database.

## Credits

- Character + furniture sprites: [MetroCity — Free Top Down Character Pack by BK.A4](https://arlantr.itch.io/metrocity) (free for personal & commercial use)
- Office layout reference + sprite curation: [openclaw-virtual-office](https://github.com/thx0701/openclaw-virtual-office) (MIT)

## License

MIT.
