---
description: Record the expected path of a Usercase research's tasks. Run with the command the dashboard's task screen shows.
argument-hint: "<research id> [task id]"
disable-model-invocation: true
---

Call the `mcp__plugin_usercase_usercase__instructions` tool with `command` set to `path`, `research` set to the first word of the user's input below, and `task` set to the second word if there is one. Then follow the instructions it returns, as the user's request.

User's input: $ARGUMENTS
