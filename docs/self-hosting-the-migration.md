# Self-hosting the migration: what you own because Kortix is open source

Kortix is open source. After you move company work off Claude Cowork, the point of the move is what you keep: the git repo that is the company, the agent and skill files in it, the memory, and the secrets, all on infrastructure you can put where you want. Self-host is free, and you connect your own model keys. What you own comes first, then the checklist to run it on your own hardware.

## What you own after the move

The repo is the company. `kortix.yaml` declares the machine image, the connectors, the triggers, and secret names and grants. Agents are `agents/<name>.md` files, skills are `skills/<name>/SKILL.md` files, and memory is a `memory/` directory. Every one of those is text you can read, fork and audit. Claude Cowork cannot give a team this, because its projects, instructions and memory live in your Claude account.

The secrets stay yours. Secrets are encrypted at rest with a key per project. Connector credentials are brokered server-side and never enter the machine, so reconnecting a service as a connector does not leave a raw key in a session. Model calls route through the platform's LLM gateway, so you decide which providers the company uses.

The machines are per session. Every session gets its own isolated sandbox on its own branch. Thousands of agents can run in parallel on one config, each on its own cloud computer. On a self-hosted instance the control plane runs on your box while agent sessions run on sandbox compute off the box by default; fully isolated topologies, where the sandbox compute also runs inside your network, are scoped with Kortix directly.

## What runs on your box and what runs off it

On the box: the frontend, the API, the LLM gateway, and the database and storage volumes. Off the box: agent session compute, from a separate sandbox provider, with Daytona as the default and Platinum and E2B also supported. That boundary matters for capacity planning and for a security review, so plan for it before you install. If the control plane must sit inside your network, self-hosting gives you that; the whole control plane for company agent work, accounts, projects, repos, secrets, connectors, policy and audit, runs inside your own network.

## Where Claude Cowork and a self-hosted Kortix differ

Claude Cowork has no self-hosted edition. Its help article states that sessions run in the cloud on Anthropic's servers, in an isolated environment, and that sessions and files are saved to your Claude account; the product page adds that Cowork runs where your data lives, on a Claude account or on your own Amazon Bedrock, Google Cloud or Microsoft Foundry account. You choose the cloud account, and you do not run the software. A self-hosted Kortix runs the platform itself on your hardware, so the runtime, the configuration and the data sit on infrastructure you control.

## The self-host checklist

Work through these in order, and confirm each one before the next.

1. **Domain and DNS.** A production self-host needs a domain, with A or AAAA records for the domain and for `api.<domain>`, both pointing at the box's IP.
2. **Ports.** Ports 80 and 443 must be reachable so the bundled Caddy proxy can get a TLS certificate.
3. **CLI.** Install the Kortix CLI, which runs on macOS and Linux, with WSL on Windows: `curl -fsSL https://kortix.com/install | bash`, then `kortix version` to confirm it is on your path.
4. **Initialize the instance.** Run `kortix self-host init --domain kortix.example.com`. This writes the Compose file, `.env`, a Caddyfile and an updater script into `~/.config/kortix/self-host/<instance>/`. The guided flow asks for the admin email, the sandbox provider, optional Pipedream credentials for the connector catalog, and the update policy.
5. **Start the stack.** Run `kortix self-host start`, which brings up the frontend, the API, the LLM gateway and a bundled Supabase distribution. Use `kortix self-host status`, `kortix self-host logs` and `kortix self-host doctor` while it comes up.
6. **Configure the sandbox provider.** Run `kortix self-host configure`, an interactive prompt for the sandbox provider key and, optionally, a managed-git token.
7. **Sign up and connect.** Open your domain and sign up with the admin email. Connect GitHub under Settings → Git, and connect your own model key in the model picker.
8. **Create a project and link the repo.** Create a project in the dashboard, point the CLI at your instance with `kortix hosts use selfhost`, and link the repo that holds `agents/`, `skills/`, `memory/` and `kortix.yaml`: `kortix projects link <project-id>`.
9. **Run the first reviewed session.** Start a session and read what it proposes: `kortix sessions new --prompt "Summarize this week's commits and open a change request"`, then `kortix cr ls`. The session works on its own sandbox and its own branch, and its work reaches the default branch only when you merge the change request.

## Operating what you own

An instance updates itself and can be pinned to a version with `kortix self-host update --tag <version>`, or updates can be turned off. To back it up, copy three things: `volumes/db/data`, `volumes/storage`, and the instance `.env` under `~/.config/kortix/self-host/<instance>/`. There is no separate backup system. Manage instance environment values with `kortix self-host env ls`, `kortix self-host env rotate` and `kortix self-host env set`. For sizing, an 8 GiB host runs the stack with its defaults, because the API ships with a 640 MiB memory limit; on a 16 GiB host whose API traffic reaches that limit, raise it with `kortix self-host env set KORTIX_API_MEMORY_LIMIT=1024m`.

A self-hosted instance has no managed model lineup. You connect the providers you already pay for, and every model call routes through the LLM gateway on your own box. Any model provider with your own keys, or the ChatGPT plan you already pay for.

## Where to read more

[Self-host an open-source Claude Cowork alternative](https://opensourceclaudecowork.com/self-hosting) is the companion walkthrough for the same install, in shorter form. The [migration guide in this repository](migrate-from-claude-cowork-to-kortix.md) covers the file moves that come before this checklist, and the [Kortix docs](https://kortix.com/docs) list the full command surface. The [self-hosted AI agent platform](https://opensourceclaudecowork.com/self-hosted-ai-agent-platform) and [self-hosted AI workspace](https://opensourceclaudecowork.com/self-hosted-ai-workspace) guides cover day-to-day operation, and [AI agent orchestration](https://opensourceclaudecowork.com/ai-agent-orchestration) covers running many sessions at once. Self-host is free; [Try Kortix](https://kortix.com) shows the managed path beside it.
