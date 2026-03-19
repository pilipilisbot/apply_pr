# apply_pr / sastre command reference

## Environment

- Required for GitHub operations:
  - `GITHUB_TOKEN`
- Optional:
  - `LOG_LEVEL` (defaults to ERROR in CLI code)

## Commands observed

- `sastre deploy`
  - Main deployment command
  - Key flags: `--pr`, `--prs`, `--host`, `--environ`, `--from-number`, `--from-commit`, `--re-deploy`, `--as-diff`, `--auto-exit`, `--exit-code-failure`, `--no-set-label`
- `sastre check_prs`
  - Check status for list of PRs
- `sastre status`
  - Mark deployment status (`success|error|failure`)
- `sastre create_changelog`
  - Build changelog from milestone
- `apply_pr`
  - Deprecated alias; prefer `sastre deploy`

## Important behavior from source

- `--re-deploy` and `--as-diff` are mutually exclusive and should fail fast.
- If neither `--pr` nor `--prs` is provided, deploy exits with error.
- For `--prs`, PR IDs are split by spaces.
- Host strings without explicit scheme are normalized to ssh format internally.

## Known modernization gaps (from analysis)

- Legacy packaging (`setup.py`, `requirements.txt`) without `pyproject.toml`
- Legacy CI file (`.travis.yml`) and no modern GitHub Actions workflow
- Minimal/no automated tests in repository
- Requests in auth flow do not use explicit timeouts or robust network error wrappers
