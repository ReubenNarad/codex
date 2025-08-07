<h1 align="center">OpenAI Codex CLI</h1>
<p align="center">Lightweight coding agent that runs in your terminal</p>

<p align="center"><code>npm i -g @openai/codex</code><br />or <code>brew install codex</code></p>

This is the home of the **Codex CLI**, OpenAI's coding agent that runs locally on your computer. If you are looking for the _cloud-based agent_ from OpenAI, **Codex [Web]**, see <https://chatgpt.com/codex>.

<!-- ![Codex demo GIF using: codex "explain this codebase to me"](./.github/demo.gif) -->

---

> ⚠️ **Experimental**
>
> Codex CLI is under active development. We welcome issues and PRs — see [contributing](./docs/contributing.md).

## Quickstart

Install globally with your preferred package manager:

```shell
npm install -g @openai/codex  # Alternatively: `brew install codex`
```

Or go to the [latest GitHub Release](https://github.com/openai/codex/releases/latest) and download the appropriate binary for your platform.

### ChatGPT Plus/Pro Users

If you have a ChatGPT Plus or Pro account, login with:

```
codex login
```

To verify whether you are currently logged in:

```
codex login status
```

This should create a `~/.codex/auth.json` file that contains the credentials that Codex will use. If you encounter problems with the login flow, please comment on <https://github.com/openai/codex/issues/1243>.


### OpenAI API Users

Next, set your OpenAI API key as an environment variable:

```shell
export OPENAI_API_KEY="your-api-key-here"
```

> [!NOTE]
> This command sets the key only for your current terminal session. You can add the `export` line to your shell's configuration file (e.g., `~/.zshrc`), but we recommend setting it for the session.

Codex also supports open-source models and custom providers. For information on how to configure these options using the `--oss` flag or a custom provider setup, see [docs/config.md](./docs/config.md).

### Running `codex`

After authentication, you can run Codex interactively:

```shell
codex
```

Or, run with a prompt as input:

```shell
codex "explain this codebase to me"
```

That's it — Codex will scaffold a file, run it inside a sandbox, install any
missing dependencies, and show you the live result. By default, Codex asks for
permission before running commands or making changes; run `--full-auto` to
auto-approve commands.


---

## Usage

Run `codex` to open the interactive TUI, or pass a prompt inline, e.g. `codex "explain this repo"`. For automation/CI, use `codex exec "update CHANGELOG for next release"`.

Examples:

```bash
codex "Refactor the Dashboard component to React Hooks"
codex "Write unit tests for utils/date.ts"
codex "Explain what this regex does: ^(?=.*[A-Z]).{8,}$"
```

By default Codex asks before running commands and executes them in a read‑only sandbox; use `--full-auto` to allow commands and workspace writes (network remains disabled). See `docs/usage.md` for examples and advanced options.



## System Requirements

- Operating systems: macOS 12+, Ubuntu 20.04+/Debian 10+, Windows 11 (WSL2)
- Git (optional, recommended): 2.23+ for built-in PR helpers
- RAM: 4 GB minimum (8 GB recommended)

---


## Configuration

Codex supports a rich set of configuration options documented in [`codex-rs/config.md`](./codex-rs/config.md).

By default, Codex loads its configuration from `~/.codex/config.toml`. Additionally, `--config` can be used to set/override ad-hoc config values for individual invocations of `codex`.

For more information, see [config.md](./docs/config.md).

---

## FAQ

<details>
<summary>OpenAI released a model called Codex in 2021 - is this related?</summary>

In 2021, OpenAI released Codex, an AI system designed to generate code from natural language prompts. That original Codex model was deprecated as of March 2023 and is separate from the CLI tool.

</details>

<details>
<summary>Which models are supported?</summary>

Any model available with [Responses API](https://platform.openai.com/docs/api-reference/responses). The default is `o4-mini`, but pass `--model gpt-4.1` or set `model: gpt-4.1` in your config file to override.

</details>
<details>
<summary>Why does <code>o3</code> or <code>o4-mini</code> not work for me?</summary>

It's possible that your [API account needs to be verified](https://help.openai.com/en/articles/10910291-api-organization-verification) in order to start streaming responses and seeing chain of thought summaries from the API. If you're still running into issues, please let us know!

</details>

<details>
<summary>How do I stop Codex from editing my files?</summary>

Codex runs model-generated commands in a sandbox. If a proposed command or file change doesn't look right, you can simply type **n** to deny the command or give the model feedback.

</details>
<details>
<summary>Does it work on Windows?</summary>

Not directly. It requires [Windows Subsystem for Linux (WSL2)](https://learn.microsoft.com/en-us/windows/wsl/install) - Codex has been tested on macOS and Linux with Node 22.

</details>

---

## Codex Open Source Fund

We're excited to launch a **$1 million initiative** supporting open source projects that use Codex CLI and other OpenAI models.

- Grants are awarded up to **$25,000** API credits.
- Applications are reviewed **on a rolling basis**.

**Interested? [Apply here](https://openai.com/form/codex-open-source-fund/).**

---

## Security & Responsible AI

Have you discovered a vulnerability or have concerns about model output? Please e-mail **security@openai.com** and we will respond promptly.

---

## License

This repository is licensed under the [Apache-2.0 License](LICENSE).
