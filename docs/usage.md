# Usage

Use Codex in two ways: interactively with `codex` (TUI) or non‑interactively with `codex exec` for automation and CI. This page highlights normal usage first, followed by advanced topics.

---

### CLI reference

| Command            | Purpose                            | Example                         |
| ------------------ | ---------------------------------- | ------------------------------- |
| `codex`            | Interactive TUI                    | `codex`                         |
| `codex "..."`      | Initial prompt for interactive TUI | `codex "fix lint errors"`       |
| `codex exec "..."` | Non‑interactive "automation mode" | `codex exec "explain utils.ts"` |

Key flags:

- `--model/-m`
- `--ask-for-approval/-a` (e.g., `untrusted`, `on-failure`; see [approval_policy](../codex-rs/config.md#approval_policy))
- `--profile <name>`
- `--config key=value`

### Approvals and sandboxing

Codex can ask before running model‑suggested commands and runs them inside an OS‑level sandbox.

- Defaults: `approval_policy=untrusted` and `sandbox_mode=read-only`
- `--full-auto`: `approval_policy=on-failure` and `sandbox_mode=workspace-write` (network stays disabled)
- Details: see [approval_policy](../codex-rs/config.md#approval_policy) and [sandbox_mode](../codex-rs/config.md#sandbox_mode)

### Recipes

Below are a few bite-size examples you can copy-paste. Replace the text in quotes with your own task. See the [prompting guide](https://github.com/openai/codex/blob/main/codex-cli/examples/prompting_guide.md) for more tips and usage patterns.

| ✨  | What you type                                                                   | What happens                                                               |
| --- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 1   | `codex "Refactor the Dashboard component to React Hooks"`                       | Codex rewrites the class component, runs `npm test`, and shows the diff.   |
| 2   | `codex "Generate SQL migrations for adding a users table"`                      | Infers your ORM, creates migration files, and runs them in a sandboxed DB. |
| 3   | `codex "Write unit tests for utils/date.ts"`                                    | Generates tests, executes them, and iterates until they pass.              |
| 4   | `codex "Bulk-rename *.jpeg -> *.jpg with git mv"`                               | Safely renames files and updates imports/usages.                           |
| 5   | `codex "Explain what this regex does: ^(?=.*[A-Z]).{8,}$"`                      | Outputs a step-by-step human explanation.                                  |
| 6   | `codex "Carefully review this repo, and propose 3 high impact well-scoped PRs"` | Suggests impactful PRs in the current codebase.                            |
| 7   | `codex "Look for vulnerabilities and create a security review report"`          | Finds and explains security bugs.                                          |

---

## Advanced usage

### Non‑interactive / CI mode

Run Codex head‑less in pipelines. Example GitHub Action step:

```yaml
- name: Update changelog via Codex
  run: |
    npm install -g @openai/codex
    export OPENAI_API_KEY="${{ secrets.OPENAI_KEY }}"
    codex exec --full-auto "update CHANGELOG for next release"
```

### Tracing / verbose logging

Because Codex is written in Rust, it honors the `RUST_LOG` environment variable to configure its logging behavior.

The TUI defaults to `RUST_LOG=codex_core=info,codex_tui=info` and log messages are written to `~/.codex/log/codex-tui.log`, so you can leave the following running in a separate terminal to monitor log messages as they are written:

```
tail -F ~/.codex/log/codex-tui.log
```

By comparison, the non‑interactive mode (`codex exec`) defaults to `RUST_LOG=error`, but messages are printed inline, so there is no need to monitor a separate file.

See the Rust documentation on [`RUST_LOG`](https://docs.rs/env_logger/latest/env_logger/#enabling-logging) for more information on the configuration options.

### Memory & project docs

You can give Codex extra instructions and guidance using `AGENTS.md` files. Codex looks for `AGENTS.md` files in the following places, and merges them top-down:

1. `~/.codex/AGENTS.md` - personal global guidance
2. `AGENTS.md` at repo root - shared project notes
3. `AGENTS.md` in the current working directory - sub-folder/feature specifics

### Pinning Codex with DotSlash

- Each GitHub release includes a DotSlash file named `codex`. With the DotSlash CLI installed, you can commit that file to your repo and run `./codex` to fetch and execute the correct binary for your platform.
- Why: ensures every contributor and CI uses the same Codex version without manual installs.
- Steps:
  - Download the `codex` DotSlash file from the release and add it to your repo.
  - Ensure the DotSlash CLI is installed on developer machines and CI.
  - Run `./codex ...` anywhere in the repo; DotSlash will download/cache the correct binary.
- Learn more: https://dotslash-cli.com/
