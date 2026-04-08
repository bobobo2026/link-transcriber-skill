# Link Transcriber Skill

`link-transcriber` is a minimal Codex-compatible skill for turning Douyin and Xiaohongshu links into a readable summary as quickly as possible.

It is meant to be a free, low-friction entry point: paste a link and get back only the final summary.

It uses the live linkTranscriber service at:

- `https://linktranscriber.store/linktranscriber-api`

The service uses server-side saved platform cookies when they are needed. End users do not need to provide cookies to use this skill.
For local validation and scripted usage, prefer the Python helper scripts in this repo over `curl`, because HTTPS compatibility can vary across system `curl` builds and TLS backends.

For local smoke or a private deployment, you can still override the default:

- `LINK_SKILL_API_BASE_URL`
- `LINK_SKILL_SUMMARY_PROVIDER_ID` (optional, default `deepseek`)
- `LINK_SKILL_SUMMARY_MODEL_NAME` (optional, default `deepseek-chat`)

Execution preference:

- use the bundled Python scripts in this skill as the primary hosted-service client
- do not treat ad-hoc `curl` commands as the canonical execution path

Published ClawHub page:

- `https://clawhub.ai/bobobo2026/link-transcriber`

The skill workflow is:

1. accept a Douyin or Xiaohongshu link
2. infer the platform when possible
3. create a transcription task using the server's saved platform cookies when required
4. poll until transcription finishes
5. call the summaries API
6. return only the final summary text

Operational guardrails:

- the stable public endpoint is `https://linktranscriber.store/linktranscriber-api`
- do not swap in a raw server IP for normal public usage
- if the service reports missing platform cookies, treat that as a hosted-service configuration problem instead of asking the end user for cookies by default
- poll through all in-progress statuses, not only `PENDING`
- when the user already pasted a supported link, execute directly instead of asking for confirmation first
- if the service fails, do not browse the page and write a fallback summary from page snippets
- final output should be either the summary itself or one short failure message

That narrow workflow is intentional. This public skill stays focused on link-to-summary only.

## Install In Codex

Install from this GitHub repository, then restart Codex so it picks up the skill.

You can also open the published ClawHub page:

- `https://clawhub.ai/bobobo2026/link-transcriber`

After installation, use it in natural language:

```text
Use $link-transcriber to summarize this link: https://xhslink.com/o/23s4jTem6em
```

Install directly from ClawHub into the Codex skills directory:

```bash
npx clawhub@latest --workdir ~/.codex --dir skills install link-transcriber --force
```

## Update

Refresh an existing local installation to the latest published version:

```bash
bash scripts/update_local_skill.sh
```

Pin to a specific version when needed:

```bash
bash scripts/update_local_skill.sh 0.1.6
```

What the update script does:

- installs the canonical slug `link-transcriber`
- removes the legacy local directory `~/.codex/skills/link-transcriber-skill-public` if it exists
- leaves one stable local entrypoint for Codex to discover

## Troubleshooting

- If you previously installed `link-transcriber-skill-public`, run `bash scripts/update_local_skill.sh` to migrate to the canonical local directory.
- If you invoke Codex from a shell, quote or stdin-wrap prompts that contain `$link-transcriber` so your shell does not expand `$...` before Codex sees it.
- If the hosted service is unreachable, the correct user-facing result is a short failure message rather than a manually inferred summary from the page.

## Behavior

- Supports Douyin and Xiaohongshu
- Uses server-side saved platform cookies when needed
- Platform is inferred from the URL when possible
- Built for quick first-use value and easy sharing
- Default summaries provider: `deepseek`
- Default summaries model: `deepseek-chat`
- Final user-facing output is only the summary text
- In-progress task states include `PARSING`, `DOWNLOADING`, `TRANSCRIBING`, `SUMMARIZING`, `FORMATTING`, and `SAVING`
- Failure behavior should be terse: no tool traces, no confirmation loops, and no substitute summary generated from manual page browsing

## Local Smoke

Check service health with Python first:

```bash
python3 scripts/check_service_health.py
```

Run the example script directly with Python:

```bash
python3 scripts/call_service_example.py 'https://xhslink.com/o/23s4jTem6em'
```

Override the API base URL if needed:

```bash
LINK_SKILL_API_BASE_URL=https://linktranscriber.store/linktranscriber-api \
python3 scripts/call_service_example.py 'https://xhslink.com/o/23s4jTem6em'
```

## Files

- `SKILL.md` - canonical skill behavior
- `agents/openai.yaml` - Codex UI metadata
- `scripts/check_service_health.py` - Python health check for the hosted service
- `scripts/call_service_example.py` - transcribe + poll + summarize example
- `scripts/update_local_skill.sh` - install or refresh the local Codex skill copy
- `CLAWHUB.md` - ClawHub-oriented publish copy
