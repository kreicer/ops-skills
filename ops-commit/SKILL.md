---
name: ops-commit
description: >-
  Writes short, specific commit messages for infrastructure and ops
  repositories (Terraform, Kubernetes, Helm, CI pipelines, Dockerfiles,
  service configs) with a fixed prefix such as tf:, k8s:, ci:, fix: or
  update:. Use when the user asks for a commit message, asks to commit, or
  wants to describe staged changes in an ops repo.
license: Apache-2.0
---

# Ops commit messages

## Read the change

Use `git diff --staged`; if nothing is staged, use `git diff` and say so. Read the diff, not just file names. If the repo has its own convention (`AGENTS.md`, `CONTRIBUTING.md`, consistent history in `git log --oneline -20`), follow it instead of this one.

Never copy secret values, tokens, internal hostnames or IPs from the diff into the message.

## Pick one prefix

Change-type prefixes win over area prefixes:

| Prefix | Use for |
| --- | --- |
| `revert:` | Reverting an earlier commit |
| `sec:` | Security: permissions, secret handling, exposure, CVE fixes |
| `fix:` | Something was broken or wrong and now works |
| `update:` | Version bumps: images, charts, providers, modules, packages |
| `docs:` | README, runbooks, comments only |

Otherwise use the area:

| Prefix | Use for |
| --- | --- |
| `tf:` | Terraform or OpenTofu |
| `k8s:` | Kubernetes manifests, Helm, Kustomize |
| `ci:` | Pipelines and CI templates |
| `docker:` | Dockerfiles and image builds |
| `cfg:` | Service configs: nginx, Prometheus, Vector, Ansible and similar |
| `chore:` | Repo housekeeping: `.gitignore`, linters, formatting |

Add `!` (`tf!:`, `k8s!:`) when the change destroys or replaces stateful resources, causes downtime, or needs a manual step. Say which in the body.

If the diff mixes unrelated changes, suggest splitting it. If the user declines, use the prefix of the main change.

## Write the subject

- `prefix: what changed, where`. Lowercase, imperative, no trailing period, at most 72 characters.
- Name the concrete thing: resource, service, environment, version. `tf: add read replica for billing db in prod`, not `tf: update main.tf`.
- Versions as `old -> new`: `update: grafana chart 8.5.1 -> 8.6.0`.
- `fix:` names the symptom that is gone: `fix: blackbox probe timeout on api healthcheck`.
- No filler: no "this commit", "various", "some changes", "improve", "minor".

## Body

Usually none. Add one to three short lines only for what the diff cannot show: why, risk, manual steps, related ticket. Separate it from the subject with a blank line. No file lists, no restating the diff, no emojis, no trailers unless asked.

## Output

Show the message in a code block. Commit only if the user asked to.
