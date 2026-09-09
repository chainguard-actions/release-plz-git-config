<!-- markdownlint-disable -->

# Hardening Report: release-plz--git-config/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **release-plz--git-config/v0.1.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v4`, which is pinned to a mutable tag (`v4`) rather than an immutable 40-character commit SHA. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. It should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/update_main_version.yml:12`

### script-injection (severity: high)

Sub-rule (a): A `${{ secrets.GITHUB_TOKEN }}` expression is interpolated directly inside a `run:` shell command string. Per the script-injection check, ANY `${{ ... }}` expression directly inside a `run:` block is a finding — the value is substituted into the shell command string before the shell parses it. The offending line is: `git remote set-url origin "https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${GITHUB_REPOSITORY}.git"`. The fix is to pass the token via an `env:` block and reference it as `$GH_TOKEN` (already double-quoted in context).

Locations:

- `.github/workflows/update_main_version.yml:16`

### script-injection (severity: high)

Sub-rule (b): The shell variable `$TAG_NAME` (derived from the workflow-controlled env var `GITHUB_REF`) is expanded unquoted in `echo $TAG_NAME | grep -Eo ...`. An unquoted expansion allows the shell to parse metacharacters (glob patterns, word-splitting) from the value. The fix is to quote the expansion: `echo "$TAG_NAME" | grep -Eo ...`.

Locations:

- `.github/workflows/update_main_version.yml:21`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed three findings in hardened/action/.github/workflows/update_main_version.yml:
1. Pinned actions/checkout@v4 to full SHA 11d5960a326750d5838078e36cf38b85af677262 (# v4 comment preserved).
2. Moved ${{ secrets.GITHUB_TOKEN }} out of the run: shell string into an env: block as GH_TOKEN, then referenced it as ${GH_TOKEN} in the git remote set-url command.
3. Quoted $TAG_NAME in both echo invocations (echo "$TAG_NAME") to prevent word-splitting and glob expansion from unquoted shell variable expansion.

