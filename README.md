# skills

My personal [agent skills](https://skills.sh) — usable across Claude Code, Codex, and any harness the [`skills`](https://github.com/vercel-labs/skills) CLI supports.

## Install

```bash
# all of them, globally, into every detected agent
npx skills@latest add sebiwnl/skills --agent '*' --global

# just one
npx skills@latest add sebiwnl/skills --skill deep-understanding
```

## Skills

| Skill | What it does |
|---|---|
| **deep-understanding** | Teaches a topic by exposing the *why* behind concepts, decisions, mechanisms, and tradeoffs — so you build real understanding instead of cognitive debt. Invoke with `/understand <topic>`. |
| **short-answer** | Manual-only mode switch. Sets concise response mode for the rest of the session. Invoke with `/short-answer`. |

## License

MIT
