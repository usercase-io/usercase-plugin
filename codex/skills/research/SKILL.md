---
name: research
description: Research this website with Usercase virtual users — each one visits the site in a real browser and answers the survey cards they meet. Use when the user asks to research, test or get feedback on their site with virtual users.
---

Call the `instructions` tool of the `usercase` MCP server with `command` set to `research` and these arguments taken from the user's message:

- `url`: the site address, if one is given
- `count`: a number of people, or `unlimited`, if one is given
- `research`: a research id from the dashboard, if one is given (e.g. "research 3f2a…", "take part in research …") — the people then do that research's tasks
- `focus`: any remaining words (when there is no research) — the part of the site the people should come for

Leave out what isn't given. Then follow the instructions it returns, as the user's request.
