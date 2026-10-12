# Migrate from Claude Cowork to Kortix: open source, one repo, one review gate

Kortix is open source. The move takes company work from Claude Cowork, Anthropic's agent for knowledge work inside your Claude account, to Kortix, the open-source AI Operating System where your agents, their skills, your company memory, and every connector live in one git repo you own. The move is a file mapping plus one behavioral change: a Cowork task returns finished files to your account, and a Kortix session works on its own isolated machine and branch and opens a change request a person reviews before it merges.

Do the move in the order below. Each step ends with something you can verify before the next one starts.

## Before you start: inventory what runs in Cowork

List what you actually use before you move anything. Pull the list from the Claude app and your Claude account, not from memory.

- **Tasks and projects.** Every Cowork project and saved task you rely on, and the instructions attached to each.
- **Skills and plugins.** The skills and plugins that carry your domain knowledge and best practices.
- **Connectors.** The Claude connectors and MCP servers you connected, and the folders each one reaches.
- **Memory and context.** The durable facts and preferences Cowork carries, and the chat memory that feeds it.
- **Scheduled work.** Every Cowork scheduled task that runs on a cadence.
- **Recurring outputs.** The decks, spreadsheets, memos and reports you produce on a schedule.
- **Model access.** Your Claude plan and any provider keys you already hold.
- **People.** Everyone who starts Cowork tasks, and the folders and tools they are allowed to reach.

## What maps to what

| Claude Cowork concept | Kortix construct | What you do |
| --- | --- | --- |
| Skills and plugins | `skills/<name>/SKILL.md` | Copy the folder in, allow it to an agent |
| Projects and instructions | `agents/<name>.md` | Move the prompt, declare its reach |
| Memory shared with chat | `memory/` files in the repo | Copy durable facts, prune stale notes |
| Connectors and MCP servers | Connector accounts | Reconnect each service as a connector |
| Claude models | Any model, your keys | Add provider keys or bring a subscription |
| Scheduled tasks | `cron` and `webhook` triggers | Recreate each with `kortix triggers add` |
| Files saved to your account | A session branch and a change request | Review the diff, then merge |
| Settings in your Claude account | `kortix.yaml` at the repo root | Recreate the settings as manifest entries |

## What to migrate first

Move the parts that carry knowledge before the parts that carry reach. Skills, the instructions behind each project, and memory are plain files that copy across and give the first session something useful to do. Connectors, model keys and credentials come next, because they give the agent reach and should be reconnected deliberately, one service at a time. Scheduled tasks come last, once a manual session produces the result you want on a clock.

One Cowork behavior changes rather than moves. A task returns finished files to your Claude account, and a Kortix session opens a change request instead. That gate is the point of the move, so plan the first session around reading a diff.

## Step 1: install the CLI and scaffold the project

Kortix's CLI runs on macOS and Linux, with WSL on Windows. Install it, sign in, and scaffold a directory:

```bash
curl -fsSL https://kortix.com/install | bash
kortix login
kortix init my-company
```

`kortix login` opens a browser, signs you in, and sets your account and a default project. `kortix init my-company` scaffolds a directory with a `kortix.yaml` manifest and starter agent and skill folders. Run `kortix ship` inside it to create the cloud project and push the repo. To bind a repo folder you already have to a project you created, run `kortix projects link <project-id>`. These are the same commands covered in [self-host an open-source Claude Cowork alternative](https://opensourceclaudecowork.com/self-hosting) on the companion site.

## Step 2: move skills and plugins

A skill is a folder with a `SKILL.md`. Cowork's skills and plugins carry domain knowledge and best practices; in Kortix each becomes `skills/<name>/SKILL.md` in the repo. Copy each folder in, then allow each agent the skills it may load through the agent's block in `kortix.yaml`. Skills load on demand, when an agent needs one.

## Step 3: move projects and instructions as agents

A Claude Cowork project holds its own files, links, instructions and memory. Those instructions become an agent in Kortix: `agents/<name>.md`, an OpenCode agent file whose frontmatter sets the model and mode and whose body is the system prompt, plus an `agents.<name>` block in `kortix.yaml` that governs what the agent may reach. Put the project instructions into the markdown body, and set the agent's scope in the manifest block so it can load only the skills and connectors it needs. Where one Cowork project served several tasks, split the instructions into one agent per job; a specialist with a narrow prompt does each part better than a generalist would.

## Step 4: move the memory

Cowork keeps memory in your Claude account, shared with chat. Kortix keeps `MEMORY.md` for durable facts, `USER.md` for preferences, and dated notes under `memory/`, all inside the repo. Copy the durable facts and the notes that still matter, and leave the rest behind. Memory that moved into a repo is versioned, so you can see how the company's context changed over time and roll a bad entry back.

## Step 5: reconnect the connectors

A Cowork connector or MCP server becomes a Kortix connector account. Reconnect each service you listed, one at a time, and check that it works before moving to the next. Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP, and connector credentials are brokered server-side and never enter the machine. Slack and Microsoft Teams are live as channels; email is experimental and enabled per project.

## Step 6: recreate the scheduled tasks as triggers

A Cowork scheduled task becomes a cron or webhook trigger defined in `kortix.yaml`, and the CLI writes the entry for you:

```bash
kortix triggers add daily-digest --type cron --cron "0 0 9 * * 1-5" \
  --timezone America/Los_Angeles \
  --prompt "Summarize yesterday's activity and save it as a daily note."
kortix ship
kortix triggers ls
```

A trigger has exactly two types, `cron` and `webhook`. A webhook trigger signs its requests with a secret you set with `kortix secrets set WEBHOOK_SECRET=<value>`. Run `kortix ship` to push the entry, then `kortix triggers ls` to confirm it is live.

## Step 7: move model access

Kortix is not tied to one provider. Any model provider with your own keys, or the ChatGPT plan you already pay for, or sign in with your OpenCode Console account for OpenCode Zen and Go. Add the provider keys you hold, and keep the model choice per agent or per session as you need it. The harness is OpenCode, and it reads the agent files, skills and harness files in your repo.

## Step 8: move the credentials

Re-enter the secrets the work used as project secrets. Secrets are encrypted at rest with a key per project. A connector's credentials are brokered server-side and never enter the machine, so a service you reconnect as a connector does not put a raw key in a session. For any secret that must be read inside a session, set the exposure deliberately and check who can use it.

## Step 9: run the first session and review it

Start a session and let it produce a change instead of a live edit:

```bash
kortix sessions new --prompt "Summarize this week's customer calls" --wait
kortix cr ls
kortix cr diff 1
kortix cr merge 1
```

The session runs in its own isolated sandbox on its own branch. When it finishes it opens a change request, and it stops there. Session work reaches `main` through a change request, and a session can never merge a change request it opened itself. Read the diff, then merge it. What lands on the default branch is a change you approved.

## What you gain and what you leave behind

What you gain is a configuration a team can clone, diff and roll back, one isolated machine per session, any model on your own keys, and a gate on every change. Thousands of agents can run in parallel on one config, each on its own cloud computer, without touching each other. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free.

What you leave behind is the account-held model. Cowork keeps projects, instructions and memory in your Claude account and runs on Anthropic's models; Kortix keeps them as files in a repo you own and runs on the model you choose. If you still want a task inside the Claude app, keep Claude Cowork running beside Kortix; the two keep separate state and separate keys.

## Next steps

The [comparison page in this repository](claude-cowork-vs-kortix.md) sets the two products side by side, and [Claude Cowork vs ChatGPT Work](https://opensourceclaudecowork.com/claude-cowork-vs-chatgpt-work) covers how the two closed platforms differ from each other. When the move is done, [self-hosting the migration](self-hosting-the-migration.md) shows what you own and how to run the stack on your own hardware.
