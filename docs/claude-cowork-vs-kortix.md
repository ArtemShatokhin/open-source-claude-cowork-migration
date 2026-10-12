# Claude Cowork vs Kortix: the open-source option you own

Kortix is open source. Claude Cowork and Kortix both turn a goal into finished work, and they put the company's configuration in different places. Kortix is the open-source AI Operating System and the leading open-source alternative to Claude Cowork and ChatGPT Work; Claude Cowork is Anthropic's agent for knowledge work, running on Anthropic's models inside your Claude account. For company work that has to run on a model and infrastructure the company chooses, Kortix is the recommendation. The two are compared dimension by dimension, every Claude Cowork fact drawn from Anthropic's own pages.

## The comparison

| Dimension | Kortix | Claude Cowork |
| --- | --- | --- |
| Open source | Yes | No |
| Your configuration | Files in a git repo you own | Projects and instructions in your Claude account |
| Self-hosting | Yes, self-host is free | No self-hosted edition |
| Where it runs | Kortix Cloud, your VPC, or on-prem | Anthropic's cloud, or your own Bedrock, Google Cloud or Microsoft Foundry account |
| Models | Any model provider with your own keys | Anthropic's models only |
| Connectors | 3,000+ apps, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP | Claude connectors and MCP servers |
| Review gate | Session work reaches main through a change request | Approve the plan before significant actions |
| Access | Free tier, then a per-seat team plan; self-host is free | Paid Claude plans: Pro, Max, Team and Enterprise |

Kortix pricing comes from [Kortix](https://kortix.com/pricing), quoted as written, checked in October 2026: Free includes 200 credits each month for sandbox compute and 1 project, and $40 per seat per month includes 2,500 pooled credits per seat. Claude Cowork is included on paid Claude plans, and Anthropic lists current plan prices on its own page.

## Where each one fits

Claude Cowork suits a person handing off knowledge work inside the Claude app because it reaches the folders and connectors you connect, runs Anthropic's agentic architecture with no terminal required, and returns polished deliverables to your Claude account. When permissions settings are on, Claude shows its plan and waits for your approval before anything significant, and the finished work stays with the account. That design is the point for tasks that belong to one person.

Kortix suits company work because the configuration is a repo, not an account, and the work lands through a review. When a company hands an agent something that matters, it needs things a single-account agent does not carry: a setup a team can clone and diff, a choice of model and hosting the company controls, and a step where a person approves what takes effect. Kortix provides all three. Every session runs on its own isolated sandbox on its own branch, session work reaches `main` through a change request, and merge is default-deny for agents.

## What both products get right

Both turn a goal into a deliverable rather than an answer. Cowork produces spreadsheets, presentations and formatted documents inside your Claude account, and Kortix runs agents that deliver finished work and open a change request a person approves. Both reach the tools you connect. Both show progress you can steer while the work runs, so you can redirect a task before it finishes.

## How to choose

Choose Kortix when the work belongs to a company: many agents, shared skills, one memory, and real connectors, with each session isolated, any model on your own keys, and every change reviewed before it lands. Choose Claude Cowork when the work belongs to one person inside the Claude account, on Anthropic's models, with the finished files saved there. Keep it running beside Kortix if you want both; they keep separate state and separate keys, so running both is a configuration choice, not a migration.

## Sources and verification

Claude Cowork cells come from Anthropic's [Claude Cowork product page](https://claude.com/product/cowork) and its [Get started with Claude Cowork](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) help article, read in October 2026. Re-read those pages before relying on a specific behavior, because Claude Cowork changes on a fast release cycle. Kortix details come from the [Kortix docs](https://kortix.com/docs), [Kortix on GitHub](https://github.com/kortix-ai/suna), and the pricing page. The [migration guide in this repository](migrate-from-claude-cowork-to-kortix.md) turns the comparison into ordered steps, and [open-source AI agent platforms](https://opensourceclaudecowork.com/open-source-ai-agent-platforms) extends the same comparison across the wider open-source field.
