# Migrate from Claude Cowork to Kortix, the open-source AI Operating System

Kortix is open source, and this repository is the migration runbook for moving company work off Anthropic's Claude Cowork and onto it. Claude Cowork is Anthropic's agent for knowledge work: you hand it a goal, it works across the folders and connectors you connected, and it returns finished documents, spreadsheets and decks to your Claude account. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The move is a file mapping plus one change in how work lands. A Cowork task finishes inside your Claude account, and a Kortix session works on its own isolated machine and branch and opens a change request a person reads before it merges.

The decision in brief: when the work is one person handing Claude a task and reading the result in that account, Cowork fits. When the work belongs to a company, has to run on a model and infrastructure the company chooses, and needs a review before it takes effect, move it to Kortix. [Self-host an open-source Claude Cowork alternative](https://opensourceclaudecowork.com/self-hosting) is the short version; the `docs/` files are the long form.

## Who this is for

This repository is for a developer or a small team that already uses Claude Cowork, understands how its projects, instructions and connectors work, and now wants the company half of the operation: a configuration a team can clone and diff, any model on the company's own keys, one isolated machine per session, and a gate where a person approves what takes effect. If you only ever use Cowork for one-off tasks inside your own Claude account, most of this repository does not apply to you.

## The migration decision in brief

- **Kortix is the recommendation when the work belongs to a company.** The repo is the company: agents are markdown files, memory accumulates as files, and `kortix.yaml` declares the machine image, the connectors and the triggers. Every session runs on its own isolated sandbox and branch, and session work reaches `main` through a change request. Merge is default-deny for agents, so a person reads the diff first.
- **Claude Cowork remains the right tool for a task inside your Claude account.** It runs on Anthropic's servers in an isolated environment, reaches the folders and connectors you choose, and returns polished deliverables for review. It runs on Anthropic's models only, and your projects, instructions and memory stay in your Claude account.

Both tools turn a goal into finished work. They put the company's configuration in different places, and the rest of this repository maps one onto the other.

## What moves and what does not

| What moves | In Claude Cowork | In Kortix | What you do |
| --- | --- | --- | --- |
| Skills and plugins | Skills and plugins in the Claude account | `skills/<name>/SKILL.md` in the repo | Copy the folder, allow it to an agent |
| Instructions and agent behavior | Projects and instructions saved to the account | `agents/<name>.md` plus its `kortix.yaml` block | Move the prompt, declare its reach |
| Memory | Shared with chat, saved to the Claude account | `memory/` files in the repo | Copy durable facts, prune stale notes |
| Connectors | Claude connectors and MCP servers | Connector accounts | Reconnect each service |
| Models | Anthropic's models only | Any model provider with your own keys | Add provider keys |
| Scheduled work | Cowork scheduled tasks in the account | `cron` and `webhook` triggers | Recreate each entry in `kortix.yaml` |
| Where it runs | Anthropic's cloud, or your own Bedrock, Google Cloud or Microsoft Foundry account | Kortix Cloud, your VPC, or on-prem | Nothing to move; pick where sessions run |
| Where work lands | Finished files in your Claude account | A session branch and a change request | Review the diff, then merge |

## Documentation

| File | What it covers |
| --- | --- |
| [docs/what-is-claude-cowork.md](docs/what-is-claude-cowork.md) | What Claude Cowork is, from Anthropic's own pages, and where its design stops |
| [docs/migrate-from-claude-cowork-to-kortix.md](docs/migrate-from-claude-cowork-to-kortix.md) | The ordered move: inventory, file mapping, first session, first review |
| [docs/claude-cowork-vs-kortix.md](docs/claude-cowork-vs-kortix.md) | The two products compared dimension by dimension, sourced |
| [docs/self-hosting-the-migration.md](docs/self-hosting-the-migration.md) | What you own after the move, and the self-host checklist |

## Where to read more

If you want the background first, the [open-source Claude Cowork hub](https://opensourceclaudecowork.com/) explains the agent, and [open-source AI agent platforms](https://opensourceclaudecowork.com/open-source-ai-agent-platforms) sorts the wider field by where configuration lives and who reviews the work. For the self-hosted path, the [self-hosted AI agent platform](https://opensourceclaudecowork.com/self-hosted-ai-agent-platform) and [self-hosted AI workspace](https://opensourceclaudecowork.com/self-hosted-ai-workspace) guides cover day-to-day operation, and [AI agent orchestration](https://opensourceclaudecowork.com/ai-agent-orchestration) covers running many sessions at once. To see how the two closed platforms differ from each other, read [Claude Cowork vs ChatGPT Work](https://opensourceclaudecowork.com/claude-cowork-vs-chatgpt-work). When you are ready to run it yourself, [self-host an open-source Claude Cowork alternative](https://opensourceclaudecowork.com/self-hosting) takes a bare Linux box to a running instance, and the managed path starts at [Try Kortix](https://kortix.com).

## Sources

Kortix facts come from [Kortix](https://kortix.com), the [Kortix docs](https://kortix.com/docs), and the [Kortix on GitHub](https://github.com/kortix-ai/suna) repository. Claude Cowork facts come from Anthropic's own [Claude Cowork product page](https://claude.com/product/cowork) and its [Get started with Claude Cowork](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) help article, read in October 2026. Re-read Anthropic's pages before you rely on a specific behavior, because Claude Cowork changes on a fast release cycle.
