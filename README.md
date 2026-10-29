# Usercase for Claude Code and Codex

Research your website with virtual users, before you have real ones.

Ask your coding agent to research your site. It reads your repository to understand what the site is and who it is for, then casts a handful of virtual users. Each one visits the site in a real browser on your machine, looks around with their own goal, and answers the Usercase survey cards they happen to meet. Their visits and answers show up in your [Usercase](https://usercase.io) dashboard.

- **The AI runs on your own subscription.** Every visit uses your Claude Code or Codex usage.
- **Runs on your machine.** Your browser, your network, your staging or demo accounts. Passwords never pass through Usercase.
- **Visitors only see the screen.** The agent may read your code to plan the research, but the visitor agent can only use three browser tools. It cannot read files.

This plugin works in **Claude Code** and **Codex** on your computer. It starts a browser on your machine, so it doesn't run in web or cloud sessions.

## Requirements

- Claude Code or Codex
- Node.js 18 or later
- Google Chrome (or run `npx playwright install chromium` once)
- A Usercase project with the Usercase script on your site, so the survey cards appear (`/usercase:install` adds it)

## Install

In Claude Code:

```
/plugin install usercase --marketplace usercase-io/usercase-plugin
```

In Codex:

```
codex plugin marketplace add usercase-io/usercase-plugin
codex plugin add usercase@usercase
```

Then run any command, such as `/usercase:install` in Claude Code or `$usercase:install` in Codex. Your browser opens on the Usercase dashboard: sign in, check that the code matches the one in your terminal, and pick the project (or create one). The key is made on your computer and saved in `~/.usercase/credentials.json`; only its hash is sent to Usercase. You can revoke it in **Settings → Integrations → API keys**.

Each repository keeps its own connection, so different sites can go to different projects. To switch this repository to another project, run `/usercase:connect` (`$usercase:connect` in Codex).

Prefer a key? In Claude Code, leave the browser and enter one under `/plugin` → **Installed** → **usercase** → **Configure**. A key entered there is used first. Anywhere, a `USERCASE_API_KEY` environment variable is used first too.

## Commands

In Codex the same commands are skills: type `$usercase:research` instead of `/usercase:research`, and so on.

| Command | What it does |
|---|---|
| `/usercase:research <url> [count] [focus]` | Casts virtual users and sends them to the site, 5 at a time. `count` is the total (default 5), or `unlimited` to keep going until you press Esc. `focus` limits them to people who come for one part of the site. |
| `/usercase:install [url]` | Adds the Usercase script to your site's root layout, shows you the diff and checks that it loads. It doesn't commit or deploy. |
| `/usercase:path <research id> [task id]` | Records the route a research task is meant to take, walks it twice in a browser, and saves it after you approve it. The dashboard's task screen shows this command. |
| `/usercase:question` | Suggests a survey question for a precise moment on your site, and saves it as a draft in your Inbox after you approve it. |
| `/usercase:new-research` | Suggests what to research from your project's findings and visits, writes the tasks and questions from your code, plans each task's route, and on your approval walks the routes in a browser and saves the research with them as a draft in your Inbox. |
| `/usercase:connect` | Connects this repository to another Usercase project. Other repositories keep theirs. |
| `/usercase:steps <window id>` | Writes the steps for a multi-step question you are making in the dashboard. The dashboard shows this command. |

You can also just ask your agent, for example "research my staging site with 10 virtual users".

## What it runs

- The MCP server [`@usercase/mcp`](https://www.npmjs.com/package/@usercase/mcp), started with `npx` at the exact version this plugin pins. It drives a Playwright browser on your machine.
- A sub-agent that plays one virtual user. It can only use the `open_visit`, `act` and `answer_card` tools. In Claude Code it comes with the plugin as `usercase:usercase-visitor`. Codex plugins can't include agents, so the MCP server adds it as `~/.codex/agents/usercase_visitor.toml`; Codex reads agents when a session starts, so restart Codex once after the first setup.
- For sites behind a sign-in, Chrome (or Edge) with a fresh, empty profile where **you** sign in with a demo account. Your usual browser profile is never touched. Only the session is saved, never the password.

## What it sends, and where

- **To your site:** the visitors' page loads. By default only `GET`, `HEAD` and `OPTIONS` requests are sent. Before the first round the agent asks whether writes (posting, saving, signing up) should reach the site, which is useful for a test or staging site. Payments, deleting, wallet connections, sign-out and password changes are always blocked. Every request carries the `X-Usercase-Agent` header and a `UsercaseQA/1.0` user-agent suffix, so you can tell visitors apart from real traffic.
- **Analytics tools** such as Google Analytics and Hotjar are blocked during visits.
- **To Usercase (`app.usercase.io`), once connected:** each finished visit, meaning the virtual user's profile and goal, the pages and actions, the survey cards they met and their answers, and screenshots of the visit. It shows up in your dashboard under **Visits**. If an upload fails, it waits in `.usercase/outbox.jsonl` and is sent with the next one. Paths you record, and questions and research drafts you approve, are saved to your project too.
- **Diagnostics to Usercase:** when the tool can't do something on your site or misreads the screen, it sends the kind of failure, the page path, the HTML around the element and a screenshot of that moment. Only the Usercase team sees them, and they are kept for 30 days. Turn this off with the **Send diagnostics** option in the plugin settings (Claude Code), or set `USERCASE_DIAGNOSTICS=off`.

The plugin sends nothing else, and nothing goes to Usercase until you connect a project.

## Files it creates

`.usercase/` in your repository:

- `people.json`: the virtual users. Edit them freely, and commit the file if you want to reuse them.
- `runs/`: visit records.
- `sessions/`: one sign-in session per demo account.
- `paths/`: screenshots of the expected paths the agent recorded.
- `settings.json`: your answer about writes.

`runs/` and `sessions/` are git-ignored by a `.gitignore` inside the folder.

## More

The full guide, including running on a production site behind a firewall, is in the [`@usercase/mcp` README](https://www.npmjs.com/package/@usercase/mcp). Privacy: [usercase.io/privacy](https://usercase.io/privacy). Help: [usercase.io](https://usercase.io).

## License

The files in this plugin are under the MIT license (see `LICENSE`). The `@usercase/mcp` package it runs has its own terms: [usercase.io/terms](https://usercase.io/terms).
