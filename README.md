# ops-skills

Agent skills for DevOps and security work, in the open [Agent Skills](https://agentskills.io) format. Each skill is a folder with a `SKILL.md`; any agent that supports the format (Cursor, Claude Code and others) loads it when the task matches its description.

## Skills

| Skill | What it does |
| --- | --- |
| [web-log-triage](web-log-triage/SKILL.md) | Reads web and HTTP request logs in any format and tells background scanning from targeted probing and from possible successful exploitation, based on known attack patterns. Treats log content as untrusted input, so payloads in URLs or User-Agents cannot steer the agent. |
| [ops-commit](ops-commit/SKILL.md) | Writes short, specific commit messages for infrastructure repositories with a fixed prefix (`tf:`, `k8s:`, `ci:`, `fix:`, `update:` and others), so history says what changed and where. |

## Install

Clone the repository and link the skills you want into your agent's skills directory:

```sh
git clone https://github.com/kreicer/ops-skills.git
ln -s "$PWD/ops-skills/web-log-triage" ~/.cursor/skills/web-log-triage   # Cursor
ln -s "$PWD/ops-skills/web-log-triage" ~/.claude/skills/web-log-triage   # Claude Code
```

Repeat the `ln -s` line for each skill you want. For a single project, link into `.cursor/skills/` or `.claude/skills/` inside that project instead. `git pull` updates linked skills.

## Usage

Ask in plain language; the agent picks the skill up from its description:

- "Here is a chunk of our access log, are we being attacked?"
- "Check the last hour of logs for api.example.com in Loki for anything malicious."
- "Write a commit message for the staged changes."

Environment-specific details (log store address, labels, your own IP ranges, a team commit convention) do not belong in the skills. Put them in your repository's `AGENTS.md` or give them in the chat; skills follow them.

## Contributing

Issues and pull requests are welcome. A skill should stay short: only instructions the agent would get wrong without it. Validate before sending:

```sh
pip install skills-ref
agentskills validate <skill-folder>
```

## License

[Apache-2.0](LICENSE)
