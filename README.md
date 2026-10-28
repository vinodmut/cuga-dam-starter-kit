# CUGA starter kit for DAM

This starter kit creates a general-purpose CUGA agent on [DAM](https://github.com/dam-agents/dam). It uses the published, tested CUGA image and DAM's Agent Client Protocol (ACP) chat path. The kit adds a repository-backed definition, an onboarding prompt, optional GitHub and Slack setup, and a disabled example schedule. It does not install CUGA at agent creation or start a background daemon; DAM starts `harness-chat` and `cuga-acp` for each ACP session.

The kit pins `docker.io/vinodmut/cuga-dam@sha256:79c388a9a01fb4693827fda8ba6c1d85eeeddf7f4ac2d9c813a461dd405c3da6`, built from [CUGA commit `c0b867ee`](https://github.com/cuga-project/cuga-agent/commit/c0b867eea0769cd0fd2785cd509ffc7a6aa21818). The image includes CUGA, its Python environment, a cached policy embedding model, ACP and terminal launchers, and native shell and filesystem tools. It contains no usable model, GitHub, or Slack credential. Shell commands require one-use approval by default.

## Create a linked-kit agent

1. Open [the linked kit in DAM](https://dam.res.ibm.com/starter-kits/link/vinodmut/cuga-dam-starter-kit), or paste `https://github.com/vinodmut/cuga-dam-starter-kit` into **Starter kits**. Review the pinned image, open network preset, and disabled schedule.
2. Name the agent and select an IBM LiteLLM model-provider connection. GitHub access is optional; grant a GitHub or GitHub Enterprise connection if the agent needs that service. A Slack channel binding is optional and can be added later.
3. Create the agent. DAM seeds this repository into its persistent work directory and opens an onboarding ACP session guided by [ONBOARDING.md](ONBOARDING.md).
4. In the web chat, send a simple text request. Refresh the page and reopen the session to check persistence.

The kit requests **1 vCPU and 2 GiB memory**, the smallest configuration that passed the prior ten-session CUGA Slack/GitHub load run without failed turns or restarts. Larger allocations remain available if a different workload needs them. The kit's sample weekly schedule is disabled; enabling it incurs model usage.

## Verify and extend

Replace `<agent>` with the created agent's name:

```sh
dam agent get <agent> --json
dam run <agent> --prompt 'Reply with the word ready and explain which DAM tool categories you can see.'
dam session list <agent> --json
dam schedule list <agent> --json
dam skill list <agent> --json
dam connection list <agent> --json
```

`dam run` and DAM web chat use ACP. `dam chat` opens the separate terminal console. To use a connected service, grant its connection to the agent; to receive Slack mentions, bind a Slack channel. DAM controls those grants independently of the image. Install skills with `dam skill install`, and add task-specific schedules with `dam schedule create` or the web UI. DAM-installed skills are exposed to CUGA through `/home/agent/work/.agents`, a link the image prepares when ACP starts.

The disabled `cuga-acp-weekly-self-check` schedule is a safe example. It writes only to its DAM session and is not needed for normal chat. Enable it after onboarding if a recurring check is useful.

## Kit design and updates

- [`kit.yaml`](kit.yaml) declares the pinned image, repository seed, optional integrations, resource size, network preset, and example schedule. It carries no credentials or instance-specific repository and channel IDs.
- [`ONBOARDING.md`](ONBOARDING.md) describes the first ACP turn and the setup check.
- The repository seeds into `/home/agent/work`. Do not add `.agents` to the repository: the image creates `/home/agent/work/.agents` as a link to DAM's installed-skill directory.
- Linked-kit updates follow this repository's default branch. An update to `kit.yaml` does not silently replace a running agent's pinned image; review and apply changes per agent.

The image itself remains in the [CUGA repository](https://github.com/cuga-project/cuga-agent/tree/main/deployment/dam). The [DAM starter-kit documentation](https://github.com/dam-agents/dam/blob/main/docs/architecture/starter-kits.md) describes linked kits, seeding, and updates.
