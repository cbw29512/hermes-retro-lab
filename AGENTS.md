# Instructions for Hermes and coding agents

**Scope**: This public repository is a beta laboratory. The agent may be fully autonomous **inside its VM**. The host is not its playground.

## Boundaries enforced outside the prompt
- Run in a **dedicated VM**, not a privileged container on the host.
- VM filesystems must not mount host home directories, Docker/Podman sockets, SSH agents, or other repo checkouts.
- VM must not have unrestricted access to the host network or local LAN. Internet access, if enabled, should be filtered; the host inference API must be reached through a **single explicitly allowed gateway**, not broad host access.
- Provision a GitHub credential limited to **cbw29512/hermes-retro-lab** only, with the minimum branch/PR permissions. Do not store any credentials in this public repository.
- Deny external publishing, payments, blockchain signing, minting, email outreach, and cloud deployments during this test.
- Snapshot the VM before each autonomous experiment. Set VM CPU/RAM/disk limits; host Colibri stays isolated and retains its 65,536-token context.
- Work on a dedicated Git branch; no automated merges, forced pushes, or changes to production branches.

## Agent execution principles
- **State first**: Read `docs/DATA_SCHEMA.md` and `state.json` before coding.
- For new products, define product-level acceptance criteria and the product's data model before implementation.
- Keep modules below 150 lines when practical; split larger modules by responsibility.
- Use meaningful tests and error handling with explicit logging in Python code.
- Never claim a task passed without running its checks.
- Don't copy trademarked art or unlicensed retro game assets.
- Make only public-safe commits. Do not commit internal network details, tokens, secrets, personal data, or private files.

This file guides agent behavior. **It is not a security barrier.** The VM boundary, network policy, credentials, and branch protections must be configured independently.
