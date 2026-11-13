---
description: Fix a finding from a Usercase research report, or an issue from Issues, in this repository's code. Run with the command the dashboard shows.
argument-hint: "<research id> <finding key, e.g. F1> [real] | issue <issue id>"
disable-model-invocation: true
---

Call the `mcp__plugin_usercase_usercase__instructions` tool with `command` set to `fix` and these arguments from the user's input below. If the first word is `issue`, set `issue` to the second word (or leave it out if there is none). Otherwise set `research` to the first word, `finding` to the second word if there is one, and `audience` to `real` if a later word is `real`. Then follow the instructions it returns, as the user's request.

User's input: $ARGUMENTS
