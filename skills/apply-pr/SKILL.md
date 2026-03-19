---
name: apply-pr
description: Operate the `apply_pr`/`sastre` deployment CLI for applying GitHub pull requests on remote hosts using git format-patch + git am workflows. Use when users ask to deploy PRs to servers, check PR deployment state, mark deployment status, create changelogs by milestone, or troubleshoot apply_pr command usage, options, environment variables, and host URL formats.
---

# apply-pr

Use this skill to safely run and troubleshoot `sastre` / `apply_pr` commands.

## Validate preconditions

1. Ensure repository context is clear (`--owner`, `--repository`).
2. Ensure `GITHUB_TOKEN` exists before actions needing GitHub API.
3. Confirm target host and remote source path before deploy (`--host`, `--src`).
4. Refuse contradictory deploy modes:
   - Do not combine `--re-deploy` with `--as-diff`.

## Core commands

- Deploy a PR:
  - `sastre deploy --pr <id_or_url> --host <ssh://user:pass@host:22> --environ <pre|pro|test> [--owner gisce] [--repository erp]`
- Deploy multiple PRs:
  - `sastre deploy --prs "123 124 130" --host <...> --environ pre`
- Check PR set state:
  - `sastre check_prs --prs "123 124" [--separator " "] [--owner ...] [--repository ...]`
- Mark deploy status manually:
  - `sastre status <deploy-id> <success|error|failure> --owner <...> --repository <...>`
- Generate changelog by milestone:
  - `sastre create_changelog -m <milestone> [--issues] [--changelog_path /tmp]`

## Operational guardrails

- Prefer explicit `--owner` and `--repository`; do not assume defaults outside known environments.
- For risky production deploys, ask for confirmation before execution.
- Use `--exit-code-failure` in automation where failure must propagate to CI.
- Use `--auto-exit` when expecting clean rollback (`git am --abort`) on apply errors.
- Use `--skip-rolling-check` only with explicit user approval.

## Troubleshooting checklist

1. Auth errors:
   - Verify `GITHUB_TOKEN` is present and valid.
2. Host parsing errors:
   - Accept raw hostname and normalize to ssh URL when needed.
3. Partial deploy failures with `--prs`:
   - Inspect failed PR list output and retry only failed IDs.
4. Deployment state mismatch:
   - Use `sastre status` with deploy id to reconcile status in GitHub.

## References

- For option map and known caveats, read `references/commands.md`.
