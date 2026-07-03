# Changelog

All notable changes to semtrim are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.3] - 2026-07-02

### Changed

- Docs: replaced the "unreliable for Bash" PostToolUse claim with validated
  findings. Verified against the installed Claude Code `2.1.199` binary: the
  PostToolUse handler parses `hookSpecificOutput.updatedToolOutput` against the
  tool's `outputSchema` and **silently reverts** to the original output on any
  schema mismatch (logged at `error`, never thrown). The replacement is
  therefore best-effort, not categorically broken.
  anthropics/claude-code#65122 is closed-as-duplicate and #54196 is closed-stale;
  neither confirms current-version honoring, so the PreToolUse command-wrap
  remains the reliable default. Read replacement is the dependable PostToolUse
  consumer.

### Fixed

- `npm test` now discovers test files through `find ... -exec`, keeping CI
  compatible with Node 20.

## [0.1.1] - 2026-06-26

### Added

- CI workflow (`.github/workflows/ci.yml`): node 20/22 test matrix running
  `node --check`, the test suite, both host-contract smoke scripts, and a
  version-sync gate; plus a lint job (actionlint, shellcheck, gitleaks).
- `scripts/bump-version.mjs`: single-pass version bump and `--check` sync gate
  across `package.json` and every plugin manifest.
- `.codex-plugin/` manifest (`plugin.json`, `hooks.json`, `marketplace.json`)
  for native Codex installation (PreToolUse-only).
- `.pre-commit-config.yaml` and `scripts/check-cleanup-traps.sh` enforcing
  secret scanning, shellcheck, syntax checks, version sync, the shell-script
  cleanup-trap convention, and the test suite.
- `justfile` task runner, `LICENSE` (MIT), GitHub issue/PR templates, and
  README badges.

### Changed

- `install/claude.sh` now traps its `mktemp` temp file on `EXIT` so a failed
  `jq` rewrite cannot leak a temp file.

### Fixed

- Claude plugin manifest no longer points at the standard `hooks/hooks.json`
  file, which Claude Code already auto-loads.

## [0.1.0] - 2026-06-26

### Added

- Initial release: command-aware semantic compression of agent tool output via
  a PreToolUse command-wrap (reliable) plus best-effort PostToolUse rewrite.
- Data-driven filter engine derived from JFrog Boost (clean-room), with a
  golden corpus of fixtures, generic salience fallback, and secret redaction.
- Claude Code and Codex install helpers and host-contract smoke scripts.
