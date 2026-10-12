# What is Claude Cowork? Anthropic's agent, and why Kortix is open source

Kortix is open source. Claude Cowork is Anthropic's agent for knowledge work, built into the Claude app: you give it a goal, it works across the folders and tools you connect, and you come back to finished work for your review. Anthropic's own product page and help center describe how Cowork runs a task, where its configuration lives, and where that design stops and a company system begins.

## Claude Cowork in one paragraph

Claude Cowork completes tasks you can steer from anywhere: give it a goal and it works across your files and tools, and you come back to polished work for your review. Anthropic describes it as built for non-coding knowledge work such as research, analysis and document creation, using the same agentic architecture that powers Claude Code, with no terminal required. It runs on desktop, with web and mobile in beta, and Anthropic is folding Cowork and chat into one Claude, so a task and a conversation start from the same message box. You can read the description on [Anthropic's Claude Cowork page](https://claude.com/product/cowork) and the [Get started with Claude Cowork](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) help article.

## How Claude Cowork runs a task

Cowork runs your task in the cloud on Anthropic's servers, in an isolated environment, and saves your sessions and files to your Claude account. When you start a task, Claude analyzes your request and creates a plan, breaks complex work into subtasks, runs code and shell commands in that environment, coordinates parallel workstreams, and delivers finished outputs to your session for preview and download. On desktop it can also read from and write to local files you point it at, without manual uploads, and it reaches your computer through the Claude desktop app when a task needs a local file or your browser. The help article states that Cowork respects your current network egress permissions, and that those permissions do not apply to the web fetch and web search tools.

## Where the configuration lives

Claude Cowork keeps its configuration in your Claude account, not in a repo you own. Projects group related tasks into workspaces with their own files, links, instructions and memory. Memory is shared with chat, so a Cowork task starts from what Claude already remembers from your chats, and what comes up in a task carries back to chat. On Team and Enterprise plans, the product page says admins set access by team, manage spend limits and control tool permissions by department, and activity streams to a SIEM through OpenTelemetry. Deleting a task removes it from your history immediately and from Anthropic's backend storage within 30 days, per the help article.

## Who Claude Cowork suits

Claude Cowork suits a person who wants to hand off knowledge work and read the result inside the Claude app. You connect the folders and connectors you choose, Claude uses Anthropic's models, and you review polished deliverables such as spreadsheets, presentations and formatted documents. Cowork is included on paid Claude plans (Pro, Max, Team and Enterprise), with Enterprise plans opting in, and Anthropic lists current plan prices on its own page. That design is deliberate: one agent run per task, driven from the Claude account, on Anthropic's models, with the finished files kept there.

## Where Cowork stops and Kortix starts

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The difference from Claude Cowork shows up on three axes.

The first axis is where the configuration lives. Cowork keeps projects, instructions and memory in your Claude account. Kortix keeps its configuration in a repo: `kortix.yaml` declares the machine image, the connectors and the triggers, while agents are `agents/<name>.md` files and skills are `skills/<name>/SKILL.md` files. An account is quick for one person. A repo can be cloned, diffed, reviewed and rolled back, which is how a team agrees on how the company runs.

The second axis is which model runs the work and where it runs. Claude Cowork runs on Anthropic's models. Kortix runs any model provider with your own keys, so the model and the platform are separate choices. Cowork runs on Anthropic's servers, or your own Amazon Bedrock, Google Cloud or Microsoft Foundry account, and Anthropic's pages do not offer a way to run the software on hardware you control. Kortix runs on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free.

The third axis is who reviews the work before it lands. In Cowork, when permissions settings are on, Claude shows its plan and waits for your approval before anything significant, and the finished files stay in your Claude account. In Kortix every session works on its own isolated machine and branch, and session work reaches `main` through a change request. Merge is default-deny for agents, so a person reads the diff before anything lands. That gate is what lets a team hand an agent work that matters, because someone can vouch for the change after reading it.

## How the two fit together

The two can run side by side. Keep Claude Cowork for tasks inside your Claude account, and run Kortix as the company repo. They keep separate state, so each uses its own accounts and keys. If you already use Cowork and want to move company work across, the [migration guide in this repository](migrate-from-claude-cowork-to-kortix.md) maps each Cowork concept to its Kortix location. For the wider field of open-source agent projects and how they differ, [open-source AI agent platforms](https://opensourceclaudecowork.com/open-source-ai-agent-platforms) compares them by where configuration lives and who reviews the work.

## Sources

Claude Cowork facts on this page come from Anthropic's [Claude Cowork product page](https://claude.com/product/cowork) and its [Get started with Claude Cowork](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) help article, read in October 2026. Kortix facts come from the [Kortix docs](https://kortix.com/docs) and the [Kortix on GitHub](https://github.com/kortix-ai/suna) repository.
