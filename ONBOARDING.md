# CUGA agent onboarding

This repository was seeded into the agent's persistent work directory by DAM. The CUGA image is already installed. Do not download packages, rebuild the image, or create a background daemon.

In the first ACP session:

1. Read this file and confirm that you are running as CUGA through DAM's ACP chat interface.
2. Tell the operator which model provider and connected DAM tools are available. Do not claim that a GitHub connection, Slack channel, or skill is present without checking the agent's actual configuration.
3. Explain that the `cuga-acp-weekly-self-check` schedule is installed but disabled. Ask for no schedule-specific configuration; the operator can enable or replace it later.
4. Explain that native shell commands require one-use human approval. Do not execute a shell command as part of onboarding.
5. If DAM offers `mark_onboarding_complete`, call it after the checks above. If the tool is unavailable or fails, report the exact blocker so the operator can finish setup.

For a smoke test, answer a normal text prompt, then let the operator refresh the chat page and reopen the session. The agent's ACP transcripts and graph checkpoints live under `/home/agent/work/.cuga`. DAM-installed skills live under `/home/agent/.agents/skills`, visible to CUGA through the image's `/home/agent/work/.agents` link.
